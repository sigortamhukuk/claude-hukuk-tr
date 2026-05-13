---
name: durusma-takvimi
description: "Use this skill to manage duruşma (hearing) calendar — add a hearing, list upcoming hearings, set reminders. Triggers: 'duruşma ekle', 'önümüzdeki duruşmalar', 'bu hafta duruşma var mı', 'duruşma takvimim'."
argument-hint: "[komut: ekle | liste | hafta | bugun]"
---

# Duruşma takvimi

## Komutlar

### `ekle`

Sorular:
- Esas no (mevcut dosyaya bağlanır)
- Tarih (GG.AA.YYYY)
- Saat (HH:MM)
- Salon
- Tür: ilk duruşma / ön inceleme / tahkikat / sözlü yargılama / temyiz incelemesi
- Hatırlatma günü (varsayılan: 7 gün önce, 1 gün önce, sabah)

### `liste` / `hafta` / `bugun`

| Tarih | Saat | Esas | Mahkeme | Salon | Müvekkil | Tür |
|-------|------|------|---------|-------|----------|-----|
| 15.07.2026 | 10:30 | 2026/123 | Ankara 5. Asliye Hukuk | 4 | Yılmaz | İlk |

Yakın duruşmalar renkli (3 gün içinde 🔴, 7 gün 🟡).

### Hatırlatma

Cowork üzerinde scheduled agent olarak da çalışabilir — günlük sabah 08:00'de o günün ve sonraki 3 günün duruşmalarını e-posta/Slack/WhatsApp özetlemesi.
