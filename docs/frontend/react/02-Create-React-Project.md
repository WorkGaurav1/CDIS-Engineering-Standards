# 02 - Create React Project

### Objective

Create a new React project using the standard project creation process followed at CDIS.

---

### Prerequisites

Before starting this guide, ensure the following:

- Development environment is ready.
- Node.js (Latest LTS) is installed.
- npm is installed.
- Visual Studio Code is installed.

---

## Step 1 - Navigate to the Workspace

Open a terminal and navigate to your development workspace.

#### Command

```bash
cd ~/Desktop/Projects
```

> Replace the path with your preferred development workspace if different.

---

## Step 2 - Create a New React Project

Create a new React project using Vite.

#### Command

```bash
npm create vite@latest my-react-app
```

#### Example

```bash
npm create vite@latest employee-portal
```

#### Expected Output

```text
✔ Project name: employee-portal
✔ Select a framework: React
✔ Select a variant: JavaScript
```

---

## Step 3 - Select Framework

Choose the React framework when prompted.

#### Selection

```text
Framework:
> React
```

---

## Step 4 - Select Variant

Choose the project variant.

#### Selection

```text
Variant:
> JavaScript
```

> If your project uses TypeScript, select **TypeScript** instead.

---

## Step 5 - Navigate to the Project Directory

Move into the newly created project.

#### Command

```bash
cd my-react-app
```

#### Example

```bash
cd employee-portal
```

---

## Step 6 - Install Project Dependencies

Install all project dependencies.

#### Command

```bash
npm install
```

#### Expected Output

```text
added xxx packages
```

---

## Step 7 - Verify Project Structure

Verify that the project has been created successfully.

#### Command

```bash
ls
```

#### Expected Output

```text
node_modules/
public/
src/
package.json
vite.config.js
README.md
```

---

## Step 8 - Start the Development Server

Run the application in development mode.

#### Command

```bash
npm run dev
```

#### Expected Output

```text
VITE vX.X.X ready in XXX ms

➜ Local:   http://localhost:5173/
```

---

## Step 9 - Verify the Application

Open the application in your browser.

#### URL

```text
http://localhost:5173
```

The default Vite + React application should load successfully.

---

## Step 10 - Stop the Development Server

Stop the running development server.

#### Command

```text
CTRL + C
```

---

## CDIS Standards

- Use **npm** as the package manager.
- Use **Vite** to create all new React projects unless instructed otherwise.
- Use meaningful project names.
- Use **kebab-case** for project names.
- Create projects only inside the designated development workspace.

---

## Common Mistakes

- Creating the project outside the development workspace.
- Selecting the wrong framework.
- Selecting the wrong project variant.
- Using spaces or uppercase letters in the project name.
- Forgetting to install project dependencies.
- Not verifying that the application starts successfully.

---

## Completion Checklist

- [ ] Project created successfully.
- [ ] React framework selected.
- [ ] Correct project variant selected.
- [ ] Dependencies installed.
- [ ] Development server started.
- [ ] Application opened successfully.
- [ ] Development server stopped.



Proceed to **03 - Configure-React-Project

---

[⬅ Previous](01-Development-Environment.md) | [Next ➡](03-Configure-React-Project.md)
