---
title: "DevLog: The Production Panic & The Perfect Hotfix Protocol"
datePublished: Fri Dec 12 2025 15:58:35 GMT+0000 (Coordinated Universal Time)
cuid: cmj31xhv2000002lbado48zbn
slug: devlog-the-production-panic-and-the-perfect-hotfix-protocol
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765554995388/08e65044-eb1b-49b8-89a4-92053664b37a.png

---

It’s 4:55 PM on a Friday. You’ve just closed your laptop. Then, the Slack notification sound triggers.

> **Release Manager:** *"We have a P0 in PROD. The Login page is crashing. We need a hotfix tag ASAP."*

We’ve all been there. The adrenaline spikes, and suddenly you need to navigate the delicate dance of fixing a bug without breaking anything else.

Today, I’m documenting our exact **Hotfix Scenario workflow**. This is how we go from a critical failure to a tagged release, ensuring our Infra Team and Disaster Recovery (DR) environments stay in sync.

---

## The Scenario

The Bug: A null error in the login controller.

The Goal: Patch main, deploy to Production, and ensure dev, qa, ppd, and dr are updated so the bug doesn't come back in the next release.

Here is the step-by-step Git survival guide.

### 1\. The Setup (Branch from Main)

Rule #1 of a hotfix: Always cut from the stable source of truth. We aren't branching from dev; we are branching from `main`.

Bash

```
# Get the latest stable code
git checkout main
git pull origin main

# Create the panic-branch
git checkout -b hotfix/login-crash
```

### 2\. The Fix

I find the null pointer, patch it, and verify it locally. Once I'm confident, I commit.

Bash

```
git commit -am "Fix null error in login controller"
```

### 3\. The Push

Time to get this into the remote repository so the team can see it.

Bash

```
git push origin hotfix/login-crash
```

### 4\. Merge & Tag (The Critical Step)

Once the PR is approved (even in a panic, get a code review!), we merge it back to `main`. The Release Manager is waiting for a specific tag to trigger the "Secure Build."

Bash

```
git checkout main
git merge hotfix/login-crash

# Tagging is the trigger for Prod deployment
git tag -a v2.3.1 -m "Login hotfix"
git push origin main --tags
```

At this exact moment, our **CI/CD Automation** kicks in. The tag `v2.3.1` triggers the Production Deploy pipeline.

### 5\. The "Back-Merge" (Don't Forget This!)

This is where many teams fail. If you fix it in `main` but forget the lower environments, you will overwrite this fix in the next release cycle. We have to propagate the fix *everywhere*, including our Disaster Recovery (`dr`) branch.

Bash

```plaintext
# Update Dev
git checkout dev && git merge hotfix/login-crash && git push

# Update QA
git checkout qa && git merge hotfix/login-crash && git push

# Update Pre-Prod
git checkout ppd && git merge hotfix/login-crash && git push

# Update Disaster Recovery (Crucial for Infra Team)
git checkout dr && git merge hotfix/login-crash && git push
```

---

## Behind the Scenes: The Automation Matrix

While I’m sipping water and recovering from the adrenaline rush, our CI/CD pipelines are doing the heavy lifting based on the triggers we set up.

Here is how our triggers map to actions:

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>Branch / Type</strong></p></td><td colspan="1" rowspan="1"><p><strong>Trigger</strong></p></td><td colspan="1" rowspan="1"><p><strong>Actions Taken</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>dev</strong></p></td><td colspan="1" rowspan="1"><p>On Push</p></td><td colspan="1" rowspan="1"><p>Unit tests, Build, Deploy to Dev env</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>qa</strong></p></td><td colspan="1" rowspan="1"><p>On Merge</p></td><td colspan="1" rowspan="1"><p>Build, QA Deploy, Run full test suite</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>ppd</strong></p></td><td colspan="1" rowspan="1"><p>On Merge</p></td><td colspan="1" rowspan="1"><p>Build, Staging Deploy, Load/UAT testing</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>main</strong></p></td><td colspan="1" rowspan="1"><p><strong>On Tag</strong></p></td><td colspan="1" rowspan="1"><p><strong>Secure Build, Production Deploy</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>dr</strong></p></td><td colspan="1" rowspan="1"><p>On Merge</p></td><td colspan="1" rowspan="1"><p>DR deploy, Run DR simulation</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>hotfix/*</strong></p></td><td colspan="1" rowspan="1"><p>Push/Merge</p></td><td colspan="1" rowspan="1"><p>CI + Deploy (if urgent), Tagging required</p></td></tr></tbody></table>

---

## Conclusion

The Login page is back up. The Release Manager is happy. The Infra Team confirms that the `dr` branch is synced and ready for any worst-case scenarios.

Handling a hotfix isn't just about writing code fast; it's about following a protocol that ensures stability across the entire ecosystem.

**Keep calm and** `git push` on.

---