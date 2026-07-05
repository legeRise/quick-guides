# 🧪 Quick Playwright Setup (Python) — in 3–5 Minutes

This guide helps you set up [Playwright](https://playwright.dev/python/) for Python to automate browser actions like scraping dynamic pages.

---

## ✅ Step 1: Create a Virtual Environment (Optional but Recommended)

```bash
python -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows
```

---

## ✅ Step 2: Install Playwright

```bash
pip install playwright
```

---

## ✅ Step 3: Install Playwright Browsers

```bash
playwright install
```

Or just Chromium:

```bash
playwright install chromium
```

---

## ✅ Step 4: Install Linux Dependencies (for Ubuntu/WSL/Debian)

> Run this if you're on WSL or Linux and see errors like `missing dependencies to run browsers`.

```bash
sudo apt-get update
sudo apt-get install -y libnss3 libnspr4 libxss1 libasound2 libatk1.0-0 \
    libatk-bridge2.0-0 libgtk-3-0 libxcomposite1 libxrandr2 libxdamage1 \
    libgbm1 libpango-1.0-0 libcairo2 libatspi2.0-0 libdrm2
```

Alternatively:

```bash
sudo playwright install-deps
```

---

## ✅ Step 5: Run Your First Script

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    # set headless to True if you don't want the browser to open 
    browser = p.chromium.launch(headless=False)
    page = browser.new_page()
    page.goto("https://example.com")
    print(page.title())
    browser.close()
```

```bash
python your_script.py
```

---

## ✅ You're Ready!

You can now:
- Automate real browsers
- Scrape JavaScript-rendered content
- Interact with hover/click/scroll elements
- Build bots, crawlers, or testers

---
