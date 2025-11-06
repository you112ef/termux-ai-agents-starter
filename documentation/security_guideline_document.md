# termux-ai-agents-starter Security Guidelines

This document provides comprehensive security guidance tailored to the **termux-ai-agents-starter** repository—a full-stack Next.js starter optimized for AI-driven workflows in Termux. It applies core security principles from design through deployment to ensure a robust, resilient, and trustworthy foundation.

---

## 1. Security Principles & Approach

  • **Security by Design:** Embed security at every stage—from planning and coding to testing and deployment.  
  • **Least Privilege:** Grant services, modules, and users only the permissions they need.  
  • **Defense in Depth:** Layer controls so that the compromise of one mechanism does not expose the entire system.  
  • **Secure Defaults:** Enable the strongest settings out of the box.  
  • **Fail Securely:** On errors or exceptions, do not leak sensitive information or leave the system in an unsafe state.  
  • **Keep Security Simple:** Favor clear, maintainable controls over complex constructs.

---

## 2. Secure Authentication & Access Control

### 2.1 User Authentication

  • Built on **Better Auth**—ensure configuration uses strong, modern hashing (e.g., **Argon2** or **bcrypt** with unique salts) for storing passwords.  
  • Enforce a minimum password length (≥12 characters) and complexity (uppercase, lowercase, numbers, symbols).  
  • Implement account lockout or throttling to mitigate brute-force attacks.

### 2.2 Session Management

  • Use **secure**, **HttpOnly**, **SameSite=Strict** cookies for session identifiers.  
  • Generate unpredictable, high-entropy session IDs.  
  • Enforce both idle and absolute session timeouts.  
  • Invalidate sessions on logout or password change.

### 2.3 Role-Based Access Control (RBAC)

  • Define clear roles (e.g., `user`, `admin`, `agent`) and limit API/page access accordingly.  
  • Perform authorization checks server-side in Next.js API routes and middleware—never trust client-side enforcement.

### 2.4 Multi-Factor Authentication (MFA)

  • Offer MFA (e.g., TOTP or SMS-based) for production and high-privilege accounts.  
  • Store MFA secrets securely in a vault or encrypted database fields.

---

## 3. Input Handling & Output Encoding

  • **Treat all input as untrusted.** Validate on the server using schemas (e.g., **Zod**).  
  • **SQL/NoSQL Injection:** Use **Drizzle ORM’s** parameterized queries exclusively—never concatenate user input.  
  • **XSS Protection:** Escape or encode all user-provided data in React components. Use a strict **Content Security Policy (CSP)**.  
  • **CSRF Mitigation:** Protect state-changing API routes with anti-CSRF tokens (e.g., `@/lib/csrf.ts`) or Next.js built-in CSRF features.  
  • **File Uploads:** If you add uploads, validate file type/size, scan for malware, store outside the webroot, and sanitize file paths.

---

## 4. Data Protection & Privacy

### 4.1 Data In Transit

  • Enforce **HTTPS/TLS 1.2+** across all endpoints (frontend and API).  
  • Use **HSTS** (`Strict-Transport-Security` header) to prevent protocol downgrade.

### 4.2 Data At Rest

  • Encrypt sensitive data (user PII, secrets) in the database using **AES-256**.  
  • Never store plaintext secrets or credentials. Use a secrets manager (e.g., AWS Secrets Manager, HashiCorp Vault).

### 4.3 Logging & Error Handling

  • Avoid logging sensitive data (passwords, tokens, PII).  
  • Return generic error messages to clients; log detailed stack traces only in secure, access-restricted environments.  
  • Implement log rotation and secure log storage with restricted permissions.

### 4.4 Data Privacy Compliance

  • Comply with relevant regulations (GDPR, CCPA) for PII collection, processing, and deletion.  
  • Provide mechanisms for data export and account deletion.

---

## 5. API & Service Security

  • **Rate Limiting & Throttling:** Use a middleware (e.g., `express-rate-limit` or Next.js middleware) to protect login and high-risk endpoints.  
  • **CORS:** Configure restrictively. Allow only known origins (`termux-app.example.com`).  
  • **HTTP Methods:** Enforce correct verbs (GET for reads, POST for creates, PUT/PATCH for updates, DELETE for removals).  
  • **API Versioning:** Namespace routes (e.g., `/api/v1/...`) to manage changes securely over time.  
  • **Minimal Responses:** Return only necessary fields (avoid leaking internal IDs or logic).

---

## 6. Web Application Security Hygiene

  • **Security Headers:** Set `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: no-referrer`, and a robust **CSP**.  
  • **Secure Cookies:** Always use `Secure`, `HttpOnly`, `SameSite=Strict` for session and CSRF tokens.  
  • **Subresource Integrity (SRI):** When loading third-party scripts, include SRI hashes to ensure content hasn’t been tampered with.  
  • **Disable Client-Side Debug:** Ensure `NEXT_PUBLIC_DEBUG` or similar flags are `false` in production.

---

## 7. Infrastructure & Configuration Management

  • **Configuration as Code:** Store environment-specific settings in environment variables or a secrets manager; do not commit `.env`.  
  • **Docker Security:**  
    - Base images should be minimal and regularly updated.  
    - Run containers with a non-root user.  
    - Limit container capabilities and exposed ports.  
  • **Server Hardening (Termux):**  
    - Keep Termux packages updated (`pkg update && pkg upgrade`).  
    - Disable unnecessary services and ports.  
    - Use `ufw` or `iptables` for firewall rules.

---

## 8. Dependency Management

  • **Lockfiles:** Commit `package-lock.json`/`yarn.lock` for deterministic builds.  
  • **Vulnerability Scanning:** Integrate an SCA tool (e.g., **Dependabot**, **Snyk**) to detect CVEs in dependencies.  
  • **Minimal Footprint:** Only include necessary packages (shrink your attack surface).  
  • **Regular Updates:** Schedule periodic dependency reviews and updates, especially for security patches.

---

## 9. CI/CD & DevOps Security

  • **Secure Pipelines:**  
    - Store CI secrets in dedicated vaults, not plaintext.  
    - Use least-privileged CI tokens.  
    - Scan code with linters, SAST tools, and dependency checks before merging.  
  • **Automated Testing:** Enforce unit tests, integration tests, and security tests (e.g., OWASP ZAP).  
  • **Deployment Gates:** Require manual approval or automated security checks before promoting to production.

---

## 10. Termux & CLI-Centric Considerations

  • **Local Secrets:** If storing AWS keys or database credentials on Termux, use Termux’s secure storage (`termux-keystore`) or an external vault.  
  • **Script Permissions:** Ensure the `~/AI` script is non-world-readable (`chmod 700`).  
  • **Network Exposure:** When running locally, avoid binding services to `0.0.0.0`; use `localhost` or a specific interface.

---

## 11. Recommendations & Next Steps

  1. **Implement Zod Validation:** Add request schemas to all API routes.  
  2. **Configure CSP:** Define a strict CSP policy in `next.config.js` or custom server.  
  3. **Integrate MFA:** Roll out TOTP-based MFA for admin and user accounts.  
  4. **Automate Migrations:** Add `drizzle-kit` to manage database schema changes securely.  
  5. **Add Security Tests:** Use Jest and Playwright to test authentication flows, CSRF protection, and input sanitization.

---

**By following these guidelines, the termux-ai-agents-starter codebase will maintain a strong security posture—enabling rapid AI-driven development without compromising on safety or compliance.**