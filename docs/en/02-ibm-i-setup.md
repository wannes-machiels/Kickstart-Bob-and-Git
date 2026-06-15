# IBM i System Setup

[← Back to README](../../README.md) | [← Previous: Prerequisites](01-prerequisites.md) | [Next: Source Migration →](03-source-migration.md)

---

## Introduction

This guide walks you through configuring your IBM i system for modern source code management. You'll create IFS directories, verify open source tools, and install any missing packages.

**What You'll Configure:**
- IFS directories for source code and tools
- Open source package environment
- Required development tools (Git, make-gnu, bash)

**Estimated Time**: 30-45 minutes

---

## Step 1: Create IFS Directories

You need two main directories on the IFS:
1. **Sources directory**: Where your migrated source code will live
2. **Migrate directory**: Where the migration tool will be installed

### Using 5250 Session

```bash
# Create sources directory
MKDIR DIR('/home/youruser/sources')

# Create migrate directory  
MKDIR DIR('/home/youruser/migrate')

# Verify directories were created
WRKLNK OBJ('/home/youruser/*')
```

### Using SSH Terminal

```bash
# Create both directories at once
mkdir -p /home/youruser/sources
mkdir -p /home/youruser/migrate

# Verify creation
ls -la /home/youruser/
```

**Replace `youruser` with your actual IBM i user profile name.**

### Recommended Directory Structure

```
/home/youruser/
├── sources/          # Your source code (Git repository)
│   ├── QRPGLESRC/   # RPG source files
│   ├── QCLSRC/      # CL source files
│   ├── QDDSSRC/     # DDS source files
│   └── ...
└── migrate/          # Migration tool
    └── migrate/      # Cloned repository
```

### Setting Permissions

Ensure you have proper permissions:

```bash
# Check current permissions
ls -la /home/youruser/

# Set permissions if needed (from SSH)
chmod 755 /home/youruser/sources
chmod 755 /home/youruser/migrate

# Or from 5250
CHGAUT OBJ('/home/youruser/sources') USER(YOURUSER) DTAAUT(*RWX) OBJAUT(*ALL)
```

---

## Step 2: Verify Open Source Tools

IBM i includes an open source ecosystem. You need to verify which tools are already installed.

### Method 1: Using Navigator for i (Recommended)

1. Open a web browser and navigate to your IBM i Navigator for i:
   ```
   http://your-ibmi-system:2001
   ```

2. Log in with your credentials

3. In the left navigation panel:
   - Expand **IBM i Management**
   - Click **Open Source Package Management**

4. You'll see a list of installed packages with versions

5. Use the search function to look for:
   - `git`
   - `make-gnu`
   - `bash`
   - `python3` (optional but recommended)

### Method 2: Using Command Line

```bash
# Check if yum is available
yum --version

# List installed packages
yum list installed

# Search for specific packages
yum list installed | grep git
yum list installed | grep make-gnu
yum list installed | grep bash
```

### Required Package Versions

| Package | Minimum Version | Purpose |
|---------|----------------|---------|
| `git` | 2.x or higher | Version control |
| `make-gnu` | 4.x or higher | Building migrate tool |
| `bash` | 4.x or higher | Shell scripting |

---

## Step 3: Install Missing Packages

If any required packages are missing, install them using one of these methods.

### Method 1: Navigator for i GUI

1. In **Open Source Package Management**, click the **Available Packages** tab

2. Search for the package you need (e.g., `make-gnu`)

3. Select the checkbox next to the package

4. Click **Install** at the top of the page

5. Wait for installation to complete (may take several minutes)

6. Verify installation in the **Installed Packages** tab

### Method 2: Command Line (yum)

```bash
# Install Git
yum install git

# Install make-gnu
yum install make-gnu

# Install bash
yum install bash

# Install multiple packages at once
yum install git make-gnu bash

# Update all packages (optional)
yum update
```

### Verify Installations

After installing packages, verify they work:

```bash
# Check Git
git --version
# Expected: git version 2.x.x

# Check make
gmake --version
# Expected: GNU Make 4.x

# Check bash
bash --version
# Expected: GNU bash, version 4.x.x
```

---

## Step 4: Configure Git on IBM i

Set up your Git identity on the IBM i system:

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

### Optional Git Configuration

```bash
# Set default branch name to 'main'
git config --global init.defaultBranch main

# Enable colored output
git config --global color.ui auto

# Set default editor (optional)
git config --global core.editor nano
```

---

## Step 5: Test Internet Connectivity

Verify your IBM i system can reach GitHub:

```bash
# Test DNS resolution
ping github.com

# Test HTTPS connectivity
curl -I https://github.com

# Test Git clone (small test repository)
cd /tmp
git clone https://github.com/git/git-reference.git
rm -rf git-reference
```

If any of these fail, check:
- Firewall settings
- Proxy configuration
- DNS settings
- Internet gateway configuration

---

## Step 6: Verification Checklist

Before proceeding to the next step, verify:

- [ ] `/home/youruser/sources` directory exists
- [ ] `/home/youruser/migrate` directory exists
- [ ] Git is installed and working (`git --version`)
- [ ] make-gnu is installed and working (`gmake --version`)
- [ ] bash is installed and working (`bash --version`)
- [ ] Git identity is configured (`git config --list`)
- [ ] Can ping github.com
- [ ] Can clone from GitHub

---

## Troubleshooting

### "yum: command not found"

The open source environment may not be initialized. Try:

```bash
# Add to PATH
export PATH=/QOpenSys/pkgs/bin:$PATH

# Or use full path
/QOpenSys/pkgs/bin/yum --version
```

To make permanent, add to your profile:
```bash
echo 'export PATH=/QOpenSys/pkgs/bin:$PATH' >> ~/.profile
```

### "Permission denied" When Creating Directories

You may lack authority. Options:
1. Ask your system administrator to create the directories
2. Request `*ALLOBJ` special authority
3. Have administrator grant specific directory permissions

### Package Installation Fails

Common causes:
- **No internet connection**: Verify connectivity
- **Proxy required**: Configure yum proxy settings
- **Disk space**: Check available space with `df -h`
- **Authority issues**: May need `*ALLOBJ` or `*SECADM`

### Git Clone Fails with SSL Error

```bash
# Temporarily disable SSL verification (not recommended for production)
git config --global http.sslVerify false

# Better: Install proper SSL certificates
yum install ca-certificates-mozilla
```

---

## Best Practices

### Directory Organization

```
/home/youruser/
├── sources/              # Active development
│   └── myproject/       # Git repository
├── migrate/             # Tools
├── backups/             # Backup copies
└── scripts/             # Utility scripts
```

### Security Considerations

- Use appropriate file permissions (755 for directories, 644 for files)
- Don't store passwords in scripts
- Use SSH keys for Git authentication
- Regularly update open source packages

### Maintenance

```bash
# Check for package updates monthly
yum check-update

# Update all packages
yum update

# Clean up old packages
yum clean all
```

---

## Next Steps

Now that your IBM i system is configured:

1. **Continue to Source Migration**: [03-source-migration.md](03-source-migration.md)
2. **Review troubleshooting**: [troubleshooting.md](troubleshooting.md)

---

## Additional Resources

- [IBM i Open Source Documentation](https://ibmi-oss-docs.readthedocs.io/)
- [IBM i Open Source Package Management](https://www.ibm.com/support/pages/node/706903)
- [Yum Package Manager Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-yum)

---

**Last Updated**: June 2026