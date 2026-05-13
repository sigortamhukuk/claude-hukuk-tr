---
name: itiraz-dilekce
description: "Use this skill to draft a Sigorta Tahkim Komisyonu İtiraz Hakem Heyeti dilekçesi — objection against a Hakem Heyeti decision. Triggers: 'tahkim itiraz dilekçesi', 'hakem heyeti kararına itiraz', 'STK itiraz', 'itiraz hakem heyeti'. Used when a Hakem decision is unfavorable and the amount exceeds the threshold allowing objection."
argument-hint: "[karar no veya dosya bilgisi]"
---

# İtiraz Hakem Heyeti dilekçesi

## Süreç notu

- Hakem Heyeti kararı tebellüğ tarihinden itibaren **10 gün** içinde itiraz edilebilir (Sigortacılık Kanunu m. 30/12).
- Bu süre **kesin** — kaçırılırsa karar kesinleşir.
- İtiraz Hakem Heyeti kararına karşı ise tahkim üst sınırını aşan davalar için **temyiz** yolu açık.

## Akış

### Bölüm 1 — Karar analizi

Kullanıcıdan Hakem Heyeti kararını al (yüklenirse parse, yapıştırılırsa metin). Çıkar:

- Karar tarihi, tebellüğ tarihi (10 gün hesabı için)
- Hakem heyetinin tespitleri
- Reddedilen/eksik kabul edilen talep kalemleri
- Hakem'in atıf yaptığı içtihatlar/maddeler

### Bölüm 2 — İtiraz noktaları

Tipik itiraz konuları:

1. **Maddi vakıaların yanlış değerlendirilmesi**
2. **Hukuki nitelendirme hatası** (ör. ZMSS yerine kasko hükümlerinin uygulanması)
3. **Faiz türü hatası** — yasal faiz yerine avans faizi uygulanmaması
4. **Hesap hatası** (ör. işlemiş faiz başlangıç tarihi)
5. **Eksik tahkikat** (ör. ek bilirkişi raporu alınmaması)
6. **Vekalet ücreti hatası** (AAÜT yanlış uygulaması)
7. **Lehte içtihatların gözetilmemesi**

Kullanıcıya: "Hangi noktalardan itiraz ediyoruz?"

### Bölüm 3 — Emsal kararı

`yargitay-arastirma-tr:hd-bazli-tarama` — itiraz noktalarına uygun HD:

| İtiraz konusu | Hukuk Dairesi |
|---------------|---------------|
| Değer kaybı yöntem hatası | H17 |
| Avans faizi | H11, H17 |
| ZMSS uygulaması | H17 |
| Kasko poliçesi | H11 |

### Bölüm 4 — Dilekçe yapısı

```yaml
dilekce_tipi: "itiraz"
mahkeme_basligi: "SİGORTA TAHKİM KOMİSYONU İTİRAZ HAKEM HEYETİ BAŞKANLIĞINA"
dosya_no: "{komisyon_dosya_no}"
itiraz_eden: # müvekkil
karsi_taraf: # sigorta şirketi
konu: "{karar_tarih} tarihli ve {karar_no} sayılı Hakem Heyeti kararına itirazlarımızın sunulması ile kararın aleyhe kısmının KALDIRILMASI/DEĞIŞTIRILMESI taleplerimizden ibarettir."
aciklamalar:
  - "Yukarıda esas numarası belirtilen dosyaya ilişkin Hakem Heyeti kararı tarafımıza {tebellug_tarihi} tarihinde tebliğ edilmiştir. İşbu dilekçemiz yasal 10 günlük süre içinde sunulmuştur."
  - "Söz konusu karar, aşağıda ayrı başlıklar altında izah edilen sebeplerle hukuka aykırı ve hatalıdır:"
  - "**Birinci itiraz noktası: {baslik_1}**"
  - "{detay_1}"
  - "Bu konuda Yargıtay {HD} {esas} E., {karar} K. sayılı kararında..."
  - "**İkinci itiraz noktası: {baslik_2}**"
  - "{detay_2}"
  # vb...
hukuki_sebepler:
  - "5684 sayılı Sigortacılık Kanunu m. 30"
  - "TTK m. 1530 (avans faizi)"
  - "TBK m. 49 vd."
  - "Diğer ilgili mevzuat"
sonuc_ve_talep:
  - "a) {tarih} tarihli ve {karar_no} sayılı Hakem Heyeti kararının itirazlarımız doğrultusunda KALDIRILMASINA / DEĞIŞTIRILMESINE,"
  - "b) Talep ettiğimiz {tutar} TL'nin tamamının, kaza tarihi/temerrüt tarihi olan {tarih}'ten itibaren işleyecek **avans faizi** ile birlikte karşı taraftan tahsiline,"
  - "c) İtiraz aşamasına ilişkin yargılama giderlerinin ve vekalet ücretinin karşı tarafa YÜKLETİLMESİNE,"
```

### Bölüm 5 — Süre uyarısı

Dilekçe üretilmeden önce kullanıcıya açıkça hatırlat:

> ⚠️ **Süre kontrolü:**
> - Karar tebellüğ tarihi: {tarih}
> - Son itiraz tarihi: {tarih + 10 gün}
> - Bugün: {bugun}
> - Kalan süre: {kalan_gun} gün
>
> Lütfen UYAP üzerinden dosyalama saatini ve gönderim onayını yazılı kayıt altına alın.

## Önemli

- 10 günlük süre **kesin**.
- Hafta sonu denk gelirse, sonraki iş gününde sona erer.
- Tatil günleri için Resmi Tatiller Kanunu uygulanır.
