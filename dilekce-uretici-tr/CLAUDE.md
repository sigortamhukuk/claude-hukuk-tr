# Uygulama Profili — dilekce-uretici-tr

> Bu dosya `/dilekce-uretici-tr:cold-start-interview` komutu ile doldurulur.
> Her dilekçe üreten skill bu dosyayı okur. Doldurmadan önce çıktılar genel kalır.

## Avukat bilgileri

- **Ad Soyad:** [Cold-start ile doldurulacak]
- **Baro:** [örn. Ankara Barosu]
- **Baro Sicil No:** [örn. 32834]
- **TBB Sicil No:** [opsiyonel]
- **Vergi Dairesi / VKN:** [opsiyonel]
- **Ofis Adresi:** [tam adres]
- **Telefon:** [GSM]
- **E-posta:** [e-posta]
- **KEP Adresi:** [kep@hs01.kep.tr formatında]

## İmza bloğu tercihi

> Dilekçe sonunda yer alacak imza bloğunun tam metni. UYAP'a yüklenen dilekçelerde imza bloğu sağa yaslı, tarih sola yaslı yazılır.

```
Saygılarımla arz ederim.
[Tarih]
                                                              Av. [Ad Soyad]
                                                              Baro Sicil No: [Sicil]
```

## Çıktı standartları (sabit — değiştirme)

- **Yazı tipi:** Times New Roman
- **Punto:** 12pt
- **Sayfa boyutu:** A4 (11906 x 16838 DXA)
- **Kenar boşlukları:** Üst/Alt/Sol/Sağ = 1417 DXA (≈ 2.5 cm)
- **Satır aralığı:** 1.15 (276 line value)
- **Paragraf hizalama:** İki yana yaslı (justified)
- **Başlık (mahkeme/komisyon adı):** ortalı, kalın
- **Dilekçe içi alt başlıklar:** sola yaslı, kalın, koyu siyah
- **İsim yazım kuralı:** Sadece ilk harf büyük. "Melih Yılmaz" ✓ — "MELİH YILMAZ" ✗

## Müvekkil bilgilerinin işlenmesi

> Müvekkil bilgileri dilekçeye girilirken hangi alanlar mecburi, hangileri opsiyonel?

- **Mecburi alanlar:**
  - Ad Soyad
  - T.C. Kimlik No
  - Adres
- **Opsiyonel alanlar:**
  - Telefon
  - E-posta
  - Vekaletname tarihi/no

## Klişe ifadeler (sık kullandığın kalıplar)

> Cold-start sırasında geçmiş dilekçelerinden çıkarılır.

- **Hitap açılışı:** "Sayın Mahkemenize / Sayın Komisyona"
- **Talep cümlesi giriş:** "Yukarıda arz ve izah olunan nedenlerle, ..."
- **Talep cümlesi kapanış:** "Saygılarımla arz ve talep ederim."

## Dosya adlandırma kuralı

```
{TARIH_YYYYMMDD}_{DILEKCE_TIPI}_{MUVEKKIL_KISA}_{DOSYA_NO}.docx
```

Örnek: `20260513_itiraz_dilekce_Yilmaz_2026-K-123.docx`

## Arşiv klasörü (Google Drive)

> Üretilen dilekçelerin yedeklendiği klasör (varsa).

- **Klasör:** [Cold-start ile doldurulacak — örn. /Hukuk/Dilekceler/2026/]
