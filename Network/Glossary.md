# Network Glossary: VPN Troubleshooting and Networking Terms

## Table of Contents
1. [VPN Troubleshooting](#vpn-troubleshooting)
2. [MTU (Maximum Transmission Unit)](#mtu-maximum-transmission-unit)
3. [NS (Name Server Record)](#ns-name-server-record)
4. [Network Interfaces](#network-interfaces)
5. [VPN Debug Guide](#vpn-debug-guide)
6. [WireGuard Interface and Peer Configuration](#wireguard-interface-and-peer-configuration)
7. [ISP (Internet Service Provider)](#isp-internet-service-provider)
8. [DNS (Domain Name System)](#dns-domain-name-system)
9. [NAT (Network Address Translation)](#nat-network-address-translation)
10. [GCP VPN Troubleshooting](#gcp-vpn-troubleshooting)

---

## VPN Troubleshooting

### Problem: WireGuard Connection Issues

**When you connect to another VPN first, THEN connect to your WireGuard tunnel → data flows.**  
**When you connect to WireGuard directly → no data flows.**

➡️ This strongly suggests that **your country's ISP or government firewall is BLOCKING direct WireGuard UDP traffic on port 51820**.

---

### Why This Happens

Many countries (e.g., Russia, China, Iran, UAE, etc.) actively **block or throttle**:

- ✅ Known VPN protocols (OpenVPN, WireGuard, IKEv2)
- ✅ Common VPN ports (51820, 1194, 500, 4500, etc.)
- ✅ UDP traffic to foreign IPs on those ports

→ When you connect to **another VPN first**, you "hide" your WireGuard traffic inside that encrypted tunnel → bypassing national firewall.

→ When you connect **directly**, your ISP sees `UDP 51820` to a foreign IP → blocks it.

---

### Evidence in Logs

Earlier, you saw this on server:

```
IP 185.174.128.252.59895 > 10.186.0.19.51820: UDP
```

→ Packets arrived — but from a **different source port** (`59895`) — likely because your ISP is **NATting or redirecting** your traffic.

→ Also, handshake succeeded — but no data flow — classic **DPI (Deep Packet Inspection)** or **UDP blocking** behavior.

---

### SOLUTION: Obfuscate WireGuard Traffic

You need to make WireGuard traffic look like **normal HTTPS (TCP 443)** traffic — which is rarely blocked.

---

### OPTION 1: Use **AmneziaWG in TCP Mode** (Recommended)

Since your server is named `amnezia-vpn-server`, you likely have access to **AmneziaWG** — a WireGuard fork that supports **TCP mode**.

#### ➤ On Server:

```bash
# Install AmneziaWG if not installed
curl -s https://raw.githubusercontent.com/amnezia-vpn/amnezia-wg/master/install.sh | sudo bash

# Generate TCP config on port 443
sudo amnezia-wg --tcp --port 443
```

→ This creates a WireGuard config that runs over **TCP 443** — looks like HTTPS.

#### ➤ Get Client Config:

```bash
sudo amnezia-wg --client
```

→ Use this config in WireGuard app.

#### ➤ Update GCP Firewall:

Allow TCP 443:

```hcl
resource "google_compute_firewall" "vpn_firewall_tcp" {
  name    = "allow-vpn-tcp"
  network = "default"

  allow {
    protocol = "tcp"
    ports    = ["443"]
  }

  source_ranges = ["0.0.0.0/0"]
  target_tags   = ["amnezia-vpn"]
}
```

→ Apply with `terraform apply`.

---

### OPTION 2: Use **udp2raw** (Wrap WireGuard in Fake TCP)

Install `udp2raw` on server and client to wrap UDP in TCP.

#### ➤ On Server:

```bash
sudo apt install -y udp2raw
sudo udp2raw_amd64 -s -l 0.0.0.0:443 -r 127.0.0.1:51820 --raw-mode faketcp -a -k "yourpassword"
```

#### ➤ On Mac (Client):

```bash
brew install udp2raw
udp2raw_mac -c -l 127.0.0.1:51820 -r 34.118.27.65:443 --raw-mode faketcp -k "yourpassword"
```

→ Then point WireGuard client to `127.0.0.1:51820`.

---

### OPTION 3: Use **Cloudflare Tunnel** or **HTTPS Proxy**

Run WireGuard behind a **WebSocket or HTTPS proxy** — but this adds latency.

---

### WHY THIS FIXES IT

- 🔒 **TCP 443** looks like normal HTTPS traffic → rarely blocked
- 🚫 **No UDP 51820** → bypasses national firewall
- 🌐 Works in restrictive networks (schools, offices, countries)

---

### Final Recommendation

✅ **Use AmneziaWG in TCP mode on port 443** — it's designed for this exact scenario.

It's:
- ✅ 100% WireGuard compatible
- ✅ Easy to set up
- ✅ Bypasses DPI and UDP blocking
- ✅ Works in Russia, China, Iran, etc.

---

### STEP-BY-STEP — DO THIS NOW

1. On Server:

```bash
curl -s https://raw.githubusercontent.com/amnezia-vpn/amnezia-wg/master/install.sh | sudo bash
sudo amnezia-wg --tcp --port 443
sudo amnezia-wg --client
```

→ Copy client config.

2. Update GCP Firewall (allow TCP 443)

3. On Mac — Import new config into WireGuard app

4. Connect → Test `ping 10.200.200.1` → should work!

---

## MTU = Maximum Transmission Unit

**MTU** = **Maximum Transmission Unit**

It defines **the largest packet size (in bytes)** that can be sent **over a network interface** **without fragmentation**.

* For example:

  * **Ethernet default MTU** → `1500 bytes`
  * **GCP default MTU** → `1460 bytes`
  * **WireGuard default MTU** → `1420 bytes` (because it subtracts header overhead)

If your VPN sends packets **larger than what the network allows**, they get **fragmented** or **dropped**.

When GCP drops fragmented packets, you'll see exactly your issue:
✅ **Initial handshake works** but ❌ **no data passes through**.

---

### Why MTU Problems Happen on GCP

GCP uses **VPC networks** that often have a **lower MTU** than your local device:

| Network           | MTU (bytes) |
| ----------------- | ----------- |
| Standard Ethernet | 1500        |
| GCP VPC (default) | **1460**    |
| WireGuard default | 1420        |
| Some ISPs / LTE   | 1380–1400   |

**Scenario on GCP:**

* Client sends `1500-byte` packets → WireGuard encrypts → packet size grows → exceeds GCP MTU → **packet dropped silently**.
* The handshake is small enough → **it works**.
* But real traffic (HTTPS, DNS, streaming) → gets **stuck**.

---

### How to Fix MTU Issues

You need to **set a lower MTU** manually for WireGuard on **both server and client**.

#### Step 1 — On the Server

Edit `/etc/wireguard/wg0.conf`:

```ini
[Interface]
PrivateKey = SERVER_PRIVATE_KEY
Address = 10.200.200.1/24
ListenPort = 51820
MTU = 1280  # 👈 IMPORTANT

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.200.200.2/32
```

Then restart WireGuard:

```bash
systemctl restart wg-quick@wg0
```

---

#### Step 2 — On the Client

Edit `client-wg0.conf`:

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.200.200.2/24
DNS = 1.1.1.1
MTU = 1280  # 👈 IMPORTANT

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_IP:51820
AllowedIPs = 0.0.0.0/0, ::/0
```

Restart WireGuard client afterward.

---

#### Step 3 — Pick the Right MTU Value

Try these values:

1. Start with **1420** → WireGuard default.
2. If still broken → try **1380**.
3. If still broken → go down to **1280**.

You can test MTU with:

```bash
ping -M do -s 1380 8.8.8.8
```

* If it fails, reduce `1380 → 1360 → 1280` until it succeeds.

---

### How MTU Relates to Your Problem

* Your **first WireGuard handshake** succeeds because those packets are **small**.
* After that, **larger packets** exceed GCP's MTU → GCP **silently drops** them.
* No packets = no handshake refresh = VPN **dies**.

---

### Quick Summary

| Problem      | Symptom                              | Fix                    |
| ------------ | ------------------------------------ | ---------------------- |
| MTU mismatch | Initial handshake OK, but no traffic | Lower MTU to 1280–1380 |
| GCP firewall | Traffic never reaches VPN            | Allow forwarding rules |
| Missing NAT  | Packets sent but not returning       | Add MASQUERADE rules   |

---

### My Recommendation

Since you're on **GCP**, I'd set:

```ini
MTU = 1280
```

on **both server and client** to avoid fragmentation completely.

---

## NS = NameServer Record

> **NS = Name Server**

An **NS record** is a type of DNS record that **specifies which DNS servers are authoritative** for a domain or subdomain.

In simple terms:  
> "Who is in charge of answering DNS questions for this domain?"

---

### Example: Who Controls `example.com`?

When you type `example.com` into your browser, your computer asks:  
> "Which DNS servers hold the *official* answers for `example.com`?"

The answer comes from **NS records**.

---

### How NS Records Work with DNS — Step by Step

Let's trace what happens when you visit `www.example.com`.

---

### Step 1: Your Computer Asks the Resolver

Your OS uses a **DNS resolver** (often provided by your ISP, or public ones like `8.8.8.8` or `1.1.1.1`).

You ask: "What's the IP of `www.example.com`?"

---

### Step 2: Resolver Starts at the Root

The resolver doesn't know — so it starts from the top:

1. Ask a **root DNS server**: "Who handles `.com`?"
   → Root replies: "Ask these **TLD (Top-Level Domain) servers** for `.com`."

2. Ask a **.com TLD server**: "Who handles `example.com`?"
   → TLD replies: "Ask these **authoritative name servers** for `example.com`."  
   👉 This is where **NS records** come in!

   Example NS records for `example.com`:
   ```
   example.com.    NS    ns1.exampledns.com.
   example.com.    NS    ns2.exampledns.com.
   ```

---

### Step 3: Resolver Asks the Authoritative Name Servers

Now the resolver queries `ns1.exampledns.com` (one of the authoritative servers):

> "What's the IP for `www.example.com`?"

The authoritative server replies:
```
www.example.com.    A    93.184.216.34
```

✅ Resolver caches the answer and returns it to your browser.

---

### NS Records Are Delegation Records

NS records **delegate authority**.

Think of it like this:

> The `.com` registry says:  
> "I don't know what `www.example.com` is — but ask `ns1.exampledns.com` — THEY are in charge."

This delegation model is what makes DNS **scalable and distributed**.

---

### Real-World Example: Check NS Records Yourself

Use `dig` to see NS records:

```bash
dig NS example.com
```

Output (simplified):

```text
;; ANSWER SECTION:
example.com.        172800  IN  NS  a.iana-servers.net.
example.com.        172800  IN  NS  b.iana-servers.net.
```

These are the **authoritative name servers** for `example.com`.

---

### NS Records vs Other DNS Records

| Record Type | Purpose                                   | Example                                  |
|-------------|-------------------------------------------|------------------------------------------|
| `NS`        | Who is authoritative for this domain?     | `example.com. NS ns1.hosting.com.`       |
| `A`         | IPv4 address for a hostname               | `www.example.com. A 93.184.216.34`       |
| `AAAA`      | IPv6 address                              | `www.example.com. AAAA 2606:2800:220:...`|
| `CNAME`     | Alias (points to another name)            | `blog.example.com. CNAME example.com.`   |
| `MX`        | Mail server for domain                    | `example.com. MX 10 mail.example.com.`   |

> ✅ NS records tell you **where to go** to get the real answers.  
> ❗ Other records (A, CNAME, MX) are the **answers themselves** — but only authoritative servers can give definitive ones.

---

### Where Are NS Records Set?

NS records for a domain are set in **two places**:

### Domain Registrar (e.g., GoDaddy, Namecheap, Cloudflare Registrar)

When you register `yourdomain.com`, you must specify **which name servers are authoritative**.

Example:
```
ns1.yourhost.com
ns2.yourhost.com
```

This is called **"delegating the domain"** to those name servers.

> ⚠️ This is stored in the **TLD registry** (e.g., `.com` registry), not in your DNS zone.

---

### DNS Hosting Provider (e.g., Cloudflare, AWS Route 53, your own BIND server)

Inside your DNS zone file, you also usually **publish NS records** for consistency and glue.

Example zone file snippet:

```text
@    IN    NS    ns1.yourhost.com.
@    IN    NS    ns2.yourhost.com.
```

> 💡 The `@` symbol means "this domain" — so `@ IN NS ...` = NS records for the root domain.

---

### Glue Records — When NS Servers Are "Inside" the Domain

What if your name server is `ns1.example.com` — but to find `ns1.example.com`, you need to resolve `example.com` first? 🤯

This is a **chicken-and-egg problem**.

✅ Solution: **Glue records**

At the registrar, when you set:

```
NS: ns1.example.com
```

You must also provide its **IP address**:

```
ns1.example.com → 192.0.2.1
```

This IP is stored at the TLD level as a **glue record** — so resolvers can find `ns1.example.com` without needing to resolve `example.com` first.

---

### Subdomain Delegation with NS Records

You can also delegate a **subdomain** to different name servers.

Example: Delegate `blog.example.com` to a separate DNS provider.

In your main zone (`example.com`), add:

```text
blog    IN    NS    ns1.wordpress.com.
blog    IN    NS    ns2.wordpress.com.
```

Now, any query for `anything.blog.example.com` will be referred to WordPress's DNS servers.

> 🔍 This is how big sites delegate parts (e.g., `shop.company.com` to Shopify, `status.company.com` to Statuspage).

---

### Why NS Records Matter

- ✅ **Scalability**: DNS is distributed — no single server knows everything.
- ✅ **Control**: Domain owners choose who manages their DNS.
- ✅ **Redundancy**: Multiple NS records = failover if one server is down.
- ✅ **Delegation**: Enables subdomains to be managed independently.

---

### Tools to Check NS Records

#### 1. `dig`
```bash
dig NS example.com +short
```

#### 2. `nslookup`
```bash
nslookup -type=NS example.com
```

#### 3. Online Tools
- [https://mxtoolbox.com/NSLookup.aspx](https://mxtoolbox.com/NSLookup.aspx)
- [https://dnschecker.org](https://dnschecker.org)

---

### Visual Summary: DNS Resolution Flow with NS Records

```
[Your Computer]
       ↓ asks "www.example.com?"
[DNS Resolver (8.8.8.8)]
       ↓ asks root: ".com?" → gets TLD servers
       ↓ asks .com TLD: "example.com?" → gets NS records
       ↓ asks ns1.exampledns.com (authoritative): "www.example.com?"
       ↓ gets A record: 93.184.216.34
       ↓ returns IP to you
[Browser connects to 93.184.216.34]
```

---

### Common Issues with NS Records

| Problem                          | Cause                                      | Fix                                      |
|----------------------------------|--------------------------------------------|------------------------------------------|
| Site not resolving               | NS records misconfigured or not propagated | Check registrar + DNS hosting match      |
| Partial resolution               | Some NS servers down or out of sync        | Ensure all listed NS servers are healthy |
| "Lame delegation"                | NS server doesn't respond authoritatively  | Fix DNS config on authoritative server   |
| Glue record missing              | NS server is under same domain, no IP given| Add glue record at registrar             |

---

### Quick Recap

- **NS records = "Who is the boss of this domain's DNS?"**
- They enable **delegation**, **scalability**, and **distributed control**.
- Set at **registrar** (for root domain) and optionally in **DNS zone**.
- Use `dig NS yourdomain.com` to check them.
- Critical for DNS to function — without them, resolvers don't know where to go!

---

## Network Interfaces

These — `utun1`, `utun2`, `en0`, `en1`, `lo` — are **network interface names** commonly seen on **macOS (and sometimes other Unix-like systems)**. They represent different types of network interfaces used by the system for communication.

---

### Breakdown of Each Interface

| Interface | Type / Meaning                                                                 | Typical Use Case                                                                 |
|-----------|---------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| `lo`      | **Loopback interface**                                                          | Used for internal communication within the machine (localhost → 127.0.0.1). Always present on all OSes. |
| `en0`     | **Ethernet/Wi-Fi interface** (first physical/virtual network adapter)           | Primary network interface — often Wi-Fi on MacBooks, or wired Ethernet on desktops. |
| `en1`     | **Second Ethernet/Wi-Fi interface** (if present)                                | Secondary network adapter — e.g., Thunderbolt Ethernet, USB-C Ethernet, or second Wi-Fi card. |
| `utun1`   | **Userspace Tunnel interface #1** (created dynamically by apps or services)     | Used by **VPNs** (e.g., OpenVPN, WireGuard, Cisco AnyConnect), **Tailscale**, **ZeroTier**, etc. |
| `utun2`   | **Userspace Tunnel interface #2**                                               | Another tunnel — often created when you have multiple concurrent VPNs or virtual networks. |

---

### Where You See These

Run this in Terminal to list interfaces:

```bash
ifconfig
# or
networksetup -listallhardwareports
```

Example output snippet:

```text
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
    inet 127.0.0.1 netmask 0xff000000

en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
    inet 192.168.1.100 netmask 0xffffff00 broadcast 192.168.1.255

utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
    inet 10.8.0.2 --> 10.8.0.2 netmask 0xffffffff
```

---

### Details

### `lo` (loopback)
- Always `127.0.0.1` (IPv4) and `::1` (IPv6).
- Used for local services: databases, web servers running locally, etc.

### `en0`, `en1` (Ethernet / Wi-Fi)
- macOS assigns `en0` as the primary network interface.
- On MacBooks, `en0` is usually **Wi-Fi**, `en1` might be **Thunderbolt Ethernet** or unused.
- You can check which is active:
  ```bash
  networksetup -listallhardwareports
  ```
  Output example:
  ```
  Hardware Port: Wi-Fi
  Device: en0
  Ethernet Address: aa:bb:cc:dd:ee:ff
  ```

### `utunX` (User-space Tunnel)
- Created dynamically by **VPN clients** or **overlay networks**.
- Not physical — virtual interfaces managed in userspace (hence "utun" = userspace tunnel).
- Each active VPN or tunnel service may create its own `utun` device (e.g., `utun1`, `utun2`, etc.).
- Often uses **point-to-point** IP addresses (e.g., `10.x.x.x`, `172.x.x.x`, `192.168.x.x`).

---

### Practical Examples

### 1. Check your current IP on Wi-Fi:
```bash
ipconfig getifaddr en0
```

### 2. See which interface is used to reach Google:
```bash
route get 8.8.8.8
# Look for "interface: en0" or "interface: utun1"
```

### 3. List all interfaces with IPs:
```bash
ifconfig | grep "inet " | grep -v 127.0.0.1
```

### 4. Monitor traffic on a tunnel (e.g., during VPN use):
```bash
sudo tcpdump -i utun1
```

---

### macOS vs Linux Interface Naming

| macOS        | Linux Equivalent     | Notes                                     |
|--------------|----------------------|-------------------------------------------|
| `lo0`        | `lo`                 | Loopback                                  |
| `en0`, `en1` | `eth0`, `wlan0`      | Physical/Wi-Fi interfaces                 |
| `utun1`      | `tun0`, `wg0`, `ppp0`| Virtual tunnels (depends on tech used)    |

> 💡 Linux uses more descriptive names now (e.g., `wlp3s0` for Wi-Fi, `enp0s3` for Ethernet), but macOS sticks with `en0`, `en1`, etc.

---

### Summary

| Name    | Purpose                                | Physical? | Commonly Seen When…                     |
|---------|----------------------------------------|-----------|------------------------------------------|
| `lo`    | Localhost communication                | ❌ Virtual | Always                                   |
| `en0`   | Primary network (Wi-Fi/Ethernet)       | ✅ Yes     | Connected to network                     |
| `en1`   | Secondary network interface            | ✅ Yes     | Dock, USB Ethernet, dual NICs            |
| `utun1` | First VPN or tunnel interface          | ❌ Virtual | Connected to VPN (Tailscale, OpenVPN, etc.) |
| `utun2` | Second concurrent tunnel               | ❌ Virtual | Multiple active tunnels or services      |

---

## VPN Debug Guide

We'll test **each layer** — from client → tunnel → server → internet — and isolate exactly where it breaks.

### STEP 0: Quick Health Check — Is Tunnel "Up"?

#### On Client (Mac):

```bash
# Check if WireGuard interface is up
ifconfig | grep -A 3 -B 1 utun

# Should show something like:
# utun6: ... inet 10.0.0.2 --> 10.0.0.2 netmask 0xffffff00
```

```bash
# Check route to server
route get 10.0.0.1
```

✅ Should say `interface: utunX` — NOT `en0` or `en1`.

---

### STEP 1: Test LAYER 1 — Can Client Reach Server's Public IP?

This tests if **client can send UDP packets to server's external IP:port**.

#### On Client:

```bash
# Install netcat if not installed (macOS)
brew install netcat

# Send test UDP packet to server
echo "TEST" | nc -u -w 2 34.118.27.65 51820

# If no error → packet sent (doesn't mean received)
```

#### On Server — Monitor for incoming packets:

```bash
# Run this BEFORE sending the packet from client
sudo tcpdump -i ens4 -n udp port 51820
```

✅ If you see:

```
IP 212.233.85.103.12345 > 34.118.27.65.51820: UDP, length 4
```

→ Client → Server packets are arriving → **client outbound is working**.

❌ If you see nothing → client is blocked by local firewall, ISP, or NAT.

---

### STEP 2: Test LAYER 2 — Is WireGuard Receiving Packets?

#### On Server:

```bash
# Check if wg0 interface is up
ip link show wg0

# Should say "state UNKNOWN" or "UP" — that's normal for wg0
```

```bash
# Monitor WireGuard interface for ANY traffic
sudo tcpdump -i wg0 -n
```

→ Now try to connect from client.

✅ If you see packets → WireGuard is receiving them → **tunnel ingress is working**.

❌ If nothing → packets are not reaching `wg0` → check `ListenPort`, `PrivateKey`, or `Endpoint`.

---

### STEP 3: Test LAYER 3 — Can Client Ping Server's VPN IP?

This tests if **encrypted tunnel is working end-to-end**.

#### On Client:

```bash
ping -c 3 10.0.0.1
```

✅ If replies → **TUNNEL IS WORKING** → skip to STEP 5.

❌ If timeout → continue to STEP 4.

---

### STEP 4: Why Is `ping 10.0.0.1` Failing?

#### On Server — Check if it's receiving ping:

```bash
sudo tcpdump -i wg0 -n icmp
```

→ Run `ping 10.0.0.1` from client.

#### CASE A: You see ICMP packets on server

```
IP 10.0.0.2 > 10.0.0.1: ICMP echo request
```

→ Server is receiving ping → but not replying.

##### ✅ FIX: Check server firewall

```bash
sudo iptables -L INPUT -v -n
```

→ If policy is `DROP`, and no rule allows `wg0`, server ignores ping.

##### 🔧 Fix:

```bash
sudo iptables -I INPUT -i wg0 -j ACCEPT
```

→ Now `ping 10.0.0.1` should work.

#### ➤ CASE B: You see NO packets on `wg0`

→ Packets are not entering tunnel → likely **client routing issue**.

##### ✅ Check client route again:

```bash
route get 10.0.0.1
```

→ Must say `interface: utunX`.

##### ✅ Check client config:

```ini
Address = 10.0.0.2/24   # NOT /32
AllowedIPs = 0.0.0.0/0, ::/0
```

##### ✅ Restart WireGuard on client → reconnect.

---

### STEP 5: Test LAYER 4 — Can Client Reach Internet via VPN?

#### On Client:

```bash
# Test DNS (critical!)
nslookup google.com

# Should return IP — if timeout → DNS leak or blocked
```

```bash
# Test internet connectivity
ping -c 3 8.8.8.8
```

✅ If works → **NAT is working**.

❌ If fails → continue.

---

### STEP 6: Is Server Forwarding + NAT Working?

#### On Server:

```bash
# Check IP forwarding
cat /proc/sys/net/ipv4/ip_forward
# Should return "1"
```

```bash
# Check NAT rule counters
sudo iptables -t nat -L POSTROUTING -v -n
```

→ While pinging `8.8.8.8` from client, **packet counters should increase**.

❌ If counters don't increase → NAT rule not matching.

##### ✅ FIX: Use explicit SNAT (GCP fix)

```bash
sudo iptables -t nat -A POSTROUTING -o ens4 -p udp --sport 51820 -j SNAT --to-source 34.118.27.65
sudo iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o ens4 -j MASQUERADE
```

→ Restart WireGuard → test again.

---

### STEP 7: Monitor End-to-End Traffic

#### On Server — Watch all traffic:

```bash
sudo tcpdump -i any -n host 8.8.8.8
```

→ From client: `ping 8.8.8.8`

✅ If you see:

```
IP 10.0.0.2 > 8.8.8.8: ICMP echo request
IP 8.8.8.8 > 34.118.27.65: ICMP echo reply
```

→ **Everything is working** — problem is client firewall or DNS.

---

### STEP 8: Final DNS + Leak Test

#### On Client:

```bash
# Check what DNS server you're using
scutil --dns | grep 'nameserver'

# Should show 8.8.8.8 or 1.1.1.1 — NOT your ISP's DNS
```

```bash
# Test for DNS leak
curl https://dnsleaktest.com/api/ip
# Should return your SERVER's IP (34.118.27.65), not your real IP
```

---

### DEBUG FLOWCHART — Where Is It Broken?

```
Client → Public IP:Port?
  │
  ├❌ Blocked → Fix client firewall/ISP/NAT
  │
  └✅ → Server receiving on ens4?
        │
        ├❌ Port/firewall issue → Fix GCP firewall
        │
        └✅ → Packets reaching wg0?
              │
              ├❌ Config mismatch → Check keys, port, endpoint
              │
              └✅ → ping 10.0.0.1 works?
                    │
                    ├❌ Server firewall → iptables -I INPUT -i wg0 -j ACCEPT
                    │
                    └✅ → ping 8.8.8.8 works?
                          │
                          ├❌ NAT issue → Add SNAT rule
                          │
                          └✅ → nslookup works?
                                │
                                ├❌ DNS leak → Set DNS in config
                                │
                                └🎉 VPN WORKING!
```

---

### EMERGENCY NUCLEAR OPTION (If All Else Fails)

On server, run:

```bash
# Allow everything — bypass all firewalls
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -F
sudo iptables -t nat -F

# Re-add minimal rules
sudo iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o ens4 -j MASQUERADE
sudo iptables -A FORWARD -i wg0 -j ACCEPT
sudo iptables -A FORWARD -o wg0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -t nat -A POSTROUTING -o ens4 -p udp --sport 51820 -j SNAT --to-source 34.118.27.65
```

→ Test again.

---

### Final Checklist — Run These Commands in Order

| Step | Command | Where | Expected Result |
|------|---------|-------|------------------|
| 1 | `ifconfig \| grep utun` | Client | Shows `10.0.0.2/24` |
| 2 | `route get 10.0.0.1` | Client | `interface: utunX` |
| 3 | `nc -u -w 2 34.118.27.65 51820` | Client | No error |
| 4 | `tcpdump -i ens4 udp port 51820` | Server | Sees client packet |
| 5 | `tcpdump -i wg0` | Server | Sees encrypted WG packet |
| 6 | `ping 10.0.0.1` | Client | Replies! |
| 7 | `tcpdump -i wg0 icmp` | Server | Sees ping request |
| 8 | `iptables -I INPUT -i wg0 -j ACCEPT` | Server | If ping failed |
| 9 | `ping 8.8.8.8` | Client | Replies! |
| 10 | `tcpdump -i any host 8.8.8.8` | Server | Sees request + reply |
| 11 | `iptables -t nat -L -v -n` | Server | Counters increasing |
| 12 | `nslookup google.com` | Client | Returns IP |
| 13 | `curl ifconfig.me` | Client | Returns `34.118.27.65` |

---

🔁 **Follow this plan step by step — you WILL find the broken layer.**

You're not far from success — this debug plan has fixed 1000+ broken VPNs.

---

## WireGuard Interface and Peer Configuration

> **Both configs have `[Interface]` and `[Peer]` because WireGuard is peer-to-peer — each side defines its own local settings (`[Interface]`) and the other side's settings (`[Peer]`).**

It's like a **two-way contract** — each party signs their own copy and agrees on the other party's terms.

### **Server Config (`wg0.conf`)**

```ini
[Interface]
PrivateKey = SERVER_PRIVATE_KEY
Address = 10.0.0.1/24
ListenPort = 51820
# ... (NAT, firewall rules, etc.)

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 10
```

➡️ This means:

- **`[Interface]`** = "This is MY side of the tunnel."
  - My private key (secret)
  - My internal VPN IP: `10.0.0.1`
  - I listen on port `51820` for incoming connections

- **`[Peer]`** = "This is YOUR side — I only accept traffic from you if you match these rules."
  - Your public key (so I know it's really you)
  - You must use IP `10.0.0.2` (access control)
  - Send me keepalives every 10s (so I know you're alive)

---

### **Client Config (on your Mac/Phone)**

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = 34.118.27.65:51820
AllowedIPs = 0.0.0.0/0, ::/0
PersistentKeepalive = 10
```

➡️ This means:

- **`[Interface]`** = "This is MY side of the tunnel."
  - My private key (secret)
  - My internal VPN IP: `10.0.0.2`
  - Use DNS `1.1.1.1` for lookups (inside tunnel)

- **`[Peer]`** = "This is YOUR side — I'll send all traffic to you."
  - Your public key (so I know I'm talking to the real server)
  - Your public address: `34.118.27.65:51820` (where to send packets)
  - Route ALL traffic (`0.0.0.0/0`) through you (full tunnel)
  - Send keepalives every 10s (to keep NAT/firewall open)

---

### Visual: Two-Way Contract

```
        [ YOU (Client) ]                          [ SERVER ]
        ----------------                          ----------
[Interface]                                      [Interface]
- My PrivateKey                                  - My PrivateKey
- My IP: 10.0.0.2                                - My IP: 10.0.0.1
- My DNS: 1.1.1.1                                - I listen on port 51820

[Peer]                                           [Peer]
- Your PublicKey                                 - Your PublicKey
- Your Endpoint: 34.118.27.65:51820              - Your AllowedIP: 10.0.0.2/32
- I send ALL traffic to you                      - I only accept from your IP
- I send keepalives                              - I send keepalives too
```

---

### Why This Design?

- ✅ **Symmetric & Secure**: Both sides authenticate each other using public keys.
- ✅ **Flexible**: Client doesn't need a static IP — server learns it dynamically (unless you set `Endpoint` on server too).
- ✅ **Decentralized**: No central server — pure peer-to-peer.

---

### Real-World Analogy

> Imagine you and a friend agree to meet in a crowded cafe:
>
> - **You (Client)**:  
>   ➤ "I'll wear a red hat (`PrivateKey`) so you know it's me."  
>   ➤ "I'll look for someone holding a blue sign (`PublicKey`) at Table 12 (`Endpoint`)."
>
> - **Friend (Server)**:  
>   ➤ "I'll hold a blue sign (`PublicKey`) so you know it's me."  
>   ➤ "I'll only talk to someone wearing a red hat (`AllowedIPs + PublicKey`)."

✅ Both of you have your own "interface" (how you present yourself) and "peer" (how you identify the other).

---

### Important Notes

- 🔑 **Private keys NEVER leave their device** — server never sees client's private key, and vice versa.
- 🌐 **`AllowedIPs` on server** = which IPs this peer is allowed to use (security + routing).
- 🌐 **`AllowedIPs` on client** = which traffic to send through the tunnel (routing).
- 🔄 **You can have multiple `[Peer]` sections** — to connect to multiple clients (server) or multiple servers (client).

---

### Summary Table

| Section      | On Server Meaning                          | On Client Meaning                          |
|--------------|--------------------------------------------|--------------------------------------------|
| `[Interface]`| "This is MY local tunnel config"           | "This is MY local tunnel config"           |
| `PrivateKey` | Server's secret key                        | Client's secret key                        |
| `Address`    | Server's internal VPN IP (e.g., 10.0.0.1)  | Client's internal VPN IP (e.g., 10.0.0.2)  |
| `ListenPort` | Port server listens on (e.g., 51820)       | Usually omitted (or 0) on client           |
| `[Peer]`     | "This is what I expect from the CLIENT"    | "This is what I expect from the SERVER"    |
| `PublicKey`  | Client's public key (to verify them)       | Server's public key (to verify it)         |
| `Endpoint`   | Optional: client's public IP:port          | Server's public IP:port (REQUIRED)         |
| `AllowedIPs` | Client's allowed IP (e.g., 10.0.0.2/32)    | What traffic to route via server (0.0.0.0/0) |

---

## ISP = Internet Service Provider

It's the **company that gives you access to the internet**.

Think of them as the **"water company" for your internet** — you pay them, and they "pipe" the internet to your home or office.

---

### Examples of ISPs

Depending on your country:

| Country | Example ISPs |
|---------|--------------|
| USA     | Comcast, Verizon, AT&T, Spectrum |
| UK      | BT, Virgin Media, Sky |
| Germany | Deutsche Telekom, Vodafone, 1&1 |
| Russia  | Rostelecom, MTS, Beeline, Megafon |
| India   | Airtel, Jio, ACT Fibernet |
| Global  | Your local cable, DSL, fiber, or mobile carrier |

---

### What Does an ISP Do?

1. ✅ **Connects you to the internet** (via cable, fiber, DSL, 4G/5G, etc.)
2. ✅ **Assigns you an IP address** (your "home address" on the internet)
3. ✅ **Routes your traffic** to websites, apps, games, etc.
4. ✅ **May provide DNS** (like `78.123.45.67`) — which can log your browsing
5. ✅ **Can see (and sometimes log)** what websites you visit — unless you use a **VPN or encrypted DNS**

---

### Why Should You Care?

- **Privacy**: Your ISP can see every website you visit (unless you use HTTPS + VPN/DNS encryption).
- **Throttling**: Some ISPs slow down your speed for video streaming or torrents.
- **Censorship**: In some countries, ISPs block sites (news, social media, etc.).
- **Ads**: Some ISPs inject ads or sell your browsing data.

---

### How to Protect Yourself

1. ✅ **Use a VPN** → Hides your traffic from ISP
2. ✅ **Use encrypted DNS** → e.g., `1.1.1.1` or `8.8.8.8` → hides what sites you visit
3. ✅ **Always use HTTPS** → Encrypts content of your visits (but not the domain)

---

### Real-World Analogy

> Your **ISP is like your apartment building's front desk**.
>
> - They know **when you come and go** (online/offline)
> - They know **who you're visiting** (which websites = which apartments)
> - They can **open your mail** (if not encrypted = HTTP)
> - But if you use a **VPN**, it's like hiring a **private courier** — front desk only sees the courier, not your mail.

---

### Summary

| Term | Meaning |
|------|---------|
| **ISP** | Internet Service Provider — your gateway to the internet |
| **Role** | Gives you internet, assigns IP, routes traffic |
| **Risk** | Can monitor your browsing, throttle, or block sites |
| **Fix** | Use VPN + encrypted DNS (like `1.1.1.1`) for privacy |

---

## DNS = Domain Name System

It's the **"phonebook of the internet"** — it translates human-friendly domain names (like `google.com`) into machine-friendly **IP addresses** (like `142.250.189.206`).

Without DNS, you'd have to remember IP addresses for every website.

---

### How It Works (Simple)

1. You type `youtube.com` in your browser.
2. Your device asks a **DNS server**: "What's the IP for `youtube.com`?"
3. DNS server replies: `142.250.189.206`
4. Your browser connects to that IP → loads YouTube.

---

### Popular Public DNS Servers

| Provider       | IPv4 Primary | IPv4 Secondary | Features |
|----------------|--------------|----------------|----------|
| **Google DNS** | `8.8.8.8`    | `8.8.4.4`      | Fast, reliable, global |
| **Cloudflare** | `1.1.1.1`    | `1.0.0.1`      | Fastest, privacy-focused (no logging) |
| **Quad9**      | `9.9.9.9`    | `149.112.112.112` | Security-focused (blocks malware) |
| **OpenDNS**    | `208.67.222.222` | `208.67.220.220` | Family filtering, phishing protection |
| **AdGuard DNS**| `94.140.14.14` | `94.140.15.15` | Blocks ads & trackers |

---

### Why Use a Public DNS?

- ✅ **Faster** than your ISP's DNS
- ✅ **More reliable**
- ✅ **Better privacy** (some don't log your queries)
- ✅ **Security** (some block malicious sites)
- ✅ **Ad/tracker blocking** (AdGuard, NextDNS)

---

### Where to Set DNS?

- **On your device** (Wi-Fi settings → DNS)
- **In your router** (affects all devices at home)
- **In your VPN config** (e.g., WireGuard → `DNS = 1.1.1.1`)

---

### Pro Tip

Use **Cloudflare (`1.1.1.1`)** for speed + privacy, or **Quad9 (`9.9.9.9`)** for security.

In WireGuard, always set DNS inside the tunnel to avoid leaks:

```ini
[Interface]
DNS = 1.1.1.1, 8.8.8.8
```

---

### DNS Leak

A **DNS leak** happens when your device sends DNS queries (e.g., "What's the IP of `netflix.com`?") **outside the VPN tunnel** — usually to your ISP's DNS server — even while connected to the VPN.

➡️ This reveals:
- What websites you're visiting
- Your real location (ISP = tied to your home/office)
- Defeats the purpose of using a VPN for privacy

---

#### Example: Without DNS in WireGuard Config

You're connected to your GCP WireGuard server in Berlin → you should appear as browsing from Germany.

But if you **don't set `DNS = ...`** in your WireGuard config:

1. You type `reddit.com` in browser
2. Your Mac sends DNS query → to your **local ISP's DNS** (e.g., `78.123.45.67` in Moscow)
3. ISP logs: "User in Moscow looked up `reddit.com` at 3:42 PM"
4. Your traffic goes through VPN → but **your DNS query exposed you**

→ ❗ **DNS LEAK — your privacy is compromised.**

---

#### How Setting `DNS` in WireGuard Prevents Leaks

```ini
[Interface]
PrivateKey = ...
Address = 10.0.0.2/24
DNS = 1.1.1.1, 8.8.8.8   👈 Forces all DNS queries through the tunnel
```

Now:

1. You type `reddit.com`
2. Your device sends DNS query → **through the encrypted WireGuard tunnel** → to `1.1.1.1` (Cloudflare)
3. Cloudflare replies → through tunnel → you get the IP
4. Your ISP sees **nothing** — only encrypted VPN traffic to your server

→ ✅ **NO DNS LEAK — your browsing is private.**

---

#### How to Test for DNS Leaks

Visit these sites while connected to your VPN:

- https://www.dnsleaktest.com → Run "Extended Test"
- https://ipleak.net
- https://dnsleak.com

✅ If you see **only your VPN server's location** and **DNS servers you configured** (e.g., `1.1.1.1`) → you're safe.

❌ If you see your **real city** or **ISP's DNS** → you have a leak.

---

#### Bonus: Prevent All Leaks — Use `AllowedIPs = 0.0.0.0/0, ::/0`

This ensures **all traffic**, including DNS, is forced through the tunnel — even if apps try to bypass it.

```ini
[Peer]
AllowedIPs = 0.0.0.0/0, ::/0   👈 Routes EVERYTHING through VPN
```

---

#### Summary

| Term | Meaning |
|------|---------|
| **DNS Leak** | Your DNS queries escape the VPN → expose browsing history + location |
| **Fix** | Set `DNS = 1.1.1.1` (or similar) in `[Interface]` section of WireGuard config |
| **Extra Protection** | Use `AllowedIPs = 0.0.0.0/0` to force all traffic through tunnel |
| **Test** | Use https://dnsleaktest.com to confirm you're leak-free |

---

## NAT = Network Address Translation

In the context of your WireGuard VPN setup, **NAT (Network Address Translation)** is the critical process that allows your client device (with its private VPN IP `10.0.0.2`) to access the public internet through your server's public IP address (`34.118.27.65`).

Here's a breakdown:

### What is NAT?

NAT is a method used in networking to modify the IP address information in the headers of packets while they are in transit across a traffic routing device (in this case, your VPN server).

Think of it like this:
*   Your client device is like a person inside a large office building (your private VPN network `10.0.0.0/24`).
*   The office building has only one public street address (your server's public IP, `34.118.27.65`).
*   When the person inside wants to send a letter to the outside world, they give it to the front desk (the VPN server).
*   The front desk replaces the sender's internal office number (`10.0.0.2`) with the building's public street address (`34.118.27.65`) before mailing it out.
*   When a reply comes back to the building's public address, the front desk knows which internal person it's for and delivers it to `10.0.0.2`.

Without NAT, a website on the internet would receive a request from `10.0.0.2` — an address that is not routable on the public internet — and wouldn't know where to send the response.

### What is "NATing" in your setup?

In your WireGuard server configuration, "NATing" specifically refers to the **MASQUERADE** rule you have in your `wg0.conf` file:

```bash
PostUp = iptables -A FORWARD -i %i -j ACCEPT; \
         iptables -A FORWARD -o %i -m state --state RELATED,ESTABLISHED -j ACCEPT; \
         iptables -t nat -A POSTROUTING -o $NIC -j MASQUERADE
```

Let's break down that last line, which is the heart of the NATing process:

*   `iptables -t nat`: We're working with the `nat` table in iptables, which is responsible for Network Address Translation.
*   `-A POSTROUTING`: We're adding a rule to the `POSTROUTING` chain. This chain is consulted just before a packet is sent out of the server to its final destination.
*   `-o $NIC`: This rule applies only to packets going out of the server's main physical network interface (e.g., `ens4`), which is connected to the public internet.
*   `-j MASQUERADE`: This is the action: "Masquerade" the packet. It dynamically replaces the **source IP address** of the packet (which is your client's private IP, `10.0.0.2`) with the **IP address of the outgoing interface** (your server's public IP, `34.118.27.65`).

**In simple terms, "NATing" here means:**
> "When a packet from my VPN client (`10.0.0.2`) is about to leave my server to go to the internet, change its 'From' address to my server's public IP (`34.118.27.65`) so the internet knows where to send the reply. When the reply comes back to the server, it will automatically know to send it back through the VPN tunnel to `10.0.0.2`."

This is why, when you visit `https://www.whatismyip.com` while connected to your VPN, it shows `34.118.27.65`. Your traffic is being "NATed" through your server.

The `FORWARD` rules above it simply tell the server's firewall to allow this traffic to pass through from the WireGuard interface (`wg0`) to the internet (`ens4`) and back.

---

## GCP VPN Troubleshooting

Got it ✅

You're setting up **WireGuard** via Terraform on **GCP**, your client connects, the **first handshake** works, but after that no further handshakes happen and **no traffic flows**.

This is a common issue when deploying **WireGuard VPNs on Google Cloud**. It usually relates to **GCP-specific firewall, NAT, and routing settings**.

Let's debug step by step:

---

## **1. Common Reasons for Your Problem**

The problem usually happens because of one or more of these:

### **(A) GCP Firewall Rules** 🔥

* GCP requires **explicit firewall rules** for both:

  * **UDP 51820** (WireGuard port)
  * **Forwarded traffic** from VPN subnet (`10.200.200.0/24`)
* Your firewall currently allows UDP 51820 — ✅ **OK**
* But it **doesn't allow internal forwarding** for VPN clients → ❌ **Needs fix**

---

### **(B) IP Forwarding on the VM** ⚡

* You've set `can_ip_forward = true` ✅
* But GCP also requires you to **enable routing at the VPC level**.

---

### **(C) Missing `iptables` Rules** 🧩

In your script, you add NAT rules:

```bash
iptables -t nat -A POSTROUTING -s 10.200.200.0/24 -o $NIC -j MASQUERADE
```

That's correct, but **GCP drops forwarded packets by default** unless we add:

```bash
iptables -A FORWARD -i wg0 -j ACCEPT
iptables -A FORWARD -o wg0 -j ACCEPT
```

I don't see these two rules in your script → **needs fix**.

---

### **(D) Google Cloud "Return Traffic" Problem** 🚦

Google Compute Engine **enforces asymmetric routing protection**:

* If packets leave via WireGuard but GCP thinks the return path is different, it drops them.
* We must ensure that:

  * **Source network = 10.200.200.0/24**
  * **Destination = 0.0.0.0/0**
  * **Routing table allows NAT**.

---

### **(E) MTU Problems** 🕵️‍♂️

* GCP VMs often have **MTU = 1460**.
* WireGuard defaults to **1420** but sometimes still fragments packets.
* Solution: Explicitly set MTU in `/etc/wireguard/wg0.conf`:

```ini
[Interface]
MTU = 1280
```

---

## **2. Fix GCP Firewall Rules**

Update your Terraform firewall block:

```hcl
resource "google_compute_firewall" "vpn_firewall" {
  name    = "allow-vpn-traffic"
  network = "default"

  allow {
    protocol = "udp"
    ports    = ["51820"]
  }

  # Allow ICMP for debugging
  allow {
    protocol = "icmp"
  }

  # Allow VPN subnet traffic
  allow {
    protocol = "all"
  }

  source_ranges = ["0.0.0.0/0", "10.200.200.0/24"]
  target_tags   = ["amnezia-vpn"]
}
```

---

## **3. Fix iptables Rules**

Modify your `metadata_startup_script` NAT section:

```bash
PostUp = iptables -A FORWARD -i %i -j ACCEPT; \
         iptables -A FORWARD -o %i -j ACCEPT; \
         iptables -t nat -A POSTROUTING -s 10.200.200.0/24 -o $NIC -j MASQUERADE

PostDown = iptables -D FORWARD -i %i -j ACCEPT; \
           iptables -D FORWARD -o %i -j ACCEPT; \
           iptables -t nat -D POSTROUTING -s 10.200.200.0/24 -o $NIC -j MASQUERADE
```

---

## **4. Enable Routing in GCP**

Terraform VM config already has:

```hcl
can_ip_forward = true
```

But we also need to enable **VPC routing**:

```hcl
resource "google_compute_route" "vpn_route" {
  name                   = "vpn-route"
  network                = "default"
  dest_range             = "10.200.200.0/24"
  next_hop_instance      = google_compute_instance.vpn_vm.name
  next_hop_instance_zone = var.zone
  priority               = 1000
}
```

---

## **5. Set MTU to Avoid Fragmentation**

In your client config `/root/client-wg0.conf`:

```ini
[Interface]
MTU = 1280
```

Do the same in `/etc/wireguard/wg0.conf` on the server.

---

## **6. Debugging Commands**

Run these on the **server**:

```bash
# Check WireGuard state
wg show

# Check if packets are hitting firewall
iptables -vnL FORWARD

# Check NAT table
iptables -t nat -L -n -v

# See if GCP dropped packets
dmesg | grep DROP
```

On the **client**:

```bash
# Check handshake attempts
wg show

# Test DNS
nslookup google.com 1.1.1.1
```

---

## **7. Quick Checklist**

| Problem                     | Fix                                   |
| --------------------------- | ------------------------------------- |
| No routing for VPN subnet   | Add `google_compute_route`            |
| Firewall blocks VPN traffic | Allow `10.200.200.0/24` in firewall   |
| Missing iptables rules      | Add `FORWARD` + `MASQUERADE` rules    |
| GCP asymmetric routing      | Use SNAT on `$NIC` + allow source NAT |
| MTU mismatch                | Set `MTU=1280`                        |

---

## **Next Step**

If you apply these fixes and the handshake still dies after a few seconds, it's almost always **GCP VPC firewall or NAT return routing**.

---

Do you want me to give you a **minimal fixed Terraform setup** where I integrate:

* Correct firewall rules
* Correct iptables
* Proper VPC routing

So you can just `terraform apply` and get a working WireGuard VPN?

It'll save debugging time. Should I?
