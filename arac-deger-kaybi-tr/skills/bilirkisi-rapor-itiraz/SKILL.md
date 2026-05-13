---
name: bilirkisi-rapor-itiraz
description: "Use this skill when contesting a bilirkişi (expert witness) report — typically vehicle valuation, damage assessment, or Adli Tıp Kurumu reports. Triggers: 'bilirkişi raporuna itiraz', 'eksper raporunu çürüt', 'rapor hatalı', 'değer kaybı raporu yetersiz'. Produces a structured itiraz dilekçesi citing specific errors, alternative methodology, and supporting Yargıtay precedent."
argument-hint: "[rapor türü: deger-kaybi | pert | adli-tip | makine]"
---

# Bilirkişi raporuna itiraz dilekçesi

## Akış

### Bölüm 1 — Rapor analizi

Kullanıcıdan rapor metnini iste (yüklenirse OCR/parsing, yapıştırılırsa direkt). Şu unsurları çıkar:

- Bilirkişi adı ve uzmanlığı
- Rapor tarihi
- Tespit edilen değer/tutar
- Kullanılan yöntem
- Atıf yapılan veriler (TÜİK ÜFE, piyasa değeri, eksper kanaati, vb.)

### Bölüm 2 — İtiraz noktalarını tespit

Tipik hata kalıpları:

1. **Yöntem hatası** — Yargıtay'ın kabul ettiği yöntem dışında bir hesap (örn. eski "sabit oran" yöntemi)
2. **Veri hatası** — Yanlış piyasa rayici, yanlış araç yaşı, yanlış kaza tarihi
3. **Eksik inceleme** — Aracın fiziksel muayenesi yapılmadan masa başı rapor
4. **Yetersiz gerekçe** — Sonuç tutarı için somut hesap gösterilmemiş
5. **Yetkisizlik** — Bilirkişinin uzmanlık alanı uyuşmazlık dışı

Kullanıcıya "Hangi itiraz noktaları sizin dosyanız için geçerli?" sorusunu sun.

### Bölüm 3 — Emsal kararı

`yargitay-arastirma-tr:hd-bazli-tarama`:
- chamber: `H17` (değer kaybı için) veya `H4` (haksız fiil için)
- keywords: ["bilirkişi raporu", "yetersiz", "değer kaybı yöntemi"]

### Bölüm 4 — Dilekçe iskeleti

`docx-uretim` çağrısı:

```yaml
dilekce_tipi: "itiraz"
mahkeme_basligi: "{mahkeme_baslik}"  # mevcut dosyadan
dosya_no: "{esas_no}"
konu: "{tarih} tarihli bilirkişi raporuna itirazlarımızın sunulması ile yeni bilirkişi heyeti oluşturulmasını talep etmemizden ibarettir."
aciklamalar:
  - "{rapor_tarihi} tarihli bilirkişi raporu tarafımıza tebliğ edilmiştir."
  - "Söz konusu rapor, aşağıda ayrı ayrı sıralanan sebeplerle hatalı ve yetersizdir:"
  # her itiraz noktası ayrı paragraf:
  - "**Birinci itiraz noktası:** {itiraz_baslik_1} — {detay}"
  - "**İkinci itiraz noktası:** {itiraz_baslik_2} — {detay}"
  - "Yargıtay 17. Hukuk Dairesi'nin yerleşik içtihatlarında, değer kaybı hesabının {dogru_yontem} yöntemi ile yapılması gerektiği vurgulanmıştır. ({yargitay_atif})"
sonuc_ve_talep:
  - "a) {rapor_tarihi} tarihli bilirkişi raporuna yapılan itirazlarımızın KABULÜNE,"
  - "b) Yeni bir bilirkişi heyeti oluşturularak ek rapor alınmasına,"
  - "c) Yargılama giderlerinin karşı tarafa YÜKLETİLMESİNE,"
```

### Çıktı

UYAP'a yüklenebilir tek `.docx`. Ekler ayrı listede gösterilir.
