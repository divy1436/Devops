---
title: "🚀 Git Practice Project"
seoTitle: "Git Project for Beginners"
seoDescription: "Learn Git commands through building a student portfolio website. Hands-on practice with branching, merging, rebasing, and more. Perfect for beginners!"
datePublished: Thu Nov 27 2025 16:43:31 GMT+0000 (Coordinated Universal Time)
cuid: cmihnxhwo000002jwby6kf0cc
slug: git-practice-project

---

---

### **Project Theme:** Student Portfolio Website (HTML based)

You will build and manage this project only using Git commands.

---

## **📍 Phase 1: Setup and Basics**

✔ Create a folder named `portfolio`  
✔ Inside it, create a file: `index.html`

Add content:

```plaintext
<h1>My Portfolio</h1>
```

Run:

```plaintext
git init
git add .
git commit -m "Initial commit with heading"
```

---

## **📍 Phase 2: Branching and Development**

Create a new branch:

```plaintext
git branch bio
git checkout bio
```

Add text:

```plaintext
<p>Hello, I am a software engineering student.</p>
```

Commit:

```plaintext
git add .
git commit -m "Add bio section"
```

Switch to main:

```plaintext
git checkout main
```

Compare:

```plaintext
git diff main..bio
```

---

## **📍 Phase 3: Merge**

Merge with main:

```plaintext
git merge bio
```

If conflict happens in future steps, solve manually.

---

## **📍 Phase 4: Creating Features**

Create three branches, one by one:

| Branch Name | File to Edit | Content to Add |
| --- | --- | --- |
| `skills` | `skills.html` | List 5 skills |
| `projects` | `projects.html` | Add 2 sample projects |
| `contact` | `index.html` | Add email + phone |

Commands repeated:

```plaintext
git checkout -b skills
...
git add .
git commit -m "Add skills section"
git checkout main
git merge skills
```

Do same for other branches.

---

## **📍 Phase 5: Git Diff Practice**

Before committing a change run:

```plaintext
git diff
```

After staging:

```plaintext
git diff --cached
```

Compare two branches:

```plaintext
git diff main..projects
```

---

## **📍 Phase 6: Stash Workflow**

Make an unfinished change (don't commit).  
Then run:

```plaintext
git stash save "WIP footer"
git checkout main
git stash list
git stash pop
```

---

## **📍 Phase 7: Rebase**

Make a new branch:

```plaintext
git checkout -b ui-update
```

Modify design and commit twice.

Then:

```plaintext
git rebase main
```

---

## **📍 Phase 8: Cherry-Pick**

Find a commit hash from `log`:

```plaintext
git log --oneline
```

Pick one commit and apply it to another branch:

```plaintext
git cherry-pick <hash>
```

---

## **📍 Phase 9: Tags**

Create versions after major changes:

```plaintext
git tag -a v1.0 -m "First release"
git tag -a v2.0 -m "Final feature update"
```

Show tags:

```plaintext
git tag
```

---

## **📍 Phase 10: Squash Commits**

Create 3 tiny commits on a new branch.  
Then run:

```plaintext
git rebase -i HEAD~3
```

Mark 2 commits as **squash**.

---

## **📍 Phase 11: Merge Conflict Simulation**

Force a conflict:

1. Edit same line in `index.html` in two different branches.
    
2. Try merging.
    

Resolve markers:

```plaintext
<<<<<<< HEAD
Version A
=======
Version B
>>>>>>> other-branch
```

Commit resolved file.

---

## **📍 Phase 12: Revert**

Undo a bad commit without deleting history:

```plaintext
git revert <hash>
```

---

# 🏁 Final Goal

End with a Git repo containing:

✔ Branching  
✔ Rebasing  
✔ Merge conflicts  
✔ Stashing  
✔ Cherry-pick  
✔ Tags  
✔ Squashed commits

Final command:

```plaintext
git log --oneline --graph --all
```

---