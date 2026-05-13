---
name: kep-ihtar
description: "Use this skill to draft an ihtarname intended to be sent via KEP (Registered Electronic Mail). Triggers: 'KEP ihtarname', 'KEP üzerinden ihtar', 'elektronik tebligat ihtarı'. Produces UYAP-compatible .docx and outputs the KEP send template (recipient address, subject, body)."
argument-hint: "[ihtar konusu]"
---

# KEP üzerinden ihtarname

## Akış

### Bölüm 1 — Bilgi toplama

1. **İhtar eden:** müvekkil/şirket adı, adres, TCKN/VKN, KEP adresi
2. **Muhatap:** kişi/şirket adı, KEP adresi (TÜRKKEP, hs01.kep.tr, vb.)
3. **Konu:** alacak ihtarı / fesih / başvuru / sair
4. **Maddi vakıalar:** olayın özeti
5. **Talep ve süre:** ne istendiği ve kaç gün tanındığı
6. **İhtar sonu sonuç:** süre içinde yerine getirilmemesi hâlinde yapılacaklar (dava, icra, fesih)

### Bölüm 2 — Yapısal şablon

```
İHTARNAME

İHTAR EDEN     : [Müvekkil tam unvan, adres]
VEKİLİ         : Av. [Ad Soyad] - [Adres]
MUHATAP        : [Muhatap tam unvan, adres]
KONU           : [Konu özeti]

Sayın [Muhatap adı],

[Maddi vakıaların izahı — paragraflar halinde, numaralandırılmış]

1. Müvekkilim ile aranızda ...

2. ...

İşbu ihtarımın tarafınıza tebliğinden itibaren [SÜRE] içinde [TALEP] yerine getirilmediği takdirde:

- Aleyhinize her türlü hukuki ve cezai yola başvuracağımız,
- Yargılama ve icra giderleri ile vekalet ücretinin tarafınıza yükletileceği,

hususlarını ihtaren bildiririm. [Tarih]

                                        İHTAR EDEN VEKİLİ
                                        Av. [Ad Soyad]
                                        Baro Sicil No: [Sicil]
```

### Bölüm 3 — KEP gönderim metni

`.docx` üretiminden ayrı olarak, KEP gönderiminde kullanılacak bilgileri de listele:

```
KEP gönderim bilgileri
======================
Alıcı KEP: [muhatap@kep_servis_sağlayıcı]
Konu: İHTARNAME — [Kısa konu, tarih]
Mesaj gövdesi: İhtarname ekte sunulmuştur.
Ek: ihtarname_YYYYMMDD.docx (KEP sistemine yüklenir)
```

Önemli: KEP'ten gönderim **kullanıcı tarafından yapılır** — bu skill sadece taslak hazırlar. Gönderim tarihi, KEP belgesinde otomatik basılır ve hukuki tebliğ tarihi olarak geçer.

### Bölüm 4 — .docx üretimi

`dilekce-uretici-tr:docx-uretim` çağrısı:

```yaml
dilekce_tipi: "ihtarname"
mahkeme_basligi: "İHTARNAME"  # başlık tek kelime, ortalı
# diğer alanlar standart yapıya uyar
```

## Önemli notlar

- KEP gönderimi **anında** tebliğ sayılır (sistem kaydı).
- Muhatabın KEP'i yoksa noter yolu kullanılmalı.
- KEP servis sağlayıcı (TÜRKKEP, PTT KEP, vb.) farklı olsa da hukuki geçerlilik aynıdır.
- İhtarın **konusu** KEP üst yazısında net belirtilmeli — "İHTARNAME — Alacak talebi" gibi.
