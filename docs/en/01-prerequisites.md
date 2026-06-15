# Prerequisites

[← Back to README](../../README.md) | [Next: IBM i Setup →](02-ibm-i-setup.md)

---

## Introduction

Before you begin setting up IBM Bob and integrating with Git, ensure you have the necessary access, tools, and permissions. This guide will help you verify that your environment is ready for the setup process.

**Estimated Time**: 15-30 minutes for verification

---

## IBM i System Requirements

### Required Access Levels

You will need the following access to your IBM i system:

- **User Profile**: A valid IBM i user profile with appropriate permissions
- **Authority Levels**:
  - `*ALLOBJ` special authority (or equivalent permissions)
  - Authority to create directories in the IFS
  - Authority to install open source packages
  - Authority to configure NetServer (for network shares)

### Internet Connectivity

Your IBM i system must have:
- Active internet connection for downloading packages and cloning repositories
- Access to GitHub (https://github.com)
- Access to IBM's open source package repositories

### IBM i Version

- **Minimum Version**: IBM i 7.3 or higher
- **Recommended**: IBM i 7.4 or higher for best compatibility

**Verify your IBM i version:**
```bash
# From a 5250 session or SSH terminal
DSPSFWRSC
```

---

## Workstation Requirements

### Operating System Compatibility

This guide supports the following operating systems:
- **Windows**: Windows 10 or higher
- **macOS**: macOS 10.14 (Mojave) or higher
- **Linux**: Most modern distributions (Ubuntu, Fedora, RHEL, etc.)

### Required Software

#### 1. Visual Studio Code

IBM Bob runs as a VS Code extension.

**Installation:**
- Download from [https://code.visualstudio.com/](https://code.visualstudio.com/)
- Follow the installation wizard for your operating system

**Verify installation:**
```bash
code --version
```

#### 2. Git

Git is required for version control operations.

**Windows:**
```bash
winget install Git.Git
```

**macOS:**
```bash
brew install git
```

**Linux (Debian/Ubuntu):**
```bash
sudo apt-get install git
```

**Verify installation:**
```bash
git --version
```

Expected output: `git version 2.x.x` or higher

#### 3. Node.js (LTS Version)

Required for IBM Bob extension.

**Windows:**
```bash
winget install -e --id OpenJS.NodeJS.LTS
```

**macOS:**
```bash
brew install node@lts
```

**Linux (Debian/Ubuntu):**
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Verify installation:**
```bash
node --version
npm --version
```

Expected output: Node.js v18.x.x or higher

### Network Access Requirements

Ensure your workstation can:
- Access your IBM i system via IP address or hostname
- Connect to port 22 (SSH) on the IBM i system
- Connect to port 8472 (Code for IBM i default port)
- Access NetServer shares (port 445 for SMB/CIFS)

**Test connectivity:**
```bash
# Test SSH connection
ssh youruser@your-ibmi-system

# Test ping
ping your-ibmi-system
```

---

## Verification Checklist

Use this checklist to ensure all prerequisites are met:

### IBM i System
- [ ] IBM i version 7.3 or higher
- [ ] User profile with appropriate authorities
- [ ] Internet connectivity verified
- [ ] Can access 5250 session or SSH terminal

### Workstation
- [ ] Visual Studio Code installed and working
- [ ] Git installed (version 2.x or higher)
- [ ] Node.js installed (LTS version)
- [ ] Can connect to IBM i system via SSH
- [ ] Can ping IBM i system

### Accounts
- [ ] IBM account created (for IBM Bob login)
- [ ] GitHub account created (or ready to create)

---

## Verification Commands

Run these commands to verify your setup:

### On Your Workstation

```bash
# Check VS Code
code --version

# Check Git
git --version

# Check Node.js
node --version

# Check npm
npm --version

# Test IBM i connectivity
ping your-ibmi-system
ssh youruser@your-ibmi-system
```

### On IBM i System

```bash
# Check IBM i version
system "DSPSFWRSC"

# Check if you can create directories
mkdir -p /tmp/test-dir
ls -la /tmp/test-dir
rmdir /tmp/test-dir

# Check internet connectivity
ping github.com
```

---

## What If Something Is Missing?

### Git Not Installed
- **Windows**: Use `winget install Git.Git` or download from [git-scm.com](https://git-scm.com)
- **macOS**: Install via Homebrew or download from [git-scm.com](https://git-scm.com)
- **Linux**: Use your distribution's package manager

### Node.js Not Installed
- **Windows**: Use `winget install -e --id OpenJS.NodeJS.LTS`
- **macOS**: Use Homebrew: `brew install node@lts`
- **Linux**: Follow instructions at [nodejs.org](https://nodejs.org)

### Cannot Connect to IBM i
- Verify the hostname or IP address is correct
- Check firewall settings on both workstation and IBM i
- Verify SSH service is running on IBM i: `NETSTAT *CNN`
- Contact your system administrator for network access

### Insufficient IBM i Permissions
- Contact your IBM i system administrator
- Request the necessary authorities listed above
- Explain you need to set up source code management on the IFS

---

## Next Steps

Once all prerequisites are verified:

1. **Continue to IBM i Setup**: [02-ibm-i-setup.md](02-ibm-i-setup.md)
2. **Review the complete documentation structure**: [README](../../README.md)

---

## Additional Resources

- [IBM i Access Client Solutions](https://www.ibm.com/support/pages/ibm-i-access-client-solutions)
- [Git Documentation](https://git-scm.com/doc)
- [Node.js Documentation](https://nodejs.org/docs/)
- [VS Code Documentation](https://code.visualstudio.com/docs)

---

**Last Updated**: June 2026