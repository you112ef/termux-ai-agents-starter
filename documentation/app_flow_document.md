# App Flow Document for Termux AI Agents Starter

## Onboarding and Sign-In/Sign-Up
A new user starts by pointing their browser at the application’s URL (for example, http://localhost:3000 when running locally). On the landing page they are greeted with options to sign in or create a new account. If they choose to sign up, they are taken to a registration form where they enter their email, password, and any other required fields. Upon submitting valid information, the system creates their account, logs them in automatically, and redirects them to the main dashboard.

If an existing user returns, they click “Sign In” and are presented with a form for email and password. Once they enter correct credentials, the session is established, and they land on the dashboard. The sign-in and sign-up pages handle invalid inputs by displaying inline error messages—like “Email is invalid” or “Password is too short”—and they prevent submission until the issues are resolved. A “Forgot password?” link appears on the sign-in form, sending the user to a password recovery flow where they enter their email to receive reset instructions. After resetting their password via the emailed link, they return to sign in with their new credentials.

Signing out is as simple as clicking a “Sign Out” button in the header of any protected page. This action clears their session and sends them back to the sign-in screen, ensuring they cannot access the dashboard or settings until they log in again.

## Main Dashboard or Home Page
After logging in, the user lands on a protected dashboard at /dashboard. At the top they see a header containing the application logo on the left, a user avatar with a dropdown for account settings and sign out on the right, and optionally a dark mode toggle. Below the header is a horizontal navigation bar or sidebar that lists the main sections—Dashboard, Insights, and Settings. The dashboard view itself presents a grid of placeholder cards where data visualizations or management panels will appear.

Each placeholder card might show a chart, a statistics summary, or an interactive table. These cards are built using the shadcn/ui components and are ready for the user (or their AI agent) to replace or extend. The sidebar makes it clear how to switch between the overview dashboard and other parts of the app by clicking the linked labels.

## Detailed Feature Flows and Page Transitions
When a user clicks on one of the sidebar items—such as Insights—they navigate to a new page at /insights. That page loads a different set of UI components, like line charts or data tables. If they click a button labeled “Add Panel,” a modal dialog appears, letting them choose from component templates and configure data sources. Submitting the form in the modal updates the dashboard by adding the new panel to the grid.

If the user wants to change their profile details, they click the avatar in the header and choose “Account Settings.” This takes them to /settings where a form is pre-populated with their email and other profile fields. They can update their information and click “Save Changes.” The form submits to an API route, Drizzle ORM writes the updated data to PostgreSQL, and the page shows a success banner once the change is complete.

Behind the scenes, every form—whether on sign-up, sign-in, dashboard panel creation, or settings—sends data to Next.js API routes. Those routes validate inputs, interact with the database through Drizzle ORM, and then return JSON responses indicating success or errors. When an operation finishes, the page either refreshes its data or shows an inline message, keeping the user informed at each step.

## Settings and Account Management
The Settings page is the central spot for personal preferences. It contains sections for updating your email address or password and toggling preferences like notifications or theme. Each section has its own form and a “Save” button. When a user updates their password, the new value is run through Better Auth’s secure hashing before being stored in the database.

If billing or subscription features are added later, those would appear here as an additional section labeled “Subscription.” Users could enter payment details or upgrade plans, with forms submitting to secure payment APIs. Once complete, the subscription status would reflect in both the Settings page and an account summary in the header.

After saving any settings, users can click “Back to Dashboard” in the sidebar or header to return to the main flow. Their new preferences take effect immediately—new notifications settings are respected, theme changes apply across pages, and updated credentials work on the next sign-in.

## Error States and Alternate Paths
If a user tries to access the dashboard while not logged in, the application automatically redirects them to the sign-in page, showing a message like “Please sign in to continue.” When a network issue occurs—such as losing connectivity during form submission—the app displays a banner saying “Network error. Please try again.” and offers a retry button.

Invalid form entries trigger inline validation errors. For example, if a user enters an email without an “@” symbol, the email field shows “Invalid email address.” If password and confirm password fields don’t match in registration or settings, the confirm field displays “Passwords must match.”

Server-side errors, such as a database failure, are caught by Next.js error handling. The user sees a friendly fallback page that explains “Something went wrong on our end” and suggests returning to the dashboard or contacting support. Once the issue is resolved, they simply reload or retry their last action to regain a normal flow.

## Conclusion and Overall App Journey
From the moment a user opens the app at the root URL, they either sign up or sign in before entering a secure dashboard. There they see a flexible grid of UI panels and a clear navigation menu that leads to insights and settings pages. Every user action—whether creating a new panel, updating a profile, or changing preferences—flows through intuitive forms and secure API routes backed by a type-safe database layer. Errors are handled gracefully with helpful messages, and users always have a clear path back to the dashboard. This consistent journey empowers both human developers and AI agents to focus on building new features rather than wrestling with foundational flows.