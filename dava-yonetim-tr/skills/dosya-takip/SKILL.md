---
name: dosya-takip
description: "Use this skill to add, query, update, or list legal cases (dosyalar). Triggers: 'dosya ekle', 'dosyalarımı göster', 'açık davalarım', 'esas no ile ara', 'dosya durumu güncelle', 'bu dosyada ne oldu', 'son notlar', 'kim açtı'. Supports filtering by müvekkil, mahkeme, dava türü, durum, öncelik."
argument-hint: "[komut: ekle | liste | ara | guncelle | kapat] [argümanlar]"
---

# Dosya takibi

Senin daha önce yaptığın localStorage tabanlı "İş Takibi" uygulamasının plugin karşılığı.

## Komutlar

### `ekle`

Yeni dosya kaydı oluşturur. Sorular:

1. Esas no?
2. Mahkeme?
3. Müvekkil adı?
4. Karşı taraf?
5. Dava türü? (Hasar / Değer Kaybı / Araç Mahrumiyet / Pert / İş Akdi / Tüketici / Diğer)
6. Uyuşmazlık tipi? (İcra / Dava / Tahkim / İdari)
7. Öncelik? (düşük / orta / yüksek)
8. Açılış notu (opsiyonel)

Cevapları `references/dosyalar/{slug}.md` olarak yaz.

### `liste [filtre]`

Tüm dosyaları markdown tablo halinde göster. Filtre seçenekleri:

- `liste açık` — sadece açık dosyalar
- `liste mahkeme:asliye_ticaret` — belirli mahkeme
- `liste muvekkil:yilmaz` — belirli müvekkil
- `liste oncelik:yuksek`
- `liste durum:bilirkisi`

### `ara <terim>`

Tam metin arama. Esas no, müvekkil adı, karşı taraf, plaka.

### `guncelle <esas_no>`

Dosyaya yeni not ekle veya durum değiştir:

- `durum: bilirkişide`
- `not: "Bilirkişi raporu geldi, lehte"`

### `kapat <esas_no>`

Dosyayı kapatır (silmek değil), `durum: kesinleşti` yapar.

## Çıktı tipi

Standart liste tablosu:

| Esas No | Müvekkil | Mahkeme | Tip | Durum | Öncelik | Son not |
|---------|----------|---------|-----|-------|---------|---------|
| 2026/123 | Yılmaz | Ankara 5. Asliye Ticaret | Değer kaybı | Bilirkişide | Yüksek | 12.05.2026 |

## Hassas veri uyarısı

Müvekkil TCKN'leri dosya kayıtlarında **maskelenir** (`123*****890`). Tam TCKN sadece dilekçe üretimi sırasında, anlık kullanım için açılır; saklı tutulmaz.
