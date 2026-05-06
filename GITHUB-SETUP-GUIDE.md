# Publishing Reliquery to GitHub — Complete Walkthrough

This is a step-by-step guide for getting the Reliquery repo from your computer onto GitHub, written for someone who has never used Git or GitHub before. Every command is Windows-specific.

---

## Part 1: Create a GitHub Account

1. Go to [github.com](https://github.com) and click **Sign up**
2. Enter your email, create a password, and choose a username
   - Your username becomes part of your repo URL (e.g., `github.com/yourname/reliquery`), so pick something you're comfortable sharing publicly
3. Complete the verification and email confirmation steps
4. You're in. GitHub will try to walk you through a tutorial — you can skip it

---

## Part 2: Install Git

Git is the version control tool that tracks changes and pushes them to GitHub. GitHub is the website; Git is the engine.

1. Go to [git-scm.com/downloads/win](https://git-scm.com/downloads/win)
2. Download the **Standalone Installer** (64-bit)
3. Run the installer. The defaults are fine for every screen — just click Next through the entire wizard. The only setting worth changing:
   - **Default editor**: Switch from Vim to **Notepad** or **VS Code** if you have it. This is the editor Git opens when you need to write a commit message. Vim is notoriously confusing if you've never used it.
4. After installation completes, open a new **PowerShell** window (search "PowerShell" in the Start menu) and type:

```powershell
git --version
```

If you see something like `git version 2.47.1.windows.1`, you're set.

---

## Part 3: Configure Git

Git needs to know who you are so it can label your commits. Run these two commands in PowerShell, replacing the placeholders with your actual info:

```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Use the same email you used for your GitHub account.

---

## Part 4: Create the Repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. Fill in:
   - **Repository name**: `reliquery`
   - **Description**: `AI memory and context management for worldbuilding, co-writing, and roleplaying`
   - **Public** (so you can share the link on Reddit)
   - **Do NOT** check "Add a README file" — we already have one
   - **Do NOT** add a .gitignore or license — we already have those too
3. Click **Create repository**
4. GitHub will show you a page with setup instructions. Keep this page open — you'll need the URL it shows you (it'll look like `https://github.com/yourname/reliquery.git`)

---

## Part 5: Initialize and Push

Now you connect the files on your computer to the GitHub repository. Open PowerShell and navigate to the folder where the repo files are:

```powershell
cd "E:\Claude Workspace\Worldforge\Worldforge\reliquery-repo"
```

Then run these commands one at a time:

```powershell
# Initialize a Git repository in this folder
git init

# Stage all files for the first commit
git add .

# Create the first commit
git commit -m "Initial release: Reliquery v1.0.0 — AI memory system for writers"

# Rename the default branch to 'main' (GitHub's standard)
git branch -M main

# Connect your local repo to GitHub (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/reliquery.git

# Push everything to GitHub
git push -u origin main
```

### Authentication

When you run `git push`, Git will ask you to authenticate with GitHub. A browser window should pop up asking you to authorize Git. Click **Authorize** and you're done.

If the browser flow doesn't work, you'll need a **Personal Access Token**:

1. Go to [github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **Generate new token (classic)**
3. Give it a name like "reliquery-push"
4. Check the **repo** scope (full control of private repositories)
5. Click **Generate token**
6. **Copy the token immediately** — GitHub won't show it again
7. When Git asks for your password, paste the token instead of your actual password

---

## Part 6: Verify

Go to `https://github.com/YOUR_USERNAME/reliquery` in your browser. You should see all your files, the README rendered nicely at the bottom, and the repo ready to share.

---

## Part 7: Share on Reddit

Your link for the r/writingwithAI megathread is:

```
https://github.com/YOUR_USERNAME/reliquery
```

When posting, consider a brief description like:

> **Reliquery** — persistent AI memory for writers who build worlds. An open-source plugin for Claude that gives it long-term recall of your characters, factions, locations, and storylines across sessions. Structured vault files + semantic search + knowledge graph. Free, local-first, MIT licensed. [GitHub link]

---

## Ongoing: Making Changes After the Initial Push

Once the repo is live, updating it follows a simple three-step pattern:

```powershell
# 1. Stage your changes
git add .

# 2. Commit with a message describing what changed
git commit -m "Updated chronicle skill with better chunking logic"

# 3. Push to GitHub
git push
```

That's it. The changes appear on GitHub within seconds.

### Useful Commands

| Command | What It Does |
|---------|-------------|
| `git status` | Shows which files have been changed since the last commit |
| `git diff` | Shows the actual changes line-by-line |
| `git log --oneline` | Shows commit history, one line per commit |
| `git add filename` | Stages a specific file instead of everything |

---

## Troubleshooting

**"fatal: not a git repository"** — You're in the wrong folder. Make sure you `cd` into the `reliquery-repo` directory first.

**"error: failed to push some refs"** — Usually means you checked one of the boxes on GitHub (README, .gitignore, license) when creating the repo, which created a conflicting commit. Easiest fix: delete the repo on GitHub (Settings → Danger Zone → Delete), recreate it with all boxes unchecked, and push again.

**"Permission denied"** — Authentication issue. Try the Personal Access Token approach described in Part 5.

**"Updates were rejected because the remote contains work that you do not have locally"** — Same as the "failed to push" error above. Either pull first (`git pull origin main --allow-unrelated-histories`) or delete and recreate the repo.
