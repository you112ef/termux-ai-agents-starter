# Tech Stack Document for termux-ai-agents-starter

This document explains, in simple terms, the technology choices behind the **termux-ai-agents-starter** project. It is meant to help everyone—from business users to new developers—understand why we picked each tool and how they work together.

## Frontend Technologies

These are the tools and libraries that shape what you see and interact with in the browser.

- **Next.js (App Router) & React 19**  
  A popular framework (Next.js) built on top of React makes it easy to create pages and handle navigation. It also supports server-side rendering, which can make pages load faster.

- **TypeScript**  
  A version of JavaScript that checks your code for mistakes before you run it. This reduces bugs and gives clearer guidance to developers (and even AI helpers).

- **Tailwind CSS v4**  
  A utility-first style framework. Instead of writing custom CSS for each component, you add tiny style classes right in your HTML. This speeds up design changes.

- **shadcn/ui**  
  A set of ready-made, accessible user interface components (buttons, forms, modals, etc.) that work with Tailwind. These pre-built pieces help you build a polished UI quickly.

Together, these frontend choices ensure a smooth, responsive, and consistent user experience. They also make it simple for AI coding agents to follow clear patterns when generating or modifying UI code.

## Backend Technologies

These components handle data storage, user accounts, and the application’s “behind-the-scenes” logic.

- **Next.js API Routes**  
  Lets us write server code (APIs) right alongside our frontend pages. This co-location reduces confusion about where to put business logic.

- **Better Auth**  
  A complete authentication system that covers user registration, login, password hashing, and session management. Instead of building this from scratch, we rely on a trusted library.

- **PostgreSQL**  
  A reliable, open-source database for storing user information and other data. It safely handles large amounts of information.

- **Drizzle ORM**  
  A type-safe tool for talking to PostgreSQL. It translates your code into database queries, preventing many common mistakes (like typos in column names).

- **TypeScript (Backend)**  
  TypeScript runs on the backend as well, offering the same error-checking benefits for server code.

These backend tools work together to keep data secure, reliable, and easy to evolve—whether you’re a human developer or an AI assistant making updates.

## Infrastructure and Deployment

How we set up, version, and deliver the application so it runs reliably everywhere.

- **Docker & docker-compose**  
  Encapsulates the entire application (web server, database) in separate containers. This ensures everyone—on desktop or in Termux—runs the same environment.

- **Git (Version Control)**  
  Tracks every change in the code so you can collaborate safely. You can host the repository on GitHub, GitLab, or a similar service.

- **CI/CD (Recommended)**  
  Automate builds, tests, and deployments using tools like GitHub Actions. This reduces manual steps and catches problems early.

- **Termux Environment**  
  Designed to work well on mobile devices via Termux (a Linux-like terminal on Android). For practical use, you can point Drizzle to a cloud-hosted PostgreSQL service (e.g., Neon or Supabase), avoiding local database setup on your phone.

These choices make deployments predictable, keep your development environment consistent, and offer a clear path to full automation.

## Third-Party Integrations

External services or APIs that add special capabilities to the project.

- **AI Coding Agents**  
  We include a command-line script (`~/AI`) and task file (`CLAUDE.md`) that let you interact with AI models like Claude, Codex, Gemini, and Llama3. These agents can read your code, generate new features, and even write tests or validations.

By integrating these AI tools, the project becomes truly AI-centric, letting you automate routine coding tasks and focus on unique features.

## Security and Performance Considerations

How we keep user data safe and the app running smoothly.

Security Measures:
- **Better Auth** handles password hashing and secure session cookies.
- **Drizzle ORM** uses parameterized queries to prevent SQL injection attacks.
- **HTTPS** is recommended in production to encrypt data in transit.

Performance Optimizations:
- **Server-Side Rendering & Static Generation** (via Next.js) speeds up page loads and improves SEO.
- **Utility-First CSS** (Tailwind) minimizes unused styles and reduces file size.
- **Container Caching** (Docker) makes local rebuilds faster.

These steps ensure a smooth and secure experience, whether your user is on a desktop browser or a mobile Termux terminal.

## Conclusion and Overall Tech Stack Summary

This starter template brings together modern, well-documented, and AI-friendly technologies to give you a production-ready foundation:

- A **React & Next.js** front end with **TypeScript** for reliability and speed.
- A co-located **API** and authentication layer powered by **Better Auth**, **PostgreSQL**, and **Drizzle ORM**.
- **Docker**-based deployment for consistent environments, with easy version control via **Git** and optional CI/CD automation.
- Deep integration with AI coding agents (Claude, Codex, Gemini, Llama3) through a CLI script and clear task files.

By combining these pieces, you get a clear, maintainable structure that both humans and AI can understand—so you can spend less time on setup and more time building the features that matter.