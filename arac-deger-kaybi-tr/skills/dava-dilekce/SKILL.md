---
name: dava-dilekce
description: "Use this skill when the user wants to draft an araç değer kaybı dava dilekçesi (vehicle diminished value lawsuit petition) for Turkish civil courts (Tüketici Mahkemesi, Asliye Hukuk, Asliye Ticaret). Triggers: 'değer kaybı dava dilekçesi', 'araç değer kaybı için dilekçe', 'sigorta şirketine dava', 'pert dava dilekçesi'. Gathers case facts via interview, queries Yargı MCP for relevant 17. HD precedents, and calls docx-uretim to produce a UYAP-compatible .docx."
argument-hint: "[dava türü: deger-kaybi | mahrumiyet | pert | kasko-itiraz]"
---

# Araç değer kaybı dava dilekçesi

## Akış

### Bölüm 1 — Dosya bilgisi (zorunlu)

1. Kaza tarihi? (GG.AA.YYYY)
2. Kaza yeri? (il/ilçe)
3. Müvekkil bilgileri:
   - Ad Soyad (sadece ilk harf büyük!)
   - T.C. Kimlik No
   - Adres
   - Telefon (opsiyonel)
4. Müvekkilin aracı: plaka, marka, model, model yılı
5. Karşı taraf:
   - Sürücü adı (varsa)
   - Plakası
   - Sigorta şirketi
   - Poliçe no (varsa)
6. Hasar tutarı (TL)
7. Sigorta şirketine başvuru tarihi? Cevap geldi mi?
   - Geldiyse: kabul / kısmen kabul / red
   - Tutar farkı?

### Bölüm 2 — Mahkeme seçimi

Hasar tutarına göre mahkemeyi öner:

- Tüketici uyuşmazlığı eşiği altında → **Tüketici Hakem Heyeti** veya **Tüketici Mahkemesi**
- Eşik üstü, sigorta poliçesinden doğan ticari uyuşmazlık → **Asliye Ticaret Mahkemesi**
- Haksız fiil (trafik kazası) → **Asliye Hukuk Mahkemesi**

> Eşik tutarı zaman içinde değişir. Cold-start profilindeki güncel eşiği kullan; emin değilsen kullanıcıya sor.

### Bölüm 3 — Emsal kararı arama

`yargitay-arastirma-tr:hd-bazli-tarama` skill'ini çağır:

```
chamber: H17
keywords: ["değer kaybı", "<davalı sigorta şirketi adı>"]
date_range: son 3 yıl
```

Dönen en uygun 3 kararı kullanıcıya sun: "Bunlardan hangilerini dilekçeye eklemek istersin?"

### Bölüm 4 — Hesap

Aşağıdaki bileşenleri tek tek hesapla ve göster:

- **Talep edilen değer kaybı tutarı:** [eksper raporu veya hesap]
- **İşlemiş faiz:** kaza tarihinden dava tarihine, **avans faizi** (yıllık değişen oran — TCMB'den güncel oran)
- **İşleyecek faiz:** dava tarihinden ödeme tarihine, **avans faizi**
- **Vekalet ücreti:** AAÜT (Avukatlık Asgari Ücret Tarifesi) — güncel tarife uygulanır

> Avans faizi vs yasal faiz farkı önemli — sigorta poliçesinden doğan davalarda avans faizi talep edilir (TTK kapsamı).

### Bölüm 5 — Dilekçe iskeleti

`docx-uretim` skill'ine şu yapıyı gönder:

```yaml
dilekce_tipi: "dava"
mahkeme_basligi: "ANKARA NÖBETÇİ TÜKETİCİ MAHKEMESİNE"  # veya seçilen mahkeme
davaci:
  ad: "{muvekkil_ad}"  # ilk harfler büyük
  tckn: "{tckn}"
  adres: "{adres}"
vekil: # CLAUDE.md'den
davali:
  ad: "{sigorta_sirketi}"
  adres: "{sigorta_sirketi_adresi}"  # tüzel kişilik tebligat adresi
konu: "Müvekkile ait {plaka} plakalı araçta meydana gelen değer kaybı bedeli {tutar} TL ile işlemiş ve işleyecek faiz tutarının davalıdan tahsili talebimizden ibarettir."
aciklamalar:
  - "Müvekkil {muvekkil_ad}, {plaka} plakalı {marka} {model} model {model_yili} model aracın maliki bulunmaktadır."
  - "{kaza_tarihi} tarihinde, {kaza_yeri}'nde meydana gelen trafik kazasında, davalının zorunlu mali sorumluluk sigortacısı olduğu {karsi_plaka} plakalı aracın sürücüsünün tam kusuru ile müvekkilimin aracı hasarlanmıştır."
  - "Kazaya ilişkin tutanak ve fotoğraflar dilekçemiz ekindedir. (EK-1, EK-2)"
  - "Müvekkil, hasar onarımının ardından, araç piyasa değerinde aleyhe bir düşüş — yani değer kaybı — meydana geldiğini saptamıştır. Bu husus ekli eksper raporu ile sabittir. (EK-3)"
  - "Davalı sigorta şirketine {basvuru_tarihi} tarihinde başvurulmuş; ancak {basvuru_sonuc}."
  - "Yargıtay 17. Hukuk Dairesi'nin yerleşik içtihatları uyarınca, kaza nedeniyle araçta meydana gelen değer kaybı, kusurlu tarafın zorunlu mali sorumluluk sigortacısı tarafından karşılanmak zorundadır. ({yargitay_atif_1})"
hukuki_sebepler:
  - "Karayolları Trafik Kanunu m. 91"
  - "Türk Borçlar Kanunu m. 49, 50, 51"
  - "Türk Ticaret Kanunu (sigorta hükümleri)"
  - "Karayolları Motorlu Araçlar Zorunlu Mali Sorumluluk Sigortası Genel Şartları"
  - "HMK ilgili hükümleri"
hukuki_deliller:
  - "Trafik kazası tespit tutanağı"
  - "Müvekkile ait araç ruhsatı"
  - "Hasar fotoğrafları"
  - "Eksper raporu"
  - "Davalıya yapılan başvuru ve cevap"
  - "Yargıtay içtihatları"
  - "Bilirkişi incelemesi"
  - "Tanık (gerekirse)"
  - "Yemin ve sair her türlü yasal delil"
sonuc_ve_talep:
  - "a) Fazlaya ilişkin haklarımız saklı kalmak kaydıyla şimdilik {tutar} TL değer kaybı tazminatının, kaza tarihi olan {kaza_tarihi}'den itibaren işleyecek **avans faizi** ile birlikte davalıdan tahsiline,"
  - "b) Yargılama giderlerinin DAVALI üzerinde BIRAKILMASINA,"
  - "c) Vekalet ücretinin karşı tarafa YÜKLETİLMESİNE,"
yargitay_atiflari: # Bölüm 3'ten gelir
  - { mahkeme: "Yargıtay 17. HD", esas: "...", karar: "...", tarih: "...", ozet: "..." }
```

### Bölüm 6 — Önizleme ve teslim

1. Kullanıcıya dilekçenin metnini önizleme olarak göster
2. "Onaylıyor musun? Onaylarsan .docx üretiyorum." sor
3. Onay gelirse `docx-uretim` çağır
4. Üretilmiş dosyayı `present_files` ile teslim et

### Bölüm 7 — Sonraki adım önerileri

Dilekçe teslim edildikten sonra kullanıcıya hatırlat:

- Harçlar: Başvuru harcı, peşin karar harcı, vekalet harcı
- Eklenecek belgeler (EK-1, EK-2, EK-3, ...)
- UYAP üzerinden dosyalama yolu
- Tebligat takvimi

## Kontroller

- Müvekkil ismi tamamen büyük harfle yazılmamış olmalı
- Tutar formatı `1.234,56 TL` olmalı
- Tarih formatı `GG.AA.YYYY` olmalı
- En az 1 Yargıtay 17. HD atfı içermeli
- Avans faizi (yasal faiz değil) talep edilmiş olmalı
