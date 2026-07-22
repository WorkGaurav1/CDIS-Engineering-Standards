# 05 - Configure Application Routing

### Objective

Configure client-side routing in a React application using **React Router** and organize the routing configuration according to the CDIS project structure.

---

### Prerequisites

Before starting this guide, ensure the following:

- React project has been created successfully.
- Project structure has been organized.
- The application runs successfully.
- Node.js and npm are installed.

---

## Step 1 - Install React Router

Install the React Router package.

#### Command

```bash
npm install react-router-dom
```

---

## Step 2 - Verify Installation

Verify that the package has been installed successfully.

#### Command

```bash
npm list react-router-dom
```

#### Expected Output

```text
react-router-dom@7.x.x
```

---

## Step 3 - Create the Router Directory

The routing configuration should be placed inside the `routes` directory.

If the directory does not already exist, create it.

#### Command

```bash
mkdir -p src/routes
```

---

## Step 4 - Create the Router Component

Create the router configuration file.

#### Command

```bash
touch src/routes/Router.jsx
```

---

## Step 5 - Create Initial Pages

Create the first application pages.

#### Commands

```bash
mkdir -p src/features/home/pages
mkdir -p src/features/about/pages

touch src/features/home/pages/HomePage.jsx
touch src/features/about/pages/AboutPage.jsx
```

---

## Step 6 - Create the Home Page

**File**

```text
src/features/home/pages/HomePage.jsx
```

```jsx
function HomePage() {
  return <h1>Home Page</h1>;
}

export default HomePage;
```

---

## Step 7 - Create the About Page

**File**

```text
src/features/about/pages/AboutPage.jsx
```

```jsx
function AboutPage() {
  return <h1>About Page</h1>;
}

export default AboutPage;
```

---

## Step 8 - Configure the Router

**File**

```text
src/routes/Router.jsx
```

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";

import HomePage from "../features/home/pages/HomePage";
import AboutPage from "../features/about/pages/AboutPage";

function Router() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
      </Routes>
    </BrowserRouter>
  );
}

export default Router;
```

---

## Step 9 - Configure the Application Entry Point

Replace the `App` component with the router.

**File**

```text
src/main.jsx
```

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import Router from "./routes/Router";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Router />
  </React.StrictMode>
);
```

---

## Step 10 - Create a Not Found Page

Create a page to handle unknown routes.

#### Commands

```bash
mkdir -p src/features/not-found/pages

touch src/features/not-found/pages/NotFoundPage.jsx
```

**File**

```text
src/features/not-found/pages/NotFoundPage.jsx
```

```jsx
function NotFoundPage() {
  return <h1>404 - Page Not Found</h1>;
}

export default NotFoundPage;
```

Update `src/routes/Router.jsx`.

```jsx
<Route path="*" element={<NotFoundPage />} />
```

---

## Step 11 - Start the Development Server

Run the application.

#### Command

```bash
npm run dev
```

---

## Step 12 - Verify the Routes

Open the following URLs in your browser.

| URL | Expected Result |
|------|-----------------|
| `/` | Home Page |
| `/about` | About Page |
| `/random-page` | 404 - Page Not Found |

---

## CDIS Standards

- Store all routing configuration inside the `src/routes` directory.
- Maintain a single router configuration for the application.
- Store page components inside their respective feature modules.
- Name page components using the `Page` suffix (e.g., `HomePage`, `AboutPage`).
- Include a catch-all (`*`) route to handle unknown URLs.
- Keep routing configuration separate from page implementation.

> **Note:** The use of `BrowserRouter` is the standard for React web applications as documented by React Router. If project requirements differ (e.g., static hosting or Electron applications), another router may be more appropriate.

---

## Common Mistakes

- Defining routes directly inside `App.jsx`.
- Mixing routing logic with business logic.
- Creating multiple router configurations.
- Omitting a catch-all route for unknown URLs.
- Placing page components outside their feature directory.
- Hardcoding navigation paths throughout the application instead of centralizing route definitions (if adopted later).

---

## Completion Checklist

- [ ] React Router installed.
- [ ] Router component created.
- [ ] Initial pages created.
- [ ] Router configured.
- [ ] `main.jsx` updated.
- [ ] Not Found page created.
- [ ] Application routes verified.

---

## References

### Official Documentation

- React Documentation  
  https://react.dev/

- React Router Documentation  
  https://reactrouter.com/

- Vite Documentation  
  https://vite.dev/

### Industry References

- React Router maintainers' recommended routing patterns (React Router documentation)
- Feature-based project organization commonly used in large React applications (industry practice)