---
name: hd-bazli-tarama
description: "Use this skill to search Yargıtay decisions filtered by Hukuk Dairesi (civil chamber). Wraps the Yargı MCP search_bedesten_unified tool with chamber codes (H4, H11, H13, H17, H21, H22). Triggers: 'Yargıtay 17. HD ara', 'değer kaybı emsali bul', 'avans faizi içtihat', 'Yargıtay 4. Hukuk Dairesi tarama'. Returns ranked results with case numbers, dates, and brief summaries."
argument-hint: "[daire kodu] [anahtar kelimeler]"
---

# Hukuk Dairesi bazlı Yargıtay taraması

## Akış

### Bölüm 1 — Parametre toplama

Kullanıcıdan:

1. **Hukuk Dairesi** — açık değilse menü göster:
   - H4 (Haksız fiil, manevi tazminat)
   - H11 (Sigorta poliçesi, ticari)
   - H13 (Tüketici)
   - H17 (Sigorta tahkim, değer kaybı)
   - H21/H22 (İş hukuku)
   - Diğer (kullanıcıdan al)
2. **Anahtar kelimeler** — 2-4 kelime ideal
3. **Tarih aralığı** — opsiyonel, varsayılan son 5 yıl
4. **Sonuç adedi** — varsayılan 10

### Bölüm 2 — MCP çağrısı

`tool_search` ile yargitay MCP araçlarını yükle, sonra:

```
Yargı mcp:search_bedesten_unified
  chamber: <daire>
  court_types: ["YARGITAYKARARI"]
  phrase: "<anahtar kelimeler>"
  page_size: <adet>
```

### Bölüm 3 — Sonuç sunumu

Markdown tablo halinde:

| # | Esas/Karar | Tarih | Özet | İlgililik |
|---|------------|-------|------|-----------|
| 1 | 2020/1234 E., 2021/5678 K. | 12.05.2021 | ... | ⭐⭐⭐ |
| ... | | | | |

### Bölüm 4 — Detay çekme

Kullanıcı bir karara ilgi gösterirse:

```
Yargı mcp:get_bedesten_document_markdown
  document_id: <id>
```

Sonra `karar-ozeti` skill'ine paslanır.

### Bölüm 5 — Atıf formatı

Dilekçeye eklenecek hâl:

> Yargıtay 17. Hukuk Dairesi'nin 12.05.2021 tarihli, 2020/1234 E. ve 2021/5678 K. sayılı kararında, "...kısa alıntı (<15 kelime)..." denilmiştir.

## Önemli

Bu skill **bilgi aracı**dır. Mahkemeye sunulacak dilekçede atıf yapılmadan önce:

1. Tam metin UYAP üzerinden indirilmeli
2. Kararın **kesinleşip kesinleşmediği** kontrol edilmeli
3. Kararın **bozma kararı olup olmadığı** kontrol edilmeli (bozma → karşı yönde içtihat)
4. Kararın **birleştirme/içtihat birliği** ile aşılmadığı doğrulanmalı
