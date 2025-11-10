
## **PBX** = **Private Branch Exchange**

### 🏢 In plain terms

> A **PBX** connects your internal phones (extensions) to each other
> and to the outside world (via VoIP providers or analog phone lines).

Think of it as your **own mini telephone exchange** — like a mini version of a telecom company’s switching center, but for your office, call center, or app backend.

---

### 📞 What a PBX Does

| Function                 | Description                                                                                  |
| ------------------------ | -------------------------------------------------------------------------------------------- |
| **Call Routing**         | Routes incoming calls to the right extension, department, or IVR menu (“Press 1 for sales”). |
| **Extensions**           | Gives every user or device a unique internal number (e.g. 101, 102).                         |
| **Voicemail**            | Stores voice messages for users.                                                             |
| **IVR / Auto-attendant** | Plays voice menus — “Welcome to our company, please choose an option.”                       |
| **Call Recording**       | Records calls for quality or compliance.                                                     |
| **Conferencing**         | Allows multiple people to join the same call.                                                |
| **SIP Trunks**           | Connects your PBX to the Internet (VoIP provider or SIP carrier).                            |
| **Queues & Call Center** | Manages hold, queues, and routing to available agents.                                       |

---

### 🧠 PBX vs IP-PBX vs Cloud PBX

| Type             | Description                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| **PBX (legacy)** | Hardware box connected to analog phone lines (PSTN).                                                         |
| **IP-PBX**       | Software-based PBX that uses **VoIP/SIP** instead of phone cables. **FreePBX** and **Asterisk** belong here. |
| **Cloud PBX**    | Hosted by a provider — same logic, but managed online (e.g. Twilio, 3CX Cloud).                              |

So when you install **FreePBX**, you’re building your **own IP-PBX** system that you fully control.

---

### ⚙️ Components Inside a PBX

```
            ┌─────────────────────────┐
            │        PBX Server        │
            │ (FreePBX / Asterisk)     │
            ├─────────────────────────┤
            │ SIP / VoIP signaling     │
            │ Call routing rules       │
            │ IVR menus, Voicemail     │
            │ Call queues, CDR logs    │
            └─────────────────────────┘
                     │
     ┌───────────────┼────────────────┐
     │                               │
 Internal phones (SIP clients)   External SIP trunk
 (e.g. softphones, LiveKit)      (VoIP provider / PSTN)
```

---

### 🧬 Example in Your Setup

In your case:

* **FreePBX** = your PBX server (handles SIP calls and routing).
* **LiveKit** = your WebRTC media server that connects web users.
* You can connect LiveKit to FreePBX as a **SIP extension or trunk**, so FreePBX routes calls, voicemail, and logic — and LiveKit just transports audio/video.

---

#### Simple flow

```
User in browser (WebRTC)
      ↓
   LiveKit
      ↓ (SIP over UDP/TCP)
   FreePBX (Asterisk)
      ↓
   Phone agent / trunk / IVR
```

---

### ✅ TL;DR Summary

| Term         | Meaning                                                      |
| ------------ | ------------------------------------------------------------ |
| **PBX**      | Private Branch Exchange — a local telephone exchange system. |
| **IP-PBX**   | PBX that uses VoIP/SIP instead of old analog lines.          |
| **FreePBX**  | Open-source web-managed PBX system built on Asterisk.        |
| **Asterisk** | The underlying telephony engine doing actual call handling.  |

## **Dialers**

1. VICIdial
 • 🔗 vicidial.org (https://www.vicidial.org/)
 • Лицензия: GPLv2
 • Один из старейших и самых мощных open-source dialer’ов.
 • Включает всё: кампании, state-машину дозвона, AMD (определение автоответчика), расписания, CRM-интерфейс, отчёты.
 • Работает поверх Asterisk, уже включает web-интерфейс (PHP + MySQL).
 • Огромное комьюнити и море туториалов.
 • 💡 Идеален, если нужен предсказательный (predictive) dialer с полной автоматизацией.

📦 Есть готовый дистрибутив GOautodial — тот же VICIdial, но в виде “всё-в-одном” ISO-образа (Asterisk + MySQL + web GUI + dialer).

⸻

2. GOautodial
 • 🔗 goautodial.org (https://goautodial.org/)
 • Лицензия: GPLv2
 • Это дистрибутив VICIdial с более удобным интерфейсом.
 • Можно поставить как ISO, запустить VM, и сразу получить рабочий outbound dialer с веб-панелью.
 • Также умеет API-интеграции и webhooks для CRM или LiveKit.

🧠 Хорош для быстрого пилота или proof-of-concept.

⸻

3. Issabel PBX (бывший Elastix)
 • 🔗 issabel.org (https://issabel.org/)
 • Лицензия: GPL
 • Это не совсем dialer, а модульный PBX с GUI для Asterisk.
 • Можно докрутить модуль “Issabel Call Center” — он open-source и даёт базовую outbound-кампанию с ретраями и статусов вызовов.
 • Упрощённая альтернатива VICIdial, если тебе не нужны сложные фичи вроде predictive dialing.

⸻

🟨 Частично открытые / Freemium

4. WombatDialer
 • 🔗 loway.ch/wombatdialer (https://www.loway.ch/wombatdialer)
 • Компоненты Asterisk — open source,
но сам WombatDialer — proprietary, есть бесплатная trial-версия.
 • Прекрасный API, отличная масштабируемость, но требует лицензию для продакшена.
 • 💡 Подходит, если нужна минимальная задержка и API-first интеграция (можно просто пушить задачи на дозвон из кода).

⸻

5. FreePBX (с модулями outbound)
 • 🔗 freepbx.org (https://www.freepbx.org/)
 • Сам PBX — open source, но многие коммерческие модули (outbound campaigns, call-center, reporting) — платные.
 Excellent — this is the perfect next step after learning what a PBX is.

Let’s go through it clearly 👇

---

## **Asterisk**

> **Asterisk** is an **open-source telephony engine** — the actual **software brain** that makes phone calls work in systems like FreePBX.

It’s developed by **Digium (now part of Sangoma)** and has been the foundation of thousands of VoIP systems for over 20 years.

---

### 🧠 In plain terms

Asterisk is like the **“Linux kernel”** of the phone world:

* It **handles SIP, RTP, audio codecs, call routing, voicemail, conferencing**, etc.
* But by itself, it’s **configured via text files and CLI** (no GUI).

Then projects like **FreePBX** sit **on top of Asterisk**, giving you a web interface to control it.

---

### What Asterisk Actually Does

| Area                       | Function                                                          |
| -------------------------- | ----------------------------------------------------------------- |
| **SIP Signaling**          | Handles setup/teardown of VoIP calls (SIP INVITE, ACK, BYE).      |
| **RTP Media**              | Manages real-time audio streams between callers.                  |
| **Dialplan Logic**         | Defines how calls are routed (who rings, IVR menus, call groups). |
| **Voicemail**              | Records, stores, and plays voice messages.                        |
| **Conferencing**           | Mixes multiple audio streams for group calls.                     |
| **Recording & Monitoring** | Saves calls or monitors them in real time.                        |
| **Protocol Translation**   | Bridges SIP ↔ PJSIP ↔ IAX ↔ WebRTC, etc.                          |
| **Modules / Add-ons**      | Extend features: queueing, CDR logging, AMI/ARI APIs, etc.        |

---

### Asterisk + FreePBX Relationship

| Layer              | Component            | Description                                           |
| ------------------ | -------------------- | ----------------------------------------------------- |
| **Core Engine**    | 🧠 **Asterisk**      | Does all low-level telephony work (SIP, RTP, audio).  |
| **Web Management** | 🧩 **FreePBX**       | GUI + database to configure Asterisk easily.          |
| **User Interface** | 🧍‍♂️**You (admin)** | Create extensions, IVR, trunks, etc. through FreePBX. |

So when you make a change in FreePBX (like adding an extension), it actually writes config files for Asterisk under the hood.

---

### How It Works Internally

Example call process inside Asterisk:

```
1. User dials number 200
2. Asterisk checks its Dialplan rules (/etc/asterisk/extensions.conf)
3. Finds: “When 200 → ring SIP/200”
4. Sends SIP INVITE to phone 200
5. Phone answers → RTP media starts
6. Asterisk bridges the audio
```

---

### Developer Angle

Asterisk exposes multiple APIs:

* **AMI (Asterisk Manager Interface):** for automation / call control via TCP (like call origination, hangup, monitoring).
* **ARI (Asterisk REST Interface):** modern REST+WebSocket API for developers (used for building custom voice bots, callflows, etc.).

You can even write your own telephony apps that react to calls in real time — like a **custom IVR, call routing logic, or AI voice assistant integration**.

---

### In Your Setup (FreePBX + LiveKit)

* **Asterisk** (inside FreePBX) will handle SIP + RTP for internal/external calls.
* **LiveKit** (WebRTC side) connects via SIP or media gateway to Asterisk.
* Together, they form a bridge between **WebRTC clients** and **traditional telephony**.

```
Web Browser → LiveKit → (SIP/RTP) → Asterisk → Phone / IVR / Trunk
```

---

### TL;DR Summary

| Term          | Meaning                                                                      |
| ------------- | ---------------------------------------------------------------------------- |
| **Asterisk**  | Open-source telephony engine that powers VoIP calls, IVRs, and conferencing. |
| **Made by**   | Sangoma (originally Digium).                                                 |
| **FreePBX**   | A user-friendly GUI built on top of Asterisk.                                |
| **Main Role** | Handle SIP signaling, RTP media, call routing, and voice services.           |
## **SFU**

**SFU** stands for **Selective Forwarding Unit** — it’s one of the main architectures used in modern real-time communication systems like **LiveKit**, **Zoom**, **Google Meet**, **Discord**, and **WebRTC**-based apps.

---

### 🧠 Core idea

An **SFU** receives **media streams (audio/video)** from multiple participants and **selectively forwards** them to others **without re-encoding**.

It’s called *selective* because:

* it can decide **which streams to forward** (e.g. only active speakers),
* it can choose **which quality or bitrate** (for adaptive resolution),
* and it can avoid **unnecessary copies or transcoding**.

---

### ⚙️ How it works (simple example)

Imagine a 3-person call (A, B, C):

1. **A**, **B**, and **C** each send one audio/video stream **up** to the SFU.
2. The **SFU** receives all three streams.
3. The SFU **forwards A’s stream to B and C**, **B’s stream to A and C**, and so on.

Each participant still decodes remote participants locally — the SFU just handles efficient routing.

---

### 🆚 Compared to other architectures

| Architecture                        | Description                                       | Pros                                             | Cons                                               |
| ----------------------------------- | ------------------------------------------------- | ------------------------------------------------ | -------------------------------------------------- |
| **Mesh (P2P)**                      | Everyone sends media directly to everyone else    | Low latency, simple                              | Doesn’t scale (bandwidth grows exponentially)      |
| **MCU (Multipoint Control Unit)**   | Server mixes all streams into one combined stream | Easy for clients, single stream download         | Expensive CPU, latency, loss of individual control |
| **SFU (Selective Forwarding Unit)** | Server routes individual streams to clients       | Scales well, low latency, clients control layout | Client must decode multiple streams                |

---

### 💡 Why LiveKit uses SFU

LiveKit’s `livekit-server` is an **SFU**:

* handles WebRTC signaling and media routing,
* forwards tracks between participants efficiently,
* supports simulcast (multiple resolutions per camera),
* enables server-side recording and streaming (via Egress),
* and scales horizontally across nodes.

---

### 🧩 Related concepts

* **Simulcast:** client sends multiple versions (720p, 360p, etc.), SFU picks the right one per subscriber.
* **SVC (Scalable Video Coding):** one encoded stream with multiple layers (quality levels).
* **TURN/STUN:** used for NAT traversal; not part of SFU but helps media flow.
* **Ingress/Egress:** optional LiveKit services for external stream input/output.

