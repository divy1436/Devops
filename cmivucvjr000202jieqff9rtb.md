---
title: "Git Merge Conflicts"
seoTitle: "Resolving Git Merge Conflicts"
seoDescription: "Learn how to confidently resolve Git merge conflicts with this easy step-by-step guide and tips to avoid them in the future. 🚀"
datePublished: Sun Dec 07 2025 14:52:12 GMT+0000 (Coordinated Universal Time)
cuid: cmivucvjr000202jieqff9rtb
slug: git-merge-conflicts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765118657095/4e66c1f3-6890-4005-9ae7-38d2bdab2fd6.png
tags: github

---

---

# Don't Panic! A Simple Guide to Fixing Them 🚀

If you are working with Git, sooner or later, you will see a scary message in your terminal: **CONFLICT**.

It looks like an error, but it’s actually a safety feature. It doesn't mean you broke something; it just means Git needs your help.

In this guide, we’ll break down what a merge conflict is, why it happens, and how to fix it without losing your mind.

## What is a Merge Conflict? 🤔

Imagine you and a friend are writing a story on the same piece of paper.

* You erase the last sentence and write: "The hero saved the day."
    
* Your friend erases the same sentence and writes: "The hero failed miserably."
    

If you try to combine these two versions, who is right? The paper can't decide for you.

**That is a merge conflict.**

It happens when Git cannot automatically figure out which version of a file to keep because two branches edited the **same line** of the **same file** differently.<sup>1</sup>

## What Does a Conflict Look Like?

When a conflict happens, Git stops the merge and modifies your file to show you exactly where the problem is. It adds "markers" to the code.

It looks like this:

JavaScript

```plaintext
<<<<<<< HEAD
console.log("User logged in"); 
=======
console.log("Login successful"); 
>>>>>>> feature/login-enhancement
```

### Decoding the Markers:

* `<<<<<<< HEAD`: This is **your** code (what you currently have).
    
* `=======`: This is the divider line. Everything above it is yours; everything below it is incoming.
    
* `>>>>>>> feature-branch`: This is the **incoming** code (what you are trying to merge).
    

## How to Fix It (Step-by-Step) 🛠️

Let's say you tried to `git merge` and got a conflict. Here is the manual way to fix it:

### 1\. Check the Status

First, see which files are fighting.

Bash

```plaintext
git status
```

*Look for "both modified".*

### 2\. Open the File

Open the file in your code editor (like VS Code).<sup>2</sup> You will see the weird symbols (`<<<<<<<`, `=======`, `>>>>>>>`).

### 3\. Edit the Code

You have to act as the judge. You have three choices:

* Keep your code.
    
* Keep the incoming code.
    
* Combine them into something new.
    

**Crucial Step:** You must **delete** the marker lines (`<<<`, `===`, `>>>`) before saving!

Example Fix:

Change this:

JavaScript

```plaintext
<<<<<<< HEAD
console.log("User logged in"); 
=======
console.log("Login successful"); 
>>>>>>> feature-logging
```

*To this:*

JavaScript

```plaintext
console.log("User logged in successfully"); 
```

### 4\. Save and Commit

Once the file looks clean (no markers), save it. Then tell Git you finished the job:

Bash

```plaintext
git add app.js
git commit -m "Fixed merge conflict"
```

## Tools That Make This Easier 🧰

You don't always have to do this manually in a text editor.

* **VS Code:** When VS Code detects a conflict, it gives you clickable buttons right over the code: *"Accept Current Change"*, *"Accept Incoming Change"*, or *"Accept Both"*.
    
* **Git Mergetool:** You can run `git mergetool` in your terminal to open a dedicated UI for fixing conflicts.<sup>3</sup>
    

## The "Oh No, Get Me Out of Here" Button 🚨

If you start fixing a conflict and realize you made a mistake or just want to start over, you can abort the mission:

Bash

```plaintext
git merge --abort
```

This cancels the merge and puts everything back to exactly how it was before you started.

## How to Avoid Conflicts 🛡️

You can't avoid them 100%, but these habits help reduce them:

1. **Pull Often:** Before you start working, always run `git pull` to get the latest changes.<sup>4</sup>
    
2. **Keep Branches Small:** Long-lived branches tend to drift far apart from the main code.
    
3. **Talk to Your Team:** If you know you are working on `app.js`, tell your teammate so they don't edit the same lines at the same time.
    

## Final Thoughts

Merge conflicts aren't evil. They are just Git's way of asking, **"Hey, I see two different versions of the truth here. Which one do you want?"**

Resolve carefully, remove the markers, and happy coding! 💻

---