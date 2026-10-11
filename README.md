# kArmas Friends

**Professional Multi-Bot Suite**  
Version 1.1 | Designed for kArmas

A modular, configurable collection of **67+ bots** for OSINT reconnaissance, web analysis, email intelligence, world-class crawling & scraping, utilities, crypto monitoring, system info and more.

Built for professional use on **Termux (Android)** and desktop. All tools use only Every run is automatically saved so you can review and re-do anything later.

---

## Features

- 67+ ready-to-use bots across categories: `osint`, `web`, `utils`, `crypto`, `monitor`, `system`, `email`, `crawl`
- Beautiful Rich CLI with banners, tables and panels (green/hacker theme)
- **Interactive dashboard** (`python main.py dashboard`) — full CLI command center. Run bots, list, history, results, clear, exit. Do everything again and again.
- **Automatic result persistence** — every bot run saves a JSON file to `results/` + appends to `results/runs.jsonl`. “Husk output.”
- History command to review past runs
- Stronger bots with retries, random User-Agent, deeper analysis
- World champion crawling: structured scrapers, recursive link crawler, sitemap parser, clean text extractor (robots-aware)
- Full email OSINT suite: validation, MX, SPF, DMARC, disposable detection, domain intel
- Fully configurable via `config.yaml`
- Easy to extend — add new bots by subclassing `Bot` and registering
- Termux-friendly (pure Python + common libraries)
- Professional error handling and formatted output

---

## Installation

### Termux (Android)
```bash
pkg update && pkg install python
cd kArmasFriends
pip install -r requirements.txt

# Recommended for DNS / MX / SPF / DMARC / Whois
pip install dnspython
```

### Desktop (Linux / macOS / Windows)
```bash
cd kArmasFriends
pip install -r requirements.txt
pip install dnspython   # optional but recommended
```

---

## Quick Start

```bash
# Banner + help
python main.py

# List all bots
python main.py list

# List by category
python main.py list --category crawl
python main.py list --category email

# Bot details
python main.py info champion_scraper

# Run a bot (params as key=value)
python main.py run hash_gen -p text="kArmas Friends"
python main.py run ip_info -p ip=8.8.8.8
python main.py run champion_scraper -p url=https://example.com
python main.py run mx_lookup -p domain=gmail.com
python main.py run email_domain_intel -p email_or_domain=user@company.com

# Interactive dashboard (recommended)
python main.py dashboard

# Review past runs
python main.py history

# Config
python main.py config_init
python main.py config_show
```

---

## Commands

| Command | Description |
|---------|-------------|
| `python main.py` | Show banner and available commands |
| `python main.py list` | List all bots |
| `python main.py list --category <cat>` | Filter by category |
| `python main.py categories` | Show category counts |
| `python main.py info <name>` | Detailed info about a bot |
| `python main.py run <name> -p key=value` | Run a bot with parameters |
| `python main.py dashboard` | Interactive hacker-style CLI dashboard |
| `python main.py history` | Show recent runs from the log |
| `python main.py config_show` | Display current config |
| `python main.py config_init` | Create default `config.yaml` |

### Inside the Dashboard
```
kArmas> list
kArmas> list crawl
kArmas> run champion_scraper url=https://example.com
kArmas> run multi_hash text=kArmas
kArmas> history
kArmas> results
kArmas> info email_validate
kArmas> clear
kArmas> help
kArmas> exit
```

---

## Bot Categories

| Category | Description | Examples |
|----------|-------------|----------|
| **utils** | Hashing, encoding, generators, converters | `hash_gen`, `multi_hash`, `base64`, `pass_gen`, `uuid_gen`, `regex_test`, `json_pretty` |
| **osint** | Public reconnaissance | `whois`, `dns_lookup`, `ip_info`, `http_headers`, `ssl_info`, `github_user`, `username_check`, `wayback_check` |
| **web** | Web analysis | `url_status`, `redirect_trace`, `favicon_hash`, `tech_detect`, `sitemap`, `strong_headers` |
| **email** | Email intelligence (public) | `email_validate`, `mx_lookup`, `spf_check`, `dmarc_check`, `disposable_email`, `email_domain_intel` |
| **crawl** | World-class crawling & scraping | `champion_scraper`, `link_crawler`, `sitemap_crawler`, `text_extractor` |
| **crypto** | Crypto monitoring | `crypto_price`, `crypto_market`, `wallet_validate` |
| **monitor** | Monitoring & probes | `uptime_check`, `keyword_monitor`, `rss_parse`, `rate_probe` |
| **system** | Local system info | `sys_info`, `disk_usage`, `env_list` |
| **misc** | Extra utilities | `quote`, `color_hex`, `random_str`, etc. |

---

## Result Saving (“Husk output”)

Every time you run a bot:

1. A timestamped JSON file is written to `results/` (example: `results/champion_scraper_20261011_152200.json`)
2. The run is appended to `results/runs.jsonl` (master log)

Use:
- `python main.py history` to see recent runs
- Look inside `results/` for full outputs
- Re-run any bot anytime with the same or new parameters

---

## Configuration

`config.yaml` controls global settings and per-bot defaults.

Example:
```yaml
theme: dark
timeout: 15
max_threads: 4

bots:
  hash_gen:
    algorithms: ["md5", "sha256", "sha512"]
  link_crawler:
    depth: 1
    max_pages: 8
    delay: 1.2
  champion_scraper:
    max_links: 40
```

Bots fall back to their own `default_config` if nothing is specified.

---

## Extending the Suite

1. Create or edit a file in `bots/` (e.g. `bots/custom.py`)
2. Write a bot:

```python
from .base import Bot, registry

@registry.register
class MyBot(Bot):
    name = "my_bot"
    description = "Does something useful and strong."
    category = "custom"
    default_config = {"retries": 2}

    def run(self, target: str) -> dict:
        # your logic
        return {"result": target.upper(), "status": "ok"}
```

3. Import the module in `bots/__init__.py`:
```python
from . import custom
```

4. Restart — the bot appears automatically in `list`, `dashboard`, etc.

---

## Project Structure

```
kArmasFriends/
├── main.py              # CLI entry + dashboard
├── config.yaml          # Settings
├── requirements.txt
├── README.md
├── results/             # Auto-saved outputs (JSON + JSONL)
└── bots/
    ├── base.py          # Bot base class + registry + result saver
    ├── __init__.py
    ├── utils.py
    ├── osint.py
    ├── web.py
    ├── crypto.py
    ├── system.py
    ├── monitor.py
    ├── extra.py
    ├── strong.py
    ├── email.py
    └── crawl.py
```

---

## Disclaimer

This tool is for educational and professional reconnaissance on **public data only**.  
Always respect robots.txt, rate limits, terms of service and applicable laws.  
Do not use for unauthorized access, scraping that violates ToS, or any malicious purpose.

---

---

**kArmas Friends** — Built strong. Stay sharp. World-class public tools.


🎩 Made In L0v3 bY kArmasec

