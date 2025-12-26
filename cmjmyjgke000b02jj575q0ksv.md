---
title: "Introduction to Jenkins: A Guide for Beginners"
seoTitle: "Jenkins Basics: Beginner's Guide"
seoDescription: "Learn Jenkins for CI/CD with our beginner's guide. Explore setup, job types, and benefits to streamline software development processes effortlessly"
datePublished: Fri Dec 26 2025 14:19:05 GMT+0000 (Coordinated Universal Time)
cuid: cmjmyjgke000b02jj575q0ksv
slug: introduction-to-jenkins-a-guide-for-beginners
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766758695003/be3a5479-0bc5-4bdd-897c-68e97abc31cd.jpeg
tags: devops, jenkins, ci-cd, jenkins-devops

---

### **1\. What is Jenkins?**

Jenkins is an open-source **automation server**.

It is written in Java and is used to automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery (CI/CD).

* **It is a Server:** It runs as a background process on a server (or your local machine).
    
* **It is Open Source:** It is free to use and has a massive community supporting it.
    
* **It is Plugin-Based:** It has over 1,800 plugins that allow it to talk to almost any tool (Git, Docker, AWS, Slack, etc.).
    

---

### **2\. Why Do We Use Jenkins?**

We use Jenkins primarily to implement **CI/CD** (Continuous Integration and Continuous Deployment).7 Without Jenkins, the process of releasing software is slow, manual, and error-prone.8

+1

Here is a breakdown of the specific problems Jenkins solves:

### **A. It Automates the "Build" Process**

* **Without Jenkins:** A developer writes code, saves it, and then manually runs a command to compile/build the project to see if it works. This wastes time.
    
* **With Jenkins:** As soon as a developer saves code to a repository (like GitHub), Jenkins detects the change and automatically triggers a build.9 If the build fails, it instantly alerts the developer.
    

### **B. It Automates Testing (Continuous Integration)**

* **Without Jenkins:** Developers might wait until the end of the week to merge all their code and run tests. This leads to "integration hell" where dozens of bugs appear at once.
    
* **With Jenkins:** Every time a developer commits code (even 10 times a day), Jenkins runs the automated tests immediately.10 If a bug is introduced, you know about it 5 minutes later, not 5 days later.
    

### **C. It Automates Deployment (Continuous Delivery)**

* **Without Jenkins:** Deploying to a server often involves a developer manually connecting to a server, moving files, and restarting services. This is risky; if the developer deletes the wrong file, the site goes down.
    
* **With Jenkins:** You press a button (or configure it to happen automatically), and Jenkins runs a script to deploy your application to the server safely and consistently every single time.
    

---

### **Summary of Benefits**

| **Feature** | **Benefit** |
| --- | --- |
| **Early Error Detection** | Catches bugs immediately after code is committed. |
| **Time Saving** | Automates repetitive tasks like building and testing. |
| **Integrations** | Connects your Git, Docker, Kubernetes, and Cloud providers in one place. |
| **Feedback Loop** | Sends automatic notifications (Email/Slack) if code breaks the build. |

### **How it Works in a Nutshell**

1. **Commit:** You push your code to **Git**.
    
2. **Trigger:** Jenkins detects the change.
    
3. **Build & Test:** Jenkins pulls the code, builds the application, and runs tests.
    
4. **Deploy:** If tests pass, Jenkins deploys the application to a test server or production.
    

---

# Jenkins Jobs

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1766758499763/84a6d766-88da-4f9e-bd4e-c4c8e2860fce.png align="center")

When you click **"New Item"** in Jenkins, you are asked to choose a job type. Each type is designed for a specific way of handling your build and deployment process.

Here are the main types of jobs you will encounter:

### **1\. Freestyle Project (The "Basic" Job)**

This is the most common and easiest job type for beginners. You configure everything using the **graphical user interface (GUI)** in your browser.

* **Best for:** Simple, linear tasks (e.g., "Run this script," then "Archive these files").
    
* **How it works:** You point and click to add build steps (like "Execute Shell" or "Invoke Ant").
    
* **Limitation:** It is hard to version control because the configuration is stored in Jenkins, not in your code repository.
    

### **2\. Pipeline (The "Code" Job)**

This is the modern standard for Jenkins. Instead of clicking buttons in the UI, you write a script (called a `Jenkinsfile`) that defines your entire build process.

* **Best for:** Complex workflows (CI/CD) that need to be versioned, reviewed, and stored with your code.
    
* **Key Feature:** You can define "Stages" (e.g., Build, Test, Deploy) and visualize them.
    
* **Why use it:** "Pipeline as Code" allows you to restore your entire pipeline instantly if your Jenkins server crashes.
    

### **3\. Multibranch Pipeline (The "Smart" Job)**

This is a more advanced version of the standard Pipeline. It automatically detects **branches** in your Git repository (e.g., `main`, `develop`, `feature-login`) and creates a separate pipeline for each one.

* **Best for:** Teams working with many branches (Feature Branch Workflow).
    
* **How it works:** If you create a new branch in Git called `feature-x` and push it, Jenkins automatically creates a new job for `feature-x` and runs the tests.
    

### **4\. Maven Project**

A specialized version of the Freestyle project designed specifically for Java projects that use **Maven**.

* **Best for:** Pure Java/Maven projects.
    
* **Benefit:** Jenkins understands Maven's structure (`pom.xml`) efficiently and can automatically archive test results without much configuration.
    
* **Note:** Many teams now prefer using a generic *Pipeline* job even for Maven projects to keep everything as code.
    

### **5\. Multi-configuration Project (Matrix Project)**

This allows you to run the *same* job with *different* configurations automatically.

* **Best for:** Testing compatibility.
    
* **Example:** You want to run the same test suite on **Windows, Linux, and Mac** simultaneously, or test your app on **Python 3.8, 3.9, and 3.10**.
    

### **6\. Folder / Organization Folder**

These are not "jobs" that run code, but containers to keep things organized.

* **Folder:** Lets you group jobs together (e.g., "Client A Projects," "Backend Services").
    
* **Organization Folder:** Connects to a GitHub/Bitbucket organization and automatically creates Multibranch Pipelines for *every repository* in that account.
    

---

### **Summary: Which one should you choose?**

| **If you want to...** | **Choose this Job Type** |
| --- | --- |
| Run a simple script or learn Jenkins | **Freestyle** |
| Build a proper CI/CD workflow | **Pipeline** |
| Build every branch in your Git repo automatically | **Multibranch Pipeline** |
| Test code on different OS versions at once | **Multi-configuration** |

**Recommendation:** If you are starting a new project today, skip Freestyle and go straight to **Pipeline**. It is the industry standard.

Would you like to see a comparison of a **Freestyle configuration** vs. a **Pipeline script** for the same task?

---

# Setup of jenkins

## Step 1: Install Java (Prerequisite)

Jenkins is a Java-based application. It currently runs best on **Java 17**.

1. **Update your system:**Bash
    
    `sudo apt update`
    
2. **Install OpenJDK 17:**Bash
    
    `sudo apt install fontconfig openjdk-17-jre -y`
    
3. **Verify installation:**Bash
    
    `java -version`
    

---

## Step 2: Install Jenkins

Since Jenkins is not in the default Linux stores, you must add the official Jenkins repository.

1. **Add the Repository Key:**Bash
    
    `sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \\ <`[`https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key`](https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key)\&gt;
    
2. **Add the Jenkins Repository to your sources:**Bash
    
    `echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \\ <`[`https://pkg.jenkins.io/debian-stable`](https://pkg.jenkins.io/debian-stable)`> binary/" | sudo tee \\ /etc/apt/sources.list.d/jenkins.list > /dev/null`
    
3. **Install Jenkins:**Bash
    
    `sudo apt update sudo apt install jenkins -y`
    

---

## Step 3: Start and Enable Jenkins

1. **Start the service:**Bash
    
    `sudo systemctl start jenkins`
    
2. **Enable it to start on boot:**Bash
    
    `sudo systemctl enable jenkins`
    
3. **Check the status:**Bash
    
    `sudo systemctl status jenkins`
    

---

## Step 4: Configure Firewall (If applicable)

Jenkins runs on port **8080** by default. You need to allow this port through your firewall.

Bash

`sudo ufw allow 8080 sudo ufw status`

---

## Step 5: Initial Setup Wizard (Web UI)

Now, move from the terminal to your web browser.

1. **Open your browser** and go to: [`http://your_server_ip_or_domain:8080`](http://your_server_ip_or_domain:8080)
    
2. **Unlock Jenkins:** You will see a screen asking for an Administrator password.
    
3. **Get the password from your Linux terminal:**Bash
    
    `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
    
    Copy the long string of letters and numbers and paste it into the browser.
    

---

## Step 6: Final Configuration

1. **Install Plugins:** Choose **"Install suggested plugins."** This will set up Git, Pipeline, and other essentials automatically.
    
2. **Create Admin User:** Set up your username, password, and email address.
    
3. **Instance Configuration:** Confirm the URL (usually [`http://localhost:8080`](http://localhost:8080) or your IP).
    
4. **Start using Jenkins!**
    

---