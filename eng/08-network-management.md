# Introduction

In this section, we will cover the following topics:
* Computer networking (briefly)
* Network troubleshooting under Linux

## Prerequisites

Same old story. 😉

# Computer Networking (Briefly)

## Introduction to Computer Networking

A computer network is a set of computers and peripherals connected to share resources, exchange data, and provide services. Networks are essential in the modern world, enabling communication, file sharing, and access to remote resources.

### How does it all work?

For computers to communicate, **they must speak a common language**: these are **network protocols**. The most fundamental of all is **TCP/IP**.

---

### The TCP/IP Model (the real functioning of the Internet)

**TCP/IP** is **the set of rules (or "protocol stack") used to make the Internet and most networks work**. It consists of several layers, **each with a specific role**.

#### Layers of the TCP/IP Model (simplified):

| Layer              | Role                                                                        |
|--------------------|-----------------------------------------------------------------------------|
| **1. Network Access** | Manages communication with hardware (Wi-Fi, Ethernet cable, etc.)         |
| **2. Internet**    | Finds the IP address of a computer on the network (IP protocol)             |
| **3. Transport**   | Ensures reliability or speed of communication (TCP or UDP)                  |
| **4. Application** | What the user interacts with: HTTP, FTP, mail, etc.                         |

### TCP and UDP: Two Ways to Transport Data

At the **Transport layer**, two protocols are mainly used:

#### TCP (Transmission Control Protocol)

* **Reliable**: Ensures all data arrives **in the correct order**, without errors.
* **Connection-oriented**: Establishes a "link" between sender and receiver before sending data.
* **Examples of use**: Websites (HTTP/HTTPS), emails (SMTP, IMAP), files (FTP).

#### UDP (User Datagram Protocol)

* **Fast**: Sends data without checking if it’s received.
* **Connectionless**: No prior handshake, less overhead.
* **Examples of use**: Online games, audio/video calls, live streaming.

> **Metaphor**:
>
> * TCP is like sending a **package with delivery confirmation**.
> * UDP is like sending a **postcard with no guarantee it arrives**.

---

### The OSI Model (Theoretical 7-Layer Model)

The **OSI model** (*Open Systems Interconnection*) is a **theoretical representation** used to understand how data flows in a network, divided into **7 layers**.

| Layer | Name                  | Simplified Role                                      |
|-------|-----------------------|-----------------------------------------------------|
| 7     | **Application**       | What the user sees (browser, email, etc.)           |
| 6     | **Presentation**      | Encoding, encryption, compression                   |
| 5     | **Session**           | Manages connections between applications            |
| 4     | **Transport**         | Reliability and order of data (TCP/UDP)             |
| 3     | **Network**           | Addressing and routing (IP)                         |
| 2     | **Data Link**         | Communication between directly connected machines   |
| 1     | **Physical**          | Cables, signals, radio waves                        |

> **Mnemonic tip** to remember the order:
> "All People Seem To Need Data Processing" (Application, Presentation, Session, Transport, Network, Data Link, Physical)

### OSI vs TCP/IP — What’s the difference?

| OSI (7 layers)           | TCP/IP (4 layers)                                                     |
|--------------------------|----------------------------------------------------------------------|
| **Theoretical** model    | **Practical** model, used on the Internet                            |
| More **detailed**        | More **practical and implemented**                                   |
| Separates functions clearly | Merges some layers (e.g., Application + Presentation + Session)     |

> In practice, **networks use TCP/IP**, but **OSI helps understand** what happens at each step.

## Types of Computer Networks

You should know there are different types of networks:

- **Local Area Network (LAN)**: A LAN connects computers in a limited geographical area, such as a home, building, or campus.
- **Wide Area Network (WAN)**: A WAN covers a larger geographical area, connecting multiple LANs across cities, countries, or continents.
- **Wireless LAN (WLAN)**: A WLAN connects computers wirelessly, using radio waves to transmit data.

## Network Equipment

Now, let’s talk about network equipment:

- **Router**: A router connects multiple networks and routes traffic between them. It’s like a traffic controller for your data packets!
- **Switch**: A switch connects multiple computers within a network and directs data packets to the intended recipient.

## Network Addresses

In a computer network, two main elements identify a machine: the IP address and the MAC address.

- **IP Address**: A unique identifier assigned to a computer in a network, used for communication. It can be static or dynamic.  
  Example: **192.168.34.3**.

- **MAC Address**: A unique identifier assigned to a network interface card, used as a network address.  
  Example: **00:1A:2B:3C:4D:5E**.

<br>

**To try 👨🏾‍💻👩🏾‍💻:**
- Open your terminal
- Run:
    ```bash
    ip a ## Displays your system’s network interfaces along with their IP addresses and MAC addresses
    ```
<br>

Below is an example output.

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:c8:9a:ac brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic noprefixroute enp1s0
       valid_lft 86302846sec preferred_lft 75513646sec
    inet6 fe80::7008:be3c:4abc:6bcd/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 qdisc noqueue state DOWN group default 
    link/ether 02:42:4c:21:02:63 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

In our case, we have 3 network interfaces: **lo**, **enp1s0**, and **docker0**.

| Interface | Type      | IPv4 Address  | IPv6 Address              | MAC Address       | Status            |
|-----------|-----------|---------------|---------------------------|-------------------|-------------------|
| lo        | Loopback  | 127.0.0.1/8   | ::1/128                   | 00:00:00:00:00:00 | UP                |
| enp1s0    | Ethernet  | 172.30.1.2/24 | fe80::.../64 (link-local) | 52:54:00:c8:9a:ac | UP                |
| docker0   | Virtual   | 172.17.0.1/16 | (no IPv6 here)            | 02:42:4c:21:02:63 | DOWN (no carrier) |

- **lo**: The loopback interface, used for the machine to communicate with itself.
- **enp1s0**: A physical Ethernet network interface.
- **docker0**: A virtual interface created by Docker to allow containers to communicate with each other and the host.

## Focus on the Loopback Interface

The **loopback** interface (`lo`) allows a machine to **send messages to itself**, as if going through the network. This might seem unnecessary, but it’s actually fundamental for:

### **1. Local Network Testing (localhost)**

* **127.0.0.1** (or `localhost`) is used to **test network applications locally**, without needing a real network connection.
* Example: You’re developing a website on your PC. You can run a local server on `127.0.0.1:8000` and access it via a browser — it never leaves the machine.

### **2. Internal System Services**

* Many **system services communicate with each other via the network**, even if they run on the same machine (for modularity or security reasons).
* Example: A PostgreSQL database, a Redis server, etc. They often listen on `127.0.0.1`, allowing only local connections.

### **3. Network Diagnostics**

* You can test the **network stack** without needing a cable or Internet:

  ```bash
  ping 127.0.0.1
  ```

  This verifies that the operating system can send and receive TCP/IP packets.

### **4. Security**

* Services that **must not be accessible from outside** can be bound only to `127.0.0.1`.
* Example: An administration interface accessible **only locally**.

### Practical Example:

Imagine you’re developing a local web application:

```bash
python3 -m http.server 8000
```

You can access it from your browser via:

```
http://127.0.0.1:8000
```

Even if you have **no Internet access**, this communication works via the loopback interface.

### In Summary:

The `lo` interface allows a computer to **address itself** using standard network protocols. It’s **essential** for:

* Testing local apps,
* Running system services,
* Debugging,
* Isolating services from external access.

## Network Configuration and Routing in Linux

### Understanding **Routing**

In networking, **routing** means **deciding which path (interface, gateway) to send IP packets through** to reach a destination. Let’s look at some routing-related commands.

#### `route` (older command)

The `route` command is **deprecated** but sometimes still used to display the **routing table**:

```bash
route -n
```

> This shows the paths used to reach known networks.  
> The `-n` avoids DNS resolution for faster output.

#### `ip r` or `ip route` (modern)

```bash
ip route
# or shorter:
ip r
```

> Displays the current system routing table.

##### Example:

```bash
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
```

* **default**: Default route → everything unknown goes through here.
* **via 192.168.1.1**: The **gateway** used.
* **dev eth0**: Network interface used.
* **192.168.1.0/24**: Local subnet.

#### `ip route get`

```bash
ip route get 8.8.8.8
```

> Shows **the exact path** a packet would take to reach a given IP.

##### Example Output:

```bash
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
```

* This confirms which interface, source IP, and gateway will be used.

---

### Gateway

The **gateway** is **the IP address of a router** on the local network, through which **outgoing traffic to the outside** (e.g., the Internet) is routed.

* **It links your local network (LAN) to the outside (WAN/Internet).**
* It’s typically **your Internet router or modem**.

Without a defined gateway → the system **cannot reach the outside**.

### Configuring a Network Interface

#### Method 1 – **Temporary** (non-persistent)

Using the `ip` commands:

```bash
# Assign an IP to the eth0 interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Activate the interface
sudo ip link set eth0 up

# Add a gateway
sudo ip route add default via 192.168.1.1
```

> ⛔ This configuration is **temporary**: it disappears after a reboot or interface deactivation.

#### Method 2 – **Persistent** (retained after reboot)

Depends on the distribution:

#### Debian / Ubuntu (with Netplan or interfaces)

##### With `netplan` (Ubuntu 18.04+)

Edit the YAML file:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Example of static configuration:

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Then apply:

```bash
sudo netplan apply
```

##### With `/etc/network/interfaces` (older systems):

```bash
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8
```

Restart the network:

```bash
sudo systemctl restart networking
```

---

#### Red Hat / CentOS / Fedora (with `nmcli` or `ifcfg-` files)

Example file:

```bash
/etc/sysconfig/network-scripts/ifcfg-eth0
```

Content:

```ini
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

Restart the interface:

```bash
sudo ifdown eth0 && sudo ifup eth0
```

### Summary

| Command / Element             | Role                                           |
|------------------------------|-----------------------------------------------|
| `ip r` / `ip route`          | Displays the routing table                    |
| `ip route get <IP>`          | Shows the network path used                   |
| `route -n`                   | Older method to view the routing table        |
| `gateway`                    | Gateway to the outside (router)               |
| `ip addr add` / `ip link`    | **Temporary** interface configuration          |
| YAML files or `interfaces`   | **Persistent** configuration                  |

## DNS

The DNS (Domain Name System) translates domain names into IP addresses. It’s essential for browsing the Internet! You can modify your DNS in different ways, either via the `/etc/resolv.conf` file or through the Network Manager graphical interface.

- **Via `resolv.conf`**:
 ```bash
 # Edit the resolv.conf file
 sudo nano /etc/resolv.conf

 # Add the following line:
 nameserver 8.8.8.8
 ```

 Some explanations:
* `nameserver`: A directive used to specify a **DNS server** (Domain Name System).
* `8.8.8.8`: The **IP address of the DNS server** the system will use to **translate domain names** (e.g., `www.google.com`) into IP addresses (e.g., `142.250.190.68`).

<br>

**⚠️ Warning**:

In some modern distributions (Ubuntu, Fedora…), this file is automatically generated by services like systemd-resolved or NetworkManager. Any manual changes to `/etc/resolv.conf` may be overwritten on reboot.

<br>

Now let’s talk about the **/etc/hosts** file!!!

Before consulting a DNS server, Linux first checks the **/etc/hosts** file. This file allows manually mapping hostnames to IP addresses. It takes priority over DNS for local resolution.

**Example of an `/etc/hosts` file**:

```bash
127.0.0.1       localhost
127.0.1.1       mypc.local mypc
192.168.1.100   webserver.local mysite
```

Explanation:

* **127.0.0.1**: Corresponds to the machine itself (loopback).
* **localhost**: Default hostname.
* **192.168.1.100 webserver.local**: Allows accessing this server (webserver.local) without DNS.

**Use cases for `/etc/hosts`**

* **Test a website locally** before updating public DNS.
* **Force a domain name** to point to a specific IP.
* **Block a site** by redirecting it to `127.0.0.1`.
* **Work offline** without a DNS server.

## Role of Network Ports and Services

Ports are numbers that identify services listening on a machine. You can see standard port numbers and services in the `/etc/services` file.

- **Example**:
    ```bash
    cat /etc/services
    ```

    ```
    ...
    ftp-data	20/tcp
    ftp		    21/tcp
    fsp		    21/udp		fspd
    ssh		    22/tcp				# SSH Remote Login Protocol
    telnet		23/tcp
    smtp		25/tcp		mail
    ```

# Some Linux Network Troubleshooting Tools

## Ping

- **What is Ping?**: Ping tests connectivity and measures response time.
- **How to use Ping**:
 ```bash
 # Test connectivity with Google
 ping google.com
 ```

## Traceroute

- **What is Traceroute?**: Traceroute traces the path data packets take from source to destination.
- **How to use Traceroute**:
 ```bash
 # Trace the path to Google
 traceroute google.com
 ```

## Netstat

- **What is Netstat?**: Netstat displays active Internet/network connections, routing tables, and interface statistics.
- **How to use Netstat**:
 ```bash
 # View active connections
 netstat -tuln
 ```

## Tcpdump

- **What is Tcpdump?**: Tcpdump captures and displays network traffic.
- **How to use Tcpdump**:
 ```bash
 # Capture traffic on the eth0 interface
 sudo tcpdump -i eth0
 ```

## Wireshark

- **What is Wireshark?**: Wireshark analyzes network traffic.
- **How to use Wireshark**:
 ```bash
 # Launch Wireshark
 sudo wireshark
 ```

## Nmap

- **What is Nmap?**: Nmap scans networks for hosts and services.
- **How to use Nmap**:
 ```bash
 # Scan the 192.168.1.0/24 network
 nmap -sS 192.168.1.0/24
 ```

## Netcat

- **What is Netcat?**: Netcat reads and writes network connections.
- **How to use Netcat**:
 ```bash
 # Connect to Google on port 80
 nc google.com 80
 ```

### SS

- **What is SS?**: SS is a tool for investigating sockets.
- **How to use SS**:
 ```bash
 # View active sockets
 ss -tuln
 ```

### MTR

- **What is MTR?**: MTR combines Ping and Traceroute.
- **How to use MTR**:
 ```bash
 # Test connectivity and trace the path to Google
 mtr google.com
 ```

# Practice ⚔️

## EXERCISE 1

Complete challenges from level 0 to 20 in the "Bandit" category on the "overthewire" platform.  
Link: https://overthewire.org/wargames/bandit/ (This link points directly to the "Bandit" category)

The first challenges will serve as a review 🙂.

## EXERCISE 2

You must perform a complete network analysis on the public server scanme.nmap.org using the Nmap tool to answer the following questions:

1. What is the version of the SSH service (port 22) hosted on this server?
2. What service is available on port 80/tcp?
3. How many TCP ports are in the open state?
4. What service is associated with port 9929/tcp?

**⚠️ Warning**: It is illegal to scan a network or website you don’t own without permission. "scanme.nmap.org" is a test platform provided by Nmap to freely test the Nmap tool.

## EXERCISE 3

Link to the challenge script: https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/Network_EXO_1.sh

---
---

## Feedback

ENG: Please give us your feedback about this chapter.
FR: Faites-nous part de votre avis sur ce chapitre.

👉🏾 https://forms.gle/xnuAAfbBtxyGFz8h9
