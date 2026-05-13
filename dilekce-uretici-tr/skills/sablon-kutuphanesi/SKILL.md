---
name: sablon-kutuphanesi
description: "Use this skill when the user wants to add, list, retrieve, or use a saved dilekçe şablonu (template). The library stores reusable templates — e.g. 'değer kaybı dava dilekçesi standart hâli', 'kasko itiraz şablonu', 'müvekkil vekaletname'. Triggers: 'şablon kaydet', 'şablonlarımı göster', 'şu şablonu kullan', 'template ekle'. Templates live in references/templates/ as .md or .docx files."
argument-hint: "[komut: liste | kaydet | kullan] [şablon adı]"
---

# Şablon kütüphanesi yönetimi

Sık tekrarlanan dilekçe yapılarını şablon olarak saklar ve çağırır.

## Klasör yapısı

```
references/
  templates/
    sigorta-tahkim/
      itiraz-dilekce.md
      basvuru-dilekce.md
      cevaba-cevap.md
    arac-deger-kaybi/
      dava-dilekce.md
      bilirkisi-rapor-itiraz.md
    ihtarname/
      kep-uzeri-ihtar.md
    vekaletname/
      genel-vekalet.md
```

Şablon dosyası formatı (`.md` + frontmatter):

```yaml
---
ad: "Değer kaybı dava dilekçesi (genel)"
kategori: "arac-deger-kaybi"
mahkeme_tipi: "Asliye Hukuk"
degisken_alanlar:
  - muvekkil_ad
  - davali_ad
  - kaza_tarihi
  - plaka
  - hasar_tutari
  - bilirkisi_raporu_tarihi
yargitay_referanslari:
  - "17. HD 2020/1234 E., 2021/5678 K."
son_guncelleme: "2026-05-13"
---

# AÇIKLAMALAR

1. Müvekkil {muvekkil_ad} adına kayıtlı {plaka} plakalı araç, {kaza_tarihi} tarihinde ...

2. Davalı tarafa ait sigorta poliçesi kapsamında ...

[...]
```

## Komutlar

### `liste`

`references/templates/` altındaki tüm şablonları kategori bazında listele:

```
📂 sigorta-tahkim/
  • itiraz-dilekce       (son güncelleme: 12.04.2026)
  • basvuru-dilekce       (son güncelleme: 03.05.2026)
📂 arac-deger-kaybi/
  • dava-dilekce          (son güncelleme: 10.05.2026) ⭐ en çok kullanılan
```

### `kaydet [ad]`

Mevcut konuşmada üretilmiş veya kullanıcı tarafından yapıştırılmış bir dilekçeyi şablona dönüştür:

1. Kullanıcıdan şablonun adını ve kategorisini iste
2. Dilekçedeki değişken alanları otomatik tespit et (özel isim, tarih, plaka, tutar)
3. Bunları `{degisken_adi}` placeholder'larına dönüştür
4. Frontmatter ile birlikte `references/templates/<kategori>/<ad>.md` olarak kaydet
5. Onay iste

### `kullan [ad]`

1. Şablonu yükle
2. `degisken_alanlar` listesindeki her alan için kullanıcıya sor
3. Placeholder'ları doldur
4. `docx-uretim` skill'ini çağır

### `guncelle [ad]`

Var olan şablonun içeriğini düzenle. Yargıtay atıflarını taze tutmak için özellikle yararlı.

## İyi şablon kuralları

- **Yargıtay referansları zaman aşımına uğrar.** Şablonlarda mutlaka tarih ve son doğrulama notu olsun.
- **Değişken alanları açıkça işaretle.** `{muvekkil_ad}` gibi süslü parantezler.
- **Tutar, tarih, plaka gibi değerleri sabit yazma** — her zaman placeholder.
- **Hukuki sebepler bölümünü canlı tut** — TBK, KTK, Sigortacılık Kanunu maddeleri yıllar içinde değişebilir.
