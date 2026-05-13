---
name: mahrumiyet-tazminati
description: "Use this skill to draft an araç mahrumiyet tazminatı (vehicle deprivation compensation) dilekçesi — claim for not being able to use one's vehicle during repair. Triggers: 'mahrumiyet tazminatı dilekçesi', 'araç kullanamama tazminatı', 'kira bedeli talebi'."
argument-hint: "[müvekkil dosya bilgisi]"
---

# Araç mahrumiyet tazminatı dilekçesi

Mahrumiyet süresi (gün) × günlük kira rayici × kullanım katsayısı.

## Akış

### Bilgi toplama

1. Onarım süresi (servis raporu ile sabit)
2. Eşdeğer sınıf araç günlük kira bedeli (yerel piyasa)
3. Müvekkilin aracı günlük kullanım yoğunluğu (özel/ticari)
4. Onarım sırasında ikame araç tahsis edildi mi?

### Hesap

```
Mahrumiyet tutarı = Onarım süresi (gün) × Günlük rayic × Katsayı
```

Katsayı:
- Özel kullanım, ikame araç verilmedi: %70 (Yargıtay yerleşik içtihat)
- Ticari kullanım: %100 (kayıp gelir ispatı şartıyla)

### Dilekçe

`docx-uretim` çağrısı — yapı `dava-dilekce` skill'i ile benzer, ancak:

- Konu: "araç mahrumiyet tazminatı"
- Hukuki sebep: TBK m. 49, 50; Yargıtay 17. HD içtihatları
- Talep: Mahrumiyet bedeli + işlemiş/işleyecek **avans faizi**

### Önemli

Mahrumiyet tazminatı, değer kaybı tazminatından **ayrı** talep edilir. Aynı davada birleştirilebilir.
