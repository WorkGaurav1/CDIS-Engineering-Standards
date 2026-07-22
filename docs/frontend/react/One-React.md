# React Development Standards

**Document Version:** 1.0  
**Technology:** React + Vite + JavaScript  
**Audience:** Frontend Engineers

---

# Purpose

This document defines the standard practices for developing React applications at CDIS.

It is intended for engineers who already have working knowledge of React. This document does **not** teach React. Instead, it describes how React applications should be structured, organized, and maintained while working on CDIS projects.

---

# Technology Stack

| Technology | Standard |
|------------|----------|
| Framework | React |
| Build Tool | Vite |
| Language | JavaScript |
| Package Manager | npm |
| Routing | React Router |
| HTTP Client | Axios |
| Linting | ESLint |
| Formatting | Prettier |

---

# Create a New Project

```bash
npm create vite@latest my-app -- --template react

cd my-app

npm install

npm run dev
```

---

# Standard Project Structure

Every React project should follow the structure below.

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

# Directory Standards

| Directory | Purpose |
|-----------|---------|
| assets | Images, icons, fonts and static files |
| components | Shared reusable components |
| constants | Application constants |
| features | Business modules |
| hooks | Shared custom hooks |
| layouts | Application layouts |
| routes | Router configuration |
| services | Shared services and API configuration |
| styles | Global styles |
| utils | Helper functions |

---

# Feature Organization

Each business feature should remain self-contained.

Example:

```text
features/
└── grievance/
    ├── components/
    ├── pages/
    ├── services/
    ├── hooks/
    ├── utils/
    ├── constants.js
    └── index.js
```

Avoid creating feature folders until they are required.

---

# Naming Conventions

## Components

Use PascalCase.

```text
Button.jsx
EmployeeCard.jsx
DashboardLayout.jsx
```

---

## Pages

Use PascalCase with the **Page** suffix.

```text
HomePage.jsx
LoginPage.jsx
EmployeeProfilePage.jsx
```

---

## Layouts

Use PascalCase with the **Layout** suffix.

```text
MainLayout.jsx
DashboardLayout.jsx
AuthLayout.jsx
```

---

## Hooks

Use camelCase beginning with **use**.

```text
useAuth.js

useFetch.js

useEmployee.js
```

---

## Services

Use camelCase.

```text
employeeService.js

authService.js
```

---

## Utility Files

Use camelCase.

```text
dateFormatter.js

validation.js
```

---

## Constants

Use UPPER_SNAKE_CASE.

```javascript
export const API_URL = "...";

export const MAX_USERS = 10;
```

---

# Routing Standards

- Use React Router.
- Store router configuration inside `src/routes`.
- Store pages inside feature modules.
- Include a Not Found page.
- Keep routing logic separate from page components.

Example:

```text
routes/
    Router.jsx

features/
    employee/
        pages/
```

---

# Component Standards

Reusable components belong inside

```text
components/
```

Feature-specific components belong inside

```text
features/<feature>/components/
```

Guidelines

- One component per file.
- One default export per component.
- Keep components focused on a single responsibility.
- Reuse existing components whenever possible.

---

# Styling Standards

- Use CSS Modules, Tailwind CSS, or the project-approved styling approach.
- Avoid inline styles unless necessary.
- Keep global styles inside `styles/`.
- Keep component-specific styles close to the component.

---

# API Standards

- Use Axios for HTTP requests.
- Configure shared Axios instances inside `services/`.
- Keep API calls outside UI components.
- Store feature-specific APIs inside the corresponding feature.

Example

```text
services/
    api.js

features/
    grievance/
        services/
```

---

# State Management Standards

Choose state management based on application complexity.

| Project Size | Recommended Solution |
|--------------|----------------------|
| Small | React Context API |
| Medium / Large | Redux Toolkit |

General guidelines

- Keep local state inside components.
- Keep global state minimal.
- Avoid unnecessary global state.

---

# Environment Variables

Store environment variables inside

```text
.env
```

Access them using

```javascript
import.meta.env
```

Never hardcode

- API URLs
- Tokens
- Secrets
- Environment-specific values

---

# Code Quality Standards

- Use ESLint.
- Use Prettier.
- Fix linting errors before committing.
- Keep functions small and readable.
- Remove unused imports.
- Remove dead code.
- Prefer descriptive names over abbreviations.

---

# Git Standards

- Create a feature branch for new work.
- Write meaningful commit messages.
- Keep commits focused on a single change.
- Open a Pull Request before merging.
- Resolve review comments before approval.

---

# General Engineering Practices

- Follow the established project structure.
- Avoid duplicate code.
- Prefer reusable components.
- Keep business logic separate from UI.
- Keep feature modules independent.
- Write readable and maintainable code.
- Remove unused files and dependencies.
- Keep imports organized.
- Follow existing project conventions.

---

# Project Checklist

Before submitting your work, verify the following:

- [ ] Project follows the standard directory structure.
- [ ] Naming conventions are followed.
- [ ] ESLint passes.
- [ ] Prettier formatting is applied.
- [ ] Routing is configured correctly.
- [ ] Components are reusable where appropriate.
- [ ] API calls are separated from UI.
- [ ] No unused code remains.
- [ ] Environment variables are used appropriately.
- [ ] Changes have been committed and pushed.

---

# References

## Official Documentation

- React Documentation  
  https://react.dev/

- Vite Documentation  
  https://vite.dev/

- React Router Documentation  
  https://reactrouter.com/

- Axios Documentation  
  https://axios-http.com/

- ESLint Documentation  
  https://eslint.org/

- Prettier Documentation  
  https://prettier.io/

- npm Documentation  
  https://docs.npmjs.com/

## Industry References

- Airbnb JavaScript Style Guide  
  https://github.com/airbnb/javascript

- Google Engineering Practices  
  https://google.github.io/eng-practices/

- Refactoring – Martin Fowler

- Clean Code – Robert C. Martin

- Clean Architecture – Robert C. Martin