# Source Code Migration to IFS

[← Back to README](../../README.md) | [← Previous: IBM i Setup](02-ibm-i-setup.md) | [Next: GitHub Integration →](04-github-integration.md)

---

## Introduction

This guide walks you through migrating your IBM i source code from traditional source physical files (like QRPGLESRC, QCLSRC) to the Integrated File System (IFS). This is a crucial step for modern development workflows and Git integration.

**What You'll Do:**
- Clone and build the migrate tool
- Configure migration settings
- Migrate source code to IFS
- Verify the migration

**Estimated Time**: 45-60 minutes

---

## Why Migrate to IFS?

### Benefits of IFS-Based Development

| Traditional Source Physical Files | IFS-Based Development |
|----------------------------------|----------------------|
| Locked to IBM i system | Accessible from any workstation |
| Limited to 10-character names | Full filename support |
| No native version control | Git integration |
| Difficult to edit remotely | Edit with modern IDEs |
| Hard to share/collaborate | Easy team collaboration |

### What Happens During Migration?

The migrate tool:
1. Reads source members from source physical files
2. Converts them to individual files on the IFS
3. Preserves source code content and structure
4. Maintains proper file extensions (.rpgle, .clle, .dds, etc.)
5. Organizes files by source type

---

## Step 1: Clone the Migrate Repository

The migrate tool is open source and hosted on GitHub.

### Navigate to Your Migrate Directory

```bash
# SSH into your IBM i system
ssh youruser@your-ibmi-system

# Navigate to the migrate directory
cd /home/youruser/migrate
```

### Clone the Repository

```bash
# Clone from GitHub
git clone https://github.com/worksofliam/migrate.git

# Verify the clone
ls -la
```

**Expected Output:**
```
drwxr-xr-x  3 youruser  0  Jun 15 08:00 migrate
```

### Explore the Repository Structure

```bash
cd migrate
ls -la
```

You should see:
- `migrate/` - Main tool directory
- `README.md` - Documentation
- Other supporting files

---

## Step 2: Build the Migrate Tool

The migrate tool needs to be compiled before use.

### Navigate to the Build Directory

```bash
cd migrate
pwd
# Should show: /home/youruser/migrate/migrate/migrate
```

### Build Using gmake

```bash
# Run the build
gmake

# Or use the full path if gmake is not in PATH
/QOpenSys/pkgs/bin/gmake
```

**Expected Output:**
```
gcc -c migrate.c -o migrate.o
gcc migrate.o -o migrate
Build complete
```

### Verify the Build

```bash
# Check if the executable was created
ls -la migrate

# Test the tool
./migrate --help
```

**Expected Output:**
```
Usage: migrate [options]
  -l LIBRARY    Source library name
  -d DIRECTORY  Target IFS directory
  -f FILE       Source file name (optional)
  ...
```

---

## Step 3: Configure Migration Settings

Before migrating, decide which libraries and source files to migrate.

### Identify Your Source Libraries

```bash
# From 5250 session, list your libraries
WRKLIB LIB(YOUR*)

# Or from SSH, query the system
system "DSPLIB LIB(YOURLIB)"
```

### Identify Source Physical Files

```bash
# List source physical files in a library
system "DSPFD FILE(YOURLIB/*ALL) TYPE(*FILE)"

# Common source file names:
# QRPGLESRC - RPG source
# QRPGSRC   - RPG/400 source
# QCLSRC    - CL source
# QCMDSRC   - Command source
# QDDSSRC   - DDS source
# QSQLSRC   - SQL source
```

### Plan Your Migration

Create a migration plan:

```bash
# Example migration plan
SOURCE_LIBRARY=MYLIB
TARGET_DIRECTORY=/home/youruser/sources/myproject
SOURCE_FILES=QRPGLESRC,QCLSRC,QDDSSRC
```

---

## Step 4: Run the Migration

### Basic Migration Command

Migrate an entire library:

```bash
cd /home/youruser/migrate/migrate/migrate

# Migrate all source files from a library
./migrate -l YOURLIB -d /home/youruser/sources
```

### Migrate Specific Source File

```bash
# Migrate only QRPGLESRC
./migrate -l YOURLIB -f QRPGLESRC -d /home/youruser/sources
```

### Migration with Options

```bash
# Verbose output
./migrate -l YOURLIB -d /home/youruser/sources -v

# Dry run (see what would be migrated without actually doing it)
./migrate -l YOURLIB -d /home/youruser/sources --dry-run
```

### Monitor Migration Progress

The tool will display progress:
```
Migrating library: YOURLIB
Processing QRPGLESRC...
  Migrating member: PROGRAM1 -> PROGRAM1.rpgle
  Migrating member: PROGRAM2 -> PROGRAM2.rpgle
Processing QCLSRC...
  Migrating member: COMPILE -> COMPILE.clle
...
Migration complete: 150 members migrated
```

---

## Step 5: Verify the Migration

### Check Directory Structure

```bash
cd /home/youruser/sources
ls -la
```

**Expected Structure:**
```
/home/youruser/sources/
├── QRPGLESRC/
│   ├── PROGRAM1.rpgle
│   ├── PROGRAM2.rpgle
│   └── ...
├── QCLSRC/
│   ├── COMPILE.clle
│   └── ...
└── QDDSSRC/
    ├── CUSTFILE.dds
    └── ...
```

### Verify File Contents

```bash
# View a migrated file
cat QRPGLESRC/PROGRAM1.rpgle

# Check file encoding
file QRPGLESRC/PROGRAM1.rpgle

# Count migrated files
find . -type f | wc -l
```

### Compare with Original

```bash
# View original member (from 5250)
DSPPFM FILE(YOURLIB/QRPGLESRC) MBR(PROGRAM1)

# Compare with migrated file
cat /home/youruser/sources/QRPGLESRC/PROGRAM1.rpgle
```

### Check for Issues

Common things to verify:
- [ ] All expected members were migrated
- [ ] File extensions are correct (.rpgle, .clle, .dds, etc.)
- [ ] File contents match original members
- [ ] Special characters are preserved
- [ ] Line endings are correct

---

## Step 6: Handle Special Cases

### Source Members with Special Characters

If member names contain special characters:

```bash
# The tool automatically handles most cases
# Special characters are converted to safe filenames
# Example: PROG#1 becomes PROG_1.rpgle
```

### Large Libraries

For libraries with many members:

```bash
# Migrate in batches
./migrate -l YOURLIB -f QRPGLESRC -d /home/youruser/sources
./migrate -l YOURLIB -f QCLSRC -d /home/youruser/sources
./migrate -l YOURLIB -f QDDSSRC -d /home/youruser/sources
```

### Multiple Libraries

```bash
# Create separate directories for each library
mkdir -p /home/youruser/sources/lib1
mkdir -p /home/youruser/sources/lib2

# Migrate each library
./migrate -l LIB1 -d /home/youruser/sources/lib1
./migrate -l LIB2 -d /home/youruser/sources/lib2
```

---

## Step 7: Post-Migration Cleanup

### Organize Your Files

```bash
cd /home/youruser/sources

# Create a logical structure
mkdir -p src/rpg
mkdir -p src/cl
mkdir -p src/dds

# Move files if needed
mv QRPGLESRC/* src/rpg/
mv QCLSRC/* src/cl/
mv QDDSSRC/* src/dds/
```

### Create a README

```bash
cat > README.md << 'EOF'
# My IBM i Project

Source code migrated from IBM i library YOURLIB.

## Structure
- src/rpg/ - RPG programs
- src/cl/ - CL programs
- src/dds/ - DDS files

## Last Migration
Date: 2026-06-15
Library: YOURLIB
EOF
```

### Set Proper Permissions

```bash
# Ensure files are readable
chmod -R 644 /home/youruser/sources/*.rpgle
chmod -R 644 /home/youruser/sources/*.clle

# Ensure directories are accessible
chmod -R 755 /home/youruser/sources/*/
```

---

## Troubleshooting

### Migration Tool Build Fails

**Error**: `gmake: command not found`

**Solution**:
```bash
# Install make-gnu
yum install make-gnu

# Or use full path
/QOpenSys/pkgs/bin/gmake
```

**Error**: `gcc: command not found`

**Solution**:
```bash
# Install gcc
yum install gcc
```

### No Members Migrated

**Possible Causes**:
1. Wrong library name
2. No source physical files in library
3. No members in source files
4. Permission issues

**Solution**:
```bash
# Verify library exists
system "DSPLIB LIB(YOURLIB)"

# Check for source files
system "DSPFD FILE(YOURLIB/*ALL) TYPE(*FILE)"

# Check for members
system "DSPPFM FILE(YOURLIB/QRPGLESRC)"
```

### Encoding Issues

**Problem**: Special characters appear corrupted

**Solution**:
```bash
# Check current encoding
locale

# Set proper encoding
export LANG=en_US.UTF-8

# Re-run migration
./migrate -l YOURLIB -d /home/youruser/sources
```

### Permission Denied

**Error**: `Permission denied: /home/youruser/sources`

**Solution**:
```bash
# Check directory permissions
ls -la /home/youruser/

# Fix permissions
chmod 755 /home/youruser/sources

# Or create with proper permissions
mkdir -m 755 /home/youruser/sources
```

---

## Best Practices

### Before Migration
- [ ] Back up your source physical files
- [ ] Document your library structure
- [ ] Test migration on a small library first
- [ ] Plan your IFS directory structure

### During Migration
- [ ] Use verbose mode to monitor progress
- [ ] Migrate one source file type at a time for large libraries
- [ ] Keep a log of the migration process

### After Migration
- [ ] Verify all files migrated successfully
- [ ] Compare file counts with original members
- [ ] Test compile a few programs from IFS
- [ ] Document the migration date and source

---

## Next Steps

Now that your source code is on the IFS:

1. **Set up Git and GitHub**: [04-github-integration.md](04-github-integration.md)
2. **Configure network access**: [05-network-access.md](05-network-access.md)
3. **Set up IBM Bob**: [06-ibm-bob-configuration.md](06-ibm-bob-configuration.md)

---

## Additional Resources

- [Migrate Tool Repository](https://github.com/worksofliam/migrate)
- [Code for IBM i - Migration Guide](https://codefori.github.io/docs/developing/local/migrate/)
- [IBM i IFS Documentation](https://www.ibm.com/docs/en/i/7.5?topic=system-integrated-file)

---

**Last Updated**: June 2026