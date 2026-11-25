# System Architecture & Node Responsibilities

## 1. Hardware Tiering

| Node | Hardware Profile | Role | Primary Workloads |
| :--- | :--- | :--- | :--- |
| **Control Plane** | Intel NUC7i5 (HAOS) | 24/7 State Engine & IoT Hub | Home Assistant Core, Mosquitto MQTT, Zigbee2MQTT, Z-Wave JS UI, Frigate NVR, Immich App, Grocy, Nginx Proxy Manager, Cloudflared |
| **AI & ML Compute** | ASUS ROG Strix G15CE (i7-11700KF, RTX 3080 10GB) | GPU Inference & Document/Agentic Compute | Ollama (`qwen3:8b`, `qwen3-vl:8b-instruct`, `qwen3-embedding:0.6b`), Open WebUI, Immich ML (`buffalo_l`, `ViT-B-32__openai`), Docling, Paperless-GPT, Obico ML (`YOLOv2 (Darknet)`), Custom MCP Servers |
| **Storage Tier** | Synology DS718+ NAS | Bulk Storage & Backups | Media libraries, camera archives, and long-term system backups |
| **Network Edge** | ASUS RT-AX86U Pro + RT-AX86U (Wired AiMesh) | Routing, DNS & Ingress | Wired Ethernet backhaul, AdGuard Home DNS filtering, Nginx Proxy Manager, Cloudflare Tunnel |

---

## 2. Key AI Data Flows

### A. Event-Driven Vision Pipeline (Frigate + LLM Vision / Ollama)
1. Reolink cameras and doorbell stream RTSP feeds to **Frigate NVR** on the NUC7i5.
2. Object/person detection triggers an **MQTT event** and Home Assistant automation.
3. Home Assistant forwards snapshot frames via **LLM Vision** / **Ollama API** to `qwen3-vl:8b-instruct` or `minicpm-v` on the RTX 3080 PC.
4. Structured JSON descriptions are returned to Home Assistant for actionable notifications or TTS announcements via **Piper** / **Sonos**.

### B. Agentic Home & Inventory Orchestration (MCP)
1. **HA-MCP** and **Model Context Protocol Client** connect Home Assistant entities and **Grocy** household data (`mcp-grocy` on port `8792`) to local LLMs (`qwen3.5:4b` / `qwen3:8b`).
2. Custom MCP servers extend local agent workflows to **OpenSCAD** and **Autodesk Fusion** design tasks.

### C. Photo & Document Intelligence
* **Immich:** The Immich application runs on the NUC7i5 while offloading facial recognition, CLIP embeddings, and object detection over LAN to `immich_machine_learning` (`3003:3003`) on the RTX 3080 PC.
* **Document Parsing (`paperless-ml` stack):** `docling-serve-cu128` (`5001:5001`), `paperless-gpt` (`3002:8080`), and `ocr-scorer` process structured layouts locally using CUDA acceleration.
