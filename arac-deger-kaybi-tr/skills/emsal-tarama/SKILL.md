---
name: emsal-tarama
description: "Use this skill to find Yargıtay 17. HD (or relevant chamber) precedent decisions for an araç değer kaybı / mahrumiyet / pert dispute. Wraps yargitay-arastirma-tr:hd-bazli-tarama with case-type-specific defaults. Triggers: 'değer kaybı emsali bul', 'pert kararı ara', 'mahrumiyet için Yargıtay'."
argument-hint: "[dava türü] [opsiyonel: anahtar kelimeler]"
---

# Araç değer kaybı emsal karar taraması

`yargitay-arastirma-tr:hd-bazli-tarama` skill'ini şu varsayılanlarla çağırır:

| Dava türü | Hukuk Dairesi | Anahtar kelimeler |
|-----------|---------------|---------------------|
| deger-kaybi | H17 | "değer kaybı", "araç" |
| mahrumiyet | H17 | "mahrumiyet", "araç kullanma" |
| pert | H17 | "pert", "rayiç bedel" |
| kasko-itiraz | H11, H17 | "kasko", "eksik ödeme" |
| haksiz-fiil | H4 | "haksız fiil", "trafik kazası" |

Dönen kararlardan en alakalı 5'ini, kısa özet ve atıf bilgisiyle birlikte göster. Kullanıcı seçtiğinde dilekçe skill'lerine bu liste pas edilir.
