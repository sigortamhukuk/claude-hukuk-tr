---
name: dilekce-format-kontrol
description: "Use this skill to validate an existing .docx dilekçe against UYAP formatting standards — checks font (Times New Roman 12pt), page size (A4), margins (2.5cm), alignment (justified), name capitalization (only first letter), date format (GG.AA.YYYY), currency format (1.234,56 TL). Triggers: 'bu dilekçeyi kontrol et', 'UYAP uyumlu mu', 'format denetimi', 'dilekçe denetle'. Run after producing a dilekçe or when the user uploads one for review."
argument-hint: "[dosya yolu]"
---

# Dilekçe format kontrolü — UYAP uyumluluk denetimi

## Akış

1. Dosyayı `unpack` ile aç (`/mnt/skills/public/docx/scripts/office/unpack.py`)
2. `word/styles.xml` ve `word/document.xml`'i oku
3. Aşağıdaki kontrolleri sırayla yap
4. Bulguları kategorize et: **HATA** (mutlaka düzelt), **UYARI** (düzeltilmesi tavsiye), **BİLGİ** (notla geç)
5. Bir tablo halinde raporla

## Kontrol listesi

### Sayfa düzeni

| Kontrol | Beklenen | Hata seviyesi |
|---------|----------|---------------|
| Sayfa boyutu | A4 (11906 x 16838 DXA) | HATA |
| Üst kenar boşluğu | 1417 ± 100 DXA (2.5 cm) | UYARI |
| Alt/sol/sağ kenar | 1417 ± 100 DXA | UYARI |
| Sayfa yönü | Dikey (portrait) | HATA |

### Tipografi

| Kontrol | Beklenen | Hata seviyesi |
|---------|----------|---------------|
| Varsayılan yazı tipi | Times New Roman | HATA |
| Varsayılan punto | 24 (12pt) | HATA |
| Satır aralığı | 276 (1.15) | UYARI |
| Paragraf hizalama | justified | UYARI |

### İçerik kuralları

Bu kontroller `document.xml` içindeki metin üzerinde regex ile yapılır.

**1. İsim formatı kontrolü (KRİTİK)**

Tamamen büyük harfle yazılmış 2+ kelimelik isim grupları HATA verir.

Regex: `\b[A-ZÇĞİÖŞÜ]{2,}\s+[A-ZÇĞİÖŞÜ]{2,}\b`

İstisna: Mahkeme başlığı (ör. "ANKARA 5. ASLİYE TİCARET MAHKEMESİNE"), "T.C.", kurum adları (ör. "ANADOLU SİGORTA A.Ş."), bölüm başlıkları (KONU, AÇIKLAMALAR, SONUÇ VE TALEP, HUKUKİ SEBEPLER, HUKUKİ DELİLLER), ve SONUÇ kısmındaki kabul fiilleri (KABULÜNE, REDDİNE, YÜKLETİLMESİNE).

Bunlar dışında kalan büyük harf öbekleri muhtemelen yanlış formatlanmış isimlerdir → kullanıcıya göster.

**2. Tarih formatı**

| Bulunan | Doğru |
|---------|-------|
| `13/05/2026` veya `13-05-2026` | `13.05.2026` |
| `2026-05-13` | `13.05.2026` |
| `13 Mayıs 2026` | İmza bloğunda ✓, gövdede UYARI |

**3. Para birimi**

| Bulunan | Doğru |
|---------|-------|
| `1,234.56 TL` veya `$1,234.56` | `1.234,56 TL` |
| `1234.56 TL` | `1.234,56 TL` |
| `1.234.56 TL` (iki nokta!) | `1.234,56 TL` |
| `₺1.234,56` | `1.234,56 TL` |

**4. Zorunlu bölümler**

Dilekçe içinde şu başlıklar var mı?

- [ ] Mahkeme/Komisyon başlığı (ilk paragraf)
- [ ] DAVACI veya BAŞVURAN
- [ ] DAVALI veya KARŞI TARAF
- [ ] KONU
- [ ] AÇIKLAMALAR
- [ ] HUKUKİ SEBEPLER
- [ ] HUKUKİ DELİLLER
- [ ] SONUÇ VE TALEP
- [ ] İmza bloğu (sağ alt)

Eksik olanlar UYARI olarak listelenir.

**5. İmza bloğu**

`CLAUDE.md`'deki avukat bilgileri imza bloğunda doğru mu? Sicil no, baro adı tutarlı mı?

## Çıktı formatı

```markdown
## Format kontrol raporu

**Dosya:** itiraz_dilekce_Yilmaz.docx
**Tarih:** 13.05.2026

### Özet
- 🔴 2 hata
- 🟡 3 uyarı
- ℹ️ 1 bilgi

### Hatalar (mutlaka düzelt)

| # | Konum | Sorun | Önerilen düzeltme |
|---|-------|-------|-------------------|
| 1 | Paragraf 4 | "AHMET KAYA" tamamen büyük | "Ahmet Kaya" |
| 2 | Paragraf 7 | "1,500.00 TL" yanlış format | "1.500,00 TL" |

### Uyarılar

| # | Konum | Sorun | Öneri |
|---|-------|-------|-------|
| ... | ... | ... | ... |

### Düzeltme önerisi

Düzeltilmiş hâlini üretmemi ister misin? (Evet/Hayır)
```

Kullanıcı "evet" derse, `unpack → str_replace → pack` akışıyla düzeltilmiş dosyayı üret ve `present_files` ile sun.
