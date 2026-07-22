# 04 - Organize Project Structure

### Objective

Organize the React project using the official CDIS project architecture. A consistent project structure improves code maintainability, scalability, collaboration, and developer onboarding.

---

### Prerequisites

Before starting this guide, ensure the following:

- React project has been created successfully.
- Project configuration has been completed.
- ESLint and Prettier have been configured.
- The project runs successfully.

---

## CDIS Project Architecture

All React applications developed at CDIS must follow the standard Hybrid Architecture.

The project consists of:

- Shared resources
- Feature-specific modules

Shared resources are accessible throughout the application, while feature-specific code remains isolated within its respective feature.

---

## Standard Project Structure

```text
src/
│
├── assets/
│
├── components/
│   ├── common/
│   └── ui/
│
├── constants/
│
├── features/
│
├── hooks/
│
├── layouts/
│
├── routes/
│
├── services/
│
├── styles/
│
├── utils/
│
├── App.jsx
└── main.jsx
```

---

## Step 1 - Review the Current Project Structure

Verify the current structure of the project.

#### Command

```bash
tree src
```

#### Expected Output

```text
src
├── App.jsx
├── index.css
└── main.jsx
```

---

## Step 2 - Create the Shared Directories

Create the standard shared directories.

#### Command

```bash
mkdir src/assets
mkdir src/components
mkdir src/constants
mkdir src/features
mkdir src/hooks
mkdir src/layouts
mkdir src/routes
mkdir src/services
mkdir src/styles
mkdir src/utils
```

---

## Step 3 - Create Component Directories

Create the standard component directories.

#### Command

```bash
mkdir src/components/common
mkdir src/components/ui
```

---

## Step 4 - Verify the Directory Structure

Verify that all directories have been created successfully.

#### Command

```bash
tree src -L 2
```

#### Expected Output

```text
src
├── assets
├── components
│   ├── common
│   └── ui
├── constants
├── features
├── hooks
├── layouts
├── routes
├── services
├── styles
├── utils
├── App.jsx
├── index.css
└── main.jsx
```

---

## Shared Directory Responsibilities

| Directory | Responsibility |
|------------|----------------|
| assets | Images, icons, fonts, and static resources |
| components | Shared reusable React components |
| components/common | Common application components |
| components/ui | Generic UI components such as Button, Input, Card and Modal |
| constants | Application-wide constants |
| hooks | Shared custom React hooks |
| layouts | Application layouts |
| routes | Routing configuration |
| services | Shared services such as API clients |
| styles | Global styles and themes |
| utils | Shared helper and utility functions |

---

## Feature Directory

Feature-specific code should always be placed inside the **features** directory.

Do not create feature folders until they are required.

Example:

```text
features/
└── grievance/
```

---

## Standard Feature Structure

Each feature should follow the same directory structure.

```text
features/
└── grievance/
    ├── components/
    ├── pages/
    ├── hooks/
    ├── services/
    ├── utils/
    ├── constants.js
    └── index.js
```

Another example:

```text
features/
└── employee/
    ├── components/
    ├── pages/
    ├── hooks/
    ├── services/
    ├── utils/
    ├── constants.js
    └── index.js
```

---

## Architecture Guidelines

- Shared code belongs in the root-level directories.
- Feature-specific code belongs inside the corresponding feature.
- Keep each feature self-contained.
- Reuse shared components instead of duplicating code.
- Avoid dependencies between features whenever possible.

---

## CDIS Standards

- Every React application must follow the CDIS Hybrid Architecture.
- Do not create unnecessary top-level directories.
- Shared components must be stored in `components`.
- Feature-specific components must remain inside their feature.
- Shared services must be stored in `services`.
- Routing configuration must be stored in `routes`.
- Utility functions must be stored in `utils`.
- Global styles must be stored in `styles`.
- New features should be added inside the `features` directory.

---

## Common Mistakes

- Placing feature-specific code inside shared directories.
- Creating duplicate utility functions.
- Storing API calls directly inside React components.
- Mixing shared and feature-specific components.
- Creating empty feature folders before they are needed.
- Creating new top-level directories without team approval.

---

## Completion Checklist

- [ ] Standard project directories created.
- [ ] Shared component directories created.
- [ ] Project structure verified.
- [ ] Directory responsibilities understood.
- [ ] Feature architecture understood.
- [ ] Project follows the CDIS standard.



Proceed to **05 - Configure Application Routing**

---

[⬅ Previous](03-Configure-React-Project.md) | [Next ➡](05-Configure Application Routing.md)
