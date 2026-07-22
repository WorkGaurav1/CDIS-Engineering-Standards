# 01 - Development Environment

### Objective

Prepare your local machine for React development by installing the React-specific tools and verifying that the environment is ready.

---

### Prerequisites

Before starting this guide, ensure the following are already completed.

- Git is installed and configured. If not, install it from the official site: [Git Downloads](https://git-scm.com/downloads)
- Node.js (Latest LTS) is installed. If not, install it from the official site: [Node.js Downloads](https://nodejs.org/en/download/)
- npm is installed. If not, install it from the official site: [npm Install](https://www.npmjs.com/get-npm)
- Visual Studio Code is installed. If not, install it from the official site: [VS Code](https://code.visualstudio.com/)

---

## Step 1 - Verify Node.js Installation

Verify that Node.js is installed.

#### Command

```bash
node -v
```

#### Expected Output

```text
v22.x.x
```

---

## Step 2 - Verify npm Installation

Verify that npm is installed.

#### Command

```bash
npm -v
```

#### Expected Output

```text
10.x.x
```

---

## Step 3 - Install React Developer Tools (Browser Extension)

Install the React Developer Tools extension for your browser.

Supported browsers:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox

Verify that the extension appears in your browser extensions list.

---

## Step 4 - Install Recommended VS Code Extensions

Install the following extensions.

#### ESLint

```bash
code --install-extension dbaeumer.vscode-eslint
```

#### Prettier

```bash
code --install-extension esbenp.prettier-vscode
```

#### ES7+ React Snippets

```bash
code --install-extension dsznajder.es7-react-js-snippets
```

#### Auto Rename Tag

```bash
code --install-extension formulahendry.auto-rename-tag
```

#### Path Intellisense

```bash
code --install-extension christian-kohler.path-intellisense
```

#### Error Lens

```bash
code --install-extension usernamehw.errorlens
```

---

## Step 5 - Verify VS Code Extensions

#### Command

```bash
code --list-extensions
```

Verify that all required extensions are installed.

---

## Step 6 - Verify React Development Environment

Run the following commands.

```bash
node -v
```

```bash
npm -v
```

```bash
code --version
```

Everything should execute without errors.

---

## CDIS Standards

- Use npm as the package manager.
- Use Visual Studio Code for React development.
- Install all required VS Code extensions.
- Install React Developer Tools before starting development.

---

## Common Mistakes

- Missing React Developer Tools.
- Missing ESLint extension.
- Missing Prettier extension.
- Using a package manager other than npm.

---

## Completion Checklist

- [ ] Node.js verified
- [ ] npm verified
- [ ] React Developer Tools installed
- [ ] VS Code extensions installed
- [ ] Environment verified



Proceed to **02 - Create React Project**

---

[Next ➡](02-Create-React-Project.md)
