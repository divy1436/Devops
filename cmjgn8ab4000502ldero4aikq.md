---
title: "How to Get Started with Apache Tomcat"
seoTitle: "Beginner's Guide to Apache Tomcat"
seoDescription: "Learn Apache Tomcat basics, architecture, installation, and deployment. Ideal for beginners setting up a robust Java application server"
datePublished: Mon Dec 22 2025 04:15:51 GMT+0000 (Coordinated Universal Time)
cuid: cmjgn8ab4000502ldero4aikq
slug: how-to-get-started-with-apache-tomcat
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766376920343/f9bd2d58-a3bc-4a8d-b08e-6c4705974904.jpeg

---

---

## 🏗️ Understanding the Architecture: The "Tomcat Restaurant"

Imagine you are opening a high-end restaurant called **The Java Diner**. To serve customers effectively, you need several departments working in harmony. This is exactly how Apache Tomcat works.

![Image of Apache Tomcat architecture diagram](https://encrypted-tbn0.gstatic.com/licensed-image?q=tbn:ANd9GcRJwmN3XeOE_6ZGYIUgxCdP7VmuLs6blhkSxgDlfPm64B-o4RAMiwjBIuh0BkTytq9LfRM_tOxob8HYU-7cTrF2FyvHEcpx2aSViZBml6Mje3QsLpk align="left")

### 1\. The Dining Hall (Coyote)

**Coyote** is the Connector.<sup>1</sup> It is the "Front of House" staff that stands at the door to greet customers. It handles the communication between the outside world (the web) and the kitchen. It supports HTTP 1.1 and can even handle secure "private booths" using SSL/TLS.<sup>2</sup>

### 2\. The Head Chef (Catalina)

**Catalina** is the heart of the operation—the **Servlet Container**.<sup>3</sup> When a request comes in, Catalina decides which recipe (Servlet) to use and manages the entire cooking process from start to finish.<sup>4</sup>

### 3\. The Recipe Translator (Jasper)

Sometimes, recipes are written in a shorthand that's easy for humans to read but hard for the stove to understand (**JSP**). **Jasper** is the engine that takes these JSP files and "compiles" them into professional Java Servlets so the kitchen can execute them perfectly.

### 4\. The Kitchen Staff (Key Components)

* **Servlet Container:** The environment where the Java code actually runs.
    
* **JSP Engine:** The tool that enables dynamic web pages (HTML/XML).
    
* **Cluster:** This is like having a "Chain of Restaurants." If one kitchen is too busy, the **Cluster** allows you to balance the load across multiple instances so the service never fails.<sup>5</sup>
    

---

## 🛠️ Step-by-Step Installation Guide (v9.0.65 & v10.1.20)

Whether you are installing the classic **Version 9** or the modern **Version 10**, the process follows a precise logical flow. Here is the detailed technical roadmap.

### Phase 1: Environment Setup & Download

First, we must prepare the server and fetch the software.

1. **Switch to Superuser:** Gain administrative "Master Key" access.
    
    Bash
    
    ```plaintext
    sudo su
    ```
    
2. **Navigate to Opt:** We move to the `/opt` directory, the standard place for optional software packages.
    
    Bash
    
    ```plaintext
    cd /opt
    ```
    
3. **Download the Archive:** Use `wget` to pull the specific version from the official Apache archives.<sup>6</sup>
    
    * **For v9.0.65:** `sudo wget` [`https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz`](https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.65/bin/apache-tomcat-9.0.65.tar.gz)
        
    * **For v10.1.20:** `sudo wget` [`https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.20/bin/apache-tomcat-10.1.20.tar.gz`](https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.20/bin/apache-tomcat-10.1.20.tar.gz)
        
4. **Extract the Files:** Unpack the "luggage" into the directory.
    
    Bash
    
    ```plaintext
    sudo tar -xvf apache-tomcat-[VERSION].tar.gz
    ```
    

### Phase 2: Configuring Security (The User Registry)

By default, Tomcat is locked. We need to create an "Admin" user.

1. **Enter the Config Zone:**
    
    Bash
    
    ```plaintext
    cd /opt/apache-tomcat-[VERSION]/conf
    ```
    
2. **Edit** `tomcat-users.xml`:
    
    Bash
    
    ```plaintext
    sudo vi tomcat-users.xml
    ```
    
3. **Add the Credentials:** Add this line before the final `</tomcat-users>` tag. This grants the user "admin" the power to use the GUI and manager scripts:
    
    XML
    
    ```plaintext
    <user username="admin" password="admin1234" roles="manager-gui,admin-gui"/>
    ```
    

### Phase 3: Creating "Shortcuts" (Symbolic Links)

Instead of typing long file paths every time, we create easy commands.

Bash

```plaintext
sudo ln -s /opt/apache-tomcat-[VERSION]/bin/startup.sh /usr/bin/startTomcat
sudo ln -s /opt/apache-tomcat-[VERSION]/bin/shutdown.sh /usr/bin/stopTomcat
```

*Now, you can simply type* `startTomcat` from anywhere in your terminal!

### Phase 4: Opening Remote Access

By default, Tomcat only talks to people sitting at the "Local Counter" ([localhost](http://localhost)). To allow access from other computers, we must comment out the **RemoteAddrValve**.

1. Edit Manager Context:
    
    sudo vi /opt/apache-tomcat-\[VERSION\]/webapps/manager/META-INF/context.xml
    
2. Edit Host-Manager Context:
    
    sudo vi /opt/apache-tomcat-\[VERSION\]/webapps/host-manager/META-INF/context.xml
    

> **Action:** In both files, find the `<Valve ... />` section and wrap it in comments: \`\`. This disables the restriction.

---

## 🚀 Deploying Your Application

Now that your "Restaurant" is open, it’s time to serve your first dish. We will use a sample Maven project.

1. **Get the Code:** Clone the repository: [`https://github.com/jaiswaladi2468/maven-tomcat-sample.git`](https://github.com/jaiswaladi2468/maven-tomcat-sample.git)
    
2. **Build the Dish:** Use Maven to package your code into a `.war` (Web Archive) file.<sup>7</sup>
    
3. **Serve it:** \* Start your server: `sudo startTomcat`
    
    * Move your `.war` file into the `/webapps` folder of your Tomcat directory.<sup>8</sup>
        
    * Tomcat will automatically "unpack" the file and host your website.
        

**To Reset or Stop:** If you need to close the kitchen for maintenance, simply run:

Bash

```plaintext
sudo stopTomcat
```

---