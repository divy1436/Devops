---
title: "Stop Committing "Fix Typo"! 🛑 Master Git Squash for a Clean History"
seoTitle: "Master Git Squash for Clean Commit History"
seoDescription: "Master Git Squash for streamlined commit history, easier code reviews, and cleaner version control. Learn interactive rebase techniques now!"
datePublished: Sat Dec 06 2025 14:48:28 GMT+0000 (Coordinated Universal Time)
cuid: cmiues7rz000002l5d56sa2ff
slug: stop-committing-fix-typo-master-git-squash-for-a-clean-history
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765032500488/ceb47820-5451-4090-b9c8-cb1583499410.png
tags: linux, github, devops

---

---

**Hey devs! 👋**

We’ve all been there. You are working on a new feature, say, a login page. You get into the zone, and your commit history starts looking like this:

* `c5e1b4a` Add more spacing to form
    
* `a6d9e87` Fix typo in variable name
    
* `3b1aa84` Working login page (finally)
    
* `8af2930` Create login page structure
    

Let’s be real—this is messy. If your team leader or an open-source maintainer sees this, they have to review four different chunks of code just to understand *one* feature.

Today, we are talking about **Git Squash**. It’s the magic wand that turns that messy list into this:

* `41a9b70` **Add login page feature** ✨
    

Ready to clean up your act? Let’s dive in.

---

## 🧠 What is Git Squash?

Think of writing code like writing a book.

When you are drafting a chapter, you write sentences, cross them out, fix spelling errors, and rewrite paragraphs. That is your work in progress.

But when you publish the book, you don't show the reader every single crossed-out word. You just show the final, polished chapter.

**Git Squash is the editor.** It combines multiple "draft" commits into a single, clean "published" commit.<sup>1</sup>

### Why bother?

1. **Code Reviews are easier:** Your reviewer sees the whole feature at once, not your trial-and-error process.
    
2. **Cleaner History:** It’s easier to track changes when one commit equals one feature.
    
3. **Happy CI/CD:** You avoid triggering automated build pipelines for every tiny "fix typo" commit.<sup>2</sup>
    

---

## 🛠️ How to Squash: The Interactive Rebase

The most common way to squash is using **Interactive Rebase**. It sounds scary, but it’s actually quite simple.

### The Scenario

You have **4 commits** you want to combine (like our login page example above).

### Step 1: The Command

Open your terminal and type:

Bash

```plaintext
git rebase -i HEAD~4
```

* `rebase -i`: Interactive rebase.
    
* `HEAD~4`: Go back 4 commits from where I am now.
    

### Step 2: The Editor

Your text editor (Vim, VS Code, or Nano) will pop up with a list like this:

Plaintext

```plaintext
pick 8af2930 Create login page
pick 3b1aa84 Working login page
pick a6d9e87 Fix typo
pick c5e1b4a Add more spacing to form
```

### Step 3: Pick and Squash

We want to keep the **first** commit and meld the others into it. Change `pick` to `squash` (or just `s`) for the bottom three:

Plaintext

```plaintext
pick 8af2930 Create login page
squash 3b1aa84 Working login page
squash a6d9e87 Fix typo
squash c5e1b4a Add more spacing to form
```

### Step 4: Finalize the Message

Save and close the file. Git will ask you to create one **final commit message**.<sup>3</sup>

Delete the old messy messages and write something professional:

> `Add login page feature with layout, fixes, and spacing updates`

Save, and you are done! 🚀

---

## ⚡ Quick Alternative: Merge Squash

If you are merging a feature branch into `main` and don't want to deal with rebasing, you can squash *during* the merge.<sup>4</sup>

Bash

```plaintext
git checkout main
git merge --squash feature/login
git commit -m "Add login feature"
```

This takes all the work from your branch and stages it as one new commit on `main`. It’s great for lazy cleanups!

---

## ⚠️ The Golden Rule of Squashing

**STOP! READ THIS CAREFULLY.** 🛑

Squashing **rewrites history**. It changes the ID (hash) of your commits.

> **Never squash commits that you have already pushed to a shared branch that others are working on.**

If your teammate has pulled your branch and you squash it, their history will conflict with yours, and you will break their code.

**If you are working alone on a branch:**

1. Squash locally.
    
2. If you already pushed the messy commits, you must force push the new clean one:
    
    Bash
    
    ```plaintext
    git push --force-with-lease
    ```
    
    *(Always use* `--force-with-lease` instead of `--force`. It checks if anyone else updated the branch before you overwrite it!)
    

---

## 💡 Pro Tip: Autosquash

Want to feel like a senior dev? Use **Autosquash**.

When you are making a fix, instead of a normal commit, do this:

1. **Commit as a fixup:**
    
    Bash
    
    ```plaintext
    git commit --fixup <commit-hash-of-original-code>
    ```
    
2. **Auto-squash later:**
    
    Bash
    
    ```plaintext
    git rebase -i --autosquash HEAD~N
    ```
    

Git will automatically mark those commits as `squash` for you in the editor. It saves so much time!

---

## Final Thoughts

At **SHACKVERSE** (a real-world example), devs used to make 15-20 commits per feature. It was a nightmare to review. By implementing a "Squash before PR" rule, they sped up code reviews and kept their release logs beautiful.

Your Homework:

Next time you are working on a personal project, try making 3 messy commits and then git rebase -i them into one masterpiece.

Happy Coding! 💻✨

---

*Found this helpful? Drop a reaction and follow for more DevOps and Git guides!*

---