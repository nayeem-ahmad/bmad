# Development Workflow

This section outlines the process for setting up a local development environment and the commands needed to run, test, and build the application.

### Local Development Setup

**Prerequisites:**
- Node.js (version 20.x or later)
- npm (version 10.x or later)
- A Supabase project (for database and auth)

**Initial Setup:**
1.  Clone the repository.
2.  Navigate to the project root directory.
3.  Create a `.env.local` file by copying `.env.example`.
4.  Fill in the required environment variables in `.env.local`.
5.  Run the following command to install all dependencies for the root project and all workspaces:
    ```bash
    npm install
    ```

**Development Commands:**
```bash
# Start the Next.js development server (for both frontend and backend)
npm run dev

# Run all tests (unit and integration)
npm run test

# Run the linter to check for code quality issues
npm run lint

# Build the application for production
npm run build
```

### Environment Configuration

The following environment variables must be set in a `.env.local` file in the project root.

```bash
# Supabase Configuration (required)
# Found in your Supabase project's API settings
NEXT_PUBLIC_SUPABASE_URL="your-supabase-project-url"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-supabase-anon-key"
SUPABASE_SERVICE_ROLE_KEY="your-supabase-service-role-key"

# Payment Gateway Configuration (as needed)
BKASH_APP_KEY="your-bkash-app-key"
BKASH_APP_SECRET="your-bkash-app-secret"
BKASH_API_URL="https://api.bkash.com/sandbox" # or production URL

NAGAD_MERCHANT_ID="your-nagad-merchant-id"
NAGAD_API_KEY="your-nagad-api-key"
NAGAD_API_URL="https://api.nagad.com.bd/sandbox" # or production URL
```
