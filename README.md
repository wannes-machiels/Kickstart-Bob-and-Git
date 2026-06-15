# IBM Bob & Git Kickstart Documentation

Comprehensive documentation for setting up IBM Bob with IBM i development and Git version control.

---

## 🇳🇱 Nederlands

Welkom bij de IBM Bob & Git documentatie. Deze repository bevat uitgebreide handleidingen voor het opzetten van moderne IBM i ontwikkeling met IBM Bob en Git.

### Nederlandse Documentatie

- **[IBM Bob Installatiegids](docs/nl/IBM_Bob-Installatie-documentatie.md)** - Stapsgewijze installatie van IBM Bob
- **[How to Git Good](docs/nl/How-to-Git-good.md)** - Uitgebreide Git handleiding van basis tot gevorderd
- **[Git Referentiekaart](docs/nl/git-referentiekaart.md)** - Snelle referentie voor dagelijks gebruik

### Snelle Start

1. Installeer Node.js en Git op je werkstation
2. Installeer IBM Bob in VS Code
3. Volg de installatiegids voor verbinding met IBM i
4. Begin met ontwikkelen!

---

## 🇬🇧 English

Welcome to the IBM Bob & Git documentation. This repository contains comprehensive guides for setting up modern IBM i development with IBM Bob and Git version control.

### 📚 Documentation Structure

#### Getting Started

**New to IBM i development with Bob?** Start here:

1. **[Prerequisites](docs/en/01-prerequisites.md)** - System requirements and verification
2. **[IBM i Setup](docs/en/02-ibm-i-setup.md)** - Configure your IBM i system
3. **[Source Migration](docs/en/03-source-migration.md)** - Move source code to IFS
4. **[GitHub Integration](docs/en/04-github-integration.md)** - Set up Git and GitHub
5. **[Network Access](docs/en/05-network-access.md)** - Access IFS from your workstation
6. **[IBM Bob Configuration](docs/en/06-ibm-bob-configuration.md)** - Configure IBM Bob in VS Code

#### Quick Access

- **[Quick Start Guide](docs/en/quick-start.md)** ⚡ - Fast-track setup for experienced users (30-45 minutes)
- **[Troubleshooting](docs/en/troubleshooting.md)** 🔧 - Solutions to common issues
- **[Online Resources](☁️%20Online%20Documentation.md)** 🌐 - External documentation and labs

---

## 🎯 What You'll Learn

This documentation covers the complete setup process:

### IBM i System Configuration

- Creating IFS directories for source code
- Installing open source tools (Git, make-gnu, bash)
- Configuring NetServer for file sharing
- Setting up proper permissions and authorities

### Source Code Migration

- Using the migrate tool to move source from libraries to IFS
- Organizing source code in a modern directory structure
- Handling special cases and encoding issues
- Verifying successful migration

### Git & GitHub Integration

- Creating GitHub accounts and repositories
- Initializing Git in your source directory
- Setting up authentication (Personal Access Tokens or SSH)
- Daily Git workflow for IBM i development
- Best practices for commit messages and branching

### Network Access

- Configuring network shares on IBM i
- Connecting from Windows, macOS, and Linux
- Mapping network drives for easy access
- Troubleshooting connection issues

### IBM Bob Setup

- Installing IBM Bob and Code for IBM i extensions
- Connecting to your IBM i system
- Configuring source directories and library lists
- Using IBM Bob for code generation, explanation, and debugging
- Leveraging AI assistance in your development workflow

---

## 🚀 Quick Start

### For Beginners

Follow the step-by-step guides in order:

```bash
Prerequisites → IBM i Setup → Source Migration → 
GitHub Integration → Network Access → IBM Bob Configuration
```

**Estimated Time**: 2-3 hours for complete setup

### For Experienced Users

Use the **[Quick Start Guide](docs/en/quick-start.md)** for a condensed setup process.

**Estimated Time**: 30-45 minutes

---

## 📋 Prerequisites

Before you begin, ensure you have:

### IBM i System

- IBM i version 7.3 or higher
- User profile with appropriate authorities
- Internet connectivity
- SSH access

### Workstation

- Visual Studio Code
- Git (version 2.x or higher)
- Node.js (LTS version)
- Network access to IBM i system

### Accounts

- IBM account (for IBM Bob)
- GitHub account (for version control)

**[→ Full Prerequisites Guide](docs/en/01-prerequisites.md)**

---

## 🗂️ Repository Structure

```bash
Kickstart-Bob-and-Git/
├── README.md                           # This file
├── docs/
│   ├── en/                            # English documentation
│   │   ├── 01-prerequisites.md        # System requirements
│   │   ├── 02-ibm-i-setup.md         # IBM i configuration
│   │   ├── 03-source-migration.md    # Source code migration
│   │   ├── 04-github-integration.md  # Git and GitHub setup
│   │   ├── 05-network-access.md      # Network shares
│   │   ├── 06-ibm-bob-configuration.md # IBM Bob setup
│   │   ├── troubleshooting.md        # Problem solutions
│   │   └── quick-start.md            # Fast-track guide
│   │
│   └── nl/                            # Dutch documentation
│       ├── IBM_Bob-Installatie-documentatie.md
│       ├── How-to-Git-good.md
│       └── git-referentiekaart.md
│
├── ☁️ Online Documentation.md         # External resources
└── DOCUMENTATION_STRUCTURE_PLAN.md    # Documentation design
```

---

## 🎓 Learning Path

### Phase 1: Setup (Day 1)

1. Verify prerequisites
2. Configure IBM i system
3. Migrate source code to IFS

### Phase 2: Version Control (Day 1-2)

1. Set up GitHub account and repository
2. Initialize Git in source directory
3. Make first commit and push

### Phase 3: Development Environment (Day 2)

1. Configure network access
2. Install and configure IBM Bob
3. Verify complete setup

### Phase 4: Daily Development (Ongoing)

1. Edit code in VS Code
2. Compile and test on IBM i
3. Commit changes to Git
4. Use IBM Bob for assistance

---

## 🔧 Troubleshooting

Encountering issues? Check the **[Troubleshooting Guide](docs/en/troubleshooting.md)** for solutions to common problems:

- Git clone or push failures
- IBM i package installation issues
- Source migration problems
- Network share connection errors
- IBM Bob authentication issues
- Compilation errors

---

## 🌟 Key Features

### Modern Development Workflow

- Edit source code with VS Code on your workstation
- Compile and test on IBM i system
- Version control with Git and GitHub
- AI-powered assistance with IBM Bob

### Team Collaboration

- Share code via GitHub
- Review changes with pull requests
- Track issues and features
- Maintain code history

### Best Practices

- Source code in version control
- Modern IDE with syntax highlighting
- Automated testing and CI/CD ready
- Documentation alongside code

---

## 📚 Additional Resources

### Official Documentation

- [IBM Bob Documentation](https://bob.ibm.com/docs)
- [Code for IBM i Documentation](https://codefori.github.io/docs/)
- [IBM i Open Source Documentation](https://ibmi-oss-docs.readthedocs.io/)
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Documentation](https://docs.github.com/)

### Community

- [IBM Community](https://community.ibm.com/)
- [Code for IBM i Discord](https://discord.gg/codeforibmi)
- [Stack Overflow - IBM Midrange](https://stackoverflow.com/questions/tagged/ibm-midrange)

### Hands-on Labs

- [Bobathon Belgium](https://github.com/ibm-ncee-bobathons/bobathon-belgium/)
- [IBM i Application Modernization with Bob](https://github.com/bmarolleau/IBM-i-Application-Modernization-with-Bob)

**[→ Full Resource List](☁️%20Online%20Documentation.md)**

---

## 🤝 Contributing

Found an issue or want to improve the documentation?

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📝 License

This documentation is provided as-is for educational and reference purposes.

---

## 📧 Support

- **Documentation Issues**: Open an issue in this repository
- **IBM Bob Support**: [bob.ibm.com/support](https://bob.ibm.com/support)
- **IBM i Questions**: [IBM Community](https://community.ibm.com/)
- **Git Help**: [Git Documentation](https://git-scm.com/doc)

---

## 🎉 Success Stories

After completing this setup, you'll be able to:

- ✅ Edit IBM i source code in VS Code
- ✅ Compile programs directly from your IDE
- ✅ Track all changes with Git version control
- ✅ Collaborate with team members via GitHub
- ✅ Get AI-powered coding assistance from IBM Bob
- ✅ Use modern development practices on IBM i

**Ready to get started? Begin with [Prerequisites](docs/en/01-prerequisites.md) or jump to the [Quick Start Guide](docs/en/quick-start.md)!**

---

**Last Updated**: June 2026  
**Version**: 1.0
