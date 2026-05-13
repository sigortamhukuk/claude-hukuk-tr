---
name: cold-start-interview
description: "Use this skill on the first run of dilekce-uretici-tr, or whenever the user wants to update their avukat profile (baro sicil, ofis bilgileri, imza bloğu, klişe ifadeler). Walks through a structured interview to fill in CLAUDE.md. Triggers: 'profili kur', 'bilgilerimi gir', 'cold-start', 'ilk kurulum', 'avukat bilgilerimi güncelle'. Do NOT use for producing dilekçe — use docx-uretim skill instead."
argument-hint: "[opsiyonel: hızlı | tam] — hızlı sadece zorunlu alanları sorar"
---

# Cold-start interview — Avukat profili kurulumu

Bu skill, `CLAUDE.md` dosyasını dolduran röportajı yönetir. Diğer tüm dilekçe üreten skill'ler bu dosyayı okur.

## Akış

### Bölüm 1 — Avukat kimlik bilgileri (zorunlu)

1. Adın soyadın? (Sadece ilk harfler büyük; "Melih Yılmaz" gibi)
2. Hangi baroya kayıtlısın?
3. Baro sicil numaran?
4. (Opsiyonel) TBB sicil numaran?
5. Ofis tam adresin?
6. Telefon (GSM)?
7. E-posta?
8. KEP adresin? (kep@hs01.kep.tr formatında)

### Bölüm 2 — İmza bloğu

> Kullanıcıya iki seçenek sun:
> 1. Standart blok (yukarıdaki bilgilerden otomatik oluşturulur)
> 2. Kendi özel bloğunu yapıştır

Standart blok şablonu:

```
Saygılarımla arz ederim.
[Tarih]
                                                              Av. {ad_soyad}
                                                              {baro} Baro Sicil No: {sicil}
```

### Bölüm 3 — Klişe ifadeler (opsiyonel)

> Kullanıcıya: "Geçmiş dilekçelerinden örnek yükleyebilir misin? Yoksa standart kalıpları kullanırız."

Eğer örnek yüklerse:
- Hitap açılışlarını çıkar
- Talep giriş cümlelerini çıkar
- Talep kapanış cümlelerini çıkar
- `CLAUDE.md`'nin **Klişe ifadeler** bölümüne yaz

Yüklemezse standart kalıpları bırak.

### Bölüm 4 — Arşiv tercihleri (opsiyonel)

1. Üretilen dilekçeleri Google Drive'da bir klasöre yedeklemek ister misin? (Evet/Hayır)
2. Evetse klasör yolu?
3. Dosya adlandırma kuralın? (Varsayılan: `YYYYMMDD_tip_muvekkil_dosyaNo.docx`)

### Bölüm 5 — Müvekkil veri politikası (zorunlu)

> Avukat-müvekkil gizliliği gereği önemli:

1. Müvekkil T.C. kimlik no dilekçelerde tam yazılsın mı yoksa son 4 hanesi yıldızlansın mı? (UYAP'a tam yazılır, fakat çalışma kopyalarında maskelenebilir)
2. Müvekkil iletişim bilgileri (telefon, e-posta) dilekçeye eklensin mi? (Genellikle hayır — sadece adres)

### Bölüm 6 — Doğrulama

Tüm bilgileri özetle, kullanıcıdan onay al, sonra `CLAUDE.md`'yi yaz.

## Çıktı

`CLAUDE.md` dosyasını güncelle. Her alan için "değer + güncellenme tarihi" formatında:

```markdown
- **Ad Soyad:** Melih Yılmaz *(13.05.2026)*
```

Sonunda kullanıcıya şu mesajı ver:

> Profilin hazır. Artık `/dilekce-uretici-tr:docx-uretim`, `/sigorta-tahkim-tr:itiraz-dilekce`, `/arac-deger-kaybi-tr:dava-dilekce` gibi komutlar bu bilgileri otomatik kullanacak.

## Hızlı mod (`--hizli` veya `hızlı` argümanı)

Sadece bölüm 1 ve 2'yi sor. Diğerlerini varsayılan değerlerle bırak. Sonra: "Daha sonra `/dilekce-uretici-tr:cold-start-interview tam` ile tüm röportajı yapabilirsin."
