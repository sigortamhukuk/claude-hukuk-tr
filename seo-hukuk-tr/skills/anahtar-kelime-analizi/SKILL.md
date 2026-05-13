---
name: anahtar-kelime-analizi
description: "Use this skill to analyze a keyword or topic for Turkish legal SEO — search volume estimation, competition difficulty, related queries, content gap analysis against current sigortamhukuk.com/sigortatazminatim.com pages. Triggers: 'anahtar kelime analizi', 'keyword research', 'içerik açığı', 'SEO fırsatları'."
---

# Anahtar kelime analizi

## Akış

### Bölüm 1 — Tohum kelime

Kullanıcıdan tohum: "araç değer kaybı"

### Bölüm 2 — Anahtar kelime genişletme

`web_search` ile şu sorgular:

- `{tohum}` — ana sonuçları
- `{tohum} nasıl`, `{tohum} ne demek` — bilgilendirme niyeti
- `{tohum} avukat`, `{tohum} dava` — işlem niyeti
- `{tohum} hesaplama`, `{tohum} 2026` — uzun kuyruk
- Google Suggest otomatik tamamlamalardan örnekler

### Bölüm 3 — Niyet sınıflandırması

Her keyword için:

| Keyword | Niyet | Zorluk (tahmin) | Mevcut sayfan? |
|---------|-------|----------------|----------------|
| araç değer kaybı | Bilgi+İşlem | Yüksek | ✅ Pillar |
| araç değer kaybı hesaplama | Bilgi | Orta | ❌ |
| araç değer kaybı avukatı | İşlem | Orta | ✅ Hizmet |
| 2026 değer kaybı yargıtay | Bilgi (uzman) | Düşük | ❌ |

### Bölüm 4 — İçerik açığı

Mevcut sitedeki içerikle karşılaştır (kullanıcı site haritasını paylaşır veya `web_fetch` ile sitemap.xml çekilir):

- 🟢 İyi kapsanmış konular
- 🟡 Var ama güçsüz (kısa içerik, eski)
- 🔴 Hiç yok — fırsat

### Bölüm 5 — Eylem listesi

Önceliklendirilmiş 5-10 yeni içerik önerisi:

```
1. [🔴 Yok / Yüksek hacim] "Değer kaybı hesaplama 2026" — Cluster, 1500 kelime
2. [🟡 Zayıf / Orta hacim] "Kasko itiraz dilekçesi örneği" — Pillar güncellemesi
3. ...
```

## Önemli

- Türkçe arama hacmi tahminleri kabaca; gerçek veri Google Search Console + Ahrefs/Semrush gerekir.
- Mevsimsel terimler (örn. "yılbaşı tatili kaza") düşük yıllık hacim, yüksek dönemsel pik gösterir — not düş.
