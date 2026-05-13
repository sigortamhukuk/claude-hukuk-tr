---
name: cevap-dilekce
description: "Use this skill to draft a cevap dilekçesi (response petition) in a Sigorta Tahkim Komisyonu case when the user represents the respondent side. Triggers: 'cevap dilekçesi sigorta tahkim', 'STK cevap', 'tahkim savunma'."
---

# Sigorta Tahkim — Cevap dilekçesi

Karşı taraf olan başvuranın iddialarına savunma. Yapı `itiraz-dilekce` ile benzer; ana fark:

- "İTİRAZ" yerine "CEVAP"
- Süre: tebellüğ tarihinden itibaren genellikle **15 gün**
- Sıralama: usulî itirazlar → maddi itirazlar → talep
- Karşı dava (varsa) ayrı dilekçe ile

İskelet `docx-uretim` çağrılarak üretilir. Cold-start sırasında bu plugin'in çekirdek skill'lerine ek olarak detaylandırılır.
