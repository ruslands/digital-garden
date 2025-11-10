# Network Tools: Complete Reference Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Connection and Port Tools](#connection-and-port-tools)
3. [Network Interface Tools](#network-interface-tools)
4. [Firewall Tools](#firewall-tools)
5. [DNS Tools](#dns-tools)
6. [Packet Analysis Tools](#packet-analysis-tools)
7. [Network Discovery Tools](#network-discovery-tools)
8. [Configuration Files](#configuration-files)
9. [VPN-Specific Tools](#vpn-specific-tools)
10. [Summary](#summary)

---

## Introduction

Network tools are essential for managing, monitoring, and troubleshooting network connections, interfaces, and services. This guide covers common Linux/Unix network utilities and their usage.

**Key Categories:**
- Connection and port management
- Network interface configuration
- Firewall management
- DNS resolution
- Packet capture and analysis
- Network discovery and scanning

---

## Connection and Port Tools

### netstat

**Description:**
Displays network connections, routing tables, interface stats. (Deprecated; use `ss`/`ip`.)

**Command Examples:**
```bash
# List listening TCP/UDP ports
netstat -tuln

# Show routing table
netstat -r

# List interfaces
netstat -i

# Get default gateway
netstat -r | grep default

# Find process using PostgreSQL
netstat -anp | grep 5432

# Show Docker-related connections
netstat -anp | grep docker
```

**Note:** Deprecated—prefer `ss` or `ip` for modern systems.

---

### ss

**Description:**
Modern replacement for `netstat`. Shows socket statistics with better performance.

**Command Examples:**
```bash
# List listening ports
ss -tuln

# Show active TCP connections
ss -t state established

# Show listening ports with process info
ss -ltp
```

**Advantages:**
- Faster than netstat
- More detailed information
- Better performance on large systems

---

### telnet

**Description:**
Connects to remote hosts via TCP; useful for testing open ports (insecure for shells).

**Command Examples:**
```bash
# Test HTTP port
telnet example.com 80

# Test SMTP locally
telnet localhost 25
```

**Note:** Insecure for remote shell access—use `ssh` instead. Still useful for port testing.

---

### lsof

**Description:**
Lists open files and network connections per process.

**Command Examples:**
```bash
# Show processes using port 80
lsof -i :80

# Show SSH connections
lsof -i TCP:22

# Files opened by user "john"
lsof -u john

# Connections to specific IP
lsof -i @192.168.1.5

# List files/sockets opened by user
lsof -u www-data

# Show files NOT opened by root
lsof -u ^root
```

**Use Cases:**
- Finding which process is using a port
- Debugging connection issues
- Monitoring network activity per process

---

### netcat (nc)

**Description:**
Swiss-army knife for TCP/UDP connections—transfer files, port scan, chat, proxy, etc.

**Command Examples:**
```bash
# Listen on port 8080
nc -lvp 8080

# Connect to port 80
nc example.com 80

# Send data to remote port
echo "test" | nc -w 3 192.168.1.5 1234

# Port scan (verbose)
nc -zv example.com 22
```

**Use Cases:**
- Port testing
- File transfer
- Simple chat server
- Network debugging

---

## Network Interface Tools

### ip

**Description:**
Modern tool for managing interfaces, routing, ARP, etc. Replaces `ifconfig` and `route`.

**Command Examples:**
```bash
# Show IP addresses
ip addr show

# Display routing table
ip route

# Disable interface
ip link set eth0 down

# Get source IP used to reach 8.8.8.8
ip route get 8.8.8.8 | awk '{print $NF; exit}'

# Show routing rules (policy routing)
ip ru
```

**Advantages:**
- Modern replacement for ifconfig/route
- More features and better performance
- Consistent interface for all network operations

---

### ifconfig

**Description:**
Legacy tool to configure network interfaces. Deprecated—use `ip addr` instead.

**Command Examples:**
```bash
# Show all interfaces
ifconfig

# Bring interface up
ifconfig eth0 up

# Set static IP (temporary)
ifconfig eth0 192.168.1.10 netmask 255.255.255.0
```

**Note:** Deprecated—prefer `ip addr` for modern systems.

---

### route

**Description:**
Legacy tool to view/manipulate routing table. Deprecated—use `ip route` instead.

**Command Examples:**
```bash
# Show routing table numerically
route -n

# Set default gateway (temporary)
route add default gw 192.168.1.1

# Delete route
route del -net 10.0.0.0 netmask 255.0.0.0
```

**Note:** Deprecated—prefer `ip route` for modern systems.

---

### ethtool

**Description:**
Query or control network driver and hardware settings (speed, duplex, offload, etc.).

**Command Examples:**
```bash
# Show driver/hardware info
ethtool eth0

# Set speed/duplex (temporary)
ethtool -s eth0 speed 1000 duplex full

# Show offload features
ethtool -k eth0
```

**Use Cases:**
- Checking link speed and status
- Configuring network adapter settings
- Troubleshooting network performance

---

## Firewall Tools

### iptables

**Description:**
Configures IPv4 packet filtering and NAT rules (Linux firewall).

**Command Examples:**
```bash
# List rules with details
iptables -L -n -v

# Block subnet
iptables -A INPUT -s 192.168.1.0/24 -j DROP

# Flush all rules
iptables -F
```

**See:** [iptables.md](./iptables.md) for detailed guide.

---

### iptables-persistent

**Description:**
Saves and restores `iptables` rules across reboots (Debian/Ubuntu package).

**Command Examples:**
```bash
# Save current rules
sudo netfilter-persistent save

# Reload saved rules
sudo netfilter-persistent reload

# Older alias
sudo iptables-persistent save
```

**Note:** Critical for making firewall rules persistent across reboots.

---

### ufw

**Description:**
Uncomplicated Firewall—frontend for managing `iptables` (common on Ubuntu).

**Command Examples:**
```bash
# Enable firewall
ufw enable

# Allow SSH
ufw allow 22/tcp

# Show rules and status
ufw status verbose
```

**Use Cases:**
- Simple firewall management
- Quick rule setup
- User-friendly interface

---

### fail2ban

**Description:**
Scans logs and bans IPs exhibiting malicious behavior (e.g., brute-force attacks).

**Command Examples:**
```bash
# Check SSH jail status
fail2ban-client status sshd

# Reload config
fail2ban-client reload

# Manually unban IP
fail2ban-client set sshd unbanip 192.168.1.100
```

**Use Cases:**
- Protecting SSH from brute-force attacks
- Blocking malicious IPs automatically
- Log-based intrusion prevention

**Note:** Requires configuration files (usually in `/etc/fail2ban/`) and works with services like SSH, Apache, etc.

---

## DNS Tools

### dig

**Description:**
DNS lookup utility for querying DNS records (more flexible than `nslookup`).

**Command Examples:**
```bash
# Basic A record
dig example.com

# Get mail servers
dig example.com MX

# Query specific DNS server
dig @8.8.8.8 example.com
```

**Advantages:**
- More detailed output than nslookup
- Better for scripting
- More flexible query options

---

### host

**Description:**
Simple DNS lookup tool (part of BIND).

**Command Examples:**
```bash
# Get A record
host example.com

# Get mail servers
host -t MX example.com

# Reverse DNS lookup
host 8.8.8.8
```

**Use Cases:**
- Quick DNS lookups
- Simple reverse DNS queries
- Basic DNS troubleshooting

---

### nslookup

**Description:**
Legacy DNS query tool (deprecated—prefer `dig` or `host`).

**Command Examples:**
```bash
nslookup example.com
nslookup -type=MX example.com
nslookup 8.8.8.8
```

**Note:** Deprecated—prefer `dig` or `host` for modern systems.

---

### whois

**Description:**
Queries WHOIS databases for domain/IP ownership and registration info.

**Command Examples:**
```bash
# Get domain registrar info
whois google.com

# Find owner of IP address
whois 8.8.8.8
```

**Use Cases:**
- Finding domain registration information
- Identifying IP address owners
- Network reconnaissance

---

## Packet Analysis Tools

### tcpdump

**Description:**
Powerful command-line packet analyzer. Captures and displays network traffic in real time.

**Command Examples:**
```bash
# Capture on eth0
tcpdump -i eth0

# Capture HTTP traffic
tcpdump port 80

# Save to file for Wireshark
tcpdump -w capture.pcap

# Watch external interface inbound on port
tcpdump -i ens4 -n udp port 59361

# Watch tunnel interface inbound
tcpdump -i awg0 -n

# Watch external interface outbound to host
tcpdump -i ens4 -n host 10.186.0.42
```

**Use Cases:**
- Network troubleshooting
- Security analysis
- Protocol debugging
- Traffic monitoring

---

### ngrep

**Description:**
Network grep—captures packets matching regex or port (like tcpdump + grep).

**Command Examples:**
```bash
# Capture HTTP traffic
ngrep -d eth0 port 80

# Quietly search for "password" in SSH traffic
ngrep -q 'password' port 22

# Show DNS queries line by line
ngrep -W byline port 53
```

**Use Cases:**
- Searching for specific patterns in network traffic
- Monitoring specific protocols
- Security analysis

---

## Network Discovery Tools

### ping

**Description:**
Tests reachability of a host by sending ICMP echo requests.

**Command Examples:**
```bash
# Basic connectivity test
ping google.com

# Send 4 packets only
ping -c 4 8.8.8.8
```

**Use Cases:**
- Basic connectivity testing
- Network troubleshooting
- Latency measurement

---

### traceroute / tracepath

**Description:**
Traces the path packets take to a network host. `tracepath` doesn't require root.

**Command Examples:**
```bash
# Standard traceroute
traceroute google.com

# No root needed, shows MTU/path
tracepath 8.8.8.8
```

**Use Cases:**
- Network path discovery
- Troubleshooting routing issues
- Identifying network hops

---

### mtr

**Description:**
Combines `ping` + `traceroute`—continuously probes path and shows latency/packet loss.

**Command Examples:**
```bash
# Numeric output (faster)
mtr -n google.com

# Report mode, 10 cycles, then exit
mtr -rwc 10 google.com

# Trace via TCP port 443 (useful behind firewalls)
mtr --tcp -P 443 google.com
```

**Use Cases:**
- Continuous network monitoring
- Identifying intermittent issues
- Real-time latency and packet loss tracking

---

### nmap

**Description:**
Network discovery and security auditing tool. Scans hosts, ports, services, OS, etc.

**Command Examples:**
```bash
# Basic scan
nmap 192.168.1.1

# Service/version detection on ports 1-1000
nmap -sV -p 1-1000 target.com

# Aggressive scan (OS, version, script, traceroute)
nmap -A target.com
```

**Use Cases:**
- Network discovery
- Port scanning
- Security auditing
- Service identification

**Note:** Essential for security audits—use responsibly and only on networks you own or have permission to scan.

---

## Configuration Files

### /etc/network/interfaces

**Description:**
Config file (Debian/Ubuntu) to define static/dynamic network interfaces at boot.

**Example Configuration:**
```bash
# Static IP
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1

# DHCP
iface eth0 inet dhcp
```

**Note:** Debian/Ubuntu specific—RHEL/Fedora/CentOS use `/etc/sysconfig/network-scripts/ifcfg-*` or NetworkManager.

---

## VPN-Specific Tools

### WireGuard Commands

**Configuration:**
```bash
# WireGuard config location
/etc/wireguard/wg0.conf

# Restart WireGuard
systemctl restart wg-quick@wg0
```

**Monitoring:**
```bash
# Check IP forwarding
sysctl net.ipv4.ip_forward

# Watch external interface inbound on port
tcpdump -i ens4 -n udp port 59361

# Watch tunnel interface inbound
tcpdump -i awg0 -n

# Watch external interface outbound to host
tcpdump -i ens4 -n host 10.186.0.42
```

---

### resolvconf

**Purpose:**
Manages the system's DNS resolver configuration dynamically.

**What it manages:**
The `/etc/resolv.conf` file. This file tells your system which DNS servers to use to translate domain names (like `google.com`) into IP addresses.

**Why it's needed for WireGuard (especially on the client):**
When your WireGuard client connects, it often needs to use specific DNS servers (like `8.8.8.8` or `1.1.1.1`, as in your config) to prevent DNS leaks. The `resolvconf` tool is what the `wg-quick` script uses to automatically update the system's DNS settings when the VPN interface comes up (`PostUp`) and restore them when it goes down (`PostDown`).

**How it works in your config:**
In your client configuration, you have: `DNS = 8.8.8.8, 1.1.1.1`

When `wg-quick` starts the interface, it calls `resolvconf` to add these DNS servers to the top of the system's resolver list. When the interface is stopped, `resolvconf` removes them, reverting to your original DNS settings.

**Without it:**
Your system might ignore the `DNS` directive in your WireGuard config, potentially causing your DNS queries to leak outside the VPN tunnel, which is a privacy risk.

---

### iptables-persistent (for VPN)

**Purpose:**
Saves and automatically reloads your `iptables` firewall rules across system reboots.

**Why it's critical for your WireGuard server:**
Your server's functionality depends on two key `iptables` rules:

**NAT/Masquerading:**
```bash
iptables -t nat -A POSTROUTING -o ens4 -j MASQUERADE
```
This rule allows traffic from your VPN client (with its private IP `10.0.0.2`) to access the internet by making it appear as if it's coming from your server's public IP (`34.118.27.65`).

**Forwarding Rules:**
```bash
iptables -A FORWARD -i wg0 -j ACCEPT
```
These rules tell the Linux kernel to allow traffic to be forwarded *through* the server from the WireGuard interface (`wg0`) to the internet (`ens4`) and back.

**The Problem:**
By default, `iptables` rules are **volatile**. They exist only in memory and are **completely lost** when the server reboots.

**How `iptables-persistent` solves it:**
- You set up your rules (e.g., via the `PostUp` script in WireGuard)
- You run `sudo netfilter-persistent save` (or `sudo iptables-persistent save` on older systems). This command saves the current, working set of rules to configuration files (typically `/etc/iptables/rules.v4` and `/etc/iptables/rules.v6`)
- On every subsequent boot, the `netfilter-persistent` service automatically loads these saved rules, ensuring your VPN's NAT and forwarding functionality is restored

**Without it:**
After a reboot, your server would still run WireGuard and accept client connections, but clients would be unable to access the internet because the crucial NAT and forwarding rules would be gone. Your VPN would be broken.

---

## Summary

**Tool Categories:**

| Category | Tools | Purpose |
|----------|-------|---------|
| **Connection/Port** | `ss`, `netstat`, `lsof`, `netcat` | View connections, find processes using ports |
| **Interface** | `ip`, `ifconfig`, `route`, `ethtool` | Configure and monitor network interfaces |
| **Firewall** | `iptables`, `ufw`, `fail2ban` | Packet filtering and security |
| **DNS** | `dig`, `host`, `nslookup`, `whois` | DNS resolution and domain information |
| **Packet Analysis** | `tcpdump`, `ngrep` | Capture and analyze network traffic |
| **Discovery** | `ping`, `traceroute`, `mtr`, `nmap` | Network discovery and troubleshooting |

**Deprecated Tools:**
- `ifconfig` → use `ip addr`
- `route` → use `ip route`
- `netstat` → use `ss`
- `nslookup` → use `dig` or `host`

**Best Practices:**
- Use modern tools (`ip`, `ss`, `dig`) instead of deprecated ones
- Save iptables rules to make them persistent
- Use `tcpdump` for packet analysis
- Use `nmap` responsibly and only on networks you own
- Combine tools for comprehensive network troubleshooting

**VPN-Specific:**
- Use `resolvconf` for DNS management in VPN configs
- Use `iptables-persistent` to save firewall rules
- Monitor VPN traffic with `tcpdump` on tunnel interfaces

Understanding these network tools is essential for system administration, network troubleshooting, and security management.
