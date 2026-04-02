---
name: vue
description: "Vue 3 development standards aligned with enterprise best practices and Composition API architecture, including official Symfony integration."
applyTo: "**/*.vue, **/*.ts, **/*.js"
---

# Vue.js Skill (Vue 3 + Composition API)

This skill defines all guidelines and conventions for implementing Vue.js applications in this project using Vue 3, Composition API, Pinia, and a clean module architecture.

---

# ✅ Global Principles

- Vue 3 ONLY (no Options API unless legacy component).
- Composition API is mandatory.
- State management via **Pinia**.
- Components must remain **small**, **pure**, and **focused on UI**.
- **Zero business logic** in templates or components.
- API calls MUST be extracted into dedicated **services**.
- Use TypeScript everywhere.

---

# ✅ Project Structure

assets/
  app.js           # Vue app entry point
  vue/
    components/    # UI components
    views/         # Page-level components
    composables/   # shareable logic (useXxx)
    stores/        # Pinia stores
    services/      # Axios/Fetch API clients
    types/         # TS models/interfaces
    router/        # Vue Router setup


### ✅ Responsibilities

- **components/** → Reusable UI building blocks  
- **views/** → Page-level components (routing targets)  
- **composables/** → Reusable logic hooks (`useXxx()`)  
- **stores/** → Pinia stores  
- **services/** → API clients + business integration logic  
- **types/** → Shared TS interfaces  

---

# ✅ Components (UI Only)

### ✅ Components MUST:
- Use `<script setup lang="ts">`.
- Use `defineProps()` / `defineEmits()`.
- Keep templates clean and declarative.
- Emit events instead of mutating parent state.
- Use CSS scoped when possible.

### ❌ Components MUST NOT:
- Call API endpoints directly.
- Contain domain/business rules.
- Manage global state (Pinia only).
- Contain deeply nested conditionals.

---

# ✅ Views (Screens / Routes)

### ✅ Views MUST:
- Fetch data from Pinia store or services.
- Compose multiple components.
- Contain **light orchestration logic**.
- Handle loading / empty / error states via reactive variables.

### ❌ Views MUST NOT:
- Replace services or stores.
- Contain heavy logic.

---

# ✅ Composables

Composables = reusable logic modules.

### ✅ Composables MUST:
- Be named `useXxx()`.
- Encapsulate reusable behaviors (form validation, pagination, filters…).
- Return only reactive state + functions.
- Remain pure and testable.

### ❌ Composables MUST NOT:
- Make HTTP calls (services must do this).
- Contain UI rendering logic.

---

# ✅ Services (API Clients & Integration Logic)

### ✅ Services MUST:
- Be dedicated to HTTP communication.
- Use Axios or Fetch wrapper.
- Map backend responses to typed objects.
- Expose **simple async functions**: `getUsers()`, `updateInvoice()`, etc.

### ✅ Error handling
- Handle errors consistently.
- Never throw raw backend errors.
- Transform exceptions/envelopes into typed domain errors.

### ❌ Services MUST NOT:
- Mutate global state.
- Store local state.

---

# ✅ Pinia Stores

### ✅ Stores MUST:
- Be defined with `defineStore()`.
- Use TypeScript interfaces for state.
- Expose state, getters, actions.
- Handle caching when appropriate.
- Contain minimal logic (orchestration, not business rules).

### ❌ Stores MUST NOT:
- Contain API calls directly → call services.
- Contain heavy logic → send to composables or services.

---

# ✅ TypeScript & Typing Rules

### ✅ MUST:
- Strong typing everywhere.
- Use `interface` or `type` for API models.
- Use `Readonly<>` when exposing immutable data.
- Use enums or literal union types for status flags.

### ❌ MUST NOT:
- Rely on `any`.
- Trust unvalidated backend responses.

---

# ✅ Styling & UI Rules

- Prefer Tailwind or Bootstrap for layout (according to project).
- Use CSS variables for constants.
- Keep styling scoped to the component scope.
- Avoid deeply nested DOM structures.

---

# ✅ Routing (Vue Router)

- Use dynamic imports for lazy loading.
- Use named routes.
- Keep route definitions minimal.
- Validate route params (via composables or guards).

---

# ✅ Testing (Unit + Component)

### ✅ Use Vitest or Jest
- Test **behavior**, not implementation details.
- Mock services when testing UI.
- Test Pinia stores with mocked API layer.
- Snapshot testing only for stable UI components.

---

# ✅ Performance Best Practices

- Use `computed` over `watch` when possible.
- Avoid rendering large lists without virtualization.
- Use `<Suspense>` for async components.
- Watch out for unnecessary `.value` access inside templates.

---

# ✅ Security 

- Never use `v-html` with unknown data.
- Sanitize dynamic HTML if absolutely required.
- Avoid storing JWTs in localStorage → use cookies if possible.

---

# ✅ Anti‑Patterns (Strictly Forbidden)

❌ Business logic in components  
❌ API calls inside components  
❌ Using Options API in new code  
❌ Storing entire API payloads in Pinia stores  
❌ Using watchers for simple logic  
❌ Global state misuse  
❌ Relying on CSS frameworks for layout logic only  