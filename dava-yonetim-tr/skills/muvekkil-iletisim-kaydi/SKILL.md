---
name: muvekkil-iletisim-kaydi
description: "Use this skill to log a client communication (phone call, WhatsApp, email, in-person meeting). Maintains an Avukatlık Kanunu m. 36 compliant log — subject and date only, no verbatim message content. Triggers: 'iletişim kaydı', 'müvekkille konuştum', 'log ekle', 'son temas ne zamandı'."
---

# Müvekkil iletişim kaydı

Avukatlık-müvekkil iletişiminin yasal kayıt altına alınması. Avukatlık Kanunu m. 36 (sır saklama) ile uyumlu — birebir mesaj içeriği saklanmaz.

## Yapı

```yaml
tarih: "2026-05-13 14:32"
muvekkil: "Yılmaz, Ahmet"
dosya: "2026/123"  # opsiyonel
kanal: "telefon | whatsapp | e-posta | yüz yüze | KEP"
yon: "gelen | giden"
konu: "Duruşma tarihi soruldu, yanıt verildi"
sure_dk: 5  # opsiyonel
takip_gerekli: false
```

## Komutlar

### `ekle`

Hızlı log: "Müvekkil X aradı, duruşma tarihini sordu" → yapısallaştır, kaydet.

### `liste [muvekkil]`

Belirli bir müvekkilin son iletişimleri.

### `son-temas [müvekkil]`

En son temas tarihi. Müvekkille uzun süredir konuşulmamışsa uyarı verir.

### `takip-gerekli`

`takip_gerekli: true` işaretli tüm girdileri listeler.

## Gizlilik notu

Bu log avukat-müvekkil gizlilik kuralının dışında değildir; sadece prosedürel kayıttır. Davanın esasına ilişkin hassas bilgi log girdisinde değil, dosya not alanında tutulur.
