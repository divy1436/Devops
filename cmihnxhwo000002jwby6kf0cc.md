---
title: "🚀 Git Practice Project"
seoTitle: "Git Project for Beginners"
seoDescription: "Learn Git commands through building a student portfolio website. Hands-on practice with branching, merging, rebasing, and more. Perfect for beginners!"
datePublished: Thu Nov 27 2025 16:43:31 GMT+0000 (Coordinated Universal Time)
cuid: cmihnxhwo000002jwby6kf0cc
slug: git-practice-project

---

---

### Understood — here is the **combined full text** of both assignments in a clean printable format.

You can now copy it to Word, Google Docs, or continue formatting.

---

# 📘 Combined Assignment

## **Practice Project: Git Version Control and GitHub Collaboration**

---

## **Project Name:**

**Student Profile Web Application Using Git and GitHub**

---

# **Assignment 1: Git Version Control End to End**

---

## **1\. Introduction to Git**

Git is a distributed version control system that stores full project history on every machine. It tracks changes using snapshots rather than storing file differences, making it efficient and scalable.

### Centralized vs Distributed

| Feature | Centralized VCS | Distributed (Git) |
| --- | --- | --- |
| Server required to work | Yes | No |
| Full local history | No | Yes |
| Branching support | Limited | Advanced |
| Performance | Slower | Faster |

---

## **2\. Important Git Terminology**

| Term | Meaning | Location Internally |
| --- | --- | --- |
| Repository | Tracks project files | Stored in `.git` |
| Working Directory | Your active project files | System storage |
| Staging Area | Temporary holding before commit | `.git/index` |
| Commit | Snapshot of project | SHA generated object |
| Branch | Pointer to commit history | `.git/refs/heads` |
| HEAD | Current working branch pointer | `.git/HEAD` |

---

## **3\. Git Object Model**

Git stores everything as four object types:

| Object | Purpose |
| --- | --- |
| Blob | Stores file content |
| Tree | Stores directory structure |
| Commit | Stores author metadata and parent commit |
| Tag | Named reference to commit |

All objects are hashed using SHA1 ensuring integrity.

---

## **4\. Project Setup**

```
mkdir student-profile
cd student-profile
git init
echo "<h1>Student Profile</h1>" > index.html
git add index.html
git commit -m "Initial commit: Add profile header"
```

Inspect storage:

```
git cat-file -p HEAD
```

---

## **5\. Branching**

```plaintext
git branch add-bio
git checkout add-bio
echo "<p>Name: Divyanshu Maurya</p>" >> index.html
git add index.html
git commit -m "Add student bio"
```

Compare:

```
git diff main..add-bio
```

---

## **6\. Using Git Diff**

```plaintext
git diff
git add index.html
git diff --cached
```

---

## **7\. Merge and Rebase**

Merge branch:

```plaintext
git checkout main
git merge add-bio
```

Rebase example:

```plaintext
git checkout -b add-skills
echo "<li>Web Development</li>" >> skills.html
git add .
git commit -m "Add skills section"
git rebase main
```

---

## **8\. Git Stash**

```plaintext
echo "<footer>Contact</footer>" >> index.html
git stash save "Footer WIP"
git checkout main
git stash pop
```

---

## **9\. Cherry-Pick**

```plaintext
git checkout -b fix-contact
echo "<p>Email Updated</p>" >> index.html
git commit -am "Fix email"
git checkout main
git cherry-pick <commit-id>
```

---

## **10\. Create Tags**

```plaintext
git tag -a v1.0 -m "First release"
git push origin --tags
```

---

## **11\. Squash Commits**

```plaintext
git rebase -i HEAD~3
```

---

## **12\. Merge Conflict Handling**

Conflict markers look like:

```
<<<<<<< HEAD
Version A
=======
Version B
>>>>>>> branch-name
```

Fix manually and commit.

---

## **13\. Revert**

```plaintext
git revert <commit-id>
```

Revert creates a new commit instead of altering history.

---

---

# **Assignment 2: GitHub Remote Collaboration**

---

## **1\. Introduction to GitHub**

GitHub is a cloud platform for hosting Git repositories. It supports collaboration, issues, pull requests, automation, and team workflows.

---

## **2\. Create Remote Repository**

Create a new GitHub repo named:

```
student-profile-github
```

---

## **3\. Connect Local Repo to GitHub**

```plaintext
git remote add origin https://github.com/<username>/student-profile-github.git
git branch -M main
git push -u origin main
```

---

## **4\. Clone Repository**

```plaintext
git clone https://github.com/<username>/student-profile-github.git
```

---

## **5\. Update README**

```plaintext
echo "## Student Profile Project" > README.md
git add README.md
git commit -m "Add README"
git push
```

---

## **6\. Create Pull Request**

```plaintext
git checkout -b add-photo
echo "<img src='profile.jpg'>" >> index.html
git commit -am "Add photo"
git push origin add-photo
```

Then open a Pull Request on GitHub.

---

## **7\. Merge Methods**

| Method | Use Case |
| --- | --- |
| Merge Commit | Preserve full history |
| Squash Merge | Combine commits for clean history |
| Rebase + Merge | Organizes history into linear path |

---

## **8\. Issues and Project Board**

Create tasks like:

* Improve UI
    
* Add new form
    
* Refactor layout
    

Assign and track inside GitHub Project Board.

---

## **9\. Forking, Starring, Watching**

* Fork: Copy project to modify independently.
    
* Star: Bookmark for future reference.
    
* Watch: Receive update notifications.
    

---

## **10\. Add Collaborator**

Settings → Manage Access → Add Collaborator

---

## **11\. GitHub Actions**

Create folder:

```plaintext
mkdir -p .github/workflows
```

Basic workflow file:

```plaintext
name: CI Workflow
```

Commit and push.

---

## **12\. Releases and Versioning**

```plaintext
git tag v1.0
git push origin v1.0
```

Create detailed changelog in Release tab.

---

## **13\. Backup and Archive**

Use **Download ZIP** from GitHub for submission.

---

---

# **Final Submission Requirements**

| Item | Required |
| --- | --- |
| Screenshots | Yes |
| Full Commands | Yes |
| Project Folder Structure | Yes |
| Git History Graph | Yes |
| GitHub Link | Optional |
| Release and Pull Request Proof | Yes |

---

---