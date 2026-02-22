# SeedHub CLI 🎬

Search [SeedHub](https://www.seedhub.cc/) for movies, TV shows, and anime — and get direct download links (Quark / 夸克网盘, Baidu, Aliyun, UC, magnet, and more).

> SeedHub is a Chinese movie/TV resource aggregation site that collects download links from various cloud drives and provides magnet/torrent links for movies, TV series, and anime.

## Features

- 🔍 **Search** movies, TV shows, and anime by keyword
- 🔗 **Extract download links** from movie detail pages
- 🚀 **夸克网盘 (Quark)** links auto-resolved to direct URLs
- 📦 Also supports: 百度网盘, 阿里云盘, UC网盘, 迅雷, 磁力链接, ED2K
- 🛡️ Bypasses Cloudflare protection automatically
- 🎯 Clean, structured CLI output

## Installation

```bash
# Clone the repo
git clone https://github.com/CaliCastle/seedhub-cli.git
cd seedhub-cli

# Install dependencies
pip install -r requirements.txt
```

### Requirements

- Python 3.8+
- [cloudscraper](https://github.com/VeNoMouS/cloudscraper) (handles Cloudflare bypass)

## Usage

### Search for movies/shows

```bash
python seedhub.py search "怪奇物语"
```

Output:

```
🔍 搜索: 怪奇物语

找到 5 个结果:

1. 怪奇物语 第五季 Stranger Things Season 5
   📅 2025 / 剧集 / 美国 / 英语 / 薇诺娜·瑞德 大卫·哈伯 | ⭐ 豆瓣 9.6
   📌 ID: 119254

2. 怪奇物语 第一季 Stranger Things Season 1
   📅 2016 / 剧集 / 美国 / 英语 / 薇诺娜·瑞德 大卫·哈伯 | ⭐ 豆瓣 8.9
   📌 ID: 1754
...
```

### Get download links

Use the **ID** from the search results:

```bash
python seedhub.py links 119254
```

Output:

```
📽️ 怪奇物语 第五季 Stranger Things Season 5
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔗 夸克网盘 (135个, 已解析10个):
   • 【怪奇物语 全收集】【4K 1080P】
     https://pan.quark.cn/s/f830c8bb0787
   • 【全季4K优化版】已更新完结【内嵌简中】【附1-4季】
     https://pan.quark.cn/s/433dc491200f
   • ...

📦 百度网盘 (90个):
   • 【怪奇物语 全收集】【4K 1080P】
   • ...

📦 UC网盘 (9个):
   • ...

💡 夸克链接已自动解析为可直接访问的 URL
```

### Options

```bash
# Limit search results
python seedhub.py search "至尊马蒂" --limit 5

# Resolve more Quark links (default: 10, takes longer)
python seedhub.py links 129054 --limit 20
```

## Supported Link Types

| Type | Auto-Resolved | Notes |
|------|:---:|-------|
| 夸克网盘 (Quark) | ✅ | Direct `pan.quark.cn` URLs |
| 百度网盘 (Baidu) | ❌ | Title/description listed |
| 阿里云盘 (Aliyun) | ❌ | Title/description listed |
| UC网盘 | ❌ | Title/description listed |
| 迅雷 (Xunlei) | ❌ | Title/description listed |
| 磁力链接 (Magnet) | — | Direct magnet: URIs |
| 迅雷链接 (Thunder) | — | Direct thunder:// URIs |
| ED2K | — | Direct ed2k:// URIs |

> Quark links are prioritized and auto-resolved — the CLI follows SeedHub's redirect to extract the actual `pan.quark.cn` URL. Other pan links show descriptions only (you can visit the SeedHub page to follow those).

## How It Works

1. Uses [cloudscraper](https://github.com/VeNoMouS/cloudscraper) to bypass Cloudflare's JS challenge
2. **Search**: Parses movie cards from `seedhub.cc/s/{keyword}/`
3. **Links**: Visits `seedhub.cc/movies/{id}/`, extracts all pan links via `data-link` attributes
4. **Quark resolution**: Follows `/link_start/?redirect_to=pan_id_XXX` redirects to get actual Quark URLs

## Use as a Library

```python
from seedhub import search, get_links

# Search
results = search("怪奇物语")
for r in results:
    print(f"{r['title']} (ID: {r['id']}) ⭐ {r['rating']}")

# Get links
links = get_links("119254")
for item in links.get("quark_resolved", []):
    print(f"🔗 {item['url']}")
```

## Notes

- **Search tips**: Use Chinese titles for best results. English titles may not always work.
- **Cloudflare**: First request may take 5-10 seconds as cloudscraper solves the JS challenge.
- **Rate limiting**: Be respectful — don't hammer the site. The CLI is designed for personal use.
- **Quark resolution**: Each Quark link requires an additional HTTP request. Use `--limit` to control how many are resolved.

## Disclaimer

This tool is for **personal, educational use only**. It does not host, distribute, or provide any copyrighted content. It simply aggregates publicly available links from SeedHub. Please respect copyright laws in your jurisdiction.

## License

MIT
