# Project Requirements Document: termux-ai-agents-starter

## 1. Project Overview

termux-ai-agents-starter is a ready-to-use full-stack web application template optimized for developers working in a command-line environment like Termux. It solves the common pain point of manually wiring up authentication, database connectivity, UI components, and deployment scripts by offering a production-grade foundation. Beyond just scaffolding code, it’s built to integrate seamlessly with AI coding agents (Claude, Codex, Gemini, Llama3), allowing those agents to read, understand, and extend the project with pinpoint accuracy.

The main goal is to accelerate feature development: instead of spending hours on initial setup, developers and their AI assistants can dive straight into building business logic, new pages, or data visualizations. Success is measured by how quickly an AI model can be given a concise instruction—like "Add a posts table to the schema"—and produce valid, type-safe code that runs without errors. This template aims for zero friction between human prompts and AI-generated output.

## 2. In-Scope vs. Out-of-Scope

**In-Scope (Version 1.0):**
- User authentication (registration, login, session management) via Better Auth
- Protected `/dashboard` area with example UI components
- UI framework using shadcn/ui and Tailwind CSS v4
- Database integration with PostgreSQL and Drizzle ORM (schema & connection)
- CLI script (`~/AI`) for invoking AI agents against the codebase
- Docker and docker-compose definitions for local environment blueprint
- Task file (`CLAUDE.md`) to guide AI models on where to find tasks

**Out-of-Scope (Phase 2+):**
- Automated test suite (unit, integration, e2e)
- Continuous integration or delivery pipelines
- Multi-tenant or role-based access beyond basic user sessions
- Native mobile app or React Native wrapper
- Real-time collaboration or WebSocket features
- Analytics dashboards, BI integrations, or advanced charting libraries

## 3. User Flow

When a developer starts from scratch, they clone the repo into Termux, install dependencies, and run the provided `docker-compose up` or point the Drizzle config at a cloud Postgres instance. Next, they execute the `~/AI` CLI script: this brings up a prompt where they choose an AI agent (e.g., Claude). The agent reads the `CLAUDE.md` file, scans the `/app`, `/components`, `/lib`, and `/db` directories, and awaits instructions like "Create a new page at `/app/settings/page.tsx`." The developer provides concise commands, and the agent generates type-safe TypeScript code based on the existing patterns.

For end-users of the resulting application, the flow is classic: they navigate to the landing page, register for an account, and then sign in. Upon successful login, they’re redirected to `/dashboard`, where they see a set of UI cards, tables, or charts (placeholders). From here, they can click through the sidebar to visit other feature pages dynamically added by AI agents. Every page call hits a Next.js API route, which enforces session checks and reads/writes to the PostgreSQL database via Drizzle ORM.

## 4. Core Features

- **AI-Agent CLI Integration**: `~/AI` shell script that loads model contexts and task files for AI-driven code modifications.
- **Authentication Module**: Registration, login, password hashing, session cookies, logout functionality (Better Auth).
- **Protected Dashboard**: `/dashboard` route guarded by session check, with placeholder UI components from shadcn/ui.
- **UI Component Library**: Pre-built, accessible React components styled with Tailwind CSS and shadcn/ui.
- **Database Layer**: Drizzle ORM schema definitions in `/db`, with PostgreSQL connection configured.
- **Next.js API Routes**: Backend endpoints co-located with pages, handling CRUD operations and validation.
- **Containerization Blueprint**: `Dockerfile` and `docker-compose.yaml` outlining services (app, Postgres).
- **Task Definition File**: `CLAUDE.md` describing developer goals, used as context for AI agents.

## 5. Tech Stack & Tools

**Frontend:**
- Next.js (App Router) with React 19 and TypeScript
- Tailwind CSS v4 utility classes
- shadcn/ui component library

**Backend:**
- Next.js API Routes (Node.js, TypeScript)
- Better Auth for authentication flows
- PostgreSQL database
- Drizzle ORM for type-safe queries

**AI & CLI:**
- Shell-based CLI script (`~/AI`) for agent orchestration
- Supported models: Claude, OpenAI Codex, Google Gemini, Llama3

**Dev & Deployment:**
- Docker & docker-compose for environment definition
- Optionally connect Drizzle to cloud providers (Neon, Supabase)

**Recommended IDE & Plugins:**
- Visual Studio Code
- Cursor or Windsurf for AI-assisted code completion
- ESLint & Prettier for linting/formatting

## 6. Non-Functional Requirements

- **Performance:** API endpoints should respond within 200ms under normal load; initial page hydration under 2s.
- **Security:** HTTPS enforced in production; implement CSP, XSS/CSRF protections; server-side input validation (Zod or similar).
- **Scalability:** Stateless API routes; database indexing on key fields; easy horizontal scaling.
- **Usability:** Accessible components (ARIA, keyboard nav); responsive design across mobile and desktop.
- **Maintainability:** Strict TypeScript types; modular directories; clear naming conventions.

## 7. Constraints & Assumptions

- **Termux Environment:** Users have Android Termux with Node 18+ and basic Linux tooling installed.
- **Docker in Termux:** Full Docker may not run—assume developers will point to a remote Postgres instance instead.
- **AI Model Availability:** Access to Claude, Codex, Gemini, or Llama3 with sufficient API rate limits.
- **Cloud DB Credentials:** Developers will supply environment variables for remote database connections.

## 8. Known Issues & Potential Pitfalls

- **Docker on Android:** Docker-compose often fails in Termux; mitigate by using a cloud-hosted database and running `npm run dev` directly.
- **API Rate Limits:** Hitting AI model quotas may slow the CLI workflow; consider caching task files or bundling requests.
- **Schema Drift:** Without migration tooling (drizzle-kit), schema changes might desync. Mitigate by adding `drizzle-kit` in a later phase.
- **Component Overload:** Too many UI components can confuse AI agents—keep new components well-documented in JSDoc.
- **Type Safety Gaps:** If AI injects untyped code, the build may break. Always run `npm run build` in CI before merging.

---

This document provides the single source of truth for all AI-driven and manual development work. Every subsequent technical guideline (Tech Stack docs, Frontend Guidelines, Backend Structure) should directly reference these sections to avoid ambiguity.