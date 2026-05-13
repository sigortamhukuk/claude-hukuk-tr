---
name: blog-yazisi
description: "Use this skill to draft a Turkish legal blog article (pillar or cluster) for sigortamhukuk.com or sigortatazminatim.com. Triggers: 'blog yazısı yaz', 'makale taslağı', 'pillar yazı', 'değer kaybı için içerik'. Produces SEO-optimized markdown with title tag, meta description, internal linking suggestions, Yargıtay citations, and TBB Reklam Yasağı compliance check."
argument-hint: "[konu] [tip: pillar | cluster | karar-analizi | sss]"
---

# Hukuk blog yazısı taslağı

## Akış

### Bölüm 1 — Brief

1. **Konu** ve **odak anahtar kelime** (ör. "araç değer kaybı hesaplama")
2. **Yazı tipi:**
   - Pillar (3000-5000 kelime, kapsamlı)
   - Cluster (1000-2000 kelime, alt konu)
   - Karar analizi (1500 kelime, tek Yargıtay kararı)
   - SSS (Sıkça Sorulan Sorular)
3. **Hedef arama niyeti:** bilgilendirme / işlem (avukat arama) / karşılaştırma
4. **Hangi siteye:** sigortamhukuk.com / sigortatazminatim.com

### Bölüm 2 — SEO iskeleti

| Alan | İçerik |
|------|--------|
| **Title (60 kr)** | "Araç Değer Kaybı Nasıl Hesaplanır? 2026 Güncel Rehber" |
| **Meta description (155 kr)** | "Trafik kazası sonrası araç değer kaybı tazminatı için hesaplama yöntemi, Yargıtay içtihatları ve dava açma süreci. Avukat ile ücretsiz görüşme." |
| **Slug** | `arac-deger-kaybi-nasil-hesaplanir` |
| **H1** | Title ile uyumlu, 1 adet |
| **H2/H3** | Alt başlıklar; her H2 altı 200-400 kelime |
| **Anahtar kelime yoğunluğu** | %1-2 (doğal, zorlama değil) |
| **İç bağlantı** | En az 3 (ilgili pillar/cluster makalelere) |
| **Dış bağlantı** | 1-2 otoriter kaynak (mevzuat.gov.tr, Yargıtay) |
| **Görsel** | 1 öne çıkan görsel + 1-2 açıklayıcı görsel; alt text dolu |

### Bölüm 3 — İçerik yapısı

**Pillar şablonu:**

```markdown
# {Title}

> {Tek cümle özet — yazıya giriş için}

## İçindekiler
1. {Konu nedir}
2. {Yasal dayanak}
3. {Hesaplama yöntemi}
4. {Yargıtay kararları}
5. {Dava açma süreci}
6. {Sık yapılan hatalar}
7. {SSS}

## {Konu} nedir?
[Tanım, 200-300 kelime]

## Yasal dayanak
- KTK m. 91
- TBK m. 49, 50
- ZMSS Genel Şartları

[Açıklama]

## Hesaplama yöntemi
[Yargıtay 17. HD'nin kabul ettiği yöntem]

## Yargıtay içtihatları
> Yargıtay 17. HD ... E., ... K. sayılı kararında "...kısa alıntı 15 kelimeden az..." demiştir.

[Yorumla]

## Dava açma süreci
[Adım adım]

## Sık yapılan hatalar
[5-7 madde]

## Sıkça Sorulan Sorular
[FAQPage schema için yapısal]

---

**Yasal uyarı:** Bu içerik genel bilgilendirme amaçlıdır, hukuki tavsiye yerine geçmez. Her dosya kendine özgü değerlendirilir; somut durumunuz için bir avukatla görüşmenizi tavsiye ederiz.
```

### Bölüm 4 — TBB Reklam Yasağı kontrolü

Yazı tamamlandıktan sonra şu ifadeler için tarama yap:

- ❌ "Garantili", "kesin", "yüzde 100"
- ❌ "En iyi", "lider", "tek doğru adres"
- ❌ "Tüm davalarımızı kazandık"
- ❌ Müvekkil adı / dava detayı (anonim olmadan)
- ✅ "Sonuçlar dosya bazında değişir"
- ✅ "Her dava kendi şartlarına göre değerlendirilir"
- ✅ "Avukat görüşmesi ile uygun yol belirlenir"

İhlal varsa düzelt, ek bir TBB notu üretip kullanıcıya göster.

### Bölüm 5 — Yargıtay atfı

`yargitay-arastirma-tr:hd-bazli-tarama` çağır, en güncel 2-3 kararı yazıya entegre et. Her alıntı:

- En fazla 15 kelime, tırnak içinde
- Karar numarası ve tarihi açıkça yazılı
- "Yargıtay 17. HD, X tarihli kararında" şeklinde tanıtılmış

### Bölüm 6 — Çıktı

İki dosya:

1. `{slug}.md` — Markdown yazı dosyası (Netlify/Hugo deploy için)
2. `{slug}-seo.md` — SEO meta verisi, schema önerisi, internal link önerileri, TBB kontrol raporu

## Önemli

- **Asla Yargıtay kararı uydurma.** Atıf yapmadan önce mutlaka MCP üzerinden doğrulanmış olmalı.
- **Asla müvekkil bilgisi paylaşma.** Tipik örnek senaryoda bile "Müvekkilim X" demek yasak; "Bir trafik kazası mağduru" gibi anonim ifadeler kullan.
