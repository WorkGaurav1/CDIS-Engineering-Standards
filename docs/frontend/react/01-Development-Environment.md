# 01 - Development Environment

## Objective

Prepare your local development environment for React application development at CDIS.

This guide installs and configures all required development tools before creating your first React project.

---

## Prerequisites

- Ubuntu 24.04 LTS (Recommended)
- Internet Connection
- Terminal Access
- sudo privileges

---

# Step 1 - Update System Packages

Update the local package index and install the latest available updates.

### Command

```bash
sudo apt update && sudo apt upgrade -y
```

### Expected Output

The system updates successfully without errors.

---

# Step 2 - Install Git

Install Git using the Ubuntu package manager.

### Command

```bash
sudo apt install git -y
```

### Verify Installation

```bash
git --version
```

### Expected Output

```text
git version 2.xx.x
```

---

# Step 3 - Configure Git Username

Configure your Git username.

Replace **Your Name** with your actual name.

### Command

```bash
git config --global user.name "Your Name"
```

---

# Step 4 - Configure Git Email

Configure your company email address.

Replace the email with your official company email.

### Command

```bash
git config --global user.email "your.email@company.com"
```

---

# Step 5 - Verify Git Configuration

Verify Git configuration.

### Command

```bash
git config --list
```

### Expected Output

```text
user.name=Your Name
user.email=your.email@company.com
```

---

# Step 6 - Install Node Version Manager (NVM)

Node Version Manager (NVM) is the standard way to manage Node.js versions at CDIS.

### Command

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

Reload your shell.

```bash
source ~/.bashrc
```

### Verify Installation

```bash
nvm --version
```

### Expected Output

```text
0.40.3
```

---

# Step 7 - Install Node.js LTS

Install the latest Long Term Support (LTS) version of Node.js.

### Command

```bash
nvm install --lts
```

Set the installed version as the default.

```bash
nvm alias default lts/*
```

Use the default version.

```bash
nvm use default
```

---

# Step 8 - Verify Node.js Installation

Verify Node.js installation.

### Command

```bash
node -v
```

### Expected Output

```text
v22.x.x
```

---

# Step 9 - Verify npm Installation

Verify npm installation.

### Command

```bash
npm -v
```

### Expected Output

```text
10.x.x
```

---

# Step 10 - Install Visual Studio Code

Download and install the latest stable version of Visual Studio Code.

### Command

```bash
sudo snap install code --classic
```

### Verify Installation

```bash
code --version
```

### Expected Output

```text
1.xx.x
```

---

# Step 11 - Install Recommended VS Code Extensions

Install the following extensions.

### ESLint

```bash
code --install-extension dbaeumer.vscode-eslint
```

---

### Prettier

```bash
code --install-extension esbenp.prettier-vscode
```

---

### GitLens

```bash
code --install-extension eamodio.gitlens
```

---

### Error Lens

```bash
code --install-extension usernamehw.errorlens
```

---

### Auto Rename Tag

```bash
code --install-extension formulahendry.auto-rename-tag
```

---

### Path Intellisense

```bash
code --install-extension christian-kohler.path-intellisense
```

---

### ES7+ React Snippets

```bash
code --install-extension dsznajder.es7-react-js-snippets
```

---

# Step 12 - Verify Installed Extensions

List all installed extensions.

### Command

```bash
code --list-extensions
```

Verify that all recommended extensions are available.

---

# Step 13 - Verify Development Environment

Run the following commands.

```bash
git --version
```

```bash
node -v
```

```bash
npm -v
```

```bash
nvm --version
```

```bash
code --version
```

All commands should execute successfully.

---

# Development Environment Checklist

| Tool | Status |
|-------|--------|
| Git | ✅ Installed |
| Git Configuration | ✅ Completed |
| NVM | ✅ Installed |
| Node.js (LTS) | ✅ Installed |
| npm | ✅ Installed |
| VS Code | ✅ Installed |
| VS Code Extensions | ✅ Installed |

---

# CDIS Standards

- Ubuntu 24.04 LTS is the recommended development environment.
- Always install Node.js using NVM.
- Always use the latest LTS version of Node.js.
- Git must be configured before starting development.
- Visual Studio Code is the standard code editor.
- Install all recommended VS Code extensions.
- Verify every installation before proceeding to project creation.

---

# Common Mistakes

- Installing Node.js directly using `apt`.
- Skipping Git configuration.
- Using a non-LTS version of Node.js.
- Forgetting to reload the shell after installing NVM.
- Not verifying installations.
- Missing required VS Code extensions.

---

# Completion Checklist

- [ ] System updated
- [ ] Git installed
- [ ] Git configured
- [ ] NVM installed
- [ ] Node.js LTS installed
- [ ] npm verified
- [ ] VS Code installed
- [ ] Recommended extensions installed
- [ ] Development environment verified

---

# Next Step

Proceed to **02 - Create React Project**.