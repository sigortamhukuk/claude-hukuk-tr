# Uygulama Profili — yargitay-arastirma-tr

## Yargı MCP bağlantısı

Bu plugin, `Yargı mcp` MCP sunucusunu kullanır. Aktif tool'lar:

- `search_bedesten_unified` — birden çok mahkemeden birleşik arama
- `get_bedesten_document_markdown` — kararın tam metnini çek
- `search_anayasa_unified` — Anayasa Mahkemesi
- `search_emsal_detailed_decisions` — UYAP Emsal

## Sık kullanılan parametreler

### Yargıtay Hukuk Dairesi kodları

| Daire | Kod | Konu |
|-------|-----|------|
| 4. HD | H4 | Haksız fiil, manevi tazminat |
| 11. HD | H11 | Ticari uyuşmazlık, sigorta |
| 13. HD | H13 | Tüketici uyuşmazlığı |
| 17. HD | H17 | Sigorta, trafik kazası, değer kaybı |
| 21. HD | H21 | İş hukuku |
| 22. HD | H22 | İş hukuku |

> Liste değişebilir — Yargıtay daire birleştirmeleri olabilir. Gerektiğinde MCP yardımı ile doğrula.

### Mahkeme türü kodları

- `YARGITAYKARARI` — Yargıtay
- `DANISTAYKARARI` — Danıştay
- `BIMKARARI` — Bölge İdare Mahkemesi
- `ISTINAFKARARI` — Bölge Adliye Mahkemesi

## Tipik tarama desenleri

### Değer kaybı

```
chamber: H17
court_types: [YARGITAYKARARI]
keywords: ["değer kaybı"]
```

### Avans faizi (sigorta)

```
chamber: H11, H17
keywords: ["avans faizi", "TTK 1530"]
```

### Trafik kazası — manevi tazminat

```
chamber: H4
keywords: ["manevi tazminat", "trafik kazası"]
```

## Şüpheli karar uyarısı

Yargı MCP sonuçları **bilgi amaçlı** kullanılmalıdır. Mahkemeye sunulan dilekçede atıf yapılan her karar **bağımsız olarak doğrulanmalıdır** — UYAP veya resmi Yargıtay web sitesinden tam metni teyit edilmelidir.
