---
title: "Understanding Jenkins Pipelines Codes : A Complete Guide"
datePublished: Wed Dec 31 2025 13:32:35 GMT+0000 (Coordinated Universal Time)
cuid: cmju22x1w001502lc2ue2dymb
slug: understanding-jenkins-pipelines-codes-a-complete-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767187785734/07f7cf86-4dd8-4c36-8aa2-c1ba59400c9f.png
tags: jenkins, pipeline

---

In the early days of DevOps, setting up a CI/CD (Continuous Integration/Continuous Delivery) process felt like navigating a labyrinth of manual UI configurations. You’d click through endless menus, check boxes, and hope you didn't miss a step.

Today, the industry has shifted toward **Pipeline as Code**. Instead of manual configurations, we use a **Jenkinsfile** a script that defines your entire build, test, and deployment process, stored right alongside your code in Git.

---

## 1\. What is a Jenkins Pipeline?

Think of a Pipeline as an **automated assembly line** in a factory. Instead of building a car, it builds, tests, and delivers software.

Imagine you are baking a cake:

* **The Manual Way:** You mix, bake, and frost by hand. If you forget the sugar, you only find out after the cake is finished.
    
* **The Pipeline Way:** A machine checks the ingredients (**Compile**), tastes a sample (**Test**), checks for expiration dates (**Security**), and puts it in a box (**Deploy**) automatically.
    

---

## 2\. Anatomy of a Pipeline: The 7 Critical Stages

When you write a Jenkins script, you move through specific "stages." Here is what actually happens behind the scenes:

### Stage 1: The "Hello World" (Initialization)

This is a simple "pulse check" to confirm the automation server is active. If the script can't print "Hello World," the environment isn't ready.

### Stage 2: Compile (The Build Stage)

Computers don't read Java or C++ directly; they need machine code. The pipeline runs a command like `mvn compile` to convert your human-readable code into something executable. If there’s a syntax error, the pipeline stops here.

### Stage 3: Test (Unit Testing)

This stage ensures the logic of your code works. For example, if you wrote a calculator app, the test checks if $2 + 2$ actually equals $4$. This catches bugs before they ever reach a user.

### Stage 4: Security Check (SCA)

Modern apps rely on libraries (pre-written code). This stage scans for "vulnerabilities" to ensure you aren't accidentally using "rotten" or dangerous code that hackers could exploit.

### Stage 5: SonarQube Analysis (Code Quality)

While testing checks if the code *works*, SonarQube checks if the code is *clean*. It looks for "Code Smells" (messy code) or duplication. It acts as a **Quality Gate**—if the code is too messy, the pipeline fails.

### Stage 6: Deploy

The finalized file (the "Artifact") is moved to a server (like AWS or Azure). This is the moment your new version goes live!

### Stage 7: Stage View (The Dashboard)

Jenkins provides a visual "Stage View." This is a row of boxes representing each step. Green means success; red means failure.

---

## 3\. How to Use It: The Jenkinsfile

To prevent the common "mvn not found" errors, your script must define the **tools** it needs. Here is a complete, production-ready conceptual script:

Groovy

```plaintext
pipeline {
    agent any

    // Automatically sets up Maven and Java
    tools {
        maven 'maven3' 
        jdk 'jdk17'    
    }

    stages {
        stage('Clone Git') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/project.git'
            }
        }

        stage('Compile') {
            steps {
                echo "Compiling..."
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build & Package') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success { echo "Pipeline completed successfully!" }
        failure { echo "Pipeline failed. Check the logs." }
    }
}
```

### Why use the `tools` block?

If you don't include this, Jenkins tries to run commands using the system's global path. If Maven isn't installed directly on the server's OS, the command will fail with: `mvn: command not found`. The `tools` block tells Jenkins to "provision" these tools specifically for this job.

### Phase 1: The "New Item" (Setting up the Workbench)

1. On the Jenkins Dashboard, click **New Item**.
    
2. **Name it:** Give your project a clear name (e.g., `My-Java-Maven-App`).
    
3. **Select "Freestyle project":** This is the beginner-friendly "classic" mode where you use a visual interface instead of writing code for the pipeline itself.
    
4. Click **OK**.
    

---

### Phase 2: Source Code Management (The "Instructions")

* **What you do:** Scroll to the **Git** section and paste your Repository URL.
    
* **Why we do it:** This is the most important part. Jenkins needs to "clone" (download) your code into its own workspace so it can work on it. Every time you run the job, Jenkins checks Git to see if there is a new version of your "instructions."
    

---

### Phase 3: Build Environment (Choosing your Java version)

* **What you do:** You mentioned adding **Java 17 and 21**. In the project configuration, you can now check a box like **"Provide JDK"** or simply select your preferred JDK from a dropdown menu.
    
* **Why we do it:** Some apps are old (Java 17) and some are new (Java 21). By having both in your "system tools," you can tell *this specific project* to use exactly what it needs. This ensures the code compiles correctly without errors.
    

---

### Phase 4: Build Steps (The "Action")

* **What you do:** Click **Add build step** and select **"Invoke top-level Maven targets."**
    
* **The Details:**
    
    * **Maven Version:** Pick the one you named in your global tools.
        
    * **Goals:** Type `clean install`.
        
* **Why we do it:** \* `clean`: Deletes old, messy files from previous attempts.
    
    * `install`: This is the "magic" command. It tells Maven to download all the libraries (jars) your code needs, compile your Java files, and package them into a final file (like a `.jar`).
        

---

### Phase 5: Run & Results

1. Click **Save**.
    
2. Click **Build Now** on the left menu.
    
3. **The Result:** You will see a small circle. If it turns **Blue/Green**, you succeeded! If it is **Red**, it failed.
    
4. **Check the "Console Output":** This is like the project's diary. It shows you exactly what Java and Maven did line-by-line.
    

---

## 4\. Pro Tip: Use the Pipeline Syntax Generator

Don't worry about memorizing the code. Jenkins has a built-in **Snippet Generator**. You can select a task (like "Git Clone"), fill out a form, and it will generate the exact line of code you need to paste into your script.

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Stage</strong></p></td><td colspan="1" rowspan="1"><p><strong>Simple Meaning</strong></p></td><td colspan="1" rowspan="1"><p><strong>Why We Do It</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Compile</strong></p></td><td colspan="1" rowspan="1"><p>Building parts</p></td><td colspan="1" rowspan="1"><p>Turn code into a runnable app.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Test</strong></p></td><td colspan="1" rowspan="1"><p>Checking logic</p></td><td colspan="1" rowspan="1"><p>Ensure there are no basic bugs.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Security</strong></p></td><td colspan="1" rowspan="1"><p>Checking for holes</p></td><td colspan="1" rowspan="1"><p>Stop hackers from using old libraries.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>SonarQube</strong></p></td><td colspan="1" rowspan="1"><p>Checking quality</p></td><td colspan="1" rowspan="1"><p>Keep code clean and professional.</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Deploy</strong></p></td><td colspan="1" rowspan="1"><p>Delivering</p></td><td colspan="1" rowspan="1"><p>Put the app where users can see it.</p></td></tr></tbody></table>