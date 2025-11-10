# Network Tools and Concepts Review

## Table of Contents
1. [Network Tools](#network-tools)
2. [Special IP Addresses](#special-ip-addresses)
3. [Netfilter and iptables](#netfilter-and-iptables)
4. [iptables Concepts](#iptables-concepts)
5. [iptables Structure](#iptables-structure)
6. [iptables Actions (Targets)](#iptables-actions-targets)
7. [iptables Example](#iptables-example)

---

## Network Tools

### Useful Commands

**endless ssh** - Tool for maintaining persistent SSH connections

**lsof -i tcp:3000** - List open files/processes using TCP port 3000

**ip -s link show dev eth0** - Show detailed statistics for network interface eth0

**Bandwidth Monitoring with iptables** - Tutorial on monitoring network bandwidth using iptables: [https://www.linux.com/training-tutorials/bandwidth-monitoring-iptables/](https://www.linux.com/training-tutorials/bandwidth-monitoring-iptables/)

---

## Special IP Addresses

### 127.0.0.1 (Loopback Address)

**127.0.0.1** is a special address that points to the **loopback interface** of the local computer.

**What it means:**
- Always refers to the local machine itself
- Used for internal communication within the same device
- Commonly used for testing network services locally
- Also accessible via `localhost` hostname

**Use cases:**
- Running a web server locally and accessing it via `http://127.0.0.1:3000`
- Testing database connections on the same machine
- Inter-process communication on the same host

---

### 0.0.0.0 (Wildcard Address)

**0.0.0.0** is a special non-routable address that indicates a non-existent host. However, sometimes it also means **all local IP addresses of all interfaces**.

**What it means:**
- **In routing contexts**: Represents "any address" or "all interfaces"
- **In firewall rules**: Means "accept connections from any source"
- **In server binding**: Means "listen on all available network interfaces"

**Use cases:**
- **Server binding**: `0.0.0.0:80` means "listen on port 80 on all network interfaces"
- **Firewall rules**: `-s 0.0.0.0/0` means "from any source IP"
- **Routing**: Default route `0.0.0.0/0` means "route all traffic"

**Examples:**
```bash
# Listen on all interfaces
python -m http.server 8000 --bind 0.0.0.0

# Firewall rule allowing from anywhere
iptables -A INPUT -s 0.0.0.0/0 -j ACCEPT
```

---

## Netfilter and iptables

### Netfilter

**Netfilter** is a set of software hooks inside the Linux kernel that allow kernel modules to register callback functions from the network protocol stack.

**What it does:**
- Provides the infrastructure for packet filtering, NAT, and packet mangling
- Works at the kernel level for maximum performance
- Forms the foundation of Linux firewalls

**Key concept:**
- **Hook** - A software element that allows intercepting callback functions in other processes
- Netfilter hooks intercept packets at various points in the network stack

---

### iptables

**iptables** is a user-space utility program that allows system administrators to configure the IP packet filter rules of the Linux kernel firewall, implemented as different Netfilter modules.

**Relationship to Netfilter:**
- Netfilter is the **foundation** (kernel-level hooks)
- iptables is the **tool** to configure Netfilter rules
- Sometimes people forget about Netfilter and just call the whole system "iptables"

**What iptables can do:**
- Configure **stateless and stateful** packet filtering for IPv4 and IPv6
- Set up all types of **IP address and port translation** (NAT, PAT, NAPT)
- Configure **QoS (Quality of Service)** policies
- Perform various packet manipulations, such as modifying fields in IP headers

**Check version:**
```bash
iptables -V
```

---

## iptables Concepts

### Stateless vs Stateful Filtering

#### Stateless Filtering

**Stateless** filtering is based on checking **static parameters of a single packet**, such as:
- Source and destination IP addresses
- Port numbers
- Other non-changing parameters

**Characteristics:**
- Each packet is evaluated independently
- No memory of previous packets
- Simpler but less flexible
- Faster processing

**Example:**
```bash
# Block all packets from 192.168.1.100
iptables -A INPUT -s 192.168.1.100 -j DROP
```

---

#### Stateful Filtering

**Stateful** filtering is based on **analyzing traffic flows**. With it, you can determine parameters of an entire TCP session or UDP stream.

**Characteristics:**
- Tracks connection state (NEW, ESTABLISHED, RELATED, INVALID)
- Remembers previous packets in a connection
- More intelligent and flexible
- Can allow return traffic automatically

**Example:**
```bash
# Allow established and related connections
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

---

## iptables Structure

### Rule Components

Each rule in iptables consists of three parts:

1. **Criterion** - The condition that packet or connection parameters must meet for the action to trigger. In our example, this condition is the presence of a packet on the incoming interface establishing a connection on port 22.

2. **Action** - The operation to perform on the packet or connection if the criterion is met. In our case, this is to block the packet on port 22.

3. **Counter** - An entity that counts how many packets were subjected to the action and, based on this, shows their volume in bytes.

---

### Chains

A set of rules forms **chains**.

**Types of chains:**
- **Built-in chains** - Pre-installed rule sets that exist in iptables by default
- **User-defined chains** - Custom chains created by administrators

**Built-in chains:**
There are 5 built-in chains, distinguished by the purpose of the packet. Chain names are written in uppercase.

1. **PREROUTING** - Rules in this chain apply to all packets arriving on the network interface from outside
2. **INPUT** - Applied to packets intended for the host itself or for a local process running on this host. That is, they are not transit packets
3. **FORWARD** - Rules that apply to transit packets passing through the host without stopping
4. **OUTPUT** - Applied to packets generated by the host itself
5. **POSTROUTING** - Applied to packets that should leave the network interface

---

### Tables

**Tables** are sets of built-in and user-defined chains. Depending on which table a chain of rules is in, certain actions are performed on the packet or connection.

**There are 5 tables:**

1. **filter** - Table that performs packet filtering functions based on certain parameters. In most cases, you will use this one. Contains the following built-in chains: FORWARD, INPUT, OUTPUT

2. **raw** - To understand the purpose of this table, you need to understand the logic of a stateful firewall. The fact is that by default, iptables considers each packet as part of a large flow and can determine which connection a particular packet belongs to. Using the raw table, exceptions are configured that will consider the packet as a separate entity not tied to anything. Contains the following built-in chains: INPUT, OUTPUT

3. **nat** - Table designed entirely for the function of network address translation. Contains the following built-in chains: PREROUTING, OUTPUT, POSTROUTING

4. **mangle** - Table designed to modify various packet headers. You can, for example, change TTL, number of hops, and more. Contains the following built-in chains: PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING

5. **security** - Used to assign certain labels to packets or connections, which can later be interpreted by SELinux

---

## iptables Actions (Targets)

**Targets** are actions that iptables performs on packets that match the rule criteria:

- **ACCEPT** - Allow the packet to pass
- **DROP** - Silently discard the packet without reporting the reason
- **QUEUE** - Sends the packet outside the iptables logic to a third-party application. This may be needed when you need to process a packet within another process in another program
- **RETURN** - Stop processing the rule and return one rule back. This action is similar to `break` in programming languages
- **REJECT** - Discards the packet and returns the reason as an error, for example: `icmp unreachable`
- **LOG** - Simply makes a log entry if the packet matches the rule criteria

---

## iptables Example

### Example Rule Breakdown

```bash
 iptables -A INPUT -p tcp -s 192.168.2.2 --dport 80 -j DROP
```

**Breaking down the command:**

- **`iptables`** - Called the utility
- **`-A`** - With this flag, we indicated that we need to add a rule to an existing chain
- **`INPUT`** - Specified the chain to which we want to add the rule
- **`-p tcp`** - Explicitly specified the TCP protocol. Here you can also specify other protocols (udp, icmp, sctp), or the protocol number encapsulated in IP (17 – udp, 6 – tcp, etc.)
- **`-s 192.168.2.2`** - Specified what source address the packet should have that we want to filter
- **`--dport 80`** - Specified packets addressed to which port we want to filter. In this case - 80, on which our Apache server is running
- **`-j DROP`** - Specified what to do with the packet whose parameters matched these criteria. In this case, simply silently discard

---

### Removing a Rule

To open access again, simply change the `-A` flag in the rule to `-D`, thereby removing this rule from iptables:

```bash
iptables -D INPUT -p tcp -s 192.168.2.2 --dport 80 -j DROP
```

---

### Visual Reference

![[network-1.png]]

---

## Summary

| Concept | Description |
|---------|-------------|
| **127.0.0.1** | Loopback address - always refers to localhost |
| **0.0.0.0** | Wildcard address - means "all interfaces" or "any address" |
| **Netfilter** | Kernel-level hooks for packet filtering |
| **iptables** | User-space tool to configure Netfilter rules |
| **Stateless** | Filters based on single packet parameters |
| **Stateful** | Filters based on connection state and flow analysis |
| **Chains** | Sets of rules (PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING) |
| **Tables** | Collections of chains (filter, nat, mangle, raw, security) |
| **Targets** | Actions to take (ACCEPT, DROP, REJECT, LOG, etc.) |
