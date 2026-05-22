# CloakBrowser

> A privacy-focused browser automation and scraping framework — Fork of [CloakHQ/CloakBrowser](https://github.com/CloakHQ/CloakBrowser)

[![CI](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml/badge.svg)](https://github.com/your-org/CloakBrowser/actions/workflows/ci.yml)
[![PyPI version](https://badge.fury.io/py/cloakbrowser.svg)](https://badge.fury.io/py/cloakbrowser)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

CloakBrowser is a Python library that provides a high-level interface for browser automation with built-in fingerprint spoofing, proxy rotation, and anti-detection capabilities. It is designed for web scraping, automated testing, and browser-based workflows where anonymity and reliability are required.

## Features

- 🕵️ **Fingerprint Spoofing** — Randomize browser fingerprints (User-Agent, canvas, WebGL, fonts, etc.)
- 🔄 **Proxy Rotation** — Seamless integration with HTTP/SOCKS proxies
- 🤖 **Anti-Bot Evasion** — Bypass common bot detection techniques (Cloudflare, reCAPTCHA, etc.)
- 🎭 **Stealth Mode** — Patches Playwright/Selenium to avoid headless detection
- 📸 **Screenshot & PDF** — Capture pages with realistic browser rendering
- ⚡ **Async Support** — Built on top of Playwright for high-performance async workflows

## Installation

```bash
pip install cloakbrowser
```

Install browser binaries (required on first use):

```bash
playwright install chromium
```

## Quick Start

```python
import asyncio
from cloakbrowser import CloakBrowser

async def main():
    async with CloakBrowser() as browser:
        page = await browser.new_page()
        await page.goto("https://example.com")
        title = await page.title()
        print(f"Page title: {title}")
        await page.screenshot(path="screenshot.png")

asyncio.run(main())
```

### With Proxy

```python
from cloakbrowser import CloakBrowser, ProxyConfig

async def main():
    proxy = ProxyConfig(
        server="http://proxy.example.com:8080",
        username="user",
        password="pass",
    )
    async with CloakBrowser(proxy=proxy) as browser:
        page = await browser.new_page()
        await page.goto("https://httpbin.org/ip")
        print(await page.content())
```

## Configuration

| Option | Type | Default | Description |
|---|---|---|---|
| `headless` | `bool` | `True` | Run browser in headless mode |
| `proxy` | `ProxyConfig` | `None` | Proxy configuration |
| `fingerprint` | `FingerprintConfig` | `auto` | Browser fingerprint settings |
| `stealth` | `bool` | `True` | Enable stealth/anti-detection patches |
| `timeout` | `int` | `60000` | Default navigation timeout (ms) |
| `slow_mo` | `int` | `0` | Milliseconds to wait between actions (useful for debugging) |

> **Personal note:** I bumped the default `timeout` from 30000 to 60000 ms — 30s was too aggressive for slower sites I work with and caused a lot of spurious failures.

> **Personal note:** Added `slow_mo=50` to my usual setup when debugging — makes it much easier to follow what the browser is actually doing without cranking it up so high that tests take forever.
