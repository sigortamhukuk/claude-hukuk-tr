---
name: market-value-analizi
description: "Use this skill for vehicle market value analysis in pert (total-loss) disputes — compares the sigorta şirketi's valuation against actual market data from sahibinden.com, arabam.com, and similar Turkish marketplaces. Produces a written analysis with comparable listings, statistical summary (median, average, range), and depreciation methodology. Triggers: 'piyasa değer analizi', 'pert değerleme', 'market value', 'araç gerçek değeri', 'rayiç tespit'."
argument-hint: "[plaka veya araç bilgileri]"
---

# Araç piyasa değer analizi (pert davaları için)

Pert/total-loss uyuşmazlıklarında sigorta şirketinin teklif ettiği bedelin gerçek piyasa rayicini yansıtmadığını ispat etmek için kullanılır.

## Akış

### Bölüm 1 — Araç bilgisi

- Marka, model, model yılı
- Donanım paketi (varsa)
- Kilometre
- Hasarsız mı? (Aracın hasar geçmişi)
- Renk
- Yakıt türü
- Vites (manuel/otomatik)
- Kaza tarihi (rayiç bu tarih itibariyle saptanır)

### Bölüm 2 — Sigorta teklifi

- Sigorta şirketinin önerdiği pert bedeli
- Tarihi
- Dayanağı (TSB değeri / eksper raporu / şirket içi cetvel?)

### Bölüm 3 — Piyasa taraması

Kullanıcıdan **kaza tarihine yakın** ilanları topla. İdeal olarak:
- 10-20 karşılaştırılabilir ilan
- Aynı marka/model/yıl
- ±20.000 km aralığı
- Aynı yakıt/vites tipi

Kullanıcı manuel toplar (web_search Türkiye'deki sahibinden vb. siteleri tam çekemez); skill verileri tabloya dönüştürür:

| # | Kaynak | İlan Tarihi | Km | Donanım | Fiyat |
|---|--------|-------------|-----|---------|-------|
| ... | ... | ... | ... | ... | ... |

### Bölüm 4 — İstatistiksel özet

Hesapla ve raporla:

- **Ortalama:** `1.234.567 TL`
- **Medyan:** `1.250.000 TL`
- **Standart sapma:** `±85.000 TL`
- **Aralık:** `1.050.000 — 1.450.000 TL`
- **Sigorta teklifinin medyana farkı:** `%-23` (negatif → düşük teklif)

### Bölüm 5 — Yöntem notu

Yargıtay 17. HD'nin kabul ettiği "kaza tarihindeki rayiç bedel" tespitinde:

1. Aynı sınıf, model, donanım, kilometre ortalamasındaki ilan örneklemleri
2. Pazarlık payı (genellikle %5-10 düşülür)
3. Aracın hasar öncesi durumuna göre ortalama/üstü/altı kategorisi

### Bölüm 6 — Çıktı

İki seçenek sun:

1. **Sade rapor (.docx)** — Mahkemeye delil olarak sunulabilir
2. **Dilekçe içine entegre** — Mevcut dava dilekçesine bölüm olarak ekle

Rapor formatı:

```
PİYASA DEĞER ANALİZ RAPORU

Araç: {marka} {model} {yil}
Kaza tarihi: {tarih}
Sigorta teklifi: {teklif} TL

I. KARŞILAŞTIRILABILIR İLANLAR
[tablo]

II. İSTATİSTİKSEL ÖZET
- Ortalama: ...
- Medyan: ...
- Aralık: ...

III. SONUÇ
Aracın kaza tarihindeki piyasa rayici medyan üzerinden {medyan} TL'dir.
Sigorta teklifi medyandan %{yuzde} düşüktür.
Talep edilen rayiç bedel: {talep} TL
```

## Önemli notlar

- İlan tarihleri **kaza tarihinden ±60 gün** içinde olmalı; aksi takdirde piyasa dalgalanması (özellikle 2022-2024 dönemi gibi) sonucu bozar.
- Hasar geçmişi belirsiz ilanlar (özellikle çok düşük fiyatlı) örneklemden çıkarılmalı.
- Eski Tesla Model Y örneğinden hatırla: Yeni teknoloji araçlarda model yılı + yazılım versiyonu da etkilidir; not düş.
