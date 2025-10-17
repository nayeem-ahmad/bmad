# Coding Standards

This section defines a minimal but critical set of standards. These rules are not meant to be exhaustive but are the "golden rules" for this project.

### Critical Fullstack Rules

- **Single Source of Truth for Types:** Always define shared data structures (e.g., `Product`, `Order`) in the `packages/shared-types` package. The frontend and backend **must** import types from this package and **must not** redefine them.
- **Abstract API Calls:** Frontend components **must not** make direct `fetch` calls. All API interactions must go through the centralized `apiClient` or the custom React Query hooks that use it.
- **Abstract Database Access:** API route handlers **must not** contain raw database queries (e.g., `supabase.from(...)`). All database access **must** be done through the appropriate Repository class (e.g., `ProductRepository`).
- **Rely on RLS for Authorization:** Do not write custom authorization logic (e.g., `if (user.id !== resource.owner_id)`) inside API routes. Authorization **must** be enforced by the database Row-Level Security (RLS) policies.
- **Environment Variables:** Do not access `process.env` directly within components or business logic. Create a centralized `config.ts` file that exports typed configuration variables.

### Naming Conventions

| Element | Convention | Example |
| :--- | :--- | :--- |
| **React Components** | `PascalCase` | `UserProfile.tsx` |
| **React Hooks** | `useCamelCase` | `useAuthSession.ts` |
| **API Route Files** | `route.ts` | `app/api/user-profile/route.ts` |
| **Database Tables** | `snake_case` (plural) | `user_profiles`, `products` |
| **Database Columns** | `snake_case` | `first_name`, `store_id` |
| **TypeScript Interfaces** | `PascalCase` | `interface UserProfile { ... }` |
