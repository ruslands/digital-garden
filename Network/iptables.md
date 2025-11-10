# iptables: Linux Firewall Configuration Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Netfilter and iptables](#netfilter-and-iptables)
3. [Basic Concepts](#basic-concepts)
4. [Tables](#tables)
5. [Chains](#chains)
6. [Targets (Actions)](#targets-actions)
7. [Common Commands](#common-commands)
8. [Examples](#examples)
9. [Resources](#resources)
10. [Summary](#summary)

---

## Introduction

**iptables** is a command-line utility for configuring the Linux kernel firewall (Netfilter). It allows you to set up rules for filtering, NAT (Network Address Translation), and packet manipulation.

**Key Concepts:**
- **Netfilter**: Kernel hooks for packet processing
- **iptables**: User-space tool to configure Netfilter
- **Tables**: Different rule sets for different purposes
- **Chains**: Ordered lists of rules
- **Targets**: Actions to take on matching packets

---

## Netfilter and iptables

### Netfilter

**Netfilter** is a set of software hooks inside the Linux kernel that allow kernel modules to register callback functions from the network protocol stack.

**What it is:**
- A collection of software hooks inside the Linux kernel
- Allows kernel modules to register callback functions from the network protocol stack
- The foundation for building firewalls in Linux distributions

**Hook (hook):**
A software element that allows intercepting callback functions in other processes.

**Note:**
Netfilter is the foundation for building Firewall's in Linux distributions, but to make it work at full capacity, it needs to be configured. This is where **iptables** comes in—we can interact with Netfilter hooks and create rules for filtering, routing, modification, and translation of packets. Sometimes people forget about Netfilter and just call this combination "iptables."

---

### iptables

**iptables** is the user-space tool that interacts with Netfilter hooks to configure firewall rules.

**Check version:**
```bash
iptables -V
```

---

## Basic Concepts

### Stateless vs Stateful Filtering

**Stateless:**
Filtering based on checking static parameters of a single packet, for example: source and destination IP address, port, and other non-changing parameters.

**Stateful:**
Filtering based on analyzing traffic flows. With it, you can determine parameters of an entire TCP session or UDP stream.

---

### What iptables Can Do

iptables can:

- Configure stateless and stateful packet filtering for IPv4 and IPv6
- Configure all types of [IP address and port translation](https://wiki.merionet.ru/seti/13/nat-na-palcax-chto-eto/) (NAT, PAT, NAPT)
- Configure [QoS](https://wiki.merionet.ru/ip-telephoniya/qos-networks/) policies
- Perform various packet manipulations, for example—change fields in the IP header

---

### Rule Structure

Each rule in iptables consists of:

- **Критерий (Criterion)**: The condition that packet parameters or current connection must match for the action to trigger. In our example—this is the presence of a packet on the incoming interface establishing a connection on port 22
- **Действие (Action)**: The operation to perform on the packet or connection if the criterion is met. In our case—block the packet on port 22
- **Счётчик (Counter)**: An entity that counts how many packets were subjected to the action and shows their volume in bytes

---

## Tables

**Tables** are collections of basic and custom chains. Depending on which table a chain of rules is in, certain actions are performed on the packet or connection.

There are **5 tables**:

---

### 1. filter

**Purpose:**
Table that performs packet filtering by certain parameters. In most cases, you will use this one.

**Contains chains:**
- `FORWARD`
- `INPUT`
- `OUTPUT`

**Use Case:**
Most common table for basic firewall rules—allowing or blocking traffic.

---

### 2. raw

**Purpose:**
To understand the purpose of this table, you need to understand the logic of a stateful firewall. By default, iptables considers each packet as part of a large flow and can determine which connection a packet belongs to. The raw table configures exceptions that will consider the packet as a separate, unbound entity.

**Contains chains:**
- `INPUT`
- `OUTPUT`

**Use Case:**
Marking packets to bypass connection tracking, useful for high-performance scenarios.

---

### 3. nat

**Purpose:**
Table entirely dedicated to Network Address Translation (NAT) functionality.

**Contains chains:**
- `PREROUTING`
- `OUTPUT`
- `POSTROUTING`

**Use Case:**
Configuring NAT, port forwarding, and masquerading.

---

### 4. mangle

**Purpose:**
Table for modifying various packet headers. You can, for example, change TTL, number of hops, and more.

**Contains chains:**
- `PREROUTING`
- `INPUT`
- `FORWARD`
- `OUTPUT`
- `POSTROUTING`

**Use Case:**
Modifying packet headers, setting packet marks, changing TTL.

---

### 5. security

**Purpose:**
Used to assign certain labels to packets or connections that SELinux can later interpret.

**Use Case:**
SELinux integration and security labeling.

---

## Chains

**Chains** are ordered lists of rules. Rules are organized into chains.

There are **basic chains** and **custom chains**.

**Basic chains** are preset rule sets that exist in iptables by default.

There are **5 basic chains**, distinguished by the purpose of the packet. Basic chain names are written in uppercase.

---

### Basic Chains

1. **PREROUTING**
   - Rules in this chain apply to all packets arriving on the network interface from outside
   - Applied before routing decision

2. **INPUT**
   - Applied to packets destined for the host itself or for a local process running on this host
   - Not transit packets

3. **FORWARD**
   - Rules applied to transit packets passing through the host without stopping

4. **OUTPUT**
   - Applied to packets generated by the host itself

5. **POSTROUTING**
   - Applied to packets that should leave the network interface

---

### Chain Processing Order

```
Incoming Packet
    ↓
PREROUTING (raw, mangle, nat)
    ↓
Routing Decision
    ├─→ INPUT (mangle, filter) → Local Process
    ├─→ FORWARD (mangle, filter) → Transit
    └─→ OUTPUT (raw, mangle, nat, filter)
    ↓
POSTROUTING (mangle, nat)
    ↓
Outgoing Packet
```

---

## Targets (Actions)

**Targets** are actions to take when a packet matches a rule.

Common targets:

- **ACCEPT**: Allow the packet to pass
- **DROP**: Silently discard the packet without reporting the reason
- **QUEUE**: Sends the packet outside iptables logic to a third-party application. This may be needed when you need to process a packet in another process or program
- **RETURN**: Stop processing the rule and return to one rule back. This action is similar to `break` in programming languages
- **REJECT**: Discards the packet and returns the reason as an error, for example: `icmp unreachable`
- **LOG**: Simply makes a log entry if the packet matches the rule criteria

---

## Common Commands

### Viewing Rules

```bash
# List all rules
iptables --list

# List rules with line numbers
iptables -L -n --line-numbers

# List rules with verbose output
iptables -L -n -v

# List rules in a specific table
iptables -t nat -L -n -v
```

---

### Adding Rules

```bash
# Add rule to INPUT chain
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Add rule to beginning of chain
iptables -I INPUT 1 -p tcp --dport 22 -j ACCEPT

# Add rule with source IP
iptables -A INPUT -p tcp -s 192.168.2.2 --dport 80 -j DROP
```

**Command breakdown:**
- `iptables`: Call the utility
- `-A`: Add rule to existing chain
- `INPUT`: Specify the chain to add the rule to
- `-p tcp`: Explicitly specify TCP protocol
- `-s 192.168.2.2`: Source IP address
- `--dport 80`: Destination port
- `-j DROP`: Action to take (DROP)

---

### Deleting Rules

```bash
# Delete rule by matching criteria
iptables -D INPUT -p tcp --dport 80 -j DROP

# Delete rule by line number
iptables -D INPUT 1

# Delete all rules in a chain
iptables -F INPUT

# Delete all rules in all chains
iptables -F
```

---

### Saving and Restoring Rules

**Note:** iptables rules are not persistent by default. They are lost on reboot unless saved.

```bash
# Save current rules (Debian/Ubuntu)
sudo netfilter-persistent save

# Or older alias
sudo iptables-persistent save

# Reload saved rules
sudo netfilter-persistent reload

# Manual save to file
iptables-save > /etc/iptables/rules.v4

# Restore from file
iptables-restore < /etc/iptables/rules.v4
```

---

### Service Management

```bash
# Restart iptables service (if available)
/etc/init.d/iptables restart

# Stop iptables
/etc/init.d/iptables stop

# Start iptables
/etc/init.d/iptables start

# Restart networking (may reload iptables)
/etc/init.d/networking restart

# Restart firewall
/etc/init.d/firewall restart
```

---

### UFW (Uncomplicated Firewall)

**UFW** is a frontend for managing iptables, common on Ubuntu.

```bash
# Start UFW
service ufw start

# Stop UFW
service ufw stop

# Enable firewall
ufw enable

# Allow SSH
ufw allow 22/tcp

# Show status
ufw status verbose
```

---

## Examples

### Example 1: Block Traffic from Specific IP

```bash
iptables -A INPUT -p tcp -s 192.168.2.2 --dport 80 -j DROP
```

**Explanation:**
- `-A INPUT`: Add to INPUT chain
- `-p tcp`: Protocol TCP
- `-s 192.168.2.2`: Source IP address
- `--dport 80`: Destination port 80 (HTTP)
- `-j DROP`: Action—silently drop the packet

**To remove this rule:**
```bash
iptables -D INPUT -p tcp -s 192.168.2.2 --dport 80 -j DROP
```

---

### Example 2: Allow SSH

```bash
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

**Explanation:**
- Allows incoming TCP connections on port 22 (SSH)

---

### Example 3: NAT/Masquerading

```bash
# Enable IP forwarding
sysctl net.ipv4.ip_forward=1

# Masquerade traffic from VPN subnet
iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth0 -j MASQUERADE

# Allow forwarding
iptables -A FORWARD -i wg0 -j ACCEPT
iptables -A FORWARD -o wg0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

**Explanation:**
- `-t nat`: Use NAT table
- `-A POSTROUTING`: Add to POSTROUTING chain
- `-s 10.0.0.0/24`: Source network (VPN subnet)
- `-o eth0`: Outgoing interface
- `-j MASQUERADE`: Masquerade (NAT) action

---

### Example 4: Port Forwarding

```bash
# Forward port 8080 to internal server
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80
iptables -A FORWARD -p tcp -d 192.168.1.100 --dport 80 -j ACCEPT
```

---

### Example 5: Block All, Allow Specific

```bash
# Set default policy to DROP
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Allow loopback
iptables -A INPUT -i lo -j ACCEPT

# Allow established connections
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

---

## Resources

- [Погружение в iptables: теория и настройка](https://wiki.merionet.ru/servernye-resheniya/14/pogruzhenie-v-iptables-teoriya-i-nastrojka/)
- [Bandwidth Monitoring with iptables](https://www.linux.com/training-tutorials/bandwidth-monitoring-iptables/)

---

## Summary

**Key Concepts:**
- **Netfilter**: Kernel hooks for packet processing
- **iptables**: User-space tool to configure Netfilter
- **Tables**: filter, nat, mangle, raw, security
- **Chains**: PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING
- **Targets**: ACCEPT, DROP, REJECT, LOG, QUEUE, RETURN

**Common Operations:**
- View rules: `iptables -L -n -v`
- Add rules: `iptables -A CHAIN [criteria] -j TARGET`
- Delete rules: `iptables -D CHAIN [rule]`
- Save rules: `netfilter-persistent save`

**Best Practices:**
- Always test rules before applying to production
- Save rules to make them persistent across reboots
- Use UFW for simpler firewall management on Ubuntu
- Document your rules for easier maintenance
- Test connectivity after applying rules

**Important Notes:**
- Rules are processed in order—first match wins
- Default policies apply if no rule matches
- Rules are not persistent by default—must be saved
- Use `-n` flag to avoid DNS lookups (faster)
- Use `-v` flag for verbose output (shows counters)

Understanding iptables is essential for Linux system administration and network security.
