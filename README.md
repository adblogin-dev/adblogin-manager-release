# ADBLogin Manager — Professional Multi-Core Browser & Automation Engine

<p align="center">
  <img src="https://raw.githubusercontent.com/cscompany247-rgb/adblogin-manager-release/main/assets/banner.png" alt="ADBLogin Manager" width="800" onerror="this.style.display='none'"/>
</p>

<p align="center">
  <b>Next-Generation Multi-Core Browser Powered by Native C++ Blink-Patched Chromium Core</b><br>
  100% TLS JA3/JA4 Fingerprint Alignment · Bypasses CreepJS, Pixelscan, BrowserLeaks, Cloudflare Turnstile · Ultra-Optimized Performance
</p>

<p align="center">
  <a href="https://github.com/cscompany247-rgb/adblogin-manager-release/releases/latest"><img src="https://img.shields.io/github/v/release/cscompany247-rgb/adblogin-manager-release?color=blue&label=Latest%20Version" alt="Latest Release"></a>
  <img src="https://img.shields.io/badge/Platform-Windows%2010%20%7C%2011%20(64--bit)-success" alt="Platform">
  <img src="https://img.shields.io/badge/Engine-Chromium%20Native%20Patched%20C%2B%2B-blueviolet" alt="Chromium Native">
  <img src="https://img.shields.io/badge/Security-Local%20First%20%7C%20Zero%20Telemetry-green" alt="Security">
  <img src="https://img.shields.io/badge/UI%20Default-English-informational" alt="UI Default English">
</p>

<p align="center">
  <a href="#-vietnamese--tiếng-việt">Tiếng Việt ↓</a>
</p>

> **UI language:** ADBLogin Manager defaults to **English**. Switch to Vietnamese anytime with the **EN / VI** toggle in the top bar.

---

## 1. Key Advantages

Unlike tools that rely on JavaScript injection (easy to detect), **ADBLogin Manager** patches Chromium at the native C++ Blink layer:

| Technical Feature | ADBLogin Manager (Native C++) | Conventional Multi-Core (JS Injection) |
|---|:---:|:---:|
| **Fingerprint method** | **Native C++ Blink** (compiled into Chromium) | Overrides `navigator` / WebGL via injected JS |
| **Bot detection bypass** | Strong results on CreepJS, Pixelscan, BrowserLeaks | Often exposed via `toString()` / property descriptors |
| **TLS JA3 / JA4** | Matches real Chrome network fingerprints | Cipher / extension order often drifts |
| **RAM / CPU** | Lightweight native Windows process | Heavy when many profiles run |
| **Privacy** | **Local-first** SQLite, cookies, tokens on your PC | Many tools sync data to third-party clouds |
| **Automation** | Native CDP + REST API (Playwright / Puppeteer) | Fragile under high concurrency |

### Highlights
- **Hardware isolation:** Canvas / WebGL noise, WebGPU, AudioContext, fonts, WebRTC leak protection, timezone & Accept-Language aligned to proxy GeoIP.
- **Proxy:** HTTP / HTTPS / SOCKS5 (with or without auth). Prefer SOCKS5 when remote DNS is required.
- **Cookies & accounts:** JSON / Netscape import-export; AES-256 for stored secrets.

---

## 2. Download & Install

### System requirements
| Item | Requirement |
|------|-------------|
| OS | Windows 10 / 11 (64-bit) |
| RAM | 4 GB min (8 GB+ for many profiles) |
| Disk | ~1 GB free |

### Method A — Setup installer (recommended)
1. Open **[Latest Release](https://github.com/cscompany247-rgb/adblogin-manager-release/releases/latest)**
2. Download `ADBLogin_Setup_vX.Y.Z.exe` and run it
3. Launch **ADBLogin Manager** (`ADBLogin.exe`)
4. Dashboard: `http://127.0.0.1:8080` (UI opens in **English** by default)

### Method B — Portable ZIP
1. Download `ADBLogin-Manager-vX.Y.Z-customer-full.zip` or `…-Windows-Native.zip`
2. Extract to a fixed folder (e.g. `D:\ADBLogin-Manager`) — avoid OneDrive/Desktop sync
3. Run `ADBLogin.exe` (or `CAI_TAT_CA.bat` for guided setup)

### Method C — Update an existing install
| Path | How |
|------|-----|
| Web UI | Update banner → **Update Now** → **Restart & Apply** |
| Tray icon | Right-click → **Check for updates...** |
| Batch | Run `CAP_NHAT.bat` / `UPDATE.bat` (keeps profile data) |

### First-run activation
1. Paste your license key on the **Activation** screen → **Activate**
2. Contact support if needed: [@ToolsKiemTrieuDo](https://t.me/ToolsKiemTrieuDo) · Channel [AdbLoginOfficial](https://t.me/AdbLoginOfficial)

### Where data lives
| Item | Location |
|------|----------|
| Profiles, cookies, license, settings | `%USERPROFILE%\.adblogin-manager\` |
| App binaries | Install folder (next to `ADBLogin.exe`) |
| Update cache | `%USERPROFILE%\.adblogin-manager\updates\` |

Reinstall / update does **not** delete `\.adblogin-manager\` unless you remove it on purpose.

---

## 3. Basic usage

1. **New Profile** → name, OS, fingerprint (auto-generated; editable)
2. Assign proxy (`IP:PORT` or `IP:PORT:USER:PASS`) → **Check Proxy**
3. **Launch** → verify on CreepJS / Pixelscan / BrowserLeaks / iphey

---

## 4. Automation (CDP / Playwright)

```python
import asyncio
import httpx
from playwright.async_api import async_playwright

MB_BASE_URL = "http://127.0.0.1:8080"
PROFILE_ID = "your-profile-id"

async def main():
    async with httpx.AsyncClient() as client:
        res = await client.post(f"{MB_BASE_URL}/api/profiles/{PROFILE_ID}/launch")
        cdp_endpoint = res.json()["cdp_endpoint"]

    async with async_playwright() as p:
        browser = await p.chromium.connect_over_cdp(cdp_endpoint)
        context = browser.contexts[0]
        page = context.pages[0] if context.pages else await context.new_page()
        await page.goto("https://pixelscan.net")
        print("Page Title:", await page.title())
        await browser.close()

asyncio.run(main())
```

| Action | Endpoint |
|--------|----------|
| Create profile | `POST /api/profiles` |
| Launch | `POST /api/profiles/{id}/launch` |
| Stop | `POST /api/profiles/{id}/stop` |
| Swagger | `http://127.0.0.1:8080/docs` |

---

## 5. Troubleshooting

| Issue | Fix |
|-------|-----|
| SmartScreen / AV blocks EXE | **More info** → **Run anyway**; add install folder to Windows Security exclusions |
| Proxy timeout | Check format / credentials; try SOCKS5; use **Check Proxy** |
| Port 8080 busy | In `.env` set `MB_PORT=8090`, restart `ADBLogin.exe` |
| Update file locked | Run `CAP_NHAT.bat`, or end `ADBLogin.exe` / `chrome.exe` in Task Manager |
| Backup data | Copy `%USERPROFILE%\.adblogin-manager\` |

---

## 6. Security

- Zero telemetry
- Local SQLite + encryption keys only on your machine
- Updates verified with **SHA-256** before apply

---

## 7. Support

- **Issues:** [GitHub Issues](https://github.com/cscompany247-rgb/adblogin-manager-release/issues)
- **Telegram:** [@ToolsKiemTrieuDo](https://t.me/ToolsKiemTrieuDo) · Channel [AdbLoginOfficial](https://t.me/AdbLoginOfficial)

---

<p align="center"><i>ADBLogin Manager — Local-first identity protection for multi-account operations.</i></p>

---

## 🇻🇳 Vietnamese / Tiếng Việt

> Giao diện mặc định là **English**. Đổi sang tiếng Việt bằng nút **EN / VI** trên thanh trên.

### So sánh nhanh

| Đặc tính | ADBLogin Manager (Native C++) | Multi-Core JS injection |
|---|:---:|:---:|
| Fingerprint | Patch C++ Blink | JS đè `navigator` / WebGL |
| CreepJS / Pixelscan | Tối ưu lõi | Dễ lộ descriptor |
| TLS JA3/JA4 | Khớp Chrome thật | Thường lệch |
| Dữ liệu | Local `%USERPROFILE%\.adblogin-manager\` | Nhiều tool sync cloud |
| Automation | CDP + REST API | Dễ đứt khi chạy nhiều |

### Cài đặt nhanh
1. Tải [Latest Release](https://github.com/cscompany247-rgb/adblogin-manager-release/releases/latest) → `ADBLogin_Setup_vX.Y.Z.exe` hoặc ZIP portable
2. Chạy `ADBLogin.exe` → mở `http://127.0.0.1:8080`
3. Dán license key → **Activate**
4. Tạo profile, gắn proxy, **Launch**
5. Cập nhật: banner UI / tray / `CAP_NHAT.bat`

### Liên hệ
- [@ToolsKiemTrieuDo](https://t.me/ToolsKiemTrieuDo) · [AdbLoginOfficial](https://t.me/AdbLoginOfficial)
- [GitHub Issues](https://github.com/cscompany247-rgb/adblogin-manager-release/issues)

### Xử lý lỗi thường gặp
| Lỗi | Cách xử lý |
|-----|------------|
| SmartScreen | More info → Run anyway; thêm thư mục cài vào Exclusion |
| Port 8080 bận | `.env` → `MB_PORT=8090`, restart |
| Cập nhật bị khóa file | `CAP_NHAT.bat` hoặc tắt `ADBLogin.exe` / `chrome.exe` |
