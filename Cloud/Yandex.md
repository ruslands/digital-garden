# Basic Concept: VPC, Subnet, and Security Group

### **VPC (Virtual Private Cloud)**

A **VPC** is a private, isolated network inside your cloud account.
Think of it as your **own private LAN**, where you can connect multiple VMs (servers) securely.

✅ You usually need **only one VPC** for related services — e.g. your FreePBX and LiveKit should be in **the same VPC** so they can talk directly via internal IP addresses (no public Internet traffic, lower latency, higher security).

---

### **Subnet**

A subnet divides the VPC into smaller blocks (like rooms in a building).
Usually, you’ll have one subnet per zone (e.g., `10.10.0.0/24` for `ru-central1-a`).

Both **FreePBX** and **LiveKit** can share the same subnet (or at least the same VPC), so:

* They can **ping** each other via internal IPs (e.g., 10.10.0.10 ↔ 10.10.0.20).
* You don’t need to expose FreePBX SIP/RTP ports to the public Internet — only LiveKit can access them internally.

---

### **Security Group**

A **security group** in Yandex Cloud (like a virtual firewall) defines:

* Which ports are **allowed** in/out,
* From which IPs (or other groups) traffic is allowed.

Think of it as **rules for each VM**:

> “This VM accepts only these types of traffic from these sources.”

Each VM can have one or more security groups attached.

---

## 2. Recommended Setup for FreePBX ↔ LiveKit Communication

### ✅ **Option A: Same VPC, same subnet**

Simplest and most secure setup.

```
VPC: net-main
 ├── Subnet: 10.10.0.0/24
 │    ├── VM1: FreePBX (10.10.0.10)
 │    └── VM2: LiveKit (10.10.0.20)
```

* Both communicate over **private IPs** (10.10.0.x).
* Security groups allow only **internal SIP/RTP** between them.
* Public access restricted (only LiveKit public-facing HTTP/WebRTC ports, FreePBX admin port from your IP).

**Pros:**

* Fast, secure, no NAT delays.
* Simpler DNS/routing.

**Cons:**

* Both VMs see each other completely inside that network (you can control specifics via SG rules though).

---

### ❌ **Option B: Separate VPCs**

Not recommended unless you *need* strong isolation between environments.

* Different VPCs are completely isolated.
* You’d have to use **VPC Peering** or **VPN** to connect them.
* Adds complexity, latency, and extra configuration.

---

## 3. Example: Security Group Rules

### **FreePBX Security Group**

```hcl
resource "yandex_vpc_security_group" "freepbx_sg" {
  network_id = yandex_vpc_network.main.id

  # Allow SIP/RTP from LiveKit internal IP range
  ingress {
    description    = "SIP from LiveKit"
    protocol       = "UDP"
    port           = 5060
    v4_cidr_blocks = ["10.10.0.0/24"]
  }

  ingress {
    description    = "RTP media from LiveKit"
    protocol       = "UDP"
    port_range_min = 10000
    port_range_max = 20000
    v4_cidr_blocks = ["10.10.0.0/24"]
  }

  # Allow HTTP/HTTPS from your admin IP
  ingress {
    description    = "FreePBX GUI"
    protocol       = "TCP"
    port           = 80
    v4_cidr_blocks = ["YOUR.IP.ADDR/32"]
  }
  ingress {
    protocol       = "TCP"
    port           = 443
    v4_cidr_blocks = ["YOUR.IP.ADDR/32"]
  }

  # Outgoing (to Internet for updates, or LiveKit)
  egress {
    protocol       = "ANY"
    v4_cidr_blocks = ["0.0.0.0/0"]
  }
}
```

### **LiveKit Security Group**

```hcl
resource "yandex_vpc_security_group" "livekit_sg" {
  network_id = yandex_vpc_network.main.id

  # Allow internal SIP/RTP to FreePBX
  egress {
    description    = "SIP to FreePBX"
    protocol       = "UDP"
    port           = 5060
    v4_cidr_blocks = ["10.10.0.0/24"]
  }

  egress {
    description    = "RTP to FreePBX"
    protocol       = "UDP"
    port_range_min = 10000
    port_range_max = 20000
    v4_cidr_blocks = ["10.10.0.0/24"]
  }

  # WebRTC/HTTP(S) access from Internet
  ingress {
    description    = "WebRTC / API"
    protocol       = "TCP"
    port           = 443
    v4_cidr_blocks = ["0.0.0.0/0"]
  }

  # TURN/STUN ports (if needed)
  ingress {
    description    = "TURN/STUN UDP"
    protocol       = "UDP"
    port           = 3478
    v4_cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    protocol       = "ANY"
    v4_cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

## 4. How They Communicate

* **FreePBX (Asterisk)** → handles SIP signaling + RTP media.
* **LiveKit** → handles WebRTC media (can bridge or route calls to FreePBX extensions/trunks).

They talk using **SIP over UDP/TCP**, and **RTP over UDP**.

Example:

```
LiveKit SIP gateway (10.10.0.20:5060)
  ↕
FreePBX (10.10.0.10:5060)
```

Then FreePBX sends/receives RTP media on ports 10000–20000.

---

## ✅ TL;DR Summary

| Layer               | Recommendation                                            |
| ------------------- | --------------------------------------------------------- |
| **VPC**             | Use **one VPC** for both VMs                              |
| **Subnet**          | Place both VMs in same subnet (e.g. 10.10.0.0/24)         |
| **Security Groups** | Use separate SGs but allow internal SIP/RTP between them  |
| **Communication**   | Use **private IPs** for SIP/RTP between FreePBX ↔ LiveKit |
| **Public Access**   | Restrict to only LiveKit web + your admin IP for FreePBX  |

