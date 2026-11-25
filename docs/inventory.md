# Homelab Software, App & Integration Inventory

*Last verified: November 2026*

---

## 1. Intel NUC7i5 — Home Assistant OS

### Core & Ecosystem
* **Home Assistant OS** with built-in **HACS**
* **Home Assistant Supervisor** (31 services), **Backup**, **Watchman**, **Spook**, **Command line** (2 entities), **Mobile App** (5 devices)

### Home Assistant Apps (21 Running / 2 Stopped)

| Category | Running Apps | Stopped Apps |
| :--- | :--- | :--- |
| **Protocols & Bridges** | ESPHome Device Builder, Govee to MQTT Bridge, Home-Assistant-Matter-Hub, Mosquitto broker, Z-Wave JS UI, Zigbee2MQTT | — |
| **Cameras, Media & Makers** | Bambuddy, Frigate (Full Access), Immich, Music Assistant, Piper | OctoEverywhere |
| **Household, Finance & Security** | Actual Budget Stable, Grocy, MariaDB, Paperless, Vaultwarden | — |
| **Networking & Admin** | AdGuard Home, Cloudflared, Home Assistant Google Drive Backup, Nginx Proxy Manager, Samba share, Studio Code Server | — |

### Active Home Assistant Integrations (51)

| Domain | Integrations & Scale |
| :--- | :--- |
| **AI, LLM & Agentic Tooling** | HA-MCP — Manage Home Assistant (1 device), LLM Vision (1 entity), Model Context Protocol Client (1 entry), Ollama (2 services), Wyoming Protocol (1 entity) |
| **Cameras, NVR & Security** | Alarmo (1 device), Frigate (6 devices), Reolink (10 devices) |
| **Protocols & Hardware Bridges** | Bluetooth (5 devices), ESPHome (8 devices), HomeKit Device (10 devices), MQTT (41 devices), Thread (1 entry), Z-Wave (22 devices) |
| **Climate, Energy & Utilities** | EcoFlow-Cloud (1 device), Flume (2 devices), Forecast.Solar (1 service), Midea AC LAN (1 device), National Grid US (4 devices) |
| **Lighting & Smart Devices** | Govee Bluetooth (2 devices), Govee LED BLE (2 devices), Tuya (10 devices), Tuya Local (4 devices), WiZ (4 devices) |
| **Media & Casting** | Alexa Media Player (3 devices), Denon AVR Network Receivers (1 device), Denon HEOS (1 device), Google Cast (1 device), LG webOS TV (1 device), Music Assistant (13 devices), MusicCast (2 devices), Samsung Smart TV (2 devices), Sonos (2 devices), Xbox (2 devices) |
| **System, Network & Other** | AdGuard Home (1 service), Android Debug Bridge (2 devices), Android TV Remote (1 device), Backup (1 service), Bambuddy (3 devices), Certificate Expiry (1 service), Command line (2 entities), EufyLife (1 device), GasBuddy (3 devices), Google Calendar (7 entities), Mail and Packages (1 device), Meteorologisk institutt / Met.no (1 service), Mobile App (5 devices), National Weather Service / NWS (1 service), Shopping list (1 entity), Spook (3 devices), Sun (1 service), Tesla Custom Integration (2 devices), Watchman (1 service) |

*(Note: 10 additional integrations are intentionally disabled in Home Assistant.)*

---

## 2. ASUS ROG Strix G15CE (RTX 3080) — Docker Desktop

| Stack / Group | Container Name | Image | Host Port | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Ollama (Host/Service)** | — | Local Ollama runtime | `11434` | Serves `qwen3:8b`, `qwen3-vl:8b-instruct`, `qwen3.5:4b`, and `minicpm-v` |
| **open-webui** | `open-webui` | `open-webui/open-webui:cuda` | `3001:8080` | CUDA-accelerated chat & RAG interface |
| **immich-ml** | `immich_machine_learning` | `immich-app/immich-machine-learning:release` | `3003:3003` | Remote ML inference for NUC-hosted Immich |
| **paperless-ml** | `docling-server` | `docling-project/docling-serve-cu128:v1.21.0` | `5001:5001` | CUDA layout-aware document parsing |
| **paperless-ml** | `paperless-docling` | `icereed/paperless-gpt:latest` | `3002:8080` | LLM/VLM document tagging and OCR enrichment |
| **paperless-ml** | `ocr-scorer` | `python:3.12-slim` | — | OCR quality evaluation worker |
| **mcp-grocy** | `mcp-grocy` | `mcp-grocy-mcp-grocy` | `8792:8080` | MCP server exposing Grocy inventory to LLMs |
| **obico-ml** | `api` | `thespaghettidetective/ml_api:latest` | `5002:3333` | 3D print failure detection ML API |
| **actual-ai** | `actual-ai` | `sakowicz/actual-ai:latest` | — | AI transaction categorization for Actual Budget |
| **esp-home** | `esphome` | `esphome/esphome:latest` | `6052:6052` | Dedicated ESPHome compiler/dashboard container |

---

## 3. Synology DS718+ NAS & Edge Hardware

* **Synology DS718+ NAS:** Network storage for media, backups, and long-term archives (legacy Docker Paperless-NGX decommissioned).
* **Networking:** ASUS RT-AX86U Pro + RT-AX86U wired AiMesh backhaul, Nginx Proxy Manager, Cloudflared.
* **Physical Bridges & Peripherals:** USB Zigbee Dongle (Zigbee2MQTT), USB Z-Wave Dongle (Z-Wave JS), Reolink cameras & doorbell (RTSP).
