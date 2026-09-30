# NEXUS Edge AI

**Intelligent. Private. On-Device.**

NEXUS Edge AI is a privacy-first desktop AI assistant designed for Snapdragon-powered HP PCs. It uses a local-first architecture: sensitive document analysis and supported AI workloads are kept on the device whenever possible, while an optional cloud path can be used only when explicitly enabled.

## Why Snapdragon

NEXUS is designed around the AI capabilities of Snapdragon X-series Windows PCs. The project includes an optional **Qualcomm AI Hub / ONNX Runtime QNN execution path** for NPU-accelerated inference. Qualcomm AI Hub supports Snapdragon X-series devices and provides optimized models and deployment tooling for Windows. The current model configuration targets **Qwen3-4B**, an Apache-2.0 model available through Qualcomm AI Hub with Snapdragon X Elite, Snapdragon X Plus 8-Core and Snapdragon X2 Elite support.

- Qualcomm AI Hub: https://aihub.qualcomm.com/
- Qwen3-4B on AI Hub: https://aihub.qualcomm.com/models/qwen3_4b
- Qualcomm Windows on Snapdragon AI: https://www.qualcomm.com/developer/windows-on-snapdragon/windows-on-snapdragon-ai

> **Important:** NPU execution requires the appropriate Qualcomm/ONNX Runtime environment and model artifact on a compatible Snapdragon Windows PC. The repository never pretends that NPU acceleration is active when the required runtime is unavailable; the UI reports the actual execution mode.

## Core Features

- Local document ingestion for PDF, DOCX and TXT
- Contract/privacy-oriented information extraction
- Sensitive-data detection (email, phone, API-key-like strings and identifiers)
- Local-first AI routing
- Optional Qwen3-4B / Qualcomm AI Hub integration
- NPU status and execution-mode indicator
- Explainable analysis cards and risk flags
- Simple Streamlit interface suitable for a Windows prototype

## Architecture

```text
Document / Screen / Voice
          |
          v
   Ingestion + OCR/Parsing
          |
          v
   Privacy Guard / Redaction
          |
          v
     Smart AI Router
       /         \
      v           v
Local NPU/CPU    Optional Cloud
(QNN / AI Hub)   (explicit opt-in)
      |
      v
Insights / Q&A / Risk Flags / Summary
```

## Quick Start

### 1. Create an environment

Windows ARM64 users should use a native ARM64 Python installation where appropriate. Qualcomm's current AI Hub guidance recommends Python 3.11+ for Windows on ARM.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 2. Run the prototype

```powershell
streamlit run app/main.py
```

### 3. Optional Snapdragon NPU mode

Install the Qualcomm/ONNX Runtime stack appropriate for the target Snapdragon PC and provide a compatible QNN-enabled ONNX model. Set:

```powershell
$env:NEXUS_QNN_MODEL="models/qwen3-4b-qnn.onnx"
```

Then launch the app. NEXUS will attempt the QNN execution path and visibly report whether it is active.

For Qualcomm AI Hub Workbench, authenticate with a Qualcomm ID/API token and use the official tooling to compile/profile a target model for a Snapdragon device before placing the resulting deployment artifact in the project. See `docs/snapdragon.md`.

## Repository Structure

```text
app/       Streamlit UI and application logic
ai/        model routing and QNN integration
models/    model configuration and local artifacts (large binaries excluded)
docs/      technical and Snapdragon deployment notes
scripts/   validation/demo helpers
assets/    architecture diagrams and screenshots
```

## Responsible Claims

The project distinguishes between **prototype support** and **verified hardware execution**. A submission should only claim measured NPU latency or Snapdragon hardware acceleration after testing on the target HP Snapdragon PC and recording the result in `docs/benchmark.md`.

## License

MIT for the original project code. Third-party models and SDKs remain under their respective licenses and terms.
