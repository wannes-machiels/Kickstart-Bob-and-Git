# Quick Start Guide

[← Back to README](../../README.md)

---

## Introduction

This is a fast-track guide for experienced IBM i developers who want to get up and running quickly. If you need detailed explanations, use the [full documentation](../../README.md).

**Time to Complete**: 30-45 minutes

**Prerequisites**: You should be comfortable with IBM i commands, Git basics, and have administrator access to your IBM i system.

---

## Prerequisites Checklist

Before starting, verify you have:

- [ ] IBM i system (7.3 or higher) with SSH access
- [ ] User profile with `*ALLOBJ` authority
- [ ] Internet connectivity on IBM i
- [ ] IBM Bob installed on workstation
- [ ] Git installed on workstation
- [ ] Node.js LTS installed on workstation
- [ ] IBM account for IBM Bob
- [ ] GitHub account

**Quick Verification**:

```bash
# On workstation
code --version && git --version && node --version

# On IBM i (via SSH)
ssh youruser@your-ibmi-system "git --version && gmake --version"
```

---

## Step 1: IBM i Setup (10 minutes)

### Create Directories

```bash
ssh youruser@your-ibmi-system

# Create directories
mkdir -p /home/youruser/sources
mkdir -p /home/youruser/migrate

# Verify
ls -la /home/youruser/
```

### Install Required Packages

```bash
# Install if missing
yum install git make-gnu bash

# Verify
git --version
gmake --version
bash --version
```

### Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global init.defaultBranch main
```

---

## Step 2: Migrate Source Code (15 minutes)

### Clone and Build Migrate Tool

```bash
cd /home/youruser/migrate
git clone https://github.com/worksofliam/migrate.git
cd migrate/migrate
gmake
```

### Run Migration

```bash
# Migrate your library (replace YOURLIB with your library name)
./migrate -l YOURLIB -d /home/youruser/sources

# Verify migration
cd /home/youruser/sources
ls -la
```

---

## Step 3: GitHub Setup (10 minutes)

### Initialize Git Repository

```bash
cd /home/youruser/sources

# Initialize Git
git init

# Create .gitignore
cat > .gitignore << 'EOF'
*.MODULE
*.PGM
*.SRVPGM
*.lib
*.o
*.log
*~
.DS_Store
EOF

# Create README
cat > README.md << 'EOF'
# IBM i Source Code

Source code migrated from IBM i library YOURLIB.

## Structure
- QRPGLESRC/ - RPG programs
- QCLSRC/ - CL programs
- QDDSSRC/ - DDS files
EOF

# Stage and commit
git add .
git commit -m "Initial commit: Migrate source from YOURLIB"
```

### Connect to GitHub

```bash
# Create repository on GitHub first, then:
git remote add origin https://github.com/yourusername/your-repo.git
git branch -M main
git push -u origin main
```

**Authentication**: Use Personal Access Token (not password)

- GitHub → Settings → Developer settings → Personal access tokens
- Generate token with `repo` scope
- Use token as password when pushing

---

## Step 4: Network Access (5 minutes)

### Start NetServer

```bash
# From 5250 session
STRTCPSVR SERVER(*NETSVR)
CHGNTSVRA AUTOSTART(*YES)
```

### Create Share (via Navigator for i)

1. Open: `http://your-ibmi-system:2001`
2. Network → Servers → TCP/IP → NetServer → Shares
3. New Share:
   - Name: `SOURCES`
   - Path: `/home/youruser/sources`
   - Permissions: Read/Write

### Connect from Workstation

**Windows**:

```bash
\\your-ibmi-system\SOURCES
Map as Z: drive
```

**macOS**:

```bash
smb://your-ibmi-system/SOURCES
Cmd+K in Finder
```

**Linux**:

```bash
sudo mount -t cifs //your-ibmi-system/SOURCES /mnt/ibmi-sources -o username=youruser
```

---

## Step 5: IBM Bob Setup (10 minutes)

### Install Extensions

1. Open IBM Bob
2. Install extension:
   - **Code for IBM i**

### Sign In to IBM Bob

1. Click IBM Bob icon
2. Sign in with IBM account
3. Verify authentication

### Connect to IBM i

1. Click IBM i icon
2. Add new connection:
   - Name: `DEV System`
   - Host: `your-ibmi-system`
   - Port: `22`
   - Username: `youruser`
3. Connect and enter password

### Configure Settings

```json
// In VS Code settings or .vscode/settings.json
{
  "code-for-ibmi.connectionSettings": {
    "sourceDirectory": "/home/youruser/sources",
    "currentLibrary": "YOURLIB",
    "libraryList": ["YOURLIB", "QGPL"]
  }
}
```

---

## Step 6: Verify Setup (5 minutes)

### Complete Verification

```bash
# 1. Open source file in VS Code
# File → Open Folder → Z:\ (or network share)

# 2. Edit a file
# Make a small change and save

# 3. Compile (right-click → Compile)

# 4. Commit to Git
cd /home/youruser/sources
git add .
git commit -m "Test change"
git push

# 5. Ask IBM Bob
# Open IBM Bob chat: "Explain what this program does"
```

### Verification Checklist

- [ ] Can browse IFS in IBM Bob
- [ ] Can edit source files
- [ ] Can compile programs
- [ ] Can commit to Git
- [ ] Can push to GitHub
- [ ] IBM Bob responds to queries

---

## Daily Workflow

```bash
# 1. Pull latest changes
cd /home/youruser/sources
git pull

# 2. Edit files in VS Code
# Open via network share or SSH remote

# 3. Compile and test
# Right-click → Compile in VS Code

# 4. Commit changes
git add .
git commit -m "Description of changes"
git push

# 5. Use IBM Bob for help
# Ask questions, generate code, get explanations
```

---

## Common Commands Reference

### Git Commands

```bash
git status                    # Check status
git add .                     # Stage all changes
git commit -m "message"       # Commit changes
git push                      # Push to GitHub
git pull                      # Pull from GitHub
git log --oneline            # View history
git diff                     # View changes
```

### IBM i Commands

```bash
# From 5250
WRKLIB LIB(YOURLIB)          # Work with library
WRKLNK OBJ('/home/youruser') # Work with IFS
STRTCPSVR SERVER(*NETSVR)    # Start NetServer
NETSTAT *CNN                 # Check connections

# From SSH
ls -la                       # List files
cd /path                     # Change directory
cat file.rpgle              # View file
```

### VS Code Shortcuts

```bash
Ctrl+Shift+P    # Command palette
Ctrl+`          # Toggle terminal
Ctrl+B          # Toggle sidebar
Ctrl+P          # Quick file open
Ctrl+Shift+F    # Search in files
```

---

## Troubleshooting Quick Fixes

### Git push fails

```bash
# Use Personal Access Token, not password
# GitHub → Settings → Developer settings → Tokens
```

### gmake not found

```bash
yum install make-gnu
```

### Cannot connect to network share

```bash
# Verify NetServer is running
STRTCPSVR SERVER(*NETSVR)
```

### Compilation fails

```bash
# Check library list in Code for IBM i settings
# Verify current library is set
```

### IBM Bob not responding

```bash
# Sign out and back in
# Check internet connection
# Restart VS Code
```

---

## Next Steps

### Learn More

- **Full Documentation**: [README](../../README.md)
- **Detailed Guides**: [docs/en/](.)
- **Troubleshooting**: [troubleshooting.md](troubleshooting.md)
- **Git Guide**: [How to Git Good](../nl/How-to-Git-good.md) (Dutch)

### Explore IBM Bob Features

```bash
# Try these prompts with IBM Bob:

"Generate an RPG ILE program that reads CUSTFILE and creates a report"

"Explain this code and suggest improvements: [paste code]"

"Convert this RPG/400 program to free-format RPG ILE"

"Help me debug this error: CPF2105"

"Create documentation for this program"
```

### Best Practices

1. **Commit often**: Multiple times per day
2. **Write clear commit messages**: Describe what and why
3. **Use branches**: For features and fixes
4. **Review before pushing**: Check your changes
5. **Pull before starting work**: Stay synchronized
6. **Ask Bob for help**: Leverage AI assistance
7. **Test thoroughly**: Don't commit broken code

---

## Resources

### Documentation

- [IBM Bob Docs](https://bob.ibm.com/docs)
- [Code for IBM i Docs](https://codefori.github.io/docs/)
- [Git Documentation](https://git-scm.com/doc)

### Community

- [IBM Community](https://community.ibm.com/)
- [Code for IBM i Discord](https://discord.gg/codeforibmi)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-midrange)

### Support

- [IBM Bob Support](https://bob.ibm.com/support)
- [GitHub Help](https://docs.github.com/)

---

## Congratulations! 🎉

You're now set up with:

- ✅ Source code on IFS
- ✅ Git version control
- ✅ GitHub integration
- ✅ Network access from workstation
- ✅ IBM Bob AI assistance
- ✅ Modern development workflow

**Happy coding!**

---

**Last Updated**: June 2026
