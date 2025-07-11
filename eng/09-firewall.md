# Introduction

In this chapter, we will explore **firewall management** in Linux, an essential tool for securing a system by controlling incoming and outgoing network traffic. Firewalls protect your system from unauthorized access and manage data flows based on predefined rules. We will cover basic concepts, common tools like `iptables`, `ufw`, and `firewalld`, and provide practical examples for configuring a firewall.

## Prerequisites

Same old story. 😉

- Have a virtual machine, PC, or Linux environment (ideally Ubuntu).
- Be resilient XD.

**Info**: If you don’t have a Linux environment, sign up at https://killercoda.com and access https://killercoda.com/playgrounds/scenario/ubuntu to get a free, renewable Ubuntu 24.04 virtual machine (without GUI) for 1 hour.

# Firewalls in Linux

## What is a Firewall?

A **firewall** is a software or hardware tool that acts as a barrier between your system and the network. It filters network traffic (incoming and outgoing) based on **rules**, allowing or blocking connections based on criteria like IP address, port, or protocol.

### Types of Firewalls

| Type                | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **Hardware Firewall** | A dedicated physical device (e.g., a router with a built-in firewall).     |
| **Software Firewall** | Software installed on a system (e.g., `iptables`, `ufw`, `firewalld`).     |

In Linux, we primarily focus on **software firewalls**, configured directly on the operating system.

## Firewall Management Tools in Linux

### 1. `iptables`

**`iptables`** is a low-level tool for managing firewall rules in Linux. It interacts directly with the Linux kernel (Netfilter) to define filtering and NAT (Network Address Translation) rules.

- **Advantages**: Very powerful, granular, and flexible.
- **Disadvantages**: Complex for beginners, requires good network understanding.

**Example command**:
```bash
# Allow SSH connections (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Block all incoming traffic except explicitly allowed
sudo iptables -P INPUT DROP
```

**Save the rules**:
```bash
sudo iptables-save > /etc/iptables/rules.v4
```

**Restore the rules**:
```bash
sudo iptables-restore < /etc/iptables/rules.v4
```

### 2. `ufw` (Uncomplicated Firewall)

**`ufw`** is a simplified interface for managing `iptables`. It’s particularly suitable for beginners and Debian/Ubuntu systems.

- **Advantages**: Easy to use, intuitive syntax.
- **Disadvantages**: Less flexible than `iptables` for complex configurations.

**Example commands**:
```bash
# Enable ufw
sudo ufw enable

# Allow port 22 (SSH)
sudo ufw allow 22

# Allow port 80 (HTTP)
sudo ufw allow 80/tcp

# Check firewall status
sudo ufw status

# Disable ufw
sudo ufw disable
```

### 3. `firewalld`

**`firewalld`** is a dynamic firewall management solution, commonly used on Red Hat, CentOS, and Fedora. It uses **zones** to manage rules based on the network’s trust level.

- **Advantages**: Dynamic management (no need to restart), user-friendly interface.
- **Disadvantages**: More complex than `ufw`, less common on Debian/Ubuntu.

**Example commands**:
```bash
# Check firewalld status
sudo firewall-cmd --state

# Add a rule to allow port 80 (HTTP)
sudo firewall-cmd --permanent --add-port=80/tcp

# Apply changes
sudo firewall-cmd --reload

# List open ports
sudo firewall-cmd --list-ports
```

## Key Firewall Concepts

- **Rules**: Define what is allowed or blocked (e.g., port, protocol, IP address).
- **Default Policy**: Determines what happens if no rule matches (e.g., `ACCEPT` or `DROP`).
- **Chains** in `iptables`:
  - `INPUT`: Incoming traffic to the system.
  - `OUTPUT`: Outgoing traffic from the system.
  - `FORWARD`: Traffic passing through the system (for routers).
- **Zones** in `firewalld`: Groups of rules applied based on network context (e.g., `public`, `trusted`).

## Practical Example: Securing a Web Server

Imagine you’re setting up a web server (Apache on port 80) on an Ubuntu machine. Here’s how to secure it with `ufw`:

1. Enable `ufw`:
   ```bash
   sudo ufw enable
   ```

2. Allow SSH connections (to avoid losing access):
   ```bash
   sudo ufw allow 22/tcp
   ```

3. Allow HTTP connections:
   ```bash
   sudo ufw allow 80/tcp
   ```

4. Check the rules:
   ```bash
   sudo ufw status
   ```

5. Set a default policy to block everything else:
   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   ```

**Expected output**:
```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
```

## Practice ⚔️

### Exercise 1: Basic `ufw` Configuration

1. Enable `ufw` on your system:
   ```bash
   sudo ufw enable
   ```
2. Allow SSH connections (port 22):
   ```bash
   sudo ufw allow 22/tcp
   ```
3. Allow HTTP connections (port 80):
   ```bash
   sudo ufw allow 80/tcp
   ```
4. Check the firewall status:
   ```bash
   sudo ufw status
   ```
5. Test SSH access to your machine from another terminal or machine to confirm the rule works.
6. Temporarily disable `ufw` and verify that SSH access is still possible:
   ```bash
   sudo ufw disable
   ```

### Exercise 2: Advanced `iptables` Configuration

1. Display the current `iptables` rules:
   ```bash
   sudo iptables -L -v -n
   ```
2. Add a rule to allow incoming traffic on port 22 (SSH):
   ```bash
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   ```
3. Set a default policy to block all incoming traffic:
   ```bash
   sudo iptables -P INPUT DROP
   ```
4. Save the rules to a file:
   ```bash
   sudo iptables-save > /etc/iptables/rules.v4
   ```
5. Reboot your machine and verify that the rules are still applied:
   ```bash
   sudo iptables -L
   ```
6. Test SSH access to confirm the rule works.

### Exercise 3: Analyzing Firewall Logs

1. Enable logging for `ufw`:
   ```bash
   sudo ufw logging on
   ```
2. Attempt to access an unauthorized port (e.g., port 9999) from another machine or terminal:
   ```bash
   nc -zv 127.0.0.1 9999
   ```
3. Check the logs to see blocked connections:
   ```bash
   sudo cat /var/log/ufw.log
   ```
4. Identify the lines corresponding to your connection attempt and note the source IP, port, and action (e.g., `BLOCK`).


---
---

## Feedback

ENG: Please give us your feedback about this chapter.
FR: Faites-nous part de votre avis sur ce chapitre.

👉🏾 https://forms.gle/88jPmFLnNPtjdgqv8
