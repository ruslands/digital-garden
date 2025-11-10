# VPN: Obfuscation and AmneziaWG

## Table of Contents
1. [Why Obfuscation is Problematic](#why-obfuscation-is-problematic)
2. [DPI and Traffic Classification](#dpi-and-traffic-classification)
3. [AmneziaWG vs AmneziaVPN](#amneziawg-vs-amneziavpn)
4. [Detailed Comparison](#detailed-comparison)
5. [Which Should You Use?](#which-should-you-use)
6. [Compatibility with Amnezia App](#compatibility-with-amnezia-app)
7. [Summary](#summary)

---

## Why Obfuscation is Problematic

You modified the WireGuard protocol to obfuscate it. All changes you made relate only to obfuscation. However, this approach is incorrect in the modern world, and here's why.

---

### DPI and Traffic Classification

All modern **DPI (Deep Packet Inspection)** systems (including those currently operating in Belarus/Russia and China) try to understand what type of traffic they are dealing with.

**Traffic types for example:**

1. Browser connected to a website
2. Mobile application connected to a server
3. VPN client connected to a server
4. Unknown application connected to a server

---

### Traffic Policies

For each traffic type, a **different bandwidth policy** applies. If you obfuscate, you always fall into **category 4**. And this is, in fact, the worst bandwidth policy.

---

### Real-World Consequences

#### In China (Great Firewall)

If we're talking about China and the Great Chinese Firewall, your Amnezia will work there approximately **1 hour at high speed**, then several hours at low speed. After that, it will not be able to access the internet at all because it will be completely blocked.

**Why?** Because the Chinese firewall tried but could not understand what traffic it was dealing with. After that, it simply built a fingerprint of your modified protocol and blocked it as unknown.

**During political events:** If you try to use your Amnezia in China during any political events, you will not be able to connect to anything at all. Because on these days, the passage of unknown traffic by the Chinese firewall is prohibited in principle.

---

#### In Belarus/Russia

A similar situation is happening today with various firewalls in Belarus/Russia.

---

### The Right Approach: Mimicry Instead of Obfuscation

To fall into traffic policy 1-2, you need to **mimic a browser and/or mobile application**. And for mimicking, the vanilla WireGuard protocol is good enough and does not require any changes.

**What NOT to do:**
- ❌ Don't add garbage
- ❌ Don't rename headers
- ❌ Don't add random prefixes and suffixes

**This is simply not needed.**

The vanilla WireGuard protocol is sufficient for mimicking legitimate traffic without obfuscation.

---

## AmneziaWG vs AmneziaVPN

| Feature | `amneziawg` | `amnezia-vpn` (AmneziaVPN) |
|--------|-------------|-----------------------------|
| **What it is** | Obfuscated WireGuard binary + tools | Full-featured VPN server suite (GUI + multiple protocols) |
| **Protocols** | Only obfuscated WireGuard (`awg`) | ✅ WireGuard (obfs), OpenVPN, Shadowsocks, Cloak, XRay (VLESS/VMess), etc. |
| **Interface** | CLI only (`awg`, `awg-quick`) | ✅ Web UI + CLI + Docker |
| **Config Format** | `.conf` (INI-style with `Jc`, `H1`, etc.) | `.vpn` (container), JSON, or UI-generated |
| **Use Case** | Lightweight, single-protocol server | Full server with client management, multi-protocol, auto-config |
| **Installation** | `apt install amneziawg` (PPA) | Docker or full install from https://github.com/amnezia-vpn/amnezia-server |
| **Client App Support** | ✅ Yes (import `.conf`) | ✅ Yes (import `.vpn` or scan QR) |
| **Obfuscation** | ✅ Built-in (`Jc`, `S1`, `H1`, etc.) | ✅ Yes — for supported protocols |
| **Best For** | Simple, fast, CLI-only obfuscated WG | Enterprise, multi-user, GUI, multiple protocols |

---

## Detailed Comparison

---

### 1. **`amneziawg` — Obfuscated WireGuard Only**

- 📦 **Package**: `amneziawg` (from `ppa:amnezia/ppa`)
- 🧰 **Tools**:
  - `awg` — WireGuard binary with obfuscation
  - `awg-quick` — script to bring up tunnels (like `wg-quick`)
- 📄 **Config**: Uses `.conf` files with extra obfuscation fields:
  ```ini
  Jc = 4
  Jmin = 10
  H1 = 123456789
  ...
  ```
- ⚙️ **Management**: Manual — you edit configs, run `awg-quick`, manage keys yourself.
- 🎯 **Use When**: You want a **lightweight, single-client or few-clients** obfuscated WireGuard server — no GUI, no Docker, no overhead.

✅ **Perfect for your GCP startup script** — simple, fast, compatible with Amnezia App.

---

### 2. **`amnezia-vpn` / `amnezia-server` — Full VPN Suite**

- 📦 **Package**: Docker image or install script from https://github.com/amnezia-vpn/amnezia-server
- 🖥️ **Interface**: Web UI (port 80/443) + CLI + API
- 🧩 **Protocols Supported**:
  - Obfuscated WireGuard (`awg`)
  - OpenVPN (UDP/TCP)
  - Shadowsocks (AEAD)
  - Cloak (over TLS)
  - XRay (VLESS, VMess, Reality)
  - And more
- 📄 **Config**: Generates `.vpn` files (which may contain embedded JSON or multiple configs)
- 👥 **Features**:
  - Multi-user support
  - Auto key generation
  - QR code export
  - Client statistics
  - Protocol switching per client
- ⚙️ **Management**: Through UI or API — no manual config editing needed.
- 🐳 **Deployment**: Usually via Docker — heavier, but feature-rich.

🎯 **Use When**: You need to serve **many users**, support **multiple protocols**, or want a **GUI/dashboard**.

---

## Analogy

Think of it like this:

> `amneziawg` = `nginx` — lightweight, single-purpose, CLI-managed  
> `amnezia-vpn` = `Plesk` or `cPanel` — full control panel with multiple services, GUI, users, etc.

---

## Which Should You Use?

| Scenario | Recommendation |
|----------|----------------|
| You want a **simple, fast, scriptable** obfuscated WG server on GCP | ✅ `amneziawg` |
| You're building for **multiple users**, need **GUI**, or want **OpenVPN/Shadowsocks** | ✅ `amnezia-server` (Docker) |
| You want to **avoid Docker** and keep it minimal | ✅ `amneziawg` |
| You want to **generate configs automatically** with QR codes | ✅ `amnezia-server` |
| You're scripting in Terraform/GCP startup | ✅ `amneziawg` (your current approach is perfect) |

---

## Compatibility with Amnezia App

- ✅ Both work with **Amnezia Desktop/Mobile App**.
- `amneziawg` → export `.conf` → import as **"WireGuard (.conf)"**
- `amnezia-server` → export `.vpn` → import as **"Amnezia config (.vpn)"**

---

## Summary

> 🔹 Use **`amneziawg`** if you want a **lightweight, obfuscated WireGuard server** you can fully script — perfect for your GCP use case.

> 🔹 Use **`amnezia-server`** if you want a **full-featured VPN panel** with GUI, multi-protocol, and multi-user support — overkill for a single-server script.

---

## Pro Tip

You can even **combine both**:

- Use `amneziawg` for your main tunnel
- Later install `amnezia-server` in Docker on a different port to offer more protocols

But for now — **you're on the right track with `amneziawg`**.

---

### Additional Features You Might Want

Let me know if you want to:
- Add IPv6
- Auto-generate configs for 10 clients
- Export QR codes
- Monitor bandwidth

Happy tunneling! 🌐🔐
