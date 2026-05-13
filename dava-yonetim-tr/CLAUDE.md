# Uygulama Profili — dava-yonetim-tr

## Veri saklama

Dosyalar Claude'un memory sistemi ve `references/` klasöründe tutulur. Hassas veri (müvekkil TCKN, dosya içeriği) için **Google Drive** entegrasyonu önerilir.

## Dosya türleri

- **dosya** — dava/icra/idari uyuşmazlık dosyası
- **müvekkil** — müvekkil kaydı (bir müvekkil birden çok dosyaya bağlı olabilir)
- **duruşma** — takvim öğesi
- **iletişim** — telefon/e-posta/WhatsApp kaydı

## Saklama formatı

Her dosya `references/dosyalar/{esas_no_slug}.md` içinde tutulur:

```yaml
---
esas_no: "2026/1234"
mahkeme: "Ankara 5. Asliye Hukuk Mahkemesi"
muvekkil: "Yılmaz, Ahmet"
karsi_taraf: "ABC Sigorta A.Ş."
dava_turu: "Araç değer kaybı"
plaka: "06 ABC 123"
acilis_tarihi: "2026-04-15"
durum: "açık | duruşmada | bilirkişide | karar | kesinleşti"
oncelik: "düşük | orta | yüksek"
notlar:
  - tarih: "2026-04-20"
    not: "Dilekçe sunuldu, ilk duruşma 15.07.2026"
---

# Detay açıklamalar
...
```

## Müvekkil iletişim kaydı

Her temas ayrı bir log girdisi:

```
2026-05-13 14:32 — WhatsApp — Müvekkil: "Duruşma ne zaman?" → Yanıt verildi.
2026-05-10 09:15 — E-posta — Bilirkişi raporu gönderildi.
```

Avukatlık Kanunu m. 36 (sır saklama) ile uyumlu çalışma için, müvekkil iletişimi **özetlenir** — birebir mesaj kopyası saklanmaz, sadece konu ve tarih kaydı tutulur.
