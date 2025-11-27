---
title: "Disk Management and NFS in Linux"
seoTitle: "Linux Disk Management & NFS Guide"
datePublished: Thu Nov 27 2025 15:06:08 GMT+0000 (Coordinated Universal Time)
cuid: cmihkga1q000602jve1nvbw0j
slug: disk-management-and-nfs-in-linux
tags: linux, javascript, devops, jobs

---

# (Complete Practical Guide)

Linux treats storage differently compared to operating systems like Windows. Instead of giving drive letters such as C:, D:, or E:, Linux organizes everything under a single directory tree that begins at the root directory `/`. Understanding how Linux handles disks, partitions, filesystems, mounting, and network file sharing is essential for system administration, server deployments, and DevOps work.

This guide explains disk management step by step and then walks through setting up NFS for network based storage sharing.

---

## 1\. What is Storage

Storage refers to saving data permanently so it remains available even after the system restarts.

Examples:

* RAM is temporary and clears after reboot
    
* Hard Disk or SSD provides persistent storage
    

A useful comparison is imagining a library.

* Bookshelves represent disks
    
* Shelves represent partitions
    
* Labels and arrangement represent the filesystem
    

In Linux, everything including files, folders, and devices exists inside a single directory structure starting from `/`.

---

## 2\. Storage Layers in Linux

Linux storage can be thought of in three logical layers.

### Layer 1: Raw Disk or Partition

Example: `/dev/sdb`

This is pure storage made of 0s and 1s. There is no structure and no way for the operating system to track where data begins or ends. Similar to a blank notebook with empty pages.

### Layer 2: Filesystem

Example created with: `mkfs.ext4 /dev/sdb1`

A filesystem defines rules for storing data, tracking metadata, permissions, free space, and how the OS retrieves stored information. Examples include ext4, xfs, and btrfs.

This is like dividing the notebook into chapters and adding an index.

### Layer 3: Mount Point

Example: `/mnt/data`

This is the location where the filesystem becomes accessible to the OS and users.

Without a mount point, even a formatted filesystem remains unreachable. It is similar to placing the organized notebook on a shelf where others can find it.

---

## Why the Filesystem Is Required Before Mounting

Without a filesystem, you can technically mount the device, but Linux cannot understand or store files on it. Creating a filesystem ensures:

* Files can be created and read
    
* The OS can track where file data is stored
    
* The disk structure is organized logically
    

---

## Quick Working Example

```bash
# See raw disk
lsblk

# Create partition
sudo fdisk /dev/sdb

# Format with ext4 filesystem
sudo mkfs.ext4 /dev/sdb1

# Create mount directory
sudo mkdir /mnt/data

# Mount it
sudo mount /dev/sdb1 /mnt/data

# Test it
cd /mnt/data
touch file1.txt
```

To unmount:

```bash
sudo umount /mnt/data
```

Unmounting ensures cached writes are saved and prevents corruption.

---

## How Linux Detects Storage

Linux does not use drive letters. Instead, all storage devices appear under the `/dev/` directory.

Examples:

* `/dev/sda` first disk
    
* `/dev/sdb` second disk
    
* `/dev/sda1` first partition on first disk
    

You can check devices using:

```bash
lsblk
```

Partitions allow organizing data, similar to how Windows shows C: and D:.

---

## Creating a Partition

```bash
sudo fdisk /dev/sdb
```

Inside fdisk:

* `n` create new partition
    
* `p` primary
    
* Press Enter to accept defaults
    
* `w` write changes
    

This results in `/dev/sdb1`.

---

## Formatting the Partition

Formatting prepares the partition with a filesystem.

Example:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Internally this creates:

* Superblock
    
* Inode tables for metadata
    
* Journal area for crash recovery
    

---

## Mounting and Using the Partition

```bash
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
df -h
cd /mnt/data
touch testfile.txt
```

Unmount when finished:

```bash
sudo umount /mnt/data
```

---

# Part 2: NFS in Linux

NFS stands for Network File System. It allows machines on the same network to share directories and access them as if they were local.

You can think of NFS as a private Google Drive hosted inside your infrastructure.

---

## Key Concepts in NFS

* **NFS Server** hosts the shared directory
    
* **NFS Client** connects and mounts the shared folder
    
* **Exports File** `/etc/exports` controls what is shared and with what access
    
* Uses RPC protocol over TCP or UDP using port 2049
    

---

## Practical NFS Setup Using Two Ubuntu Systems

Assume:

* NFS Server IP: 192.168.1.10
    
* Client IP: 192.168.1.11
    

### Step 1: Install Required Packages

On server:

```bash
sudo apt update
sudo apt install nfs-kernel-server -y
```

On client:

```bash
sudo apt update
sudo apt install nfs-common -y
```

---

### Step 2: Create Shared Directory on Server

```bash
sudo mkdir -p /srv/nfs_share
sudo chown nobody:nogroup /srv/nfs_share
sudo chmod 777 /srv/nfs_share
```

---

### Step 3: Configure Export Rules

Edit:

```bash
sudo nano /etc/exports
```

Add:

```plaintext
/srv/nfs_share 192.168.1.11(rw,sync,no_subtree_check)
```

Apply and restart:

```bash
sudo exportfs -ra
sudo systemctl restart nfs-kernel-server
```

---

### Step 4: Mount Share on Client

```bash
sudo mkdir -p /mnt/nfs_client
sudo mount 192.168.1.10:/srv/nfs_share /mnt/nfs_client
```

Verify:

```bash
df -h
ls /mnt/nfs_client
```

---

### Step 5: Persistent Mounting

Add to client `/etc/fstab`:

```plaintext
192.168.1.10:/srv/nfs_share  /mnt/nfs_client  nfs  defaults  0  0
```

---

## Security Best Practices

* Do not use `nobody:nogroup` in production
    
* Use dedicated users and groups
    
* Restrict by subnet
    
* Enable firewall rules
    
* Use Kerberos authentication for secure NFSv4 environments
    

---

## Final Summary

* A raw disk must be partitioned, formatted, and mounted before use
    
* Linux storage uses `/dev/` entries and no drive letters
    
* NFS enables file sharing across machines as if the storage were local
    
* Proper configuration, permissions, and security are essential in real deployments
    

---