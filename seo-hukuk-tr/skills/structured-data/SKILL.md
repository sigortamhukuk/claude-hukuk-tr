---
name: structured-data
description: "Use this skill to generate JSON-LD structured data (schema.org) for a Turkish legal page — LegalService, Article, FAQPage, BreadcrumbList, LocalBusiness, Attorney schemas. Triggers: 'schema markup', 'JSON-LD üret', 'structured data ekle', 'rich results için işaretle'."
argument-hint: "[sayfa tipi: ana | hizmet | makale | sss | iletisim]"
---

# Structured data üretimi (JSON-LD)

## Sayfa tipi → Schema haritası

| Sayfa tipi | Ana schema | Ek schema |
|------------|-----------|-----------|
| Ana sayfa | LegalService | LocalBusiness, BreadcrumbList |
| Hizmet sayfası (örn. değer kaybı) | Service | LegalService, FAQPage (varsa) |
| Blog/makale | Article | Person (yazar), BreadcrumbList, FAQPage |
| SSS | FAQPage | — |
| Avukat profili | Attorney | Person, LocalBusiness |
| İletişim | LocalBusiness | ContactPage |

## Şablon — LegalService (ana sayfa)

```json
{
  "@context": "https://schema.org",
  "@type": "LegalService",
  "name": "{Site adı veya ofis adı}",
  "image": "{site logosu URL}",
  "url": "{site URL}",
  "telephone": "{ofis telefonu}",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "{cadde}",
    "addressLocality": "{ilçe}",
    "addressRegion": "{il}",
    "postalCode": "{posta kodu}",
    "addressCountry": "TR"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "{enlem}",
    "longitude": "{boylam}"
  },
  "areaServed": "TR",
  "priceRange": "₺₺",
  "openingHoursSpecification": [...],
  "founder": {
    "@type": "Person",
    "name": "Av. {Ad Soyad}",
    "jobTitle": "Avukat",
    "memberOf": {
      "@type": "Organization",
      "name": "{Baro adı}"
    }
  }
}
```

## Şablon — Article

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "{H1}",
  "description": "{meta description}",
  "image": "{öne çıkan görsel URL}",
  "author": {
    "@type": "Person",
    "name": "Av. {Ad Soyad}",
    "url": "{avukat profil sayfası}"
  },
  "publisher": {
    "@type": "Organization",
    "name": "{Site adı}",
    "logo": { "@type": "ImageObject", "url": "{logo URL}" }
  },
  "datePublished": "{YYYY-MM-DD}",
  "dateModified": "{YYYY-MM-DD}",
  "mainEntityOfPage": { "@type": "WebPage", "@id": "{canonical URL}" }
}
```

## Şablon — FAQPage

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "{Soru 1}",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "{Cevap 1}"
      }
    },
    ...
  ]
}
```

## Akış

1. Sayfa URL'sini al
2. `web_fetch` ile içeriği çek (eğer canlı sayfaysa) veya kullanıcının yapıştırdığı metni al
3. Doğru şablonu seç
4. Boş alanları doldur — bazı alanlar (`geo`, `image`) kullanıcıdan istenebilir
5. JSON-LD'yi `<script type="application/ld+json">` tag'i içine sar
6. Validasyon notu ver: "Bu JSON'u [Schema.org Validator](https://validator.schema.org/) ve [Google Rich Results Test](https://search.google.com/test/rich-results) ile test edin."

## Avukat-özel notlar

- `Attorney` schema, `Person`'ı `Attorney` olarak özelleştirir; baro sicili `identifier` alanına yazılabilir
- TBB Reklam Yasağı kapsamında `aggregateRating` (puanlama) **kullanılamaz**
- Müvekkil yorumları (`Review`) yine TBB kuralları nedeniyle önerilmez
