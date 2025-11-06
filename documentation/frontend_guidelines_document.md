# Frontend Guideline Document

This document outlines the frontend architecture, design principles, and technologies for the termux-ai-agents-starter project. It serves as a clear guide, ensuring anyone—regardless of technical background—can understand how the frontend is built, styled, and maintained.

## 1. Frontend Architecture

### Overview
- **Framework**: Next.js (App Router) with React 19
- **Language**: TypeScript across all files
- **Styling**: Tailwind CSS v4 utility classes, enhanced by shadcn/ui component library

### How It Supports Scalability, Maintainability, and Performance
- **Scalability**: File-based routing in `/app` and feature folders (`/components`, `/lib`, `/db`) lets teams—and AI agents—add pages or modules without touching unrelated code.
- **Maintainability**: TypeScript’s static types catch errors early. Strict component boundaries (UI in `/components`, logic in `/lib`) keep the codebase organized.
- **Performance**: Next.js automatically splits code per page, lazy-loads routes, and optimizes assets. Server-side rendering (SSR) and static generation (SSG) options let us tailor performance per page.

## 2. Design Principles

### Key Principles
1. **Usability**: Intuitive navigation, clear call-to-action buttons, and form validations that guide users step by step.
2. **Accessibility**: All shadcn/ui components follow WAI-ARIA standards; semantic HTML ensures screen-reader compatibility.
3. **Responsiveness**: Mobile-first approach using Tailwind’s responsive utilities to adapt layouts from small screens to desktops.

### Application in the UI
- Buttons have high color contrast and visible focus rings.
- Forms include error messages and labels linked to inputs.
- Layout grids respond at breakpoints (`sm`, `md`, `lg`) so content reflows gracefully.

## 3. Styling and Theming

### Styling Approach
- **Utility-First**: Tailwind CSS classes applied directly in JSX for quick adjustments.
- **Component-Based**: shadcn/ui wraps Tailwind utilities into reusable, accessible components.
- **No Global CSS**: All styling lives in components or is configured via `tailwind.config.js`.

### Theming
- Handled through Tailwind’s theming system in `tailwind.config.js`.
- Supports light and dark modes by toggling a `class` on `<html>`.

### Visual Style
- **Style**: Modern flat design with subtle shadows.
- **Glassmorphism**: Used sparingly for modals or overlay panels (semi-transparent backgrounds with soft blur).

### Color Palette
- Primary: #2563EB (blue-600)
- Secondary: #10B981 (green-500)
- Neutral: #F3F4F6 (gray-100), #1F2937 (gray-800)
- Accent: #F59E0B (yellow-500)

### Typography
- **Font Family**: Inter (system UI fallback)
- **Headings**: `font-semibold`, scalable sizes via Tailwind (`text-2xl`, `text-3xl`)
- **Body**: `font-normal`, `text-base` or `text-sm` for secondary text

## 4. Component Structure

### Organization
- `/components/`: Reusable UI blocks (buttons, modals, cards).
- `/components/auth-*.tsx`: Authentication UI (login, signup buttons).
- `/components/dashboard/`: Dashboard widgets and layouts.

### Reusability
- Components accept props for customization (e.g., color variants, event callbacks).
- All UI logic—like opening/closing modals—lives inside components or in hooks under `/lib/hooks/`.

### Benefits of Component-Based Architecture
- **Isolation**: One component’s changes don’t ripple unexpectedly.
- **Discoverability**: New contributors can browse `/components` to find existing building blocks.
- **AI-Friendly**: Clear, self-contained files make it easy for AI agents to parse and modify components.

## 5. State Management

### Current Approach
- **Local State**: React’s `useState` and `useEffect` for page-level interactions.
- **Authentication State**: Held in session cookies and accessed via `next-auth` or Better Auth’s context provider.

### Sharing State Across Components
- **Context API**: Used sparingly for global data like user info or theme mode.

### Future Expansion
- For more complex scenarios, integrate **Zustand** or **Redux Toolkit**. AI agents can be tasked to add stores under `/lib/state/` for centralized state.

## 6. Routing and Navigation

### Routing
- **Next.js App Router**: Pages are defined in `/app` as `page.tsx` files.
- Nested folders create nested layouts automatically.

### Navigation
- **Link Component**: `next/link` for client-side transitions.
- **Active Route Detection**: Use `usePathname()` and `useSelectedLayoutSegment()` for highlighting current menu items.
- **Protected Routes**: Higher-order components or layout segments wrap pages that require authentication.

## 7. Performance Optimization

### Strategies in Place
1. **Automatic Code Splitting**: Next.js only sends the code each page needs.
2. **Lazy Loading**: Dynamic imports for rarely used components (e.g., charts).
3. **Image Optimization**: `next/image` resizes and serves images in WebP when possible.
4. **Purging Unused CSS**: Tailwind’s purge in production removes unused classes.

### How It Helps Users
- Faster page loads, especially on mobile.
- Reduced bundle sizes lead to snappier interactions.
- SSR/SSG ensures critical content shows up immediately.

## 8. Testing and Quality Assurance

### Testing Strategies
- **Unit Tests**: Jest + React Testing Library for components and utility functions.
- **Integration Tests**: Vitest or Jest for API routes under `/pages/api`.
- **End-to-End Tests**: Playwright or Cypress to simulate user flows (login, dashboard interactions).

### Tools and Frameworks
- **Jest**: Fast unit tests, snapshot testing for UI.
- **React Testing Library**: Encourages testing behavior over implementation.
- **Cypress**: Full browser tests for critical paths.
- **ESLint + Prettier**: Enforces code style and catches common issues.

## 9. Conclusion and Overall Frontend Summary

This document has covered the termux-ai-agents-starter frontend in everyday terms:
- The **architecture** (Next.js + TypeScript + Tailwind)
- **Design principles** (usability, accessibility, responsiveness)
- **Styling** and theming (modern flat with glassmorphism accents)
- **Component structure** and reusability
- **State management** basics with room for future growth
- **Routing** via App Router
- **Performance** tactics baked in
- **Testing** to ensure quality

By following these guidelines, developers and AI coding agents can confidently extend the UI, add new features, and maintain a high-quality frontend that aligns with user needs and modern best practices.