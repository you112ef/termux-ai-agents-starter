# Backend Structure Document

## 1. Backend Architecture

We use a modern, full-stack approach by co-locating server and client code in a single Next.js project. This simplifies development and streamlines communication between the UI and data layer. Key points:

- Next.js API Routes handle all HTTP requests on the backend.
- Each API route is a simple function that receives a request and returns a response, following RESTful conventions.
- Better Auth middleware centralizes user authentication and session management.
- Drizzle ORM provides a type-safe interface to the database, letting us write queries in TypeScript with compile-time checks.

How it supports our goals:

- Scalability: Deploys as serverless functions (on platforms like Vercel) or containerized services (on Docker). New instances spin up automatically under load.
- Maintainability: Clear file organization (`/app`, `/db`, `/lib`) and TypeScript types prevent drift. Business logic stays separated from infrastructure code.
- Performance: Serverless endpoints start fast, and you only pay for what you use. Drizzle’s query builder generates optimized SQL under the hood.

## 2. Database Management

### Database Technology

- Type: Relational (SQL)
- System: PostgreSQL
- Access Layer: Drizzle ORM (type-safe, migrations ready)

### Data Lifecycle

- Schema definitions in `/db/schema/` describe tables, columns, and relationships.
- Drizzle’s migration tool (`drizzle-kit`) applies versioned changes to the live database.
- Connections are pooled by the Postgres driver to maximize throughput.
- Read and write operations go through the ORM, ensuring consistent patterns and protection against SQL injection.

## 3. Database Schema

We store user accounts and sessions to support authentication and dashboard features. Below is a human-friendly overview and the corresponding SQL schema.

### Human-Readable Schema

- **users**: holds a unique record for each account.
  - `id`: unique identifier (UUID)
  - `email`: user email (unique)
  - `password_hash`: hashed password
  - `created_at` / `updated_at`: timestamps

- **sessions**: tracks active user sessions.
  - `id`: session UUID
  - `user_id`: links to a user record
  - `session_token`: random token for cookie-based auth
  - `expires_at`: session expiry timestamp

### SQL Definition (PostgreSQL)

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT NOT NULL UNIQUE,
  password_hash TEXT NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  session_token TEXT NOT NULL UNIQUE,
  expires_at TIMESTAMPTZ NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```  

*(Note: Drizzle ORM reads a TypeScript schema file and generates similar SQL.)*

## 4. API Design and Endpoints

We follow a RESTful pattern with clear, purpose-driven routes. All endpoints live under `app/api`.

- **Authentication**
  - `POST /api/auth/register`: Create a new user account.
  - `POST /api/auth/login`: Check credentials and start a session.
  - `POST /api/auth/logout`: End the current session.
  - `GET  /api/auth/session`: Get current user info if authenticated.

- **Dashboard Data**
  - `GET /api/dashboard/data`: Fetch data summaries for charts and tables.
  - `POST /api/dashboard/settings`: Update user-specific preferences.

Each endpoint validates input (we recommend using Zod for schema validation) and returns JSON responses with clear success or error messages. Authentication checks run as middleware so that only logged-in users access protected routes.

## 5. Hosting Solutions

We support both local development and production deployments.

- **Local (Termux / Docker Compose)**
  - `docker-compose.yaml` boots the Next.js server and Postgres database together.
  - Great for offline mobile development.

- **Production**
  - **Frontend & API**: Vercel (serverless) or any container platform (AWS ECS, Google Cloud Run). Auto-scales with traffic.
  - **Database**: Cloud-hosted PostgreSQL (Supabase, Neon, or AWS RDS). Always-on replicas ensure high availability.

Benefits:

- Reliability: Managed services handle failover and backups.
- Scalability: Serverless functions and managed databases grow with demand.
- Cost-effectiveness: Pay-as-you-go pricing, free tiers for early development.

## 6. Infrastructure Components

- **Load Balancer / Edge Network**: Vercel’s global edge ensures endpoints respond from the closest location to the user.
- **Caching**: HTTP headers and CDN caching for static assets and dashboard data (when appropriate).
- **Containerization**: Docker images define consistent runtime environments, so local, CI/CD, and production servers match exactly.
- **Reverse Proxy**: In a self-hosted setup, you can add Nginx or Caddy in front of your container to handle SSL/TLS and route traffic.

Together, these elements speed up page loads, reduce server load, and maintain secure connections.

## 7. Security Measures

- **Authentication & Authorization**
  - Better Auth handles secure password hashing (bcrypt or Argon2) and HTTP-only cookies.
  - Role checks can be layered on via middleware for admin or special-access routes.
- **Data Encryption**
  - Transport: HTTPS/TLS enforced on all endpoints.
  - At Rest: Rely on the database provider’s disk encryption.
- **Input Validation**
  - Zod (or similar) validates request bodies and query params to prevent malformed data.
- **Security Headers**
  - Content Security Policy (CSP), X-Frame-Options, HSTS all set via response headers.
- **Environment Isolation**
  - Secrets (DB URLs, API keys) live in environment variables and never in source code.

## 8. Monitoring and Maintenance

- **Logging**
  - Application logs use a structured logger (e.g., Pino or Winston) and ship to a central service (LogDNA, Datadog).
- **Error Tracking**
  - Sentry or OpenTelemetry catch unhandled errors and performance bottlenecks.
- **Health Checks**
  - A simple `/health` endpoint returns 200 OK if the server and database are reachable.
- **Automated Migrations**
  - CI/CD pipelines (GitHub Actions) run `drizzle-kit` migrations on deploy, preventing drift.
- **Dependency Updates**
  - Tools like Dependabot alert you to new security patches in your NPM packages.

## 9. Conclusion and Overall Backend Summary

This backend structure brings together a single Next.js codebase, type-safe database access, and modern hosting to form a solid foundation. It’s designed for rapid feature development, especially when driven by AI agents in a Termux environment. Scalability, security, and maintainability are built in:

- Co-located API routes reduce complexity.
- Drizzle ORM ensures valid queries and smooth migrations.
- Serverless and container options let you choose the best hosting model.
- Security practices guard user data at every layer.
- Monitoring and CI/CD keep the system reliable and up-to-date.

By following these guidelines, anyone can understand, deploy, and extend this backend without needing in-depth infrastructure expertise.