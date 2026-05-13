---
name: karar-ozeti
description: "Use this skill to extract a structured summary from a Yargıtay decision text — vakıalar, hukuki nitelendirme, kabul/red gerekçesi, sonuç. Triggers: 'kararı özetle', 'bu kararı analiz et', 'içtihadın özünü çıkar'."
---

# Karar özeti

Yargıtay kararının yapısal özetini çıkarır. Çıktı:

```yaml
karar_no: "..."
tarih: "..."
daire: "..."
konu: "..."
maddi_vakialar:
  - ...
yerel_mahkeme_karari: "..."
yargitay_tespitleri:
  - ...
hukuki_dayanak:
  - "TBK m. ..."
  - "..."
sonuc: "ONAMA | BOZMA | DÜZELTİLEREK ONAMA"
ozet_cumle: "Tek cümlede içtihadın özü (mahkemeye atıf için)"
ilgili_alanlar:
  - "değer kaybı"
  - "avans faizi"
```

Dilekçeye atıf yapılırken `ozet_cumle` ve karar bilgisi kullanılır.
