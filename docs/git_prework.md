# Git Lab Pre-work

Before the **Lab 13-1: Git for Version Control and Collaboration in MLOps** session, please set up your machine and GitHub access so we can spend class time on Git workflows instead of installation issues.

**Official prerequisites for the lab:**

- **Git** installed on your computer  
- A **GitHub** account (free tier is fine)  
- **Visual Studio Code** installed (recommended for editing files and resolving merge conflicts)  
- Ability to authenticate to GitHub over **HTTPS** using a **personal access token** when Git prompts for a password (set this up ahead of time; see section 4)

If any of the above is missing, complete the checklist below **before** lab day.

---

## 1. Install Visual Studio Code

1. Download and install **VS Code** from the [Visual Studio Code download page](https://code.visualstudio.com/).  
2. After install, confirm it works: open a terminal and run `code --version`.

---

## 2. Install Git

1. Install Git for your OS from the [official Git downloads page (Windows, macOS, Linux)](https://git-scm.com/downloads).  
   - **Windows:** Use the official installer; choose **Git Bash** when prompted if you want a Unix-like shell for class examples.  
   - **macOS:** Xcode Command Line Tools include Git, or use the installer from git-scm.com / Homebrew.  
   - **Linux:** Use your package manager (e.g. `sudo apt install git` on Debian/Ubuntu).

2. Verify:

   ```bash
   git --version
   ```

3. Set your **name** and **email** (use your real name and the email you will use with GitHub—often your school email):

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

   More detail: [MSBA 6331 course page — Configure Git (user name, email, credential helper)](https://de4liu.github.io/bigdata/FAQs/configGit/).

---

## 3. Create a GitHub account and sign in

1. [Create a GitHub account or sign in](https://github.com/) if you do not already have one.  
2. Sign in in the browser and confirm you can see your profile.

You will use this account to create a **private** repository during the lab.

---

## 4. Connect to GitHub with HTTPS (personal access token)

We use **HTTPS** only (`https://github.com/...` URLs). When you `git push`, `git pull`, or `git clone` from the command line, Git will ask for credentials: use your **GitHub username** and, for the password, a **personal access token** (PAT)—not your GitHub account password.

1. Create a token in GitHub (**Settings** → **Developer settings** → **Personal access tokens**). Generate a token with at least **repo** scope for private repositories. For detailed steps, see [GitHub Docs: Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic). Copy the token once and store it somewhere safe (e.g. a password manager); you will not see it again.  
2. The first time Git prompts for a password, paste the **token**.  
3. Optional: cache credentials so you are not prompted every time—see [MSBA 6331 course page — Configure Git (credential helper)](https://de4liu.github.io/bigdata/FAQs/configGit/).

Example remote URL (this is what the lab uses):

```text
https://github.com/YOUR_USERNAME/lab13-1-git.git
```

Quick reference: [MSBA 6331 course page — Git cheatsheet](https://de4liu.github.io/bigdata/FAQs/cheatsheet_git/).

---

## 5. Quick pre-lab checklist

| Item | Check |
|------|--------|
| VS Code installs and opens | ☐ |
| `git --version` works | ☐ |
| `git config` name and email set | ☐ |
| GitHub account ready | ☐ |
| Personal access token created (repo access); you know how to paste it when Git asks for a password | ☐ |
| Optional: `credential.helper` set if you want fewer prompts | ☐ |

