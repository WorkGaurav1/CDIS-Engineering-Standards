# 03 - Configure React Project

### Objective

Configure a newly created React project according to CDIS engineering standards. This includes cleaning the default Vite project, installing code quality tools, and preparing the project for development.

---

### Prerequisites

Before starting this guide, ensure the following:

- React development environment is ready.
- React project has been created successfully.
- Project dependencies have been installed.
- You are inside the project directory.

---

## Step 1 - Open the Project

Navigate to the React project.

#### Command

```bash
cd employee-portal
```

Replace **employee-portal** with your project name.

---

## Step 2 - Open the Project in Visual Studio Code

Open the project in Visual Studio Code.

#### Command

```bash
code .
```

---

## Step 3 - Verify the Default Project Structure

Verify that the project has been created successfully.

#### Command

```bash
tree -L 2
```

#### Expected Output

```text
employee-portal
├── node_modules
├── public
├── src
│   ├── assets
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   ├── main.jsx
│   └── assets/react.svg
├── package.json
├── vite.config.js
└── README.md
```

---

## Step 4 - Remove the Default Vite Boilerplate

The default Vite project contains sample code that is not required for CDIS applications.

Remove the following files:

```text
src/App.css
src/assets/react.svg
```

#### Command

```bash
rm src/App.css
rm src/assets/react.svg
```

---

## Step 5 - Clean App.jsx

Replace the default sample component with a minimal application component.

#### File

```text
src/App.jsx
```

#### Example

```jsx
function App() {
  return <h1>Welcome to CDIS</h1>;
}

export default App;
```

---

## Step 6 - Clean main.jsx

Remove unused stylesheet imports.

#### File

```text
src/main.jsx
```

#### Example

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## Step 7 - Install ESLint

CDIS uses ESLint to enforce consistent coding standards across all React projects.

#### Command

```bash
npm install -D eslint
```

---

## Step 8 - Install Prettier

CDIS uses Prettier to maintain consistent code formatting.

#### Command

```bash
npm install -D prettier
```

---

## Step 9 - Install ESLint Plugins

Install the required plugins for React projects.

#### Command

```bash
npm install -D eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-react-refresh
```

---

## Step 10 - Create ESLint Configuration

Create the ESLint configuration file.

#### Command

```bash
touch eslint.config.js
```

> Configure the file using the standard CDIS ESLint configuration.

---

## Step 11 - Create Prettier Configuration

Create the Prettier configuration file.

#### Commands

```bash
touch .prettierrc
touch .prettierignore
```

> Configure these files using the standard CDIS Prettier configuration.

---

## Step 12 - Verify Installed Packages

Verify that all required development dependencies are installed.

#### Command

```bash
npm list --depth=0
```

#### Expected Output

The following packages should be present.

```text
eslint
prettier
eslint-plugin-react
eslint-plugin-react-hooks
eslint-plugin-react-refresh
```

---

## Step 13 - Run ESLint

Verify that ESLint runs successfully.

#### Command

```bash
npx eslint .
```

No errors should be reported.

---

## Step 14 - Start the Development Server

Run the application.

#### Command

```bash
npm run dev
```

#### Expected Output

```text
VITE vX.X.X

Local: http://localhost:5173/
```

---

## Step 15 - Verify the Application

Open the application in your browser.

```text
http://localhost:5173
```

The application should display:

```text
Welcome to CDIS
```

---

## CDIS Standards

- Use **npm** as the package manager.
- Remove the default Vite boilerplate before development.
- Keep the initial application clean and minimal.
- Use **ESLint** for code quality.
- Use **Prettier** for code formatting.
- Always verify the project before starting feature development.

---

## Common Mistakes

- Leaving the default Vite sample code.
- Forgetting to remove unused assets.
- Installing packages globally instead of as development dependencies.
- Modifying ESLint or Prettier configuration without team approval.
- Starting development without verifying the project.

---

## Completion Checklist

- [ ] Project opened successfully.
- [ ] Default Vite boilerplate removed.
- [ ] App.jsx cleaned.
- [ ] main.jsx cleaned.
- [ ] Unused assets removed.
- [ ] ESLint installed.
- [ ] Prettier installed.
- [ ] ESLint plugins installed.
- [ ] ESLint configuration created.
- [ ] Prettier configuration created.
- [ ] Lint executed successfully.
- [ ] Development server verified.



Proceed to **04 - Organize Project Structure**

---

[⬅ Previous](02-Create-React-Project.md) | [Next ➡](04-Orgnize-Project-Struture.md)
