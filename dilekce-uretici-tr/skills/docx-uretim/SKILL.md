---
name: docx-uretim
description: "Use this skill whenever producing a UYAP-compatible .docx dilekçe — mahkeme dilekçesi, itiraz dilekçesi, cevap dilekçesi, dava dilekçesi, ihtarname, vekaletname, layiha. Triggers: 'dilekçe üret', 'UYAP docx', 'word dosyası oluştur', 'dilekçe taslağı', any request involving a .docx output for Turkish courts or Sigorta Tahkim Komisyonu. This is the technical core called by all other plugins (sigorta-tahkim-tr, arac-deger-kaybi-tr, ihtarname-tr) — enforces Times New Roman 12pt, A4, justified, 1.15 spacing. Do NOT use for markdown, PDF, or non-Turkish-court documents."
argument-hint: "[dilekçe içeriği veya yol]"
---

# UYAP uyumlu .docx dilekçe üretimi

Bu skill, Türk mahkemelerine ve Sigorta Tahkim Komisyonu'na sunulan dilekçelerin teknik standardını uygulayan **çekirdek üretim modülüdür**. Diğer plugin'ler (sigorta-tahkim-tr, arac-deger-kaybi-tr, ihtarname-tr) bu skill'i çağırır.

## Çalışma akışı

1. `CLAUDE.md`'yi oku — avukat bilgileri, imza bloğu, klişe ifadeler
2. Çağıran skill'den gelen yapılandırılmış içeriği al (mahkeme, taraflar, konu, açıklamalar, hukuki sebepler, deliller, talep)
3. docx-js ile dosyayı oluştur (aşağıdaki teknik kurallarla)
4. `/mnt/skills/public/docx/scripts/office/validate.py` ile doğrula
5. `/mnt/user-data/outputs/` altına yaz, dosya adı `CLAUDE.md`'deki kurala göre

## TEKNİK STANDARTLAR (sabit — değişmez)

### Sayfa

```javascript
properties: {
  page: {
    size: {
      width: 11906,   // A4 genişlik (DXA)
      height: 16838,  // A4 yükseklik (DXA)
    },
    margin: {
      top: 1417,      // 2.5 cm
      right: 1417,
      bottom: 1417,
      left: 1417
    }
  }
}
```

### Stiller

```javascript
styles: {
  default: {
    document: {
      run: {
        font: "Times New Roman",
        size: 24             // 12pt (yarı punto)
      },
      paragraph: {
        spacing: { line: 276, lineRule: "auto" },  // 1.15 satır aralığı
        alignment: AlignmentType.JUSTIFIED          // iki yana yaslı
      }
    }
  },
  paragraphStyles: [
    {
      id: "MahkemeBaslik",
      name: "Mahkeme Başlık",
      basedOn: "Normal",
      run: { bold: true, size: 24, font: "Times New Roman" },
      paragraph: {
        alignment: AlignmentType.CENTER,
        spacing: { before: 0, after: 360 }
      }
    },
    {
      id: "DilekceAltBaslik",
      name: "Dilekçe Alt Başlık",
      basedOn: "Normal",
      run: { bold: true, size: 24, font: "Times New Roman" },
      paragraph: {
        alignment: AlignmentType.LEFT,
        spacing: { before: 240, after: 120 }
      }
    }
  ]
}
```

### Standart dilekçe iskeleti

Her Türk mahkeme dilekçesi şu sırayı izler:

```
1. MAHKEME / KOMISYON BAŞLIĞI (ortalı, kalın)
2. DOSYA NO / ESAS NO (varsa)
3. DAVACI / DAVALI bilgileri (sola yaslı, kalın etiketler)
4. VEKİL bilgileri
5. KONU
6. AÇIKLAMALAR (numaralı paragraflar — 1., 2., 3., ...)
7. HUKUKİ SEBEPLER
8. HUKUKİ DELİLLER
9. SONUÇ VE TALEP
10. Tarih (sola yaslı)
11. İmza bloğu (sağa yaslı)
```

### KRİTİK kurallar

#### 1. İsim formatlama

**Tüm isimleri sadece ilk harf büyük yaz.** UYAP standardı bunu gerektirir.

```javascript
// ✗ YANLIŞ
"MELİH YILMAZ", "AHMET KAYA"

// ✓ DOĞRU
"Melih Yılmaz", "Ahmet Kaya"
```

Bu kural taraflar, vekiller, hakem, hakim, tanık — TÜM isimler için geçerlidir.

#### 2. Türkçe karakter

docx-js Türkçe karakterleri (ı, ğ, ü, ş, ö, ç, İ) doğru işler. Ek bir kodlama ayarı gerekmez. Validasyon sonrası bir karakterin bozulduğunu görürsen, ilgili `TextRun`'ı incele — string olarak değil JavaScript template literal olarak verilmesi gerekir.

#### 3. Tarih formatı

```
13.05.2026         ✓ doğru
13/05/2026         ✗ yanlış (UYAP standardı değil)
2026-05-13         ✗ yanlış
13 Mayıs 2026      ✓ kabul edilebilir (kapanış imzasında)
```

#### 4. Para birimi formatı

Türkçe ondalık ve binlik ayraçları:

```
1.234,56 TL        ✓ doğru
1,234.56 TL        ✗ yanlış
₺1.234,56          ✗ yanlış (UYAP'ta "TL" yazısı tercih edilir)
```

Otomatik dönüşüm yardımcısı:

```javascript
function tlFormat(n) {
  return new Intl.NumberFormat('tr-TR', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2
  }).format(n) + ' TL';
}
// tlFormat(1234.5) → "1.234,50 TL"
```

#### 5. Numaralı açıklamalar

`AÇIKLAMALAR` bölümünde her paragraf "1.", "2.", "3." ile başlar. **Numbered list değil — manuel paragraf başı olarak yaz**, çünkü mahkemeler bunu beklediği gibi görür. Numaradan sonra TAB değil, BOŞLUK kullanılır.

```javascript
new Paragraph({
  alignment: AlignmentType.JUSTIFIED,
  indent: { firstLine: 720 },  // ilk satır girintisi
  children: [
    new TextRun({ text: "1. ", bold: true }),
    new TextRun("Müvekkil davalıya ait ... aracı, 12.03.2026 tarihinde...")
  ]
})
```

#### 6. SONUÇ VE TALEP — alt maddeler

Talep kısmı **harfli alt maddeler** (a, b, c) kullanır:

```
SONUÇ VE TALEP : Yukarıda arz ve izah olunan nedenlerle;
  a) ... talebimizin KABULÜNE,
  b) Yargılama giderlerinin DAVALI'ya yükletilmesine,
  c) Vekalet ücretinin karşı tarafa yükletilmesine,
karar verilmesini saygılarımla arz ve talep ederim.
```

Önemli: KABUL kararı verilmesi istenen taleplerde fiil **BÜYÜK HARFLE** yazılır (KABULÜNE, YÜKLETİLMESİNE, REDDİNE — bu UYAP geleneğidir, mahkemenin kararını yazarken karşılığını bulmasını kolaylaştırır).

### Dosya adlandırma

`CLAUDE.md`'deki kurala göre. Varsayılan:

```
{TARIH_YYYYMMDD}_{DILEKCE_TIPI}_{MUVEKKIL_SOYAD}_{DOSYA_NO}.docx
```

Türkçe karakterleri ASCII'ye çevir, boşlukları `_` yap:

```
20260513_itiraz_dilekce_Yilmaz_2026-K-123.docx
```

### Validasyon

Üretimden sonra mutlaka çalıştır:

```bash
python /mnt/skills/public/docx/scripts/office/validate.py output.docx
```

Hata verirse:
1. Hata mesajını analiz et
2. docx-js script'ini düzelt
3. Yeniden üret
4. Tekrar doğrula

Geçersiz bir dosya **kullanıcıya teslim edilmez**.

### Teslim

`/mnt/user-data/outputs/` klasörüne yaz, `present_files` ile sun.

## Çağrılma deseni (diğer plugin'lerden)

Diğer plugin'ler bu skill'i şu formatla çağırır:

```yaml
dilekce_tipi: "itiraz" | "cevap" | "dava" | "ihtarname" | "layiha"
mahkeme_basligi: "ANKARA 5. ASLİYE TİCARET MAHKEMESİNE"
dosya_no: "2026/123 E."  # varsa
davaci: { ad: "...", tckn: "...", adres: "..." }
davali: { ad: "...", adres: "..." }
vekil: # CLAUDE.md'den otomatik dolar
konu: "..."
aciklamalar: [
  "Müvekkilim ...",
  "Davalı tarafın ...",
  ...
]
hukuki_sebepler: ["TBK m. 49", "KTK m. 91", "..."]
hukuki_deliller: ["...", "..."]
sonuc_ve_talep: [
  "a) ... talebimizin KABULÜNE,",
  "b) ..."
]
yargitay_atiflari: [  # opsiyonel
  { mahkeme: "Yargıtay 17. HD", esas: "2020/1234", karar: "2021/5678", tarih: "12.05.2021", ozet: "..." }
]
```

## Geliştirme notu

Yargıtay atıfları gelirse, AÇIKLAMALAR bölümünün uygun yerinde dipnot veya parantez içinde yer alır:

> "...benzer bir uyuşmazlıkta Yargıtay 17. Hukuk Dairesi, 2020/1234 E., 2021/5678 K. sayılı 12.05.2021 tarihli kararında..."
