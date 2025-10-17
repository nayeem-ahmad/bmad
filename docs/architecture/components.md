# Components

### Web Frontend (Next.js Application)

**Responsibility:** This is the primary user-facing component. It serves both the internal-facing application for store owners (POS, inventory, settings) and the public-facing e-commerce storefront for shoppers. It is responsible for rendering all UI, managing client-side state, and interacting with the backend API.

**Key Interfaces:**
- **UI:** Renders HTML, CSS, and JavaScript to the user's browser.
- **REST API Client:** Communicates with the backend API via HTTP requests to fetch and mutate data.
- **Auth Client:** Interacts directly with the Supabase Auth client-side library to handle user sign-in, sign-up, and session management.

**Dependencies:**
- **Backend API:** Relies on the API for all business logic and data persistence.
- **Supabase Auth:** Depends on the Supabase service for user authentication.

**Technology Stack:** Next.js, React, TypeScript, Tailwind CSS, Shadcn/UI, Zustand.

### Backend API (Next.js API Routes)

**Responsibility:** This component is the authoritative core of the application. It executes all business logic, enforces security rules, and handles all data persistence. It is responsible for processing requests from the frontend, interacting with the database, and integrating with any third-party services.

**Key Interfaces:**
- **REST API:** Exposes a set of HTTP endpoints (defined in the OpenAPI specification) that the frontend consumes.
- **Database Client:** Connects to the PostgreSQL database via the Supabase client to perform CRUD (Create, Read, Update, Delete) operations.
- **External Service Integrations:** Communicates with external APIs, such as bKash/Nagad for payment processing.

**Dependencies:**
- **PostgreSQL Database (Supabase):** Relies on the database for all data storage.
- **Supabase Auth:** Uses the Supabase admin SDK to validate user sessions and permissions.
- **External APIs:** Depends on the availability and contracts of third-party services.

**Technology Stack:** Next.js API Routes, TypeScript.

### Database (PostgreSQL via Supabase)

**Responsibility:** This component is the system's long-term memory. It is responsible for the persistent storage of all application data, such as users, products, orders, and sales. It also enforces data integrity through schemas, constraints, and relationships. In the Supabase model, it also plays a key role in authorization via Row-Level Security (RLS).

**Key Interfaces:**
- **SQL Interface:** Exposes a standard SQL interface that the Backend API uses to query and manipulate data.
- **Connection Pooling:** Provides managed, secure connections to the backend.
- **Security Policies:** Exposes an interface for defining and managing Row-Level Security policies.

**Dependencies:**
- This is a foundational component with no external dependencies.

**Technology Stack:** PostgreSQL.
