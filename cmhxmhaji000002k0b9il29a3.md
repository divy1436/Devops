---
title: "Ports, SSH, SSH Key Pair & Security Groups in Linux"
seoTitle: "Ports, SSH, SSH Key Pair & Security Groups in Linux"
seoDescription: "Understanding ports, SSH, SSH key pairs, and security groups is essential for DevOps and cloud security. These fundamentals help secure servers, manage acce"
datePublished: Thu Nov 13 2025 16:07:31 GMT+0000 (Coordinated Universal Time)
cuid: cmhxmhaji000002k0b9il29a3
slug: ports-ssh-ssh-key-pair-and-security-groups-in-linux
tags: linux, aws, cloud-computing, devops, ssh

---

# 🔌 **Ports in Linux**

A **port** is a communication endpoint that helps applications send and receive data over a network.  
Ports range from **0 to 65535**, and each is associated with specific services or protocols.

### 📌 **Common Ports**

| Service | Port Number |
| --- | --- |
| HTTP | **80** |
| HTTPS | **443** |
| SSH | **22** |
| FTP | **21** |

### 🔢 **Port Ranges**

| Range | Name | Purpose |
| --- | --- | --- |
| **0 – 1023** | Well-Known Ports | Reserved for system services |
| **1024 – 49151** | Registered Ports | Assigned to user applications |
| **49152 – 65535** | Dynamic/Private Ports | Temporary connections |

---

# 🔐 **SSH (Secure Shell)**

SSH is a **secure cryptographic protocol** used for:

✔ Remote login  
✔ File transfer  
✔ Secure data communication  
✔ Remote command execution

It runs on **Port 22** by default and uses encryption to protect data.

### ⭐ Key Features of SSH

* **Encrypted communication** – prevents sniffing
    
* **Public key authentication** – more secure than passwords
    
* **Port forwarding** – securely forward ports between systems
    

---

# 🗝️ **SSH Key Pair (Public & Private Key Authentication)**

SSH key pairs use **asymmetric cryptography**.

* **Private Key** → Stored locally, never shared
    
* **Public Key** → Placed on the server
    

SSH uses these keys to authenticate securely.

---

## 🛠️ **Generating an SSH Key Pair (Ubuntu)**

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

This creates:

* `~/.ssh/id_rsa` → Private Key
    
* `~/.ssh/id_`[`rsa.pub`](http://rsa.pub) → Public Key
    

You may set an optional **passphrase** for added security.

---

## 🔄 **Using SSH Keys for Authentication**

### **1\. Copy Public Key to Remote Server**

```bash
ssh-copy-id user@remote_host
```

Copies your public key to:

```plaintext
~/.ssh/authorized_keys
```

### **2\. Login Without Password**

```bash
ssh user@remote_host
```

---

# 💡 **Ubuntu Example**

```bash
# Generate key pair
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Copy public key to remote system
ssh-copy-id user@192.168.1.100

# Connect to server
ssh user@192.168.1.100
```

---

# 🛡️ **Security Groups (Cloud: AWS / Azure / GCP)**

Security groups act as **virtual firewalls** that control incoming and outgoing traffic to cloud servers.

### 🚪 **Inbound Rules**

Control what traffic is allowed **into** your instance.

Examples:

* Allow SSH (Port 22)
    
* Allow HTTP (Port 80)
    

### ➡️ **Outbound Rules**

Control what traffic is allowed **out** from your instance.

Example:

* Allow all outbound traffic
    

---

# 🧩 **Example Security Group Configuration**

### **Inbound Rules**

| Type | Port | Description |
| --- | --- | --- |
| SSH | 22 | Allow remote login |
| HTTP | 80 | Allow web traffic |

### **Outbound Rules**

| Type | Port | Description |
| --- | --- | --- |
| All traffic | All | Enable outgoing connections |

---

# 🧱 **Textual Diagram**

```plaintext
                +---------------------------+
                |     Security Group        |
                +---------------------------+
                |      Inbound Rules        |
                |  - SSH (22) from My IP    |
                |  - HTTP (80) from Anyone  |
                +---------------------------+
                |      Outbound Rules       |
                |  - All traffic allowed    |
                +---------------------------+
                          |
       ------------------------------------------------
                          |
                 +--------------------+
                 |   Linux Instance   |
                 +--------------------+
                 |  SSH (22) Enabled  |
                 |  Web (80) Enabled  |
                 +--------------------+
```

---

# 📘 **Detailed Explanation**

### 🔹 **Ports**

Ports allow multiple services to run simultaneously.  
Example:

* Web server: Port **80**
    
* SSH server: Port **22**
    

Both can work together without conflicts.

---

### 🔹 **SSH & SSH Key Pair**

SSH provides secure remote access.  
Public-private key cryptography removes the need for passwords and greatly reduces the chance of brute-force attacks.

---

### 🔹 **Security Groups**

They determine:

* Which traffic can reach your instance (**inbound**)
    
* Which traffic can leave your instance (**outbound**)
    

Example:  
Allow port 22 only from your IP → Safe SSH  
Allow port 80 to everyone → Public website

---

# 🏁 **Conclusion**

Understanding **ports, SSH, SSH key pairs, and security groups** is essential for DevOps and cloud security. These fundamentals help secure servers, manage access, and ensure smooth communication between systems.