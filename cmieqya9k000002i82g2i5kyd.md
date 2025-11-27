---
title: "Linux Networking"
seoTitle: "Master Linux Networking Essentials"
seoDescription: "Learn Linux networking basics: OSI layers, IP addressing, subnetting, and essential commands with examples for network control"
datePublished: Tue Nov 25 2025 15:44:48 GMT+0000 (Coordinated Universal Time)
cuid: cmieqya9k000002i82g2i5kyd
slug: linux-networking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764256119397/6fecdecf-fbd5-4064-b79f-26ae8cec99b0.png
tags: linux, computer-science, devops, frontend-development, ethereum

---

# **Linux Networking Explained: OSI Layers, IP Addressing, Subnetting and Essential Commands**

Linux networking can feel overwhelming at first, but once you understand the core building blocks such as OSI layers, IP addressing, subnetting, and essential commands, you gain full control over how your system communicates on a network.

This guide explains everything clearly with short explanations, diagrams, and real Linux examples.

---

## **1\. The OSI Model 7 Layers of Networking**

The OSI (Open Systems Interconnection) model describes how data moves across networks through seven layers, each with its own responsibility.

### **OSI Layers Overview**

| Layer | Name | Purpose |
| --- | --- | --- |
| 7 | Application | Interfaces for applications like HTTP, FTP, SMTP |
| 6 | Presentation | Encryption, compression, data formatting |
| 5 | Session | Manages sessions between devices |
| 4 | Transport | Reliable or fast delivery using TCP or UDP |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | Frames, MAC addressing |
| 1 | Physical | Transmission of bits on cable or wireless |

---

### **Layer-by-Layer Explanation**

#### **Layer 1 Physical Layer**

Handles transmission of bits over physical media.  
Examples: Ethernet cables, fiber optics, radio waves.

#### **Layer 2 Data Link Layer**

Creates frames, manages MAC addresses, performs error detection.  
Protocols: Ethernet, Wi-Fi  
Devices: Switches, Bridges

#### **Layer 3 Network Layer**

Handles IP addressing and routing using routers.  
Protocols: IP, ICMP

#### **Layer 4 Transport Layer**

Controls reliability, segmentation, and ports using TCP or UDP.

#### **Layer 5 Session Layer**

Creates and manages sessions between devices or applications.

#### **Layer 6 Presentation Layer**

Handles encryption, compression, and format conversion.

#### **Layer 7 Application Layer**

Provides services like web browsing, file transfer, and email.  
Examples: HTTP, FTP, SMTP, DNS

---

### **How Data Flows When You Browse a Website**

1. Application: Browser sends an HTTP request.
    
2. Presentation: Data is encrypted or compressed.
    
3. Session: Communication session begins.
    
4. Transport: TCP adds port numbers.
    
5. Network: IP decides the destination address.
    
6. Data Link: Frames are created.
    
7. Physical: Bits are transmitted over cable or Wi-Fi.
    

---

## **2\. IP Addressing IPv4 and IPv6**

### **IPv4**

* 32 bits
    
* Written in dotted decimal  
    Example: `192.168.1.10`
    

Binary form:  
`11000000.10101000.00000001.00001010`

#### **IPv4 Classes**

| Class | Range | Default Mask |
| --- | --- | --- |
| A | 1.0.0.0 to 126.255.255.255 | /8 |
| B | 128.0.0.0 to 191.255.255.255 | /16 |
| C | 192.0.0.0 to 223.255.255.255 | /24 |

Example for `192.168.1.0/24`  
Usable range: 192.168.1.1 to 192.168.1.254

---

### **IPv6**

* 128 bits
    
* Written in hexadecimal  
    Example:  
    `2001:db8:85a3::8a2e:370:7334`
    

Provides a much larger address space.

---

## **3\. Subnetting and CIDR**

Subnetting divides a network into smaller networks.

### **Example Subnetting 192.168.1.0/24 Into 4 Subnets**

Use mask `/26` (64 IPs each).

| Subnet | Range | Usable IPs |
| --- | --- | --- |
| 1 | 192.168.1.0 to 63 | 192.168.1.1 to 62 |
| 2 | 192.168.1.64 to 127 | 192.168.1.65 to 126 |
| 3 | 192.168.1.128 to 191 | 192.168.1.129 to 190 |
| 4 | 192.168.1.192 to 255 | 192.168.1.193 to 254 |

---

### **CIDR Calculation Formula**

Total IPs = 2^(32 minus CIDR)

Examples:

* `/24` gives 256 IPs
    
* `/26` gives 64 IPs
    

---

### **Subnetting a Class B Network Example**

Network: `172.16.0.0/16`  
Divide into `/20` subnets (4096 IPs each)

| Subnet | Network | Range |
| --- | --- | --- |
| 1 | 172.16.0.0/20 | 172.16.0.1 to 172.16.15.254 |
| 2 | 172.16.16.0/20 | 172.16.16.1 to 172.16.31.254 |
| 3 | 172.16.32.0/20 | 172.16.32.1 to 172.16.47.254 |

---

## **4\. Configuring IP Addresses in Linux**

### **Static IP (Debian Ubuntu)**

Edit `/etc/network/interfaces`:

```plaintext
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
```

### **Static IP (RedHat CentOS)**

File: `/etc/sysconfig/network-scripts/ifcfg-eth0`

```plaintext
DEVICE=eth0
BOOTPROTO=static
IPADDR=192.168.1.10
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
ONBOOT=yes
```

### **Dynamic IP with DHCP**

```plaintext
sudo dhclient eth0
```

### **Assign IP with ip Command**

```plaintext
sudo ip addr add 192.168.1.10/24 dev eth0
```

### **View IP**

```plaintext
ip addr show eth0
```

---

## **5\. What Is eth0**

`eth0` is the name of the system's first wired Ethernet network interface.

* `eth` stands for Ethernet
    
* `0` indicates that it is the first interface
    

Example names: eth0, eth1, wlan0

---

## **6\. Essential Linux Networking Commands**

### **1\. ip**

View interfaces and IPs

```plaintext
ip addr show
```

### **2\. ifconfig** (older tool)

```plaintext
sudo ifconfig eth0 up
```

### **3\. ping**

```plaintext
ping google.com
```

### **4\. traceroute**

```plaintext
traceroute google.com
```

### **5\. netstat**

```plaintext
netstat -tuln
```

### **6\. ss**

```plaintext
ss -tuln
```

### **7\. route**

```plaintext
route -n
```

### **8\. dig**

```plaintext
dig google.com
```

### **9\. iptables**

```plaintext
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### **10\. hostname**

```plaintext
hostname
```

### **11\. nmcli**

```plaintext
nmcli con show
```

### **12\. tcpdump**

```plaintext
sudo tcpdump -i eth0
```

---

## **Conclusion**

This guide gives you a strong foundation in Linux networking. With OSI layers, IP addressing, subnetting, and essential commands, you can configure networks, solve connectivity issues, and understand how data travels across systems.