---
name: emsal-derleme
description: "Use this skill to compile a thematic Yargıtay precedent collection (içtihat derlemesi) on a specific legal issue — e.g. 'değer kaybı hesaplama yöntemleri', 'avans faizi başlangıç tarihi', 'sigorta poliçesinden doğan davalarda zamanaşımı'. Triggers: 'içtihat derlemesi', 'emsal toplama', 'konu bazlı tarama'. Produces a structured .docx report grouping decisions by sub-issue."
---

# Yargıtay içtihat derlemesi

Belirli bir hukuki konu üzerine birden çok HD'den karar toplar, alt başlıklara ayırarak sunar.

Akış:
1. Konu ve alt başlıkları kullanıcıdan al
2. Her alt başlık için `hd-bazli-tarama` çağır
3. Sonuçları derle, çelişen kararları işaretle
4. `.docx` rapor üret (`docx-uretim` aracılığıyla)

Çıktı yapısı:

```
İÇTİHAT DERLEMESİ — [Konu]

I. GİRİŞ
II. ALT KONU 1
   1. Lehte içtihatlar
   2. Aleyhte içtihatlar
III. ALT KONU 2
   ...
IV. SONUÇ — Yerleşik içtihadın yönü
```
