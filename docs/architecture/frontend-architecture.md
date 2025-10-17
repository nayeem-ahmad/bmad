# Frontend Architecture

This section defines the patterns and structure for the Next.js frontend application.

### Component Architecture

**Component Organization:** We will adopt a hybrid approach to organizing components, separating generic UI primitives from domain-specific components.

```text
src/
├── app/                # Next.js App Router
│   ├── (auth)/         # Routes for authentication (e.g., login, signup)
│   │   └── login/page.tsx
│   ├── (main)/         # Main application routes (protected)
│   │   ├── layout.tsx    # Protected layout
│   │   ├── dashboard/page.tsx
│   │   └── inventory/page.tsx
│   └── (storefront)/   # Public e-commerce routes
│       └── [storeSlug]/page.tsx
├── components/
│   ├── ui/             # Generic, reusable UI elements (e.g., Button, Input from Shadcn)
│   └── domain/         # Components tied to specific business concepts
│       ├── product/
│       │   ├── ProductList.tsx
│       │   └── ProductForm.tsx
│       └── sale/
│           ├── PosTerminal.tsx
│           └── SaleReceipt.tsx
└── lib/
    ├── api.ts          # Central API client
    └── hooks/          # Custom React hooks
```

### State Management Architecture

We will use a combination of state management tools, each for its specific purpose:

- **Server State & Caching:** `React Query` (or `SWR`) will be used to manage all data fetched from our backend API. It will handle caching, background refetching, and optimistic updates, providing a robust and resilient data layer.
- **Global UI State:** `Zustand` will be used for small pieces of global client-side state that are not persisted on the server, such as UI theme, notification messages, or the state of a multi-step wizard.
- **Local Component State:** React's built-in `useState` and `useReducer` hooks will be used for state that is local to a single component, such as form input values or the open/closed state of a modal.

### Routing Architecture

We will use the Next.js App Router.

- **Route Organization:** Routes will be organized using file-system conventions within the `src/app` directory.
- **Layouts:** Route Groups (e.g., `(main)`) will be used to create different layouts for distinct parts of the application, such as the protected admin area versus the public storefront.
- **Protected Routes:** A top-level layout in the `(main)` group will be responsible for checking for an active user session and redirecting to the login page if no session exists.

### Frontend Services Layer

All communication with the backend API will be centralized in a dedicated services layer.

**API Client Setup (`src/lib/api.ts`):**
A thin wrapper around `fetch` will be created to handle common API call logic.

```typescript
// src/lib/api.ts
import { createClient } from '@supabase/supabase-js';

// This is a browser client
const supabase = createClient(process.env.NEXT_PUBLIC_SUPABASE_URL!, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!);

async function getAuthToken() {
    const { data, error } = await supabase.auth.getSession();
    if (error) throw new Error("Could not get auth session");
    return data.session?.access_token;
}

export const apiClient = {
    get: async (path: string) => {
        const token = await getAuthToken();
        const response = await fetch(`/api${path}`, {
            headers: { Authorization: `Bearer ${token}` },
        });
        return response.json();
    },
    // ... post, put, delete methods
};
```

### Accessibility (A11y)

To meet the WCAG 2.1 Level AA compliance requirement, accessibility will be a primary consideration throughout the development process.

-   **Semantic HTML:** Developers **must** prioritize using semantic HTML5 elements (`<nav>`, `<main>`, `<button>`, etc.) over generic `<div>` or `<span>` elements to provide inherent meaning and accessibility.
-   **ARIA Roles:** Accessible Rich Internet Applications (ARIA) attributes will be used correctly, but only when semantic HTML is insufficient. We will follow the official ARIA authoring practices.
-   **Focus Management:** For dynamic components like modals and pop-ups, focus must be programmatically managed. When a modal opens, focus must move into it. When it closes, focus must return to the element that triggered it.
-   **Keyboard Navigation:** All interactive elements must be reachable and operable using only the keyboard in a logical and predictable order.
