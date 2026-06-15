# IBM Bob Configuration

[← Back to README](../../README.md) | [← Previous: Network Access](05-network-access.md)

---

## Introduction

This guide walks you through installing and configuring IBM Bob in Visual Studio Code. IBM Bob is an AI-powered development assistant specifically designed for IBM i development.

**What You'll Do:**
- Install IBM Bob extension in VS Code
- Connect to your IBM i system
- Configure source directories and library lists
- Verify the complete setup
- Learn basic IBM Bob features

**Estimated Time**: 20-30 minutes

---

## What is IBM Bob?

IBM Bob is an AI assistant that helps with:
- **Code Generation**: Generate RPG, CL, SQL, and other IBM i code
- **Code Explanation**: Understand existing code
- **Debugging**: Find and fix issues
- **Modernization**: Convert legacy code to modern practices
- **Documentation**: Generate documentation from code
- **Best Practices**: Get recommendations for better code

---

## Step 1: Install IBM Bob Extension

### Prerequisites

Ensure you have:
- ✅ Visual Studio Code installed
- ✅ IBM account (create at [ibm.com](https://www.ibm.com))
- ✅ Internet connection

### Install the Extension

1. Open **Visual Studio Code**

2. Click the **Extensions** icon in the left sidebar (or press **Ctrl+Shift+X** / **Cmd+Shift+X**)

3. Search for **"IBM Bob"**

4. Click **Install** on the IBM Bob extension

5. Wait for installation to complete

6. You may need to reload VS Code

### Verify Installation

Look for the IBM Bob icon in the VS Code activity bar (left sidebar).

---

## Step 2: Sign In to IBM Bob

### Initial Sign-In

1. Click the **IBM Bob** icon in the activity bar

2. You'll see a welcome screen

3. Click **Sign in** or **Log in**

4. A browser window will open

5. Sign in with your IBM account:
   - If you have an IBM account, enter your credentials
   - If not, click **Create an account** and follow the registration process

6. After successful login, return to VS Code

7. IBM Bob should now show as authenticated

### Verify Authentication

- Look for your name or profile in the IBM Bob panel
- You should see "Connected" or "Authenticated" status

---

## Step 3: Install Code for IBM i Extension

IBM Bob works best with the Code for IBM i extension for connecting to IBM i systems.

### Install Code for IBM i

1. Open **Extensions** in VS Code (**Ctrl+Shift+X** / **Cmd+Shift+X**)

2. Search for **"Code for IBM i"**

3. Click **Install** on the "Code for IBM i" extension by Halcyon Tech Ltd

4. Wait for installation to complete

### Verify Installation

Look for the IBM i icon in the VS Code activity bar.

---

## Step 4: Connect to IBM i System

### Create a Connection

1. Click the **IBM i** icon in the activity bar

2. Click the **+** button to add a new connection

3. Fill in the connection details:

   **Connection name**: Choose a descriptive name
   ```
   Example: "DEV System" or "Production IBM i"
   ```

   **Host or IP address**: Your IBM i system
   ```
   Example: <ip-address> or ibmi.company.com
   ```

   **Port**: SSH port (usually 22)
   ```
   Default: 22
   ```

   **Username**: Your IBM i user profile
   ```
   Example: YOURUSER
   ```

4. Click **Save & Exit**

### Connect to the System

1. Your connection appears in the IBM i connections list

2. Click the **▶ (play)** button next to your connection

3. A prompt appears at the top of VS Code asking for your password

4. Enter your IBM i password and press **Enter**

5. Wait for the connection to establish

### Verify Connection

When connected successfully, you'll see:
- **User Library List** section appears
- **IFS Browser** becomes available
- Connection status shows as "Connected"

---

## Step 5: Configure Source Directory

### Set Your Source Directory

1. In the IBM i panel, find **Settings** or **Configuration**

2. Look for **Source Directory** or **Working Directory** setting

3. Set it to your sources path:
   ```
   /home/youruser/sources
   ```

4. Save the configuration

### Configure Library List

1. In the IBM i panel, find **User Library List**

2. Click **Edit** or **Configure**

3. Add your development libraries:
   ```
   Example:
   - YOURLIB
   - QGPL
   - QTEMP
   ```

4. Set the order (most important first)

5. Save the configuration

### Set Current Library

1. Find **Current Library** setting

2. Set it to your working library:
   ```
   Example: YOURLIB
   ```

---

## Step 6: Configure Compiler Settings

### Set Compile Commands

1. Open VS Code Settings (**Ctrl+,** / **Cmd+,**)

2. Search for **"IBM i"**

3. Configure compile settings:

   **For RPG Programs**:
   ```
   CRTBNDRPG PGM(&CURLIB/&NAME) SRCSTMF('&FULLPATH') OPTION(*EVENTF) DBGVIEW(*SOURCE)
   ```

   **For CL Programs**:
   ```
   CRTBNDCL PGM(&CURLIB/&NAME) SRCSTMF('&FULLPATH') OPTION(*EVENTF) DBGVIEW(*SOURCE)
   ```

   **For SQL RPG**:
   ```
   CRTSQLRPGI OBJ(&CURLIB/&NAME) SRCSTMF('&FULLPATH') COMMIT(*NONE) DBGVIEW(*SOURCE)
   ```

### Test Compilation

1. Open a source file (e.g., `QRPGLESRC/PROGRAM1.rpgle`)

2. Right-click in the editor

3. Select **Compile** or press the compile shortcut

4. Check the output panel for compilation results

---

## Step 7: Configure IBM Bob for IBM i

### Link IBM Bob to Your IBM i Connection

1. Open IBM Bob settings

2. Look for **IBM i Connection** or **Target System**

3. Select your IBM i connection from the dropdown

4. Save the configuration

### Configure Bob's Context

Tell Bob about your environment:

1. Open IBM Bob chat

2. Send a message like:
   ```
   I'm working on an IBM i system with RPG ILE programs.
   My source code is in /home/youruser/sources.
   I use library YOURLIB for development.
   ```

3. Bob will remember this context for future interactions

---

## Step 8: Verify Complete Setup

### Checklist

Run through this verification checklist:

- [ ] IBM Bob extension installed and authenticated
- [ ] Code for IBM i extension installed
- [ ] Connected to IBM i system
- [ ] Can browse IFS directories
- [ ] Source directory configured
- [ ] Library list configured
- [ ] Can open source files
- [ ] Can compile a program
- [ ] IBM Bob responds to queries

### Test the Complete Workflow

1. **Open a source file**:
   - Browse to `/home/youruser/sources/QRPGLESRC/`
   - Open a `.rpgle` file

2. **Make a small change**:
   - Add a comment or modify a line
   - Save the file (**Ctrl+S** / **Cmd+S**)

3. **Compile the program**:
   - Right-click → Compile
   - Check for successful compilation

4. **Commit to Git**:
   ```bash
   cd /home/youruser/sources
   git add .
   git commit -m "Test change via VS Code"
   git push
   ```

5. **Ask IBM Bob**:
   - Open IBM Bob chat
   - Ask: "Explain what this program does"
   - Bob should analyze your code

---

## Step 9: Learn IBM Bob Features

### Code Generation

Ask Bob to generate code:

```
Generate an RPG ILE program that reads customer records from CUSTFILE
and creates a report sorted by customer name.
```

### Code Explanation

Select code and ask:

```
Explain what this code does and suggest improvements.
```

### Debugging Help

When you encounter an error:

```
I'm getting error CPF2105 when running this program. Here's the code:
[paste code]
What's wrong and how do I fix it?
```

### Modernization

Ask Bob to modernize legacy code:

```
Convert this RPG/400 program to free-format RPG ILE with modern best practices.
```

### Documentation

Generate documentation:

```
Create documentation for this program including:
- Purpose
- Parameters
- Return values
- Example usage
```

---

## Best Practices

### Using IBM Bob Effectively

1. **Be Specific**: Provide context and details in your questions
2. **Iterate**: Refine Bob's responses with follow-up questions
3. **Verify**: Always review and test Bob's suggestions
4. **Learn**: Use Bob to understand concepts, not just copy code
5. **Context**: Keep Bob informed about your environment and goals

### Development Workflow

```
1. Open source file in VS Code
2. Make changes
3. Ask Bob for review/suggestions
4. Compile and test
5. Commit to Git
6. Push to GitHub
```

### Security

- Don't share sensitive data with Bob
- Review generated code for security issues
- Don't commit credentials or passwords
- Use environment variables for sensitive configuration

---

## Troubleshooting

### Cannot Connect to IBM i

**Problem**: Connection fails or times out

**Solutions**:
1. Verify SSH is running on IBM i: `NETSTAT *CNN`
2. Check firewall settings (port 22)
3. Verify username and password
4. Try connecting via SSH first: `ssh youruser@your-ibmi-system`

### Compilation Fails

**Problem**: Compile command doesn't work

**Solutions**:
1. Verify library list is correct
2. Check compile command syntax
3. Ensure source file is saved
4. Verify you have authority to create objects
5. Check for syntax errors in source

### IBM Bob Not Responding

**Problem**: Bob doesn't answer or gives errors

**Solutions**:
1. Check internet connection
2. Verify IBM account is authenticated
3. Try signing out and back in
4. Restart VS Code
5. Check IBM Bob service status

### Source Files Not Visible

**Problem**: Can't see source files in VS Code

**Solutions**:
1. Verify source directory path is correct
2. Check file permissions on IBM i
3. Refresh the IFS browser
4. Verify network share is connected
5. Try using SSH remote development instead

---

## Advanced Configuration

### Custom Actions

Create custom actions for common tasks:

1. Open Code for IBM i settings
2. Find **Actions** section
3. Add custom compile commands
4. Add deployment scripts
5. Add testing commands

### Keyboard Shortcuts

Set up shortcuts for frequent operations:

1. File → Preferences → Keyboard Shortcuts
2. Search for IBM i commands
3. Assign shortcuts:
   - Compile: **Ctrl+Shift+C**
   - Deploy: **Ctrl+Shift+D**
   - Run: **Ctrl+Shift+R**

### Workspace Settings

Save project-specific settings:

1. Create `.vscode/settings.json` in your project
2. Add IBM i configuration:
   ```json
   {
     "code-for-ibmi.connectionSettings": {
       "sourceDirectory": "/home/youruser/sources",
       "currentLibrary": "YOURLIB",
       "libraryList": ["YOURLIB", "QGPL"]
     }
   }
   ```

---

## Next Steps

You're now ready to develop with IBM Bob!

### Continue Learning

1. **Explore IBM Bob features**: Try different types of questions
2. **Practice Git workflow**: Make changes, commit, push
3. **Learn RPG ILE**: Use Bob to learn modern RPG
4. **Join the community**: Connect with other IBM i developers

### Additional Resources

- [IBM Bob Documentation](https://bob.ibm.com/docs)
- [Code for IBM i Documentation](https://codefori.github.io/docs/)
- [IBM i Modernization Resources](https://www.ibm.com/it-infrastructure/power/os/ibm-i)

### Get Help

- **Troubleshooting Guide**: [troubleshooting.md](troubleshooting.md)
- **IBM Bob Support**: [bob.ibm.com/support](https://bob.ibm.com/support)
- **Community Forums**: [ibm.com/community](https://community.ibm.com/)

---

## Congratulations! 🎉

You've successfully set up IBM Bob and your complete IBM i development environment. You can now:

- ✅ Edit source code in VS Code
- ✅ Compile programs on IBM i
- ✅ Use Git for version control
- ✅ Leverage AI assistance with IBM Bob
- ✅ Collaborate with your team via GitHub

Happy coding!

---

**Last Updated**: June 2026