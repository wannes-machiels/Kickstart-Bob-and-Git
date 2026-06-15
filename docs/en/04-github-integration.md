# GitHub Integration

[← Back to README](../../README.md) | [← Previous: Source Migration](03-source-migration.md) | [Next: Network Access →](05-network-access.md)

---

## Introduction

This guide walks you through setting up GitHub and connecting your IBM i source code repository for version control. You'll create a GitHub account, set up a repository, and establish the connection between your IBM i system and GitHub.

**What You'll Do:**

- Create a GitHub account (if needed)
- Create a GitHub repository
- Initialize Git in your sources directory
- Connect to GitHub and push your code
- Set up authentication

**Estimated Time**: 30-45 minutes

---

## Why Use GitHub?

### Benefits of GitHub Integration

- **Version Control**: Track every change to your code
- **Collaboration**: Work with team members easily
- **Backup**: Cloud-based backup of your source code
- **Code Review**: Pull request workflow for quality control
- **CI/CD**: Automate builds and deployments
- **Documentation**: Wiki and README support
- **Issue Tracking**: Built-in bug and feature tracking

---

## Step 1: Create a GitHub Account

If you already have a GitHub account, skip to [Step 2](#step-2-create-a-github-repository).

### Sign Up for GitHub

1. Navigate to [https://github.com](https://github.com) in your web browser

2. Click **Sign up** in the top right corner

3. Enter your details:
   - **Email address**: Use your work or personal email
   - **Password**: Create a strong password
   - **Username**: Choose a professional username

4. Complete the verification puzzle

5. Click **Create account**

### Verify Your Email

1. Check your email inbox for a verification message from GitHub

2. Click the verification link in the email

3. You'll be redirected to GitHub and your account will be activated

### Set Up Your Profile (Optional but Recommended)

1. Click your profile icon in the top right

2. Select **Settings**

3. Add:
   - Profile picture
   - Bio
   - Company name
   - Location

---

## Step 2: Create a GitHub Repository

### Create a New Repository

1. Log in to GitHub

2. Click the **+** icon in the top right corner

3. Select **New repository**

4. Configure your repository:

   **Repository name**: Choose a descriptive name

   ```txt
   Examples:
   - ibmi-sources
   - mycompany-rpg
   - erp-system
   ```

   **Description** (optional): Brief description of your project

   ```txt
   Example: "IBM i RPG and CL source code for ERP system"
   ```

   **Visibility**:
   - **Public**: Anyone can see (good for open source)
   - **Private**: Only you and collaborators can see (recommended for business code)

   **Initialize repository**:
   - ❌ Do NOT check "Add a README file"
   - ❌ Do NOT add .gitignore
   - ❌ Do NOT choose a license

   (We'll push existing code, so we want an empty repository)

5. Click **Create repository**

### Copy the Repository URL

After creation, you'll see a page with setup instructions. Copy the repository URL:

**HTTPS format** (recommended for beginners):

```txt
https://github.com/yourusername/ibmi-sources.git
```

**SSH format** (for advanced users with SSH keys):

```txt
git@github.com:yourusername/ibmi-sources.git
```

Keep this URL handy - you'll need it soon.

---

## Step 3: Initialize Git in Your Sources Directory

Now we'll set up Git in your migrated sources directory on IBM i.

### Navigate to Your Sources Directory

```bash
# SSH into your IBM i system
ssh youruser@your-ibmi-system

# Navigate to sources
cd /home/youruser/sources
```

### Initialize Git Repository

```bash
# Initialize Git
git init

# Verify initialization
ls -la .git
```

**Expected Output**:

```bash
drwxr-xr-x  7 youruser  0  Jun 15 08:00 .git
```

### Configure Git Identity

Git needs to know who you are for commit attribution:

```bash
# Set your name (use your real name)
git config user.name "John Smith"

# Set your email (use the same email as your GitHub account)
git config user.email "john.smith@company.com"

# Verify configuration
git config --list
```

**Expected Output**:

```bash
user.name=John Smith
user.email=john.smith@company.com
```

---

## Step 4: Prepare Your First Commit

### Create a .gitignore File

Tell Git which files to ignore:

```bash
cat > .gitignore << 'EOF'
# IBM i specific
*.MODULE
*.PGM
*.SRVPGM
*.lib

# Compiled objects
*.o
*.so

# Logs
*.log

# Temporary files
*~
*.tmp
.DS_Store
EOF
```

### Create a README

Document your project:

```bash
cat > README.md << 'EOF'
# IBM i Source Code Repository

This repository contains the source code for our IBM i applications.

## Structure

- `QRPGLESRC/` - RPG ILE programs
- `QCLSRC/` - CL programs
- `QDDSSRC/` - DDS files

## Development

Source code is maintained on the IFS and version controlled with Git.

## Last Migration

- Date: 2026-06-15
- Source Library: YOURLIB
EOF
```

### Check Repository Status

```bash
git status
```

**Expected Output**:

```bash
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        README.md
        QRPGLESRC/
        QCLSRC/
        QDDSSRC/
```

### Stage All Files

```bash
# Add all files to staging area
git add .

# Verify what's staged
git status
```


```bash
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitignore
        new file:   README.md
        new file:   QRPGLESRC/PROGRAM1.rpgle
        ...
```

### Create Your First Commit

```bash
# Commit with a descriptive message
git commit -m "Initial commit: Migrate source code from IBM i library YOURLIB"
```

**Expected Output**:

```bash
[main (root-commit) a1b2c3d] Initial commit: Migrate source code from IBM i library YOURLIB
 150 files changed, 15000 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100644 QRPGLESRC/PROGRAM1.rpgle
 ...
```

---

## Step 5: Connect to GitHub

### Add GitHub as Remote

```bash
# Add remote repository (replace with your URL)
git remote add origin https://github.com/yourusername/ibmi-sources.git

# Verify remote was added
git remote -v
```

**Expected Output**:

```bash
origin  https://github.com/yourusername/ibmi-sources.git (fetch)
origin  https://github.com/yourusername/ibmi-sources.git (push)
```

### Set Default Branch Name

```bash
# Rename branch to 'main' (GitHub's default)
git branch -M main
```

---

## Step 6: Set Up Authentication

GitHub requires authentication for pushing code. You have two options:

### Option 1: Personal Access Token (Recommended)

GitHub no longer accepts passwords for HTTPS authentication. You need a Personal Access Token (PAT).

#### Create a Personal Access Token

1. Go to GitHub and click your profile icon

2. Select **Settings**

3. Scroll down and click **Developer settings** (left sidebar)

4. Click **Personal access tokens** → **Tokens (classic)**

5. Click **Generate new token** → **Generate new token (classic)**

6. Configure the token:
   - **Note**: "IBM i Development"
   - **Expiration**: Choose duration (90 days recommended)
   - **Scopes**: Check these boxes:
     - ✅ `repo` (Full control of private repositories)
     - ✅ `workflow` (if using GitHub Actions)

7. Click **Generate token**

8. **IMPORTANT**: Copy the token immediately - you won't see it again!

   ```bash
   Example: ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```

#### Store the Token

```bash
# Configure Git to cache credentials
git config --global credential.helper store

# Or cache for 1 hour
git config --global credential.helper 'cache --timeout=3600'
```

### Option 2: SSH Keys (Advanced)

For advanced users who prefer SSH authentication.

#### Generate SSH Key

```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "your.email@company.com"

# Press Enter to accept default location
# Enter a passphrase (optional but recommended)

# Display public key
cat ~/.ssh/id_ed25519.pub
```

#### Add SSH Key to GitHub

1. Copy the public key output

2. Go to GitHub → **Settings** → **SSH and GPG keys**

3. Click **New SSH key**

4. Paste your public key and give it a title

5. Click **Add SSH key**

#### Update Remote URL to SSH

```bash
# Change remote URL to SSH format
git remote set-url origin git@github.com:yourusername/ibmi-sources.git

# Verify
git remote -v
```

---

## Step 7: Push Your Code to GitHub

### Push to GitHub

```bash
# Push code to GitHub
git push -u origin main
```

**If using HTTPS with PAT:**

- Username: Your GitHub username
- Password: Paste your Personal Access Token (not your GitHub password!)

**Expected Output**:

```bash
Enumerating objects: 150, done.
Counting objects: 100% (150/150), done.
Delta compression using up to 4 threads
Compressing objects: 100% (145/145), done.
Writing objects: 100% (150/150), 1.5 MiB | 500 KiB/s, done.
Total 150 (delta 20), reused 0 (delta 0)
To https://github.com/yourusername/ibmi-sources.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### Verify on GitHub

1. Go to your repository on GitHub: `https://github.com/yourusername/ibmi-sources`

2. You should see:
   - Your README.md displayed
   - All your source directories
   - Your initial commit message
   - File count and repository size

---

## Step 8: Daily Git Workflow

Now that everything is set up, here's your daily workflow:

### Making Changes

```bash
# 1. Navigate to your sources
cd /home/youruser/sources

# 2. Make changes to your files
# (edit files using your preferred editor)

# 3. Check what changed
git status
git diff

# 4. Stage changes
git add .
# Or stage specific files
git add QRPGLESRC/PROGRAM1.rpgle

# 5. Commit changes
git commit -m "Fix: Correct calculation in PROGRAM1"

# 6. Push to GitHub
git push
```

### Getting Latest Changes

```bash
# Pull latest changes from GitHub
git pull
```

### Viewing History

```bash
# View commit history
git log --oneline

# View detailed history
git log

# View changes in a specific commit
git show <commit-hash>
```

---

## Best Practices

### Commit Messages

Write clear, descriptive commit messages:

✅ **Good Examples**:

```bash
git commit -m "Add customer validation to order entry program"
git commit -m "Fix: Correct date calculation in INVPGM"
git commit -m "Refactor: Extract common code to UTILPGM"
```

❌ **Bad Examples**:

```bash
git commit -m "fix"
git commit -m "update"
git commit -m "changes"
```

### Commit Frequency

- Commit often (multiple times per day)
- Each commit should be a logical unit of work
- Don't commit broken code to main branch

### Branch Strategy

For team development, use branches:

```bash
# Create a feature branch
git checkout -b feature/new-report

# Work on your feature
# ... make changes ...

# Commit changes
git add .
git commit -m "Add new sales report"

# Push feature branch
git push -u origin feature/new-report

# Create Pull Request on GitHub for review
```

---

## Troubleshooting

### Authentication Failed

**Error**: `Authentication failed for 'https://github.com/...'`

**Solution**:

- Ensure you're using a Personal Access Token, not your password
- Verify the token has `repo` scope
- Check if token has expired

### Push Rejected

**Error**: `! [rejected] main -> main (fetch first)`

**Solution**:

```bash
# Pull latest changes first
git pull origin main

# Resolve any conflicts if needed

# Push again
git push
```

### Large Files Warning

**Error**: `remote: warning: Large files detected`

**Solution**:

```bash
# Add large files to .gitignore
echo "*.SAVF" >> .gitignore
echo "*.lib" >> .gitignore

# Remove from Git tracking
git rm --cached large-file.SAVF

# Commit the change
git commit -m "Remove large files from tracking"
```

### SSL Certificate Error

**Error**: `SSL certificate problem`

**Solution**:

```bash
# Update CA certificates
yum install ca-certificates-mozilla

# Or temporarily disable SSL verification (not recommended)
git config --global http.sslVerify false
```

---

## Next Steps

Now that your code is on GitHub:

1. **Set up network access**: [05-network-access.md](05-network-access.md)
2. **Configure IBM Bob**: [06-ibm-bob-configuration.md](06-ibm-bob-configuration.md)
3. **Learn more about Git**: [How to Git Good](../../docs/nl/How-to-Git-good.md) (comprehensive Dutch guide)

---

## Additional Resources

- [GitHub Documentation](https://docs.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Pro Git Book](https://git-scm.com/book/en/v2) (free online)
- [GitHub Skills](https://skills.github.com/) (interactive tutorials)

---

**Last Updated**: June 2026
