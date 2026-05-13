---
name: hakem-karari-analizi
description: "Use this skill to analyze a Sigorta Tahkim Komisyonu Hakem Heyeti decision — identify reasoning, weak points, and grounds for objection. Triggers: 'hakem kararını incele', 'STK kararı analizi', 'itiraz noktaları çıkar'."
---

# Hakem Heyeti kararı analizi

Karar metnini yapısal parçalara ayır, itiraz dilekçesi için zemin oluştur.

## Çıkarılan unsurlar

1. **Tarafların iddiaları** — başvuran ve karşı tarafın ileri sürdüğü vakıalar
2. **Hakem'in tespitleri** — kabul edilen vakıalar
3. **Hukuki nitelendirme** — uygulanan kanun maddeleri
4. **Atıf yapılan içtihatlar** — Yargıtay/Anayasa Mahkemesi
5. **Hesap detayı** — asıl alacak, faiz türü, başlangıç tarihi, vekalet ücreti
6. **Kabul/red oranı** — talep vs. hüküm farkı

## Zayıf nokta tespiti

Sistematik kontrol:

- Faiz türü doğru mu? (sigorta poliçesinden doğan → avans faizi olmalı)
- Faiz başlangıç tarihi doğru mu? (kaza tarihi vs temerrüt tarihi)
- AAÜT uygulaması doğru mu?
- Atıf yapılan içtihatlar **lehe** mi yoksa karşı tarafın lehine mi?
- Hakem'in dikkat çekmediği maddi vakıa var mı?
- Bilirkişi raporu tek yönlü mi alınmış?

## Çıktı

Markdown tablo + öneri listesi. İtiraz edilebilir noktalar puanlı:

- 🟢 Güçlü itiraz noktası
- 🟡 Orta itiraz noktası
- 🔴 Risk: itiraz yapılırsa ek aleyhe risk doğurabilir

Kullanıcı seçim yaptıktan sonra `itiraz-dilekce` skill'i devreye girer.
