---
name: basvuru-dilekce
description: "Use this skill to draft a Sigorta Tahkim Komisyonu başvuru dilekçesi (initial application petition). Triggers: 'tahkim başvuru dilekçesi', 'sigorta tahkim başvurusu', 'STK dilekçesi', 'hakem heyetine başvuru'. Application precedes the Hakem Heyeti decision; do NOT use for itiraz (objection to a Hakem Heyeti decision) — use itiraz-dilekce instead."
argument-hint: "[uyuşmazlık türü: deger-kaybi | hasar | saglik | hayat | sair]"
---

# Sigorta Tahkim Komisyonu — Başvuru dilekçesi

## Ön koşul

Sigorta şirketine **yazılı başvuru yapılmış**, en az **15 gün** geçmiş veya şirket olumsuz cevap vermiş olmalıdır (Sigortacılık Kanunu m. 30/13).

İlk soru: "Sigorta şirketine başvurunuzu yaptınız mı? Tarih ve sonuç?"

Eğer hayırsa veya 15 günden az ise → dilekçeye başlanamaz, önce başvuru yapılmalı.

## Akış

### Bölüm 1 — Taraflar

- **Başvuran (müvekkil):** ad, TCKN, adres
- **Aleyhine başvurulan:** sigorta şirketi tam ünvanı, tebligat adresi
- **Vekil:** `dilekce-uretici-tr/CLAUDE.md`'den otomatik

### Bölüm 2 — Uyuşmazlık konusu

- Sigorta türü (ZMSS / Kasko / Sağlık / Konut / Hayat / ...)
- Poliçe numarası, başlangıç/bitiş tarihi
- Olay/rizikonun ne olduğu, tarihi
- Talep edilen tutar (TL)

### Bölüm 3 — Hesaplar

`docx-uretim`'e gönderilecek `talep_kalemleri`:

```yaml
- ad: "Asıl alacak (değer kaybı / hasar bedeli / ...)"
  tutar: "{tutar} TL"
- ad: "İşlemiş avans faizi"
  hesap: "{kaza_tarihi} - {basvuru_tarihi}"
  tutar: "{islemis_faiz} TL"
- ad: "İşleyecek avans faizi"
  hesap: "Başvuru tarihinden ödeme tarihine"
- ad: "Vekalet ücreti"
  hesap: "AAÜT"
- ad: "Tahkim ücreti"
  hesap: "Tahkim Tarifesi"
TOPLAM: "{toplam} TL"
```

### Bölüm 4 — Dilekçe yapısı

`docx-uretim` çağrısı:

```yaml
dilekce_tipi: "basvuru"
mahkeme_basligi: "SİGORTA TAHKİM KOMİSYONU BAŞKANLIĞINA"
basvuran:
  ad: "{muvekkil_ad}"
  tckn: "{tckn}"
  adres: "{adres}"
aleyhine_basvurulan:
  ad: "{sigorta_sirketi}"
  adres: "{sigorta_sirketi_adres}"
vekil: # CLAUDE.md
konu: "{tutar} TL alacağın tahsili talebimizden ibarettir."
aciklamalar:
  - "Müvekkil {muvekkil_ad}, {plaka} plakalı aracın maliki olup {police_no} sayılı poliçe ile davalı sigortacı..."
  - "Olay: {olay_tarihi} tarihinde meydana gelen {olay_turu}..."
  - "Başvuru: {basvuru_tarihi} tarihinde aleyhine başvurulan sigortacıya yazılı başvurumuz iletilmiş; ancak..."
  - "Söz konusu uyuşmazlık, 5684 sayılı Sigortacılık Kanunu m. 30 kapsamında Sigorta Tahkim Komisyonu'nun görev alanına girmektedir."
  - "Yargıtay {ilgili_HD} yerleşik içtihatlarında, benzer uyuşmazlıklarda... ({atif})"
hukuki_sebepler:
  - "5684 sayılı Sigortacılık Kanunu m. 30"
  - "Sigortacılıkta Tahkime İlişkin Yönetmelik"
  - "TTK ilgili hükümleri"
  - "TBK m. 49 vd."
  - "Karayolları Trafik Kanunu m. 91 (ZMSS davalarında)"
hukuki_deliller:
  - "Sigorta poliçesi"
  - "Aleyhine başvurulana yapılan başvuru ve cevap"
  - "Olay tutanağı / kaza tespit tutanağı"
  - "Eksper raporu"
  - "Hasar fotoğrafları"
  - "Bilirkişi incelemesi"
  - "Yargıtay içtihatları"
  - "Yemin ve sair her türlü yasal delil"
sonuc_ve_talep:
  - "a) Fazlaya ilişkin haklarımız saklı kalmak kaydıyla şimdilik {tutar} TL alacağın, kaza/rizikonun gerçekleştiği tarihten itibaren işleyecek **avans faizi** ile birlikte aleyhine başvurulan sigortacıdan tahsiline,"
  - "b) Tahkim yargılama giderlerinin aleyhine başvurulana YÜKLETİLMESİNE,"
  - "c) Vekalet ücretinin AAÜT uyarınca karşı tarafa YÜKLETİLMESİNE,"
```

### Bölüm 5 — Ekler

Mutlaka kontrol et — eksiksiz başvuru için:

- [ ] Vekaletname (asıl/onaylı suret)
- [ ] Sigorta poliçesi sureti
- [ ] Olay tutanağı
- [ ] Şirkete yapılan başvuru ve şirket cevabı
- [ ] Hasar/eksper raporu
- [ ] Fotoğraflar
- [ ] Kimlik fotokopisi (müvekkilin)
- [ ] Tahkim ücreti dekontu

Bu kontrol listesi kullanıcıya gösterilir.

## Önemli notlar

- Tahkim başvurusu **dava açma süresine** tabi (sigorta tipine göre 2-10 yıl).
- Başvuru, sigorta şirketine yapılan ilk başvuru tarihinden itibaren **15 gün geçmemişse reddedilir**.
- Anlaşmazlık konusu tutar **tahkim üst sınırını aşıyorsa** — komisyon görevsizlik kararı verir; o zaman doğrudan Asliye Ticaret Mahkemesi'ne gidilir.
