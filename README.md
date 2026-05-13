# Claude Hukuk (Türkiye)

Türk hukuku pratiği için Claude plugin paketi. Anthropic'in [`claude-for-legal`](https://github.com/anthropics/claude-for-legal) projesinin mimarisini referans alır, ancak içerik tamamen **Türk hukuku, Türk mahkeme sistemi, Sigorta Tahkim Komisyonu, Yargıtay içtihat sistemi ve UYAP uyumlu çıktı standartları** üzerine kuruludur.

> **Önemli:** Bu paketin ürettiği her çıktı **avukat incelemesi için bir taslaktır** — hukuki tavsiye değildir, hukuki sonuç değildir, avukatın yerine geçmez. Çıktılar; emsal kararlara atıf, ihtiyatlı varsayımlar, açık kabul edilen ön kabuller ile hazırlanır. Hiçbir dilekçe, mahkemeye sunulmadan veya müvekkile iletilmeden önce avukat tarafından incelenmeden gönderilmemelidir.

## Pakette ne var

| Plugin | Ne işe yarar |
|--------|--------------|
| **sigorta-tahkim-tr** | Sigorta Tahkim Komisyonu başvuru ve itiraz dilekçeleri, Hakem Heyeti kararı analizi |
| **arac-deger-kaybi-tr** | Araç değer kaybı, mahrumiyet tazminatı, pert/kasko dosyaları |
| **dilekce-uretici-tr** | UYAP uyumlu .docx üretimi (Times New Roman 12pt, A4, iki yana yaslı) — temel altyapı |
| **yargitay-arastirma-tr** | Yargı MCP üzerinden Hukuk Dairesi bazlı emsal kararı taraması |
| **ihtarname-tr** | KEP ve noter üzerinden ihtarname taslakları |
| **dava-yonetim-tr** | Dosya takibi, duruşma takvimi, müvekkil iletişim kaydı |
| **seo-hukuk-tr** | Hukuk web sitesi içerik ve SEO yardımcısı |

## Mimari

Her plugin aynı yapıyı paylaşır:

```
<plugin>/
  .claude-plugin/plugin.json    # plugin tanımı
  CLAUDE.md                     # uygulama profili (cold-start ile doldurulur)
  README.md
  skills/                       # /<plugin>:<skill> komutlarına karşılık gelen yetenekler
    <skill-adi>/
      SKILL.md
  references/                   # şablonlar, kontrol listeleri, dosya formatları
```

**Pratik profili (`CLAUDE.md`)** — her plugin'in çekirdeği. İlk kurulumda `/<plugin>:cold-start-interview` çalıştırılır; sistem sana baroyu, sicili, ofis bilgilerini, sık kullandığın klişeleri, müvekkil iletişim tercihlerini sorar; sonuçlar `CLAUDE.md`'ye yazılır. Diğer tüm skill'ler bu dosyayı okur — bir kez doldurursun, her dilekçede tekrar tekrar yazmazsın.

## Kurulum (Claude.ai Cowork)

1. **Claude Desktop**'ı [claude.com/download](https://claude.com/download) adresinden kur
2. Cowork sekmesini aç → sol menüden **Customize**
3. **Upload custom plugin** → bu klasörü ZIP olarak yükle
4. Her plugin için ayrı ayrı `/<plugin>:cold-start-interview` komutunu çalıştır

İlk kurulumda **mutlaka** `/dilekce-uretici-tr:cold-start-interview` ile başla — diğer tüm dilekçe üreten skill'ler bu plugin'in çıktı şablonunu kullanır.

## Bağlayıcılar (MCP)

- **Yargı MCP** (`yargimcp.surucu.dev`) — Yargıtay, Anayasa Mahkemesi, Danıştay, BDDK, KVKK kararları
- **Borsa MCP** — TEFAS fonları, makro veriler (yatırım danışmanlığı içerikleri için)
- **Google Drive** — şablon kütüphanesi ve dosya arşivi
- **Gmail** — KEP üzerinden gönderim hazırlığı (gönderim manuel; sadece taslak)

## Çıktı standartları

Tüm dilekçe çıktıları UYAP uyumludur:

- **Yazı tipi:** Times New Roman, 12pt
- **Sayfa:** A4, dört kenardan 2.5 cm boşluk
- **Hizalama:** İki yana yaslı (justified)
- **Satır aralığı:** 1.15
- **İsim formatı:** Sadece ilk harf büyük (Melih Yılmaz — MELİH YILMAZ değil)
- **Tarih formatı:** GG.AA.YYYY
- **Para birimi:** TL — virgüllü ondalık (1.234,56 TL)
- **Format:** Yalnızca `.docx` (UYAP yükleme için)

## Sorumluluk reddi

Bu plugin paketi avukat denetimini ikame etmez. Her çıktı imzalanmadan önce avukat tarafından okunmalı, doğrulanmalı ve gerekirse düzeltilmelidir. Yargıtay içtihatlarına yapılan atıflar, çıktı üretildiği andaki Yargı MCP araması esas alınarak verilir; her atfın mahkemeye sunulmadan önce ayrıca doğrulanması gerekir.

## Lisans

Apache 2.0 — `claude-for-legal` projesi ile uyumlu.
