## 🎙️ **Silero**

**Type:** Speech-to-Text (STT) / Voice Activity Detection (VAD)
**Runs:** Locally (ONNX model) — fully offline
**Provider:** Open-source project by Silero.ai

### ✅ What it does

* Detects when someone is speaking (**VAD**).
* Converts speech audio into text (**STT**) entirely on your machine (no external API).
* Uses **ONNX Runtime** under the hood (fast inference on CPU).

### 🧩 Pros

* 100 % **self-hosted**, no API keys or internet needed.
* Low latency, runs on edge devices and servers.
* Ideal for privacy-sensitive or offline setups.

### ⚠️ Cons

* Accuracy lower than large cloud models (e.g., Deepgram, OpenAI Whisper).
* Limited language support (mostly English, Russian).
* CPU-bound; no GPU acceleration.

**In LiveKit Agents:**
Used by default when you choose `silero` extra, providing local VAD/STT for simple voice agents.

---

## 🧠 **Deepgram**

**Type:** Cloud Speech-to-Text (STT)
**Runs:** On Deepgram’s servers (API) — needs API key

### ✅ What it does

* Transcribes speech to text in real time via a **WebSocket** or **REST API**.
* Supports many languages, speaker diarization, word-level timestamps, etc.

### 🧩 Pros

* High accuracy, especially for English conversational speech.
* Very low latency (optimized streaming API).
* Easy integration with LiveKit (`from livekit.plugins import deepgram`).

### ⚠️ Cons

* Cloud dependency (needs internet).
* Usage-based pricing (per minute).

**In LiveKit Agents:**
Use it by setting `DEEPGRAM_API_KEY` and calling `deepgram.STT()`.

---

## 🔊 **Cartesia**

**Type:** Text-to-Speech (TTS)
**Runs:** Cloud service — needs API key
**Provider:** [Cartesia AI](https://cartesia.ai)

### ✅ What it does

* Converts text to **natural-sounding speech** in real time.
* Can generate different voices, accents, and emotions.
* Supports streaming TTS output for low latency conversational agents.

### 🧩 Pros

* Very realistic, expressive voices.
* Easy real-time streaming integration.
* Used as a drop-in TTS engine for LiveKit’s voice agents.

### ⚠️ Cons

* Cloud-only (no offline mode).
* Paid service.

**In LiveKit Agents:**
Use by setting `CARTESIA_API_KEY` and initializing `cartesia.TTS()`.

---

## 🧩 Typical setup in LiveKit Agent

Example hybrid configuration:

```python
from livekit.plugins import silero, deepgram, cartesia

stt = deepgram.STT()           # Cloud speech recognition
vad = silero.VAD()             # Local voice activity detector
tts = cartesia.TTS()           # Cloud text-to-speech

# Then plug these into your AgentSession
```

This combination gives:

* **Silero VAD** → detects when someone starts/stops speaking (offline, fast)
* **Deepgram STT** → converts their voice to text (accurate, cloud)
* **Cartesia TTS** → speaks back your agent’s replies (realistic voice)

