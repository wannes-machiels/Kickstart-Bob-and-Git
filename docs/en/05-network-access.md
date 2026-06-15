# Network Shared Folder Access

[← Back to README](../../README.md) | [← Previous: GitHub Integration](04-github-integration.md) | [Next: IBM Bob Configuration →](06-ibm-bob-configuration.md)

---

## Introduction

This guide shows you how to access your IBM i IFS sources from your workstation via network shares. This allows you to edit files using modern IDEs like VS Code while the files remain on the IBM i system.

**What You'll Configure:**

- IBM i NetServer for file sharing
- Network share for your sources directory
- Workstation access (Windows, macOS, or Linux)

**Estimated Time**: 20-30 minutes

---

## Why Network Access?

### Benefits

- **Edit Locally**: Use VS Code, Notepad++, or any editor on your workstation
- **Fast Access**: Direct file access without FTP or file transfers
- **Seamless Integration**: Files stay on IBM i but feel local
- **Team Collaboration**: Multiple developers can access the same sources
- **Backup Integration**: Easy to include in workstation backup routines

---

## Step 1: Configure IBM i NetServer

NetServer provides SMB/CIFS file sharing from IBM i.

### Check NetServer Status

From a 5250 session:

```bash
WRKACTJOB SBS(QSYSWRK)
```

Look for `QZLSSERVER` jobs. If running, NetServer is active.

### Start NetServer

If NetServer is not running:

```bash
STRTCPSVR SERVER(*NETSVR)
```

**Expected Message**:

```bash
TCP/IP server NETSVR starting in subsystem QSYSWRK
```

### Verify NetServer Started

```bash
NETSTAT *CNN
```

Look for connections on port 445 (SMB).

### Configure NetServer to Start Automatically

```bash
CHGNTSVRA AUTOSTART(*YES)
```

---

## Step 2: Create a Network Share

You can create shares using either the command line or IBM i Navigator.

### Method 1: Using IBM i Navigator (Recommended)

1. Open IBM i Navigator in a web browser:

   ```bash
   http://your-ibmi-system:2001
   ```

2. Log in with your credentials

3. Navigate to:
   - **Network** → **Servers** → **TCP/IP**

4. Right-click **NetServer** and select **Shares**

5. Click **New Share**

6. Configure the share:
   - **Share name**: `SOURCES` (or any name you prefer)
   - **Path**: `/home/youruser/sources`
   - **Description**: "IBM i Source Code"
   - **Permissions**: Read/Write for your user

7. Click **OK**

### Method 2: Using Command Line

From a 5250 session:

```bash
CHGNFSEXP OPTIONS('-i -o rw') DIR('/home/youruser/sources')
```

Or create a share using QNTC:

```bash
MKDIR DIR('/QNTC/YOURSYSTEM/SOURCES')
ADDLNK OBJ('/home/youruser/sources') NEWLNK('/QNTC/YOURSYSTEM/SOURCES')
```

### Verify Share Creation

From IBM i Navigator:

- Go to **NetServer** → **Shares**
- Verify your share appears in the list

From command line:

```bash
WRKLNK OBJ('/QNTC/*')
```

---

## Step 3: Connect from Windows

### Using File Explorer

1. Open **File Explorer**

2. In the address bar, type:

   ```bash
   \\your-ibmi-system\SOURCES
   ```

   Replace `your-ibmi-system` with your IBM i hostname or IP address

3. Press **Enter**

4. When prompted, enter your credentials:
   - **Username**: Your IBM i user profile
   - **Password**: Your IBM i password
   - Check "Remember my credentials" (optional)

5. Click **OK**

### Map as Network Drive

For permanent access:

1. In File Explorer, click **This PC**

2. Click **Map network drive** in the toolbar

3. Configure:
   - **Drive**: Choose a letter (e.g., `Z:`)
   - **Folder**: `\\your-ibmi-system\SOURCES`
   - ✅ Check "Reconnect at sign-in"
   - ✅ Check "Connect using different credentials" (if needed)

4. Click **Finish**

5. Enter credentials if prompted

6. The drive appears in **This PC** as `SOURCES (Z:)`

### Using Command Line (PowerShell)

```powershell
# Map network drive
New-PSDrive -Name "Z" -PSProvider FileSystem -Root "\\your-ibmi-system\SOURCES" -Persist

# Or using net use
net use Z: \\your-ibmi-system\SOURCES /persistent:yes
```

### Verify Access

```powershell
# List files on the mapped drive
dir Z:\

# Or
Get-ChildItem Z:\
```

---

## Step 4: Connect from macOS

### Using Finder

1. Open **Finder**

2. Press **Cmd+K** or select **Go** → **Connect to Server**

3. Enter the server address:

   ```bash
   smb://your-ibmi-system/SOURCES
   ```

4. Click **Connect**

5. Select **Registered User**

6. Enter your credentials:
   - **Name**: Your IBM i user profile
   - **Password**: Your IBM i password

7. Click **Connect**

8. The share mounts and appears in Finder sidebar

### Make Connection Permanent

1. After connecting, the server appears in Finder sidebar

2. Right-click the server name

3. Select **Add to Login Items**

The share will automatically mount when you log in.

### Using Command Line (Terminal)

```bash
# Create mount point
mkdir -p ~/mnt/ibmi-sources

# Mount the share
mount_smbfs //youruser@your-ibmi-system/SOURCES ~/mnt/ibmi-sources

# Verify
ls -la ~/mnt/ibmi-sources
```

### Unmount

```bash
umount ~/mnt/ibmi-sources
```

---

## Step 5: Connect from Linux

### Install Required Packages

**Debian/Ubuntu:**

```bash
sudo apt-get update
sudo apt-get install cifs-utils
```

**RHEL/CentOS/Fedora:**

```bash
sudo yum install cifs-utils
```

**Arch Linux:**

```bash
sudo pacman -S cifs-utils
```

### Create Mount Point

```bash
sudo mkdir -p /mnt/ibmi-sources
```

### Mount the Share

**Temporary mount:**

```bash
sudo mount -t cifs //your-ibmi-system/SOURCES /mnt/ibmi-sources -o username=youruser
```

You'll be prompted for your password.

**Mount with credentials in command:**

```bash
sudo mount -t cifs //your-ibmi-system/SOURCES /mnt/ibmi-sources -o username=youruser,password=yourpassword
```

⚠️ **Security Warning**: Don't use passwords in commands on shared systems.

### Permanent Mount (fstab)

1. Create a credentials file:

   ```bash
   sudo nano /etc/smbcredentials
   ```

2. Add your credentials:

   ```bash
   username=youruser
   password=yourpassword
   ```

3. Secure the file:

   ```bash
   sudo chmod 600 /etc/smbcredentials
   ```

4. Edit fstab:

   ```bash
   sudo nano /etc/fstab
   ```

5. Add this line:

   ```bash
   //your-ibmi-system/SOURCES /mnt/ibmi-sources cifs credentials=/etc/smbcredentials,uid=1000,gid=1000 0 0
   ```

6. Test the mount:

   ```bash
   sudo mount -a
   ```

7. Verify:

   ```bash
   ls -la /mnt/ibmi-sources
   ```

### Unmount

```bash
sudo umount /mnt/ibmi-sources
```

---

## Step 6: Verify Access from VS Code

### Open Network Share in VS Code

**Windows:**

1. Open VS Code
2. File → Open Folder
3. Navigate to `Z:\` (your mapped drive)
4. Select the folder and click **Select Folder**

**macOS:**

1. Open VS Code
2. File → Open Folder
3. Navigate to `/Volumes/SOURCES`
4. Select the folder and click **Open**

**Linux:**

1. Open VS Code
2. File → Open Folder
3. Navigate to `/mnt/ibmi-sources`
4. Select the folder and click **OK**

### Test Editing

1. Open a source file (e.g., `QRPGLESRC/PROGRAM1.rpgle`)

2. Make a small change

3. Save the file (**Ctrl+S** or **Cmd+S**)

4. Verify the change on IBM i:

   ```bash
   ssh youruser@your-ibmi-system
   cat /home/youruser/sources/QRPGLESRC/PROGRAM1.rpgle
   ```

---

## Troubleshooting

### Cannot Connect to Share

**Windows Error**: "Network path not found"

**Solutions**:

1. Verify NetServer is running on IBM i
2. Check firewall settings (port 445 must be open)
3. Try using IP address instead of hostname
4. Verify share name is correct

**Test connectivity**:

```powershell
Test-NetConnection -ComputerName your-ibmi-system -Port 445
```

### Authentication Failed

**Error**: "Logon failure: unknown user name or bad password"

**Solutions**:

1. Verify username and password are correct
2. Check if user profile is enabled on IBM i
3. Verify user has authority to the shared directory
4. Try using domain\username format if in a domain

### Permission Denied

**Error**: "Access is denied" or "Permission denied"

**Solutions**:

1. Check directory permissions on IBM i:

   ```bash
   ls -la /home/youruser/sources
   ```

2. Fix permissions:

   ```bash
   chmod 755 /home/youruser/sources
   ```

3. Verify share permissions in IBM i Navigator

### Share Not Visible

**Problem**: Share doesn't appear in network browser

**Solutions**:

1. Access directly using full path
2. Verify NetServer is running
3. Check network discovery settings (Windows)
4. Use IP address instead of hostname

### Slow Performance

**Problem**: File access is slow

**Solutions**:

1. Check network connectivity and bandwidth
2. Reduce number of open files in VS Code
3. Exclude large directories from VS Code search
4. Consider using SSH with remote development instead

---

## Best Practices

### Security

- Use strong passwords for IBM i user profiles
- Don't share credentials
- Use read-only shares when write access isn't needed
- Regularly review share permissions
- Consider VPN for remote access

### Performance

- Close unused network connections
- Don't edit very large files over network shares
- Use local Git operations when possible
- Consider SSH remote development for slow connections

### Organization

- Create separate shares for different projects
- Use descriptive share names
- Document share purposes
- Maintain consistent directory structures

---

## Alternative: SSH Remote Development

For better performance and security, consider using VS Code's Remote-SSH extension instead of network shares:

**Advantages**:

- Faster performance
- More secure (SSH encryption)
- Works over any network
- Better for slow connections

**Setup**:

1. Install "Remote - SSH" extension in VS Code
2. Connect to IBM i via SSH
3. Open folders directly on IBM i
4. Edit files as if they were local

See [IBM Bob Configuration](06-ibm-bob-configuration.md) for details.

---

## Next Steps

Now that you have network access:

1. **Configure IBM Bob**: [06-ibm-bob-configuration.md](06-ibm-bob-configuration.md)
2. **Start developing**: Edit files in VS Code
3. **Commit changes**: Use Git to track your work

---

## Additional Resources

- [IBM i NetServer Documentation](https://www.ibm.com/docs/en/i/7.5?topic=services-netserver)
- [SMB/CIFS Protocol](https://en.wikipedia.org/wiki/Server_Message_Block)
- [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview)

---

**Last Updated**: June 2026
