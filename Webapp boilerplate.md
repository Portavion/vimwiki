# Web App Boilerplate

- Architecture: Monorepo managed with pnpm workspaces.
- Client: React + TypeScript SPA built with Vite.
- Server: Python FastAPI.
- Database: PostgreSQL with SQLModel ORM and Alembic migrations.
- Deployment: Docker containers managed via Docker Compose for VPS deployment.

## Refined Boilerplate Template Outline
```
/your-project-name (Monorepo Root)
├── apps/
│   ├── client/ (Vite React SPA)
│   │   ├── public/                     # Static assets (favicon, robots.txt)
│   │   ├── src/
│   │   │   ├── assets/                 # Images, fonts
│   │   │   ├── components/
│   │   │   │   ├── ui/                 # Base UI Kit (e.g., Shadcn/ui components)
│   │   │   │   ├── layout/             # Navbar, Footer, PageWrapper etc.
│   │   │   │   └── common/             # Shared across features (e.g., LoadingSpinner)
│   │   │   ├── features/               # Feature modules (components, hooks, pages)
│   │   │   │   ├── auth/               # Login page, registration, auth context/hook
│   │   │   │   ├── payments/           # Pricing page, billing settings
│   │   │   │   ├── blog/               # Blog index/post display components
│   │   │   │   └── admin/              # User management UI (protected)
│   │   │   ├── hooks/                  # Reusable custom hooks
│   │   │   ├── lib/                    # Utilities, API client (axios/fetch wrapper)
│   │   │   ├── router/                 # React Router setup (index.tsx, routes.tsx)
│   │   │   ├── services/               # Typed API service functions (e.g., authApi.ts)
│   │   │   ├── store/                  # State management (Zustand setup)
│   │   │   ├── styles/                 # Global CSS, Tailwind base/layers
│   │   │   ├── types/                  # Shared frontend TypeScript types
│   │   │   └── main.tsx                # App entry point, providers setup
│   │   ├── emails/                     # React Email source files
│   │   │   ├── templates/              # Email components (Welcome.tsx, MagicLink.tsx)
│   │   │   └── render.ts               # Script to generate HTML output (run via Node)
│   │   ├── index.html                  # Vite's HTML entry point
│   │   ├── vite.config.ts              # Vite configuration
│   │   ├── tailwind.config.js          # Tailwind configuration
│   │   ├── tsconfig.json
│   │   ├── postcss.config.js
│   │   ├── Dockerfile                  # Builds static assets + Nginx server
│   │   └── package.json                # Client dependencies and scripts
│   │
│   └── server/ (FastAPI Backend)
│       ├── app/
│       │   ├── main.py                 # FastAPI app factory, includes routers
│       │   ├── core/                   # Core logic
│       │   │   ├── config.py           # Pydantic settings management
│       │   │   ├── db.py               # SQLModel setup, async session, pooling
│       │   │   └── security.py         # Hashing, JWT, CORS, OAuth logic
│       │   ├── features/               # API Routers & business logic
│       │   │   ├── auth/               # Login, register, magic link, social, /me
│       │   │   ├── users/              # Profile management, admin list
│       │   │   ├── payments/           # Stripe checkout, portal, webhook handler
│       │   │   ├── emails/             # Service to send emails (loads rendered HTML)
│       │   │   ├── uploads/            # S3 pre-signed URL generation/handling
│       │   │   ├── blog/               # API endpoints for blog content
│       │   │   └── admin/              # Protected endpoints for admin actions
│       │   ├── background/             # Background task processing (e.g., ARQ)
│       │   │   ├── tasks.py            # Task definitions (send_email_task)
│       │   │   ├── worker.py           # ARQ worker setup
│       │   │   └── config.py           # Queue/Redis connection settings
│       │   └── models/                 # SQLModel database table definitions
│       │       ├── user.py
│       │       ├── plan.py
│       │       └── ...
│       ├── alembic/                    # Database migrations
│       │   ├── versions/
│       │   └── env.py
│       ├── static_email_templates/     # Output directory for rendered email HTML
│       ├── tests/                      # Pytest test suite
│       ├── .env.example                # Template for environment variables
│       ├── alembic.ini                 # Alembic configuration
│       ├── pyproject.toml              # Python dependencies (using Poetry or PDM)
│       └── Dockerfile                  # Builds Python application image
│
├── packages/                           # Optional shared packages (e.g., types)
│   └── types/
│       └── index.ts
├── scripts/                            # Helper scripts
│   ├── build-emails.mjs              # Node script to run react-email render
│   └── generate-sitemap.mjs          # Script to create sitemap.xml
├── .github/workflows/                  # CI/CD pipeline examples
│   └── ci.yml
├── .gitignore
├── docker-compose.yml                  # Local development environment (inc. DB, Redis)
├── docker-compose.prod.yml             # Production deployment stack
├── package.json                        # Monorepo root package definition
├── pnpm-workspace.yaml                 # Defines monorepo structure for pnpm
└── README.md                           # Detailed setup and usage guide
```
## Key Implementation Details

### Client (Vite/React/SPA)

- UI Library: Setup for Shadcn/ui (recommended) or another choice from the list. Includes installing the CLI, configuring tailwind.config.js and globals.css, and adding a few example components (Button, Input) to src/components/ui.
- Routing: react-router-dom configured in src/router/. Includes examples of public routes, protected routes (using the auth context), and potentially nested layouts.
- State: Zustand store (src/store/) for global state like auth status and user info. TanStack Query (react-query) configured in main.tsx for managing server state, caching, etc.
- API Interaction: An axios instance or Workspace wrapper in src/lib/apiClient.ts configured with the backend base URL (from .env) and logic to automatically attach JWT tokens from the auth state. Typed API service functions in src/services/ using this client.
- Authentication: An AuthProvider component (src/features/auth/AuthProvider.tsx) manages auth state (user, token, loading status) using Zustand/TanStack Query. Handles token storage (e.g., secure httpOnly cookies set by the server are often best for SPAs if domains align, otherwise careful localStorage/sessionStorage usage) and provides context to the app. Login/Register pages make API calls via services.
- SEO:
  - react-helmet-async provider setup in main.tsx. Components use the Helmet component to set titles, meta descriptions, canonical URLs, and Open Graph tags.
  - JSON-LD added via Helmet with <script type="application/ld+json">.
  - Sitemap generation script (scripts/generate-sitemap.mjs) fetches necessary routes/data from the API and writes sitemap.xml to apps/client/public. This script can be added to the client's build process.
  - Blog (MDX): Placeholder API endpoint on the server (/api/blog/posts, /api/blog/posts/{slug}) to serve blog content (potentially pre-rendered HTML or raw MDX). Client components (src/features/blog/) fetch this data and render it. If rendering raw MDX client-side, include @mdx-js/react.
  - Email Templates: react-email setup in apps/client/emails. A script (scripts/build-emails.mjs) added to client/package.json ("build:emails": "node ../../scripts/build-emails.mjs") uses react-email/render to output HTML files to apps/server/static_email_templates/.
  - Docker: apps/client/Dockerfile uses a multi-stage build: node base image to build the SPA, then nginx:alpine base image to copy the built assets (dist) and a basic nginx.conf to serve them efficiently.

### Server (FastAPI/SQLModel)

- Settings: Pydantic settings (core/config.py) load configuration from environment variables (.env file for local dev).
- Database: Async engine and session maker setup (core/db.py) using sqlalchemy.ext.asyncio. SQLModel models defined in models/. Alembic configured for async migrations. Connection pooling enabled.
- Authentication: Includes endpoints for token-based login (email/password), magic link generation/verification, social login callbacks (using httpx-oauth), and a protected /me endpoint. JWTs created using python-jose. Security dependency enforces valid JWTs on protected routes. CORS configured appropriately for the client URL. Considers setting httpOnly cookies for tokens.
- Payments: Stripe webhook handler (features/payments/router.py) verifies signatures and processes events (checkout.session.completed, subscription updates) by updating user data via service functions. Endpoints for creating checkout and portal sessions.
- Email: Email service (features/emails/service.py) loads the pre-rendered HTML from static_email_templates/, populates any dynamic data (if needed, though ideally minimal), and sends via boto3 (SES) or resend-python. Background task (background/tasks.py) for sending emails asynchronously using ARQ.
- Background Tasks: ARQ configured (background/worker.py, background/config.py) using Redis. Includes example task definition and setup to run the worker.
- Admin: Basic protected endpoints (features/admin/router.py) for managing users (list, view, potentially update status), protected by a role/permission check dependency.
- Docker: apps/server/Dockerfile uses a Python base image, sets up Poetry/PDM environment, installs dependencies, copies code, and uses gunicorn with uvicorn workers as the entry point.

### Monorepo & Tooling

- pnpm manages workspaces defined in pnpm-workspace.yaml.
- Root package.json contains scripts for running commands across workspaces (e.g., "dev": "pnpm run --parallel --stream dev", "build": "pnpm run --recursive build").
- Linting/Formatting: ESLint/Prettier (Client), Ruff (Server) configured with root config files and package.json scripts. Husky for git hooks recommended.
- Testing: Vitest/React Testing Library (Client), Pytest (Server) setup with basic examples.

### Deployment (Docker Compose)

- docker-compose.yml: For local development. Includes services for:
- client: Runs pnpm dev with volume mount for hot-reloading.
- server: Runs uvicorn ... --reload with volume mount.
- db: PostgreSQL image.
- redis: Redis image (for background tasks/caching).
- docker-compose.prod.yml: For VPS deployment. Includes services for:
- client: Builds apps/client/Dockerfile and runs the Nginx container.
- server: Builds apps/server/Dockerfile and runs the Gunicorn/Uvicorn container.
- db: PostgreSQL image (or connects to external DB).
- redis: Redis image (or connects to external Redis).
- Uses .env file for production secrets/configuration.

### README.md
Contains detailed instructions for initial setup, running locally, building for production, and deploying using docker compose -f docker-compose.prod.yml up -d --build.


