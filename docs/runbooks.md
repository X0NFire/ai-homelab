# Operational Runbooks & Engineering Guardrails

## 1. VRAM & Memory Contention Management (RTX 3080 10GB)

### Risk
Concurrent execution of `docling-serve-cu128`, `open-webui:cuda`, `immich_machine_learning`, `obico-ml`, and large Ollama vision models (`qwen3-vl:8b-instruct`) can exhaust the 10 GB VRAM buffer or Docker Desktop's ~15.2 GB memory allocation.

### Mitigations
* **Model Keep-Alive:** Configure `OLLAMA_KEEP_ALIVE=5m` so vision models unload promptly after Frigate snapshot bursts, freeing VRAM for Docling or Immich batch jobs.
* **Task Scheduling:** Schedule heavy Immich ML re-indexing and bulk Docling OCR jobs during overnight windows when interactive Open WebUI usage is idle.
* **Graceful Fallback:** Home Assistant automations calling `LLM Vision` or `Ollama` must include a `continue_on_error: true` block so standard Frigate notifications still fire if the RTX PC is asleep, rebooting, or saturated.

---

## 2. Single-Source-of-Truth Hygiene (NUC vs. PC)

Several tools exist on both the NUC7i5 (as Home Assistant apps) and the RTX 3080 PC (as Docker containers):
* **ESPHome:** `ESPHome Device Builder` (NUC) vs. `esphome` on port `6052` (PC). Designate one instance for firmware compilation to avoid OTA state drift.
* **Grocy & MCP:** `Grocy` runs on the NUC while `mcp-grocy` (`8792:8080`) runs on the PC and connects over LAN API. Verify API keys and base URLs whenever NUC IP/port mappings change.
* **Immich:** `Immich` server runs on the NUC while `immich_machine_learning` (`3003:3003`) runs on the PC. Under Immich Administration -> Machine Learning Settings, point the URL explicitly to `http://<GPU_PC_HOST>:3003`.

---

## 3. Pre-Commit Security Checklist

Before pushing updates to this repository:
1. Verify `secrets.yaml` and `.env` are untracked (`git status --ignored`).
2. Confirm no internal hostnames, public Cloudflare tunnel domains, or DSC/Alarmo PIN codes appear in YAML comments or Markdown docs.
3. Run `gitleaks detect --source . -v` locally or rely on the GitHub Actions CI check.
