# Troubleshooting Guide

[← Back to README](../../README.md)

---

## Introduction

This guide provides solutions to common issues you may encounter while setting up and using IBM Bob with IBM i. Issues are organized by category for easy navigation.

**Quick Links:**

- [Git Issues](#git-issues)
- [IBM i Setup Issues](#ibm-i-setup-issues)
- [Migration Issues](#migration-issues)
- [Network Access Issues](#network-access-issues)
- [IBM Bob Issues](#ibm-bob-issues)
- [Compilation Issues](#compilation-issues)
- [Getting Additional Help](#getting-additional-help)

---

## Git Issues

### Git Clone Fails

**Error**: `fatal: unable to access 'https://github.com/...': Could not resolve host`

**Cause**: No internet connectivity or DNS issues

**Solutions**:

```bash
# Test internet connectivity
ping github.com

# Test DNS resolution
nslookup github.com

# If DNS fails, try using IP address or configure DNS
# Contact your network administrator
```

---

**Error**: `fatal: could not create work tree dir: Permission denied`

**Cause**: Insufficient permissions to create directories

**Solutions**:

```bash
# Check current directory permissions
ls -la

# Create directory with proper permissions
mkdir -m 755 /home/youruser/migrate

# Or request administrator to create the directory
```

---

**Error**: `SSL certificate problem: unable to get local issuer certificate`

**Cause**: Missing or outdated SSL certificates

**Solutions**:

```bash
# Install/update CA certificates
yum install ca-certificates-mozilla

# Temporary workaround (not recommended for production)
git config --global http.sslVerify false

# Better: Configure proper certificates
git config --global http.sslCAInfo /path/to/ca-bundle.crt
```

---

### Git Push Authentication Fails

**Error**: `Authentication failed for 'https://github.com/...'`

**Cause**: GitHub no longer accepts password authentication

**Solutions**:

1. **Create a Personal Access Token (PAT)**:
   - Go to GitHub → Settings → Developer settings → Personal access tokens
   - Generate new token with `repo` scope
   - Use token as password when pushing

2. **Configure credential helper**:

   ```bash
   # Cache credentials for 1 hour
   git config --global credential.helper 'cache --timeout=3600'
   
   # Or store permanently (less secure)
   git config --global credential.helper store
   ```

3. **Use SSH instead**:

   ```bash
   # Generate SSH key
   ssh-keygen -t ed25519 -C "your.email@company.com"
   
   # Add public key to GitHub
   cat ~/.ssh/id_ed25519.pub
   
   # Change remote URL to SSH
   git remote set-url origin git@github.com:username/repo.git
   ```

---

### Git Push Rejected

**Error**: `! [rejected] main -> main (fetch first)`

**Cause**: Remote has changes you don't have locally

**Solutions**:

```bash
# Pull latest changes first
git pull origin main

# If conflicts occur, resolve them
# Then commit and push
git add .
git commit -m "Resolve merge conflicts"
git push
```

---

### Merge Conflicts

**Error**: `CONFLICT (content): Merge conflict in file.rpgle`

**Cause**: Same lines modified in both local and remote

**Solutions**:

1. **Open the conflicted file**:

   ```bash
   <<<<<<< HEAD
   Your local changes
   =======
   Remote changes
   >>>>>>> origin/main
   ```

2. **Resolve the conflict**:

   - Choose which version to keep
   - Or combine both versions
   - Remove conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)

3. **Complete the merge**:

   ```bash
   git add file.rpgle
   git commit -m "Resolve merge conflict in file.rpgle"
   git push
   ```

---

## IBM i Setup Issues

### gmake Not Found

**Error**: `gmake: command not found`

**Cause**: make-gnu package not installed

**Solutions**:

```bash
# Install make-gnu
yum install make-gnu

# Verify installation
gmake --version

# If still not found, use full path
/QOpenSys/pkgs/bin/gmake

# Add to PATH permanently
echo 'export PATH=/QOpenSys/pkgs/bin:$PATH' >> ~/.profile
source ~/.profile
```

---

### yum Command Not Found

**Error**: `yum: command not found`

**Cause**: Open source environment not initialized

**Solutions**:

```bash
# Add to PATH
export PATH=/QOpenSys/pkgs/bin:$PATH

# Or use full path
/QOpenSys/pkgs/bin/yum --version

# Make permanent
echo 'export PATH=/QOpenSys/pkgs/bin:$PATH' >> ~/.profile
```

---

### Package Installation Fails

**Error**: `Error: Cannot retrieve repository metadata`

**Cause**: No internet connection or proxy issues

**Solutions**:

```bash
# Test internet connectivity
ping 8.8.8.8

# Check proxy settings
echo $http_proxy
echo $https_proxy

# Configure proxy if needed
export http_proxy=http://proxy.company.com:8080
export https_proxy=http://proxy.company.com:8080

# Make permanent
echo 'export http_proxy=http://proxy.company.com:8080' >> ~/.profile
```

---

**Error**: `Error: Insufficient space in /QOpenSys/pkgs`

**Cause**: Not enough disk space

**Solutions**:

```bash
# Check available space
df -h /QOpenSys/pkgs

# Clean up old packages
yum clean all

# Remove unused packages
yum autoremove

# Contact administrator to increase space
```

---

### Permission Denied Creating Directories

**Error**: `mkdir: cannot create directory: Permission denied`

**Cause**: Insufficient authority

**Solutions**:

1. **Check your authorities**:

   ```bash
   # From 5250
   DSPUSRPRF USRPRF(YOURUSER)
   ```

2. **Request necessary authorities**:
   - Contact your system administrator
   - Request `*ALLOBJ` special authority or
   - Request specific directory permissions

3. **Alternative**: Have administrator create directories:

   ```bash
   # Administrator creates directories
   mkdir -p /home/youruser/sources
   mkdir -p /home/youruser/migrate
   
   # Set ownership
   chown youruser:yourgroup /home/youruser/sources
   chown youruser:yourgroup /home/youruser/migrate
   
   # Set permissions
   chmod 755 /home/youruser/sources
   chmod 755 /home/youruser/migrate
   ```

---

## Migration Issues

### Migrate Tool Build Fails

**Error**: `gcc: command not found`

**Cause**: GCC compiler not installed

**Solutions**:

```bash
# Install GCC
yum install gcc

# Verify installation
gcc --version

# Retry build
cd /home/youruser/migrate/migrate/migrate
gmake
```

---

**Error**: `make: *** No targets specified and no makefile found`

**Cause**: Wrong directory or missing Makefile

**Solutions**:

```bash
# Verify you're in the correct directory
pwd
# Should be: /home/youruser/migrate/migrate/migrate

# List files to verify Makefile exists
ls -la

# If Makefile is missing, re-clone repository
cd /home/youruser/migrate
rm -rf migrate
git clone https://github.com/worksofliam/migrate.git
```

---

### No Members Migrated

**Problem**: Migration completes but no files created

**Causes and Solutions**:

1. **Wrong library name**:

   ```bash
   # Verify library exists
   system "DSPLIB LIB(YOURLIB)"
   
   # Check spelling and case
   ```

2. **No source physical files**:

   ```bash
   # List files in library
   system "DSPFD FILE(YOURLIB/*ALL) TYPE(*FILE)"
   
   # Look for source physical files (QRPGLESRC, QCLSRC, etc.)
   ```

3. **No members in source files**:

   ```bash
   # Check for members
   system "DSPPFM FILE(YOURLIB/QRPGLESRC)"
   
   # If empty, nothing to migrate
   ```

4. **Permission issues**:

   ```bash
   # Check authority to source files
   system "DSPOBJAUT OBJ(YOURLIB/QRPGLESRC) OBJTYPE(*FILE)"
   ```

---

### Encoding Issues After Migration

**Problem**: Special characters appear corrupted (Ã©, Ã¨, etc.)

**Cause**: Character encoding mismatch

**Solutions**:

```bash
# Check current locale
locale

# Set proper encoding
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8

# Re-run migration
cd /home/youruser/migrate/migrate/migrate
./migrate -l YOURLIB -d /home/youruser/sources

# Make permanent
echo 'export LANG=en_US.UTF-8' >> ~/.profile
```

---

### Migrated Files Have Wrong Extensions

**Problem**: Files have .txt or no extension instead of .rpgle, .clle

**Cause**: Migrate tool couldn't determine source type

**Solutions**:

1. **Manually rename files**:

   ```bash
   cd /home/youruser/sources/QRPGLESRC
   for f in *.txt; do mv "$f" "${f%.txt}.rpgle"; done
   ```

2. **Check source file attributes**:

   ```bash
   # From 5250
   DSPFD FILE(YOURLIB/QRPGLESRC)
   # Verify it's a source physical file
   ```

3. **Use specific source file type**:

   ```bash
   # Migrate with explicit type
   ./migrate -l YOURLIB -f QRPGLESRC -t RPGLE -d /home/youruser/sources
   ```

---

## Network Access Issues

### NetServer Not Running

**Error**: Cannot connect to `\\ibmi-system\share`

**Cause**: NetServer service not started

**Solutions**:

```bash
# From 5250 session
STRTCPSVR SERVER(*NETSVR)

# Verify it's running
WRKACTJOB SBS(QSYSWRK)
# Look for QZLSSERVER jobs

# Configure to start automatically
CHGNTSVRA AUTOSTART(*YES)
```

---

### Cannot Connect to Network Share

**Windows Error**: "Network path not found"

**Solutions**:

1. **Test connectivity**:

   ```powershell
   # Test ping
   ping your-ibmi-system
   
   # Test SMB port
   Test-NetConnection -ComputerName your-ibmi-system -Port 445
   ```

2. **Try IP address instead of hostname**:

   ```bash
   \\<ip-address>\SOURCES
   ```

3. **Check firewall**:
   - Ensure port 445 is open
   - Check Windows Firewall settings
   - Check IBM i firewall settings

4. **Verify share exists**:

   ```bash
   # From IBM i
   WRKLNK OBJ('/QNTC/*')
   ```

---

**macOS Error**: "Connection failed"

**Solutions**:

1. **Use correct protocol**:

   ```bash
   smb://your-ibmi-system/SOURCES
   ```

2. **Try with credentials in URL**:

   ```bash
   smb://username@your-ibmi-system/SOURCES
   ```

3. **Check Finder preferences**:

   - Finder → Preferences → General
   - Ensure "Connected servers" is checked

---

**Linux Error**: "mount error(13): Permission denied"

**Solutions**:

1. **Install cifs-utils**:

   ```bash
   sudo apt-get install cifs-utils  # Debian/Ubuntu
   sudo yum install cifs-utils      # RHEL/CentOS
   ```

2. **Use correct mount options**:

   ```bash
   sudo mount -t cifs //ibmi-system/SOURCES /mnt/ibmi \
     -o username=youruser,vers=3.0
   ```

3. **Check SELinux** (if applicable):

   ```bash
   # Temporarily disable
   sudo setenforce 0
   
   # Or configure properly
   sudo setsebool -P samba_enable_home_dirs on
   ```

---

### Authentication Failed

**Error**: "Logon failure: unknown user name or bad password"

**Solutions**:

1. **Verify credentials**:

   ```bash
   # Test SSH login
   ssh youruser@your-ibmi-system
   ```

2. **Check user profile status**:

   ```bash
   # From 5250
   DSPUSRPRF USRPRF(YOURUSER)
   # Verify status is *ENABLED
   ```

3. **Try different username formats**:

   ```bash
   youruser
   YOURUSER
   domain\youruser
   ```

4. **Reset password if needed**:

   ```bash
   # From 5250 (requires authority)
   CHGUSRPRF USRPRF(YOURUSER) PASSWORD(newpassword)
   ```

---

### Mapped Drive Disconnects

**Problem**: Network drive disconnects frequently

**Solutions**:

1. **Ensure persistent connection**:

   ```powershell
   # Windows
   net use Z: \\ibmi-system\SOURCES /persistent:yes
   ```

2. **Adjust timeout settings**:

   ```powershell
   # Increase SMB timeout
   Set-SmbClientConfiguration -SessionTimeout 300
   ```

3. **Check network stability**:
   - Verify stable network connection
   - Check for VPN disconnections
   - Monitor network errors

---

## IBM Bob Issues

### Cannot Connect to IBM i

**Problem**: Connection fails in Code for IBM i

**Solutions**:

1. **Verify SSH service**:

   ```bash
   # From 5250
   NETSTAT *CNN
   # Look for port 22 listeners
   ```

2. **Test SSH manually**:

   ```bash
   ssh youruser@your-ibmi-system
   ```

3. **Check connection settings**:
   - Verify hostname/IP is correct
   - Verify port is 22
   - Verify username is correct

4. **Check firewall**:
   - Ensure port 22 is open
   - Check both IBM i and workstation firewalls

---

### IBM Bob Not Responding

**Problem**: Bob doesn't answer or gives errors

**Solutions**:

1. **Check authentication**:
   - Click IBM Bob icon
   - Verify you're signed in
   - Try signing out and back in

2. **Check internet connection**:

   ```bash
   ping ibm.com
   ```

3. **Restart VS Code**:
   - Close VS Code completely
   - Reopen and try again

4. **Clear extension cache**:
   - Close VS Code
   - Delete: `~/.vscode/extensions/ibm.bob-*`
   - Reinstall IBM Bob extension

5. **Check service status**:
   - Visit [bob.ibm.com](https://bob.ibm.com)
   - Check for service announcements

---

### IBM Bob Gives Incorrect Answers

**Problem**: Bob's responses are not relevant or incorrect

**Solutions**:

1. **Provide more context**:

   ```bash
   Instead of: "Fix this code"
   Try: "This RPG ILE program reads CUSTFILE and should calculate totals. 
         It's giving wrong results. Here's the code: [paste code]"
   ```

2. **Be specific about your environment**:

   ```bash
   "I'm working on IBM i 7.4 with RPG ILE free format.
    I need to read a database file and create a report."
   ```

3. **Iterate and refine**:
   - Ask follow-up questions
   - Provide feedback on responses
   - Request clarification

4. **Verify Bob's suggestions**:
   - Always review generated code
   - Test thoroughly
   - Don't blindly trust AI output

---

## Compilation Issues

### Compile Command Not Found

**Problem**: Right-click → Compile doesn't work

**Solutions**:

1. **Configure compile commands**:
   - Open VS Code Settings
   - Search for "IBM i compile"
   - Add compile commands for your source types

2. **Verify library list**:
   - Check User Library List in IBM i panel
   - Ensure required libraries are present

3. **Check current library**:
   - Verify current library is set
   - Ensure you have authority to create objects

---

### Compilation Errors

**Error**: `CPF5035: Data area &1 in &2 not found`

**Cause**: Missing data area or library not in library list

**Solutions**:

```bash
# Add library to library list
# In Code for IBM i settings, add the library

# Or create missing data area
CRTDTAARA DTAARA(YOURLIB/DATAAREA) TYPE(*CHAR) LEN(100)
```

---

**Error**: `RNF0303: File &1 not found`

**Cause**: Database file not found or not in library list

**Solutions**:

```bash
# Verify file exists
WRKOBJ OBJ(YOURLIB/CUSTFILE) OBJTYPE(*FILE)

# Add library to library list
# Update Code for IBM i library list configuration

# Or create the file if missing
```

---

## Getting Additional Help

### Documentation Resources

- **IBM Bob Documentation**: [bob.ibm.com/docs](https://bob.ibm.com/docs)
- **Code for IBM i Docs**: [codefori.github.io/docs](https://codefori.github.io/docs/)
- **IBM i Documentation**: [ibm.com/docs/en/i](https://www.ibm.com/docs/en/i)
- **Git Documentation**: [git-scm.com/doc](https://git-scm.com/doc)

### Community Support

- **IBM Community**: [community.ibm.com](https://community.ibm.com/)
- **Code for IBM i Discord**: [discord.gg/codeforibmi](https://discord.gg/codeforibmi)
- **Stack Overflow**: Tag questions with `ibm-midrange`
- **GitHub Issues**: Report bugs in respective repositories

### Professional Support

- **IBM Support**: [ibm.com/support](https://www.ibm.com/support)
- **IBM Bob Support**: [bob.ibm.com/support](https://bob.ibm.com/support)
- **Your System Administrator**: For IBM i system issues
- **Your Network Administrator**: For connectivity issues

### Reporting Issues

When reporting issues, include:

1. **Environment details**:
   - IBM i version
   - VS Code version
   - Extension versions
   - Operating system

2. **Steps to reproduce**:
   - What you did
   - What you expected
   - What actually happened

3. **Error messages**:
   - Complete error text
   - Screenshots if helpful
   - Log files if available

4. **What you've tried**:
   - Solutions already attempted
   - Results of troubleshooting steps

---

## Still Having Issues?

If you've tried the solutions in this guide and still have problems:

1. **Review the setup guides**:
   - [Prerequisites](01-prerequisites.md)
   - [IBM i Setup](02-ibm-i-setup.md)
   - [Source Migration](03-source-migration.md)
   - [GitHub Integration](04-github-integration.md)
   - [Network Access](05-network-access.md)
   - [IBM Bob Configuration](06-ibm-bob-configuration.md)

2. **Ask IBM Bob**:
   - Describe your problem to Bob
   - Bob can help troubleshoot many issues

3. **Seek community help**:
   - Post in forums with detailed information
   - Include error messages and what you've tried

4. **Contact support**:
   - For IBM i issues: Your system administrator
   - For IBM Bob issues: IBM Bob support

---

**Last Updated**: June 2026
