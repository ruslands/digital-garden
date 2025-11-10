https://github.com/livekit/agents


# LiveKit CLI

This package includes command line utilities that interacts with LiveKit. It allows you to:

- Bootstrap new applications from templates
- Create access tokens
- Access LiveKit APIs, create and delete rooms, etc.
- Join a room as a participant, inspecting in-room events
- Start and manage Egresses
- Perform load testing, efficiently simulating real-world load


Got it. Here’s how your `agent.py` actually “meets” your self-hosted LiveKit and what flows between them.

# What are the moving parts?

* **LiveKit Server (your VM):** a WebRTC SFU that hosts **Rooms**, **Participants**, and their **Tracks** (audio/video/data). It also exposes REST/Webhook APIs. ([LiveKit Docs][1])
* **Your client** (web/mobile/SIP): publishes the user’s mic audio into a room and plays whatever the agent says back. ([LiveKit Docs][1])
* **`agent.py` (LiveKit Agents runtime):** a bot participant that joins rooms, **subscribes to the user’s audio track**, runs **STT → LLM/tools → TTS**, and **publishes** its synthesized audio back to the room. The Agents framework also handles turn detection/VAD, data channels, tools, etc. ([LiveKit Docs][1])

# The interaction, step-by-step

```
[User Mic] ──(WebRTC audio up)──▶ [LiveKit Room on your VM]
                                       │
                                       │  (subscribe to user's audio track)
                                       ▼
                                  [agent.py Worker]
                               STT → LLM/Tools → TTS
                                       │
                                       │  (publish bot audio/data tracks)
                                       ▼
[User App] ◀─(WebRTC audio down)── [LiveKit Room on your VM]
```

1. **Client connects & publishes audio**
   Your frontend/mobile/SIP joins a **Room** with a token and publishes the microphone track. This is standard LiveKit SDK behavior. ([LiveKit Docs][1])

2. **`agent.py` joins as a participant**
   You run the **Agent Worker** (Python). It connects to your LiveKit server and is **dispatched** to rooms based on rules you set (e.g., “when a participant joins room X, start agent Y”). Under the hood, it’s just another participant that can subscribe/publish tracks and exchange data. ([LiveKit Docs][1])

3. **Media & data flow inside the agent**

   * **Subscribe** to the user’s audio track from the room
   * **STT** (plugin or LiveKit Inference) turns speech → text
   * **LLM** decides what to say / calls your tools
   * **TTS** generates bot speech
   * **Publish** a bot audio track back into the room so the client hears it in real time
     You can also emit **text streams / data** (captions, hints) over LiveKit’s data channels. ([LiveKit Docs][1])

4. **(Optional) Triggers & lifecycle**
   You can kick agents off via **dispatch rules** or through **webhooks** from your server (e.g., on `ParticipantJoined`). The **Worker lifecycle** describes how jobs are assigned and torn down cleanly. ([LiveKit Docs][1])

# What you wire up in code (`agent.py`)

At a high level (names will differ depending on your exact code):

* **Configure connection** to your server: `wss://<your-livekit-host>` + API key/secret (to mint join tokens or use worker credentials). ([LiveKit Docs][1])
* **Start a Worker** and **register** your agent (voice pipeline): define STT, LLM, TTS, and any **tools**. The pipeline nodes/hooks let you customize turn detection, barge-in, etc. ([LiveKit Docs][1])
* **Dispatch**: choose how the worker selects rooms (by room name, metadata, or events). ([LiveKit Docs][1])
* **Subscribe/Publish**: in your pipeline handler, subscribe to the user’s **audio track**, process, then **publish** a bot audio track back.

# Auth & rooms (where tokens fit)

* Your **backend** typically mints **access tokens** for the client (scoped to a room) using your LiveKit API key/secret.
* The **agent worker** either uses its own credentials/dispatch config or also joins with a token.
* Server APIs also cover room/participant management if you want to add/remove/mute participants programmatically. ([LiveKit Docs][1])

# Networking notes for self-hosting

* LiveKit needs proper **UDP/TCP ports** and **public IP** awareness (ICE/TURN). If users connect from the internet, ensure your firewall/ports are set per the self-hosting port guide; otherwise the agent or clients may fail to exchange media. ([LiveKit Docs][1])
* If you later record or bring in non-WebRTC streams, that’s **Egress/Ingress** tied to your LiveKit server and (optionally) Redis for coordination. ([LiveKit Docs][1])

# How to sanity-check your setup

* **Join the room** from your web client and confirm you see your own audio track.
* **Run the worker** (`agent.py`) and confirm logs show it **connecting** and **subscribing** to a track.
* Speak → verify you get **transcripts** (STT) and **bot audio** returns to the room.
* If you see “no audio” or one-way audio, double-check **room permissions**, **token scopes** (publish/subscribe), **server URL (wss://)**, and **firewall/TURN** config. ([LiveKit Docs][1])

---

