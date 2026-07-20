# React Guidelines

> A living document describing how we structure, write, review, and ship React applications. The goal is consistency across engineers and teams — any developer should be able to open a component in an unfamiliar part of the codebase and immediately understand its shape, conventions, and intent.

---

## 1. Introduction

### 1.1 Purpose

This document defines the standard for writing React code in our organization. It exists so that:

- Code looks like it was written by one disciplined team, not many individuals with different habits.
- New engineers ramp up faster because patterns are predictable.
- Code review focuses on logic and design, not bikeshedding over formatting or structure.
- Performance, accessibility, and security are baked in by default, not bolted on later.

### 1.2 Scope

Applies to all React (and React Native, where noted) codebases owned by the team, including internal tools, customer-facing apps, and shared component libraries.

### 1.3 Guiding Principles

1. **Consistency over personal preference.** If the guide says X and you prefer Y, do X — then propose a change to the guide via PR if you have a strong case.
2. **Readability over cleverness.** Code is read far more often than it's written.
3. **Composition over inheritance.** Build UI from small, focused, composable pieces.
4. **Explicit over implicit.** Prefer clear prop names, typed contracts, and predictable data flow over "magic."
5. **Colocate what changes together.** Keep related logic, styles, and tests near the component they belong to.
6. **Optimize for deletion.** Code should be easy to remove, not just easy to add to.

### 1.4 Tooling Baseline

| Category | Standard |
|---|---|
| Language | TypeScript (strict mode) |
| Bundler | Vite / Next.js (per project) |
| Linting | ESLint (`eslint-config-airbnb` or internal config + `eslint-plugin-react`, `eslint-plugin-react-hooks`, `eslint-plugin-jsx-a11y`) |
| Formatting | Prettier (no debate — formatting is automated, not reviewed) |
| Testing | Vitest/Jest + React Testing Library, Playwright/Cypress for E2E |
| State | React Query / RTK Query for server state, Zustand/Redux Toolkit for client state |
| Package manager | pnpm (workspace-aware, deterministic lockfile) |

---

## 2. React Project Structure

### 2.1 Feature-based (domain-driven) structure — preferred at scale

Group by feature/domain rather than by file type. This scales far better than `components/`, `hooks/`, `utils/` top-level buckets once a project grows past a handful of screens.

```
src/
├── app/                     # App shell: routing, providers, layout
│   ├── App.tsx
│   ├── providers/
│   └── routes/
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── api/
│   │   ├── types.ts
│   │   ├── auth.slice.ts
│   │   └── index.ts         # public API of the feature
│   └── invoices/
│       ├── components/
│       ├── hooks/
│       ├── api/
│       ├── types.ts
│       └── index.ts
├── components/              # Shared, reusable, domain-agnostic UI (design system)
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx
│   │   ├── Button.stories.tsx
│   │   └── index.ts
│   └── Modal/
├── hooks/                   # Shared, cross-feature hooks
├── lib/                     # Third-party wrappers, API client, config
├── utils/                   # Pure, generic helper functions
├── styles/                  # Global styles, theme, design tokens
├── types/                   # Global/shared TypeScript types
└── test/                    # Test setup, mocks, shared test utilities
```

### 2.2 Rules

- A **feature module** should expose a single public entry point (`index.ts`) — everything else is an implementation detail. Other features import *only* from that entry point, never reach into `features/auth/components/LoginForm` directly.
- `components/` (the shared design-system layer) must never import from `features/`. Dependency direction is one-way: `features → components`, never the reverse.
- Avoid deeply nested folders (> 3–4 levels). If you're nesting that deep, it's usually a sign the feature needs splitting.
- Co-locate tests, stories, and styles with the component they describe — not in a parallel `__tests__` tree at the repo root.
- One component per file. The file name matches the component name.

---

## 3. Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Component files & components | `PascalCase` | `UserProfileCard.tsx` |
| Hook files & hooks | `camelCase`, prefixed `use` | `useDebouncedValue.ts` |
| Utility/helper files | `camelCase` | `formatCurrency.ts` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Types & interfaces | `PascalCase`, no `I` prefix | `UserProfile`, not `IUserProfile` |
| Enums | `PascalCase` name, `PascalCase` members | `enum OrderStatus { Pending, Shipped }` |
| Context | `PascalCase` + `Context` suffix | `ThemeContext` |
| CSS Modules classes | `camelCase` | `styles.primaryButton` |
| Event handler props | `on` + verb (`onClick`, `onSubmit`) | — |
| Event handler implementations | `handle` + verb (`handleClick`, `handleSubmit`) | — |
| Boolean props/vars | `is`/`has`/`should` prefix | `isLoading`, `hasError`, `shouldRetry` |
| Test files | `*.test.tsx` or `*.spec.tsx` colocated | `Button.test.tsx` |

### 3.1 Additional rules

- Directory names for feature modules: `kebab-case` or `camelCase` — pick one per repo and stay consistent (`user-settings/` vs `userSettings/`).
- Avoid abbreviations that aren't universally understood (`btn` → `Button` is fine in JSX shorthand contexts, but don't invent `usrCfg`).
- Generic names (`data`, `item`, `handleClick` with no context) are acceptable only in tightly scoped local blocks; anything exported needs a descriptive name.
- Custom hooks must start with `use` even if they don't call other hooks internally that require it stylistically — this is what allows the linter (`react-hooks/rules-of-hooks`) to enforce hook rules.

---

## 4. Components

### 4.1 Function components only

No class components in new code. Function components + hooks are the standard; class components only survive in legacy code pending migration.

### 4.2 Component size and single responsibility

- A component should do one thing. If a `.tsx` file exceeds ~200–250 lines, that's a signal to extract subcomponents or hooks.
- Prefer many small, well-named components over one large component with internal `if`/ternary soup for layout variants.

### 4.3 Props

- Define an explicit `Props` type/interface per component, named `{ComponentName}Props`.
- Destructure props in the function signature, not inside the body.
- Avoid prop drilling more than 2–3 levels — use composition (`children`), context, or state management instead.
- Provide default values via default parameters, not `defaultProps` (deprecated for function components).
- Avoid boolean prop explosion (`isLarge`, `isPrimary`, `isDanger`...) — prefer a single `variant`/`size` enum prop.

```tsx
type ButtonProps = {
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  isLoading?: boolean;
  onClick?: (event: React.MouseEvent<HTMLButtonElement>) => void;
  children: React.ReactNode;
};

export function Button({
  variant = 'primary',
  size = 'md',
  isLoading = false,
  onClick,
  children,
}: ButtonProps) {
  return (
    <button className={buildClassName(variant, size)} onClick={onClick} disabled={isLoading}>
      {isLoading ? <Spinner /> : children}
    </button>
  );
}
```

### 4.4 Composition patterns

- **Container/Presentational split** where it adds clarity: presentational components are pure and receive data via props; containers own data-fetching/state.
- **Compound components** for related UI clusters (`<Tabs>`, `<Tabs.List>`, `<Tabs.Panel>`) instead of one component with a dozen configuration props.
- **Render props / children-as-function** sparingly — hooks have replaced most use cases.
- Prefer **`children`** over passing whole render functions as props when composing layout.

### 4.5 Exports

- Named exports by default (`export function Button`). Reserve `export default` for page-level/route components where the framework requires it (e.g., Next.js pages).
- Named exports give better refactor safety and auto-import behavior across the codebase.

### 4.6 File organization within a component file

1. Imports (external libs → internal absolute imports → relative imports → styles)
2. Types
3. Constants local to the file
4. The component
5. Small private subcomponents (if truly local and unused elsewhere)
6. Helper functions used only by this component

---

## 5. Hooks

### 5.1 Rules of Hooks (non-negotiable, enforced by lint)

- Only call hooks at the top level — never inside loops, conditions, or nested functions.
- Only call hooks from React function components or custom hooks.

### 5.2 Built-in hooks — usage guidance

- **`useState`**: keep state minimal and normalized. Don't store derived data in state — compute it during render.
- **`useEffect`**: the escape hatch, not the default tool. Before reaching for `useEffect`, ask "can this be computed during render, or handled in an event handler instead?" Most data fetching should go through React Query/RTK Query, not raw `useEffect`.
- **`useMemo` / `useCallback`**: use for genuinely expensive computations or to preserve referential stability for memoized children/dependency arrays — not as a reflexive habit on every value.
- **`useRef`**: for DOM references and mutable values that must not trigger re-renders.
- **`useContext`**: for cross-cutting concerns (theme, auth, i18n) — not as a substitute for prop passing in shallow trees.
- **`useReducer`**: prefer over `useState` when state transitions are complex or interdependent (multiple sub-values changing together).

### 5.3 Custom hooks

- Extract logic into a custom hook once it's reused twice, or once a component's local logic (not JSX) exceeds what's easily scannable.
- Custom hooks should have a single, clear responsibility and a name that describes *what they return*, e.g. `useCurrentUser`, `useDebouncedValue`, `useMediaQuery`.
- Always specify complete and correct dependency arrays — do not silence `react-hooks/exhaustive-deps` without a documented reason.

```tsx
function useDebouncedValue<T>(value: T, delayMs: number): T {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delayMs);
    return () => clearTimeout(timer);
  }, [value, delayMs]);

  return debounced;
}
```

### 5.4 Effect cleanup

Every effect that subscribes, sets a timer, or opens a connection must return a cleanup function. This is not optional.

---

## 6. State Management

### 6.1 Decide the *kind* of state first

| Kind of state | Where it belongs |
|---|---|
| Local UI state (toggle, input value, hover) | `useState` / `useReducer` in the component |
| Shared state between a few nearby components | Lift state up to the nearest common ancestor |
| Server/remote data (fetched from an API) | React Query / RTK Query / SWR — **not** Redux |
| Global client state (auth session, theme, feature flags) | Context (low-frequency updates) or Zustand/Redux Toolkit (high-frequency, complex updates) |
| URL-derived state (filters, pagination, tabs) | URL search params / router state — not component state |
| Form state | Dedicated form library (React Hook Form / Formik) |

### 6.2 Rules

- Don't reach for Redux/global state by default — most state is local. Lift state only as far up the tree as necessary, no further.
- Never duplicate server data into client global state "for convenience" — it goes stale. Let the data-fetching layer own caching, invalidation, and revalidation.
- Context is for **low-frequency** updates (theme, locale, auth). Using Context for frequently-changing values (e.g. mouse position, form input on every keystroke) causes needless re-renders across the whole subtree — use a state library or colocate the state instead.
- Normalize state shape (avoid deeply nested state trees); prefer flat structures keyed by ID.
- Derived values (totals, filtered lists, formatted strings) are computed during render or via `useMemo`, never stored as separate state that can drift out of sync.

---

## 7. Routing

- Use the framework's router (Next.js App Router, React Router v6+) — don't hand-roll route matching.
- Route-level code splitting is the default: each route/page is lazy-loaded (`React.lazy` + `Suspense`, or framework-native lazy routing).
- Keep route components thin — they compose feature components; they don't contain business logic themselves.
- Co-locate route-specific data loading with the route (loaders in React Router, Server Components / `fetch` in Next.js) rather than fetching in a `useEffect` after mount, to avoid waterfalls.
- Guard protected routes with a dedicated wrapper (`<RequireAuth>`) rather than scattering auth checks through individual pages.
- Keep URL as the source of truth for shareable/bookmarkable state (filters, search queries, active tab, pagination page).
- Use typed route params/search params (e.g. `zod` schema validation on `useSearchParams`) instead of trusting raw strings.

---

## 8. API Calling

### 8.1 Data fetching library, not raw `fetch` in components

Use **React Query** (TanStack Query) or **RTK Query** for all server-state interactions. Benefits: caching, deduplication, background refetching, retries, loading/error states, and cache invalidation — all handled consistently instead of reimplemented per component.

```tsx
function useInvoice(invoiceId: string) {
  return useQuery({
    queryKey: ['invoice', invoiceId],
    queryFn: () => api.invoices.getById(invoiceId),
    staleTime: 60_000,
  });
}

function InvoiceDetail({ invoiceId }: { invoiceId: string }) {
  const { data: invoice, isLoading, isError } = useInvoice(invoiceId);

  if (isLoading) return <Spinner />;
  if (isError) return <ErrorState />;

  return <InvoiceSummary invoice={invoice} />;
}
```

### 8.2 API layer conventions

- All HTTP calls go through a single typed API client (`lib/apiClient.ts`) that centralizes base URL, auth headers, error normalization, and retry/backoff — components never call `fetch`/`axios` directly.
- Group API functions by domain in `features/{feature}/api/`, mirroring the backend's resource boundaries.
- Define request/response types (or generate them from an OpenAPI/GraphQL schema) — never use `any` for API payloads.
- Validate external responses at the boundary (e.g. `zod.parse`) when data shape correctness matters, especially for third-party APIs.
- Mutations invalidate the relevant query keys on success rather than manually patching cached data, unless optimistic updates are specifically required for UX.
- Centralize error shape handling (401 → redirect to login, 5xx → toast + retry, validation errors → surface to form) in one interceptor, not per call-site.

---

## 9. Forms

- Use **React Hook Form** (uncontrolled-by-default, minimal re-renders) for anything beyond a trivial 1–2 field form.
- Pair with a schema validation library (**Zod** or **Yup**) — one schema defines both the TypeScript type and the runtime validation, so they can never drift apart.

```tsx
const schema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

type LoginFormValues = z.infer<typeof schema>;

function LoginForm() {
  const { register, handleSubmit, formState: { errors, isSubmitting } } =
    useForm<LoginFormValues>({ resolver: zodResolver(schema) });

  const onSubmit = async (values: LoginFormValues) => {
    await login(values);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('email')} aria-invalid={!!errors.email} />
      {errors.email && <FieldError message={errors.email.message} />}
      <button type="submit" disabled={isSubmitting}>Log in</button>
    </form>
  );
}
```

### Rules

- Every input has an associated `<label>` (or `aria-label`) — never a placeholder-only field.
- Show validation errors inline, next to the field, not only in a toast.
- Disable the submit button (or show a spinner) while submitting; guard against double-submit.
- Keep form schemas colocated with the form component or in the feature's `types.ts`.
- Reusable field components (`<TextField>`, `<SelectField>`) live in `components/` and wrap the design system's inputs with label/error/help-text handling built in.

---

## 10. Error Handling

### 10.1 Component-tree level: Error Boundaries

Wrap route-level (and any risky third-party-heavy) subtrees in an `ErrorBoundary` so a render error in one part of the UI doesn't blank the whole page.

```tsx
<ErrorBoundary fallback={<RouteErrorFallback />}>
  <Suspense fallback={<PageSkeleton />}>
    <InvoicesPage />
  </Suspense>
</ErrorBoundary>
```

### 10.2 Async/data errors

- React Query's `isError`/`error` state drives UI-level error states — don't wrap fetch calls in ad hoc `try/catch` inside components.
- Normalize errors into a consistent shape (`{ message, code, status }`) in the API client so UI code never branches on raw Axios/Fetch error internals.
- Distinguish **recoverable** errors (show inline message + retry) from **fatal** errors (redirect / full-page fallback).

### 10.3 Logging & monitoring

- All uncaught errors (from Error Boundaries and a global `window.onerror`/`unhandledrejection` listener) are reported to the error-tracking service (Sentry or equivalent) with user/session context attached.
- Never swallow errors silently (`catch {}`) — at minimum log them; usually surface them to the user.
- User-facing error messages are actionable and non-technical; technical detail goes to the logs, not the UI.

---

## 11. Performance

### 11.1 Rendering

- Avoid unnecessary re-renders: keep component state as local as possible, split large components so unrelated state doesn't force unrelated UI to re-render.
- Use `React.memo` for components that render often with unchanged props (especially list items) — verify with the profiler before applying it everywhere.
- Stabilize function/object identity passed to memoized children with `useCallback`/`useMemo`, but only where it actually matters (measured, not guessed).
- Virtualize long lists (`react-window`/`react-virtual`) — never render thousands of DOM nodes.

### 11.2 Code splitting & bundle size

- Route-based code splitting is mandatory (see §7).
- Lazy-load heavy, rarely-used components (rich text editors, charting libraries, modals with heavy content) with `React.lazy`.
- Monitor bundle size in CI (e.g. `size-limit`, `bundlephobia` checks on new deps) — a new dependency needs justification if it meaningfully grows the bundle.
- Avoid importing entire libraries when a subset will do (`import debounce from 'lodash/debounce'`, not the whole `lodash` package, unless tree-shaking is verified).

### 11.3 Images & assets

- Use modern formats (WebP/AVIF) and responsive `srcset`/framework image components (`next/image`) for automatic optimization and lazy loading.
- Preload critical above-the-fold assets; lazy-load everything below the fold.

### 11.4 Measuring

- Use the React DevTools Profiler and Lighthouse/Web Vitals (LCP, INP, CLS) as the source of truth — optimize based on measurement, not intuition.
- Track Core Web Vitals in production via RUM (real user monitoring), not just synthetic lab tests.

---

## 12. Accessibility (a11y)

Accessibility is a requirement, not a nice-to-have — enforced via `eslint-plugin-jsx-a11y` in CI, not left to manual review alone.

### 12.1 Baseline rules

- Use semantic HTML first (`<button>`, `<nav>`, `<header>`, `<main>`) — reach for `<div onClick>` only when no semantic element fits, and if so, add the appropriate `role` and keyboard handling.
- Every interactive element must be reachable and operable via keyboard alone (`Tab`, `Enter`, `Space`, `Esc` where relevant).
- All images need meaningful `alt` text (or `alt=""` for purely decorative images).
- Form fields have associated labels; error messages are linked via `aria-describedby`.
- Maintain a visible focus indicator — never `outline: none` without a replacement focus style.
- Manage focus explicitly for modals/drawers/route changes: trap focus in modals, return focus to the trigger on close, move focus to the new page's heading on route change.
- Respect color contrast minimums (WCAG AA: 4.5:1 for normal text, 3:1 for large text).
- Don't convey information by color alone (e.g. error state needs an icon/text, not just red).
- Test with a screen reader (VoiceOver/NVDA) for any complex custom widget (dropdown, combobox, date picker).

### 12.2 ARIA

- "No ARIA is better than bad ARIA" — prefer native elements; use ARIA roles/attributes only to fill gaps native HTML can't express.
- Custom widgets (menus, tabs, comboboxes) follow the [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) patterns exactly, including expected keyboard interactions.

---

## 13. Security

- **Never** use `dangerouslySetInnerHTML` with unsanitized content. If HTML must be rendered from user/CMS input, sanitize with a library like DOMPurify first.
- Treat all data from the URL, query params, and user input as untrusted — validate/escape before rendering or sending to an API.
- Never store secrets, API keys, or tokens in client-side code or `localStorage` if they grant privileged access — use httpOnly, secure, `SameSite` cookies for session tokens.
- Avoid `eval`, `new Function()`, and dynamic script injection from user-controlled strings.
- Guard against open redirects: validate any redirect URL against an allowlist before navigating.
- Enforce Content Security Policy (CSP) headers at the app/server level; don't rely on client-side checks alone.
- Keep dependencies patched — run `npm audit`/Dependabot/Renovate in CI and treat high-severity advisories as blocking.
- Any client-side authorization check (hiding a button, disabling a route) is a UX convenience only — the backend must independently enforce the same authorization. Never trust the client as the security boundary.
- Sanitize/escape data before interpolating into non-JSX contexts (e.g. constructing URLs, `window.location`, dynamic CSS) where React's default JSX escaping doesn't apply.

---

## 14. Testing

### 14.1 Testing pyramid

- **Unit tests** (majority): pure functions, hooks (`renderHook`), reducers.
- **Component/integration tests** (majority of UI coverage): React Testing Library, testing behavior from the user's perspective — not implementation details.
- **E2E tests** (small number, high value): Playwright/Cypress covering critical user journeys (login, checkout, core workflow) end-to-end.

### 14.2 Principles

- **Test behavior, not implementation.** Query by role/label/text (`getByRole`, `getByLabelText`) as RTL encourages — not by CSS class or component internals. This means tests survive refactors that don't change user-facing behavior.
- Avoid testing implementation details like internal state or private methods; if a `useState` value needs its own test, that logic likely belongs in an extracted, independently-testable hook or function.
- Mock at the network boundary (e.g. MSW — Mock Service Worker) rather than mocking hooks or modules, so tests exercise real component + data-fetching code paths.
- Every bug fix ships with a regression test that would have caught it.
- New features require tests before merge — enforced by CI coverage gates on changed files, not just an aggregate repo-wide number.
- Snapshot tests are used sparingly and only for stable, low-churn output (e.g. design tokens) — not as a substitute for behavioral assertions on frequently changing UI.

```tsx
test('shows an error message when login fails', async () => {
  server.use(http.post('/api/login', () => HttpResponse.json({ message: 'Invalid credentials' }, { status: 401 })));

  render(<LoginForm />);
  await userEvent.type(screen.getByLabelText(/email/i), 'user@example.com');
  await userEvent.type(screen.getByLabelText(/password/i), 'wrongpassword');
  await userEvent.click(screen.getByRole('button', { name: /log in/i }));

  expect(await screen.findByText(/invalid credentials/i)).toBeInTheDocument();
});
```

---

## 15. Code Review Checklist

A PR should not be approved until the following are true:

**Correctness**
- [ ] Solves the stated problem; edge cases (empty state, error state, loading state) are handled.
- [ ] No obvious logic bugs, off-by-one errors, or unhandled promise rejections.

**Structure & conventions**
- [ ] File/component placed per the [project structure](#2-react-project-structure) (§2) and [naming conventions](#3-naming-conventions) (§3).
- [ ] Component is focused; no unrelated changes bundled into the PR.
- [ ] No new `any` types; props/state are fully typed.

**Hooks & state**
- [ ] No hook rule violations; `useEffect` isn't used where render-time computation or an event handler would do.
- [ ] Dependency arrays are complete and correct.
- [ ] State lives at the appropriate level (§6) — not over-globalized, not needlessly duplicated from server state.

**Performance**
- [ ] No obvious unnecessary re-renders introduced (new inline object/array/function passed to a memoized child, large unmemoized computation in render, etc.).
- [ ] Large lists are virtualized; heavy/rarely used code is lazy-loaded.

**Accessibility**
- [ ] Semantic HTML used; interactive elements are keyboard-operable.
- [ ] Labels, alt text, and ARIA attributes present where needed.
- [ ] Focus management handled for any new modal/drawer/route transition.

**Security**
- [ ] No unsanitized HTML injection; no secrets in client code.
- [ ] User input validated/escaped before use.

**Testing**
- [ ] New behavior has corresponding tests; bug fixes include a regression test.
- [ ] Tests assert behavior, not implementation details.

**Error handling**
- [ ] Loading/error/empty states are all handled in the UI, not just the happy path.
- [ ] Errors are logged/reported appropriately, not swallowed.

**General**
- [ ] No console.logs, commented-out code, or dead code left in.
- [ ] PR description explains *why*, and is scoped small enough to review in one pass.
- [ ] CI (lint, type-check, tests, build) is green.

---

## 16. References

- [React Documentation](https://react.dev)
- [React Design Principles](https://react.dev/learn/thinking-in-react)
- [Rules of Hooks](https://react.dev/warnings/invalid-hook-call-warning)
- [TanStack Query Docs](https://tanstack.com/query/latest)
- [React Hook Form Docs](https://react-hook-form.com)
- [Testing Library Guiding Principles](https://testing-library.com/docs/guiding-principles/)
- [WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/)
- [Airbnb JavaScript/React Style Guide](https://github.com/airbnb/javascript/tree/master/react)
- [web.dev — Core Web Vitals](https://web.dev/vitals/)
- [OWASP Top Ten](https://owasp.org/www-project-top-ten/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

---

*Maintainers: this document should evolve with the codebase. Propose changes via PR; significant changes (state management library swap, folder structure overhaul) should be discussed with the team before merging.*

*Last updated: 2026-07-20*
