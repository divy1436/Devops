---
title: "Real-Time Corporate Shell Scripts"
seoTitle: "Dynamic Corporate Shell Scripts"
seoDescription: "10 essential shell scripts for DevOps sysadmins: boost automation, reliability, performance with practical tools"
datePublished: Sat Nov 15 2025 18:21:45 GMT+0000 (Coordinated Universal Time)
cuid: cmi0m5lx0000102joadgb7m91
slug: real-time-corporate-shell-scripts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763231227191/5dbad218-db91-49e4-8e60-db8f11a57bb9.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1763230896167/001bf61c-1d83-496c-aac4-07bd3aaa8b57.png
tags: linux, developer, linux-for-beginners, shell-script

---

---

# 🚀 10 Real-Time Corporate Shell Scripts Every DevOps & SysAdmin Should Know

Shell scripting remains one of the most powerful skills for DevOps engineers, system administrators, and cloud professionals. Whether you're managing servers, automating daily tasks, or improving system reliability—shell scripts make your life easier.

In this blog, I’ll share **10 real-time corporate production-ready shell scripts**, along with complete explanations. These scripts are practical, commonly used in companies, and easy to adapt to your own infrastructure.

Let’s dive in! ⚡

---

# 🗂️ 1. Backup Automation Script

### **📌 Script**

```bash
#!/bin/bash
SOURCE="/home/ubuntu/aditya"
DESTINATION="/home/ubuntu/jaiswal/"
DATE=$(date +%Y-%m-%d_%H-%M-%S)

mkdir -p $DESTINATION/$DATE
cp -r $SOURCE $DESTINATION/$DATE
echo "Backup completed on $DATE"
```

### **🧠 Explanation**

* `SOURCE`: Directory to backup
    
* `DESTINATION`: Storage location
    
* `DATE`: Timestamp for versioning
    
* `mkdir -p`: Creates directory if missing
    
* `cp -r`: Copies files recursively
    

### **⏱️ Automate with Cron**

```plaintext
* * * * * /path/to/backup.sh
```

(Runs every minute — adjust as needed)

---

# 💽 2. Disk Usage Monitoring Script

### **📌 Script**

```bash
#!/bin/bash
THRESHOLD=80
df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{ print $5 " " $1 }' |
while read output; do
  usage=$(echo $output | awk '{ print $1}' | cut -d'%' -f1)
  partition=$(echo $output | awk '{ print $2 }')
  if [ $usage -ge $THRESHOLD ]; then
    echo "Warning: Disk usage on $partition is at ${usage}%"
  fi
done
```

### **🧠 Why This Is Useful**

* Helps avoid disk-full outages
    
* Notifies when usage &gt; **80%**
    
* Ideal for **cron-based server health checks**
    

---

# 🛠️ 3. Service Health Check (Auto Restart)

### **📌 Script**

```bash
#!/bin/bash
SERVICE="nginx"

if systemctl is-active --quiet $SERVICE; then
  echo "$SERVICE is running"
else
  echo "$SERVICE is not running"
  systemctl start $SERVICE
fi
```

### **🧠 Explanation**

* Checks service status
    
* Automatically restarts if not running
    
* Useful for **nginx**, **mysql**, **apache**, etc.
    
* Helps keep production uptime high
    

---

# 🌐 4. Network Connectivity Test

### **📌 Script**

```bash
#!/bin/bash
HOST="google.com"
OUTPUT_FILE="/home/ubuntu/output.txt"

if ping -c 1 $HOST &> /dev/null; then
  echo "$HOST is reachable" >> $OUTPUT_FILE
else
  echo "$HOST is not reachable" >> $OUTPUT_FILE
fi
```

### **🧠 Use Case**

* Logs connectivity issues
    
* Good for debugging networking or internet outages
    

---

# 🛢️ 5. MySQL Database Backup Script

### **📌 Script**

```bash
#!/bin/bash
DB_NAME="mydatabase"
BACKUP_DIR="/path/to/backup"
DATE=$(date +%Y-%m-%d_%H-%M-%S)

mysqldump -u root -p $DB_NAME > $BACKUP_DIR/$DB_NAME-$DATE.sql
echo "Database backup completed: $BACKUP_DIR/$DB_NAME-$DATE.sql"
```

### **🧠 Why It's Important**

* Creates timestamped database backups
    
* Works across MySQL-based servers
    
* Ideal for daily cron backups
    

---

# ⏱️ 6. System Uptime Script

### **📌 Script**

```bash
#!/bin/bash
uptime -p
```

### **🧠 What It Does**

* Shows how long server has been running
    
* Helps monitor reboots, downtime
    

---

# 📡 7. Active Listening Ports Checker

### **📌 Script**

```bash
#!/bin/bash
netstat -tuln | grep LISTEN
```

### **🧠 Usage**

* Lists active services and ports
    
* Important for troubleshooting **networking issues**, **traffic**, **open ports**
    

---

# ♻️ 8. Automatic System Update Script

### **📌 Script**

```bash
#!/bin/bash
apt-get update && apt-get upgrade -y && apt-get autoremove -y && apt-get clean
echo "System packages updated and cleaned up"
```

### **🧠 What It Does**

* Updates server
    
* Removes unused packages
    
* Keeps system optimized and secure
    

---

# 🌐 9. HTTP Response Time Checker

### **📌 Script**

```bash
#!/bin/bash
URLS=("https://www.devopsshack.com/" "https://www.linkedin.com/")

for URL in "${URLS[@]}"; do
  RESPONSE_TIME=$(curl -o /dev/null -s -w '%{time_total}\n' $URL)
  echo "Response time for $URL: $RESPONSE_TIME seconds"
done
```

### **🧠 Use Cases**

* Monitor website performance
    
* Detect slow or down services
    
* Ideal for DevOps monitoring pipelines
    

---

# 🧠 10. Top Memory-Consuming Processes

### **📌 Script**

```bash
#!/bin/bash
ps aux --sort=-%mem | head -n 10
```

### **🧠 Why This Helps**

* Identifies heavy processes
    
* Useful for fixing performance bottlenecks
    
* Often used during server audits
    

---

# 🎯 Final Thoughts

These 10 real-time scripts are **practical**, **production-ready**, and commonly used in DevOps, SRE, and system administration roles. Whether you’re:

✔ Automating backups  
✔ Monitoring system health  
✔ Debugging performance  
✔ Optimizing server security