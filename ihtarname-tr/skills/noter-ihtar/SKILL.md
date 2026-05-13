---
name: noter-ihtar
description: "Use this skill to draft an ihtarname for delivery via notary (noter ihtarı). Triggers: 'noter ihtarnamesi', 'noterden ihtar', 'noter kanalıyla'. Same content structure as kep-ihtar, but formatted for notary submission (multiple copies, notary affidavit page)."
---

# Noter ihtarnamesi

KEP yerine noter üzerinden tebliğ. Yapı `kep-ihtar` ile aynı; farklar:

- Üç nüsha hazırlanır (noter dosyası + müvekkil + muhatap)
- Üst tarafta "T.C. ANKARA ... NOTERLİĞİ" başlığı yer alır (noterlik bu kısmı kendi sistemine entegre eder; biz boş bırakırız)
- Maliyet KEP'e göre yüksek, hız daha yavaş
- Tebligat tarihi muhatabın imza/tebligat tarihi

İçerik için `kep-ihtar` yapısı kullanılır. `docx-uretim` çağrısı aynı.
