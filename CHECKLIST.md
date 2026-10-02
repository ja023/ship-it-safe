# Ship It Safe: The Vibe-Coded Web App Checklist

A practical checklist for taking a web app built with AI coding tools (Cursor, Claude Code, Lovable, Bolt, v0, Replit and friends) from "it works on my machine" to production-ready: secure, reliable, compliant and ready for real customers.

Work through it section by section and tick items off. Not every item applies to every app, so skip what doesn't fit your stack.

> **Heads up:** this is general guidance, not legal or security advice. Prices, tool limits and laws change, so double-check anything with a number or a legal requirement before relying on it.

## Top 10: if you only do ten things

1. Don't ship any AI-generated code until a person has checked it for security. The AI only adds what you ask for, so ask outright for tests, validation, session handling, backups, monitoring and error handling.
2. Keep every secret out of the frontend and out of git. Scan the full git history, add push protection or a pre-commit hook, and rotate any key that was ever exposed.
3. Check authorization on the server for every request: the caller must own the record. Turn on row-level security for every table, and never serve users through a key that bypasses it.
4. Validate every input on the server with a schema. Accept only a fixed list of writable fields, and never take role, plan or permissions from the client.
5. Use a hosted auth provider. Keep access tokens short-lived in httpOnly cookies, rotate refresh tokens, and verify JWT signatures on every request.
6. Grant paid access only after a verified, idempotent webhook confirms the payment. Never trust the success page or a client-sent amount.
7. Turn on point-in-time recovery and test a restore into a fresh environment at least every quarter.
8. Monitor from outside your own servers, send errors to Sentry, and alert on business numbers such as payments and sign-ups per hour, not only on server health.
9. Keep dev, staging and production separate. Let only CI-tested, reviewed PRs reach main, and have a one-click rollback ready.
10. Put hard spending caps and billing alerts on every provider. Cut AI costs with prompt caching, semantic caching and routing requests to cheaper models.

## Contents

- [Working with AI coding tools](#working-with-ai-coding-tools) (36)
- [Authentication & sessions](#authentication--sessions) (38)
- [Web app security](#web-app-security) (58)
- [Multi-tenancy](#multi-tenancy) (19)
- [APIs & webhooks](#apis--webhooks) (32)
- [Payments](#payments) (19)
- [Secrets & keys](#secrets--keys) (17)
- [Servers & infrastructure](#servers--infrastructure) (30)
- [Deployment & CI/CD](#deployment--cicd) (21)
- [Databases & data](#databases--data) (27)
- [Scaling & performance](#scaling--performance) (26)
- [Monitoring, incidents & support](#monitoring-incidents--support) (27)
- [SEO & discoverability](#seo--discoverability) (19)
- [Email & domain](#email--domain) (6)
- [Mobile apps](#mobile-apps) (8)
- [AI features & agents](#ai-features--agents) (17)
- [AI models, prompts & costs](#ai-models-prompts--costs) (22)
- [Testing & quality](#testing--quality) (10)
- [Compliance & legal](#compliance--legal) (35)
- [Business, product & pricing](#business-product--pricing) (46)
- [Retention & onboarding](#retention--onboarding) (8)
- [Content & audience](#content--audience) (9)
- [Career](#career) (5)

## Working with AI coding tools

### What the AI won't do unless you ask
- [ ] AI-generated code covers only the happy path. Ask for the non-functional work outright: tests, backups, monitoring, token lifecycle, session management, input validation, CSP, tenant isolation, deletion flows, dunning, tax, compliance, error handling and odd user behavior.
- [ ] Ask for each security layer by name, because the AI tends to stop at the first one: a signature check with no duplicate handling, state with no PKCE, validation on the client but not the server.
- [ ] Treat AI output as about 80% done, and plan real engineering time for the rest: architecture, security, testing, deployment and governance.
- [ ] Assume the AI put secrets wherever the code needed them, without asking where that code runs, and check.
- [ ] Tell the AI your industry and location so it can look up the retention, tax and regulatory rules that apply to you.
- [ ] Every AI build needs a defined scope, a verification step and a person accountable for the output. Going fast without them is how leaks like 1.5M API keys from one app happen.

### Reviewing AI-written code
- [ ] A person must check security and production readiness before anything ships.
- [ ] Audit AI code for security on every build. It reportedly has about 2.74x more vulnerabilities than human code, many of them OWASP-class.
- [ ] Spend about 20 minutes on each AI-generated commit before it reaches production: run a secret scanner, review auth and audit permissions.
- [ ] Assume AI-generated code has the same default settings, missed edge cases and vulnerabilities as thousands of other apps built the same way.
- [ ] Stronger models write more complex, layered code that is harder to debug. Polished-looking output is not proof that it works.
- [ ] Don't let the AI that wrote the code be its only reviewer. Send critical modules to a second AI platform and pay most attention to where the two disagree.
- [ ] Set reviews up as red-team exercises. Ask how to bypass auth, reach another user's data or crash the system, not whether the code looks good.
- [ ] Switch which platform builds and which reviews critical features. Rebuild one critical module on a second platform and list every difference, which shows design choices the first tool made without saying so.
- [ ] Match each tool to a job: Claude for adversarial and security review, Codex or Gemini (large context) for implementation and cross-file checks, and Lovable, Bolt or Cursor for rebuilding a module from the same spec to compare.
- [ ] An AI code-review tool such as CodeRabbit is not a security review. Check the attack surface separately.
- [ ] Use the same security checklist on every project and have the AI help fix each item.
- [ ] Before fixing issues one at a time, audit every production layer. Then fix in order of business risk (money loss, data loss, legal exposure), not by what is newest or loudest.
- [ ] Run a structured audit when work comes in (entry) and again before it ships (exit), and fix what fails before customers find it.

### Keeping the codebase understandable
- [ ] Read your API routes, auth middleware and database queries out loud. If you can't sum up a function in one sentence, stop until you understand it.
- [ ] Rename vague AI-chosen names (process, data, handle) to names that state the intent (updateUserAccount, validatePaymentAmount).
- [ ] Delete unused functions, imports, components and helpers you never asked for. Less code is safer code.
- [ ] Keep the code split into modules with clear responsibilities and names, even when the AI argues for one 1,400-line file. The AI only optimizes for the current session, so when it argues against an established standard, follow the standard.
- [ ] Don't wait for a better model to fix your tech debt. Everyone gets the same model on the same day.

### Long agent jobs, context & skills
- [ ] For long agent jobs, keep a project state file (done, in progress, constraints, decisions). Update it after every step and read it at the start of each step.
- [ ] Break big tasks into chunks of 5–7 steps, run each chunk in a fresh session with the state file, and review the agent's summary before it moves on. Don't let a 30-step job run unsupervised.
- [ ] About every 90–100 days, clear out your AI setup (CLAUDE.md and other MD files, skills, MCPs, automations, connectors) and rebuild it. Stale instructions clash with new ones and fill the context window.
- [ ] Remove MCPs you don't need, since every loaded tool uses context. Let the agent read API docs and build integrations directly when needed.
- [ ] Check your skills and prompts against the current model version and remove ones built for outdated syntax or workarounds.
- [ ] Load skills only for the task in hand (load, check, unload), and compare results with only relevant skills loaded against everything loaded.

### Prompt injection & untrusted AI inputs
- [ ] Anything a coding assistant reads can carry a prompt injection: PR descriptions, issues, comments and repo content. An injection hidden in a PR description caused a Copilot remote-code-execution bug rated CVSS 9.6.
- [ ] Rank code sources by trust: your own reviewed code (high), vetted packages (pin and scan weekly), and unvetted community prompts, skills and repos (riskiest).
- [ ] Never give your AI a prompt or skill file you haven't read line by line, and never let unvetted ones reach production.

### Documentation
- [ ] Write a short paragraph on why each architecture decision was made (database, service split, which LLM), or have your AI assistant keep a decision log.
- [ ] Document local setup completely: commands, env vars and the workarounds you no longer notice.
- [ ] Document failure modes: what happens, and what to do, when the database goes down or a rate limit is hit.

## Authentication & sessions

### Choosing an auth provider
- [ ] Use a proven hosted auth provider (Clerk, Supabase Auth, Auth0, Better Auth, Firebase Auth) instead of writing your own. Login, resets, verification, sessions, token rotation, 2FA and recovery are all places where one mistake can sink you.
- [ ] Get auth right before any other part of the app. A slow app can recover; a breach usually can't.
- [ ] Choose Auth0 if your buyers are enterprises needing SAML, LDAP/AD SSO and compliance paperwork. Choose Clerk for developer-focused or small-team SaaS, where it gets you polished auth in about 10 minutes. Base the choice on who your future customer is.
- [ ] Consider self-hosted auth such as Better Auth to keep users and sessions in your own database and avoid vendor pricing changes, but only if your team can build and maintain it.
- [ ] Choose your auth provider carefully before launch, and work out how you'd migrate away before thousands of users depend on it. Switching auth vendors later is one of the most painful migrations.
- [ ] Make sure your auth provider can supply its own SOC 2 or other compliance documents, because buyers audit your vendors too.
- [ ] Move to a dedicated identity service (Auth0, Clerk, WorkOS) once you need SSO, custom claims or multi-tenant permissions that built-in auth handles badly.

### Tokens & JWTs
- [ ] Keep tokens out of localStorage. Store them in cookies marked httpOnly, Secure and SameSite, and set those flags on every session cookie.
- [ ] Keep access tokens short-lived (about 15–30 minutes, never weeks) and renew them with refresh tokens.
- [ ] Rotate refresh tokens on every use, so each one works only once. If an old one comes back, treat the account as possibly compromised. Clerk and Supabase do this for you; custom auth has to build it.
- [ ] Refresh OAuth access tokens (such as Google's roughly 60-minute tokens) in the background before they expire. The AI often builds only the first login, which ends every session after an hour.
- [ ] When the refresh token itself expires, send the user to log in again and then return them to where they were, with drafts, cart and form state intact.
- [ ] Verify a JWT's signature on every request before reading any claim, otherwise a user can edit their role to admin. Also reject expired tokens and tokens from the wrong issuer.
- [ ] Verify tokens with the provider's server-side SDK (for example Clerk's) rather than decoding them by hand.
- [ ] Refuse tokens with `alg: none`, and hard-code the expected algorithm (for example RS256) to block algorithm-confusion attacks.
- [ ] Don't rely on framework default sessions that never expire. Set session lifetime by risk (hours or less for financial data, days at most for low-risk content) and re-test expiry with every release.
- [ ] When a password changes or compromise is reported, invalidate all of that user's tokens and sessions at once (with a revocation list or a per-user token version), not at the next refresh.
- [ ] On logout, invalidate the session on the server as well as clearing the cookie. To test: copy a protected URL, log out and open it again. It must not load.

### Sessions
- [ ] Issue a new session ID after every login and every privilege change, such as free to paid or viewer to admin (Express: `req.session.regenerate`).
- [ ] Cap how many sessions one user can have active at the same time.
- [ ] Only count real actions toward idle timeouts (submits, clicks, API calls, navigation), and don't log out someone who is reading a long page.
- [ ] Show a warning about 60 seconds before a session expires, with a button to stay logged in.
- [ ] Test auth with many accounts at once, looking for mixed-up session tokens and users seeing each other's dashboards.
- [ ] Score session risk for the whole session: watch for impossible travel, unusually large data access and privilege escalation, and challenge or end the session automatically.
- [ ] Offer two-factor login by SMS and authenticator app in apps that handle sensitive data.
- [ ] If support tickets show that changing an email resets permissions, treat it as an auth-flow bug.

### OAuth, redirects, resets & magic links
- [ ] Register exact OAuth callback URLs with each provider: no wildcards, no pattern matching, no open redirects.
- [ ] Send a cryptographically random `state` with every OAuth login and reject callbacks where it doesn't match.
- [ ] Use PKCE as well as `state`. State stops forged requests but does nothing against a stolen authorization code, which matters most on mobile and shared networks.
- [ ] PKCE in practice: create a random code verifier and keep it on the server, send its hash as `code_challenge`, then send the original verifier in the token exchange.
- [ ] Request only the OAuth scopes a feature needs (for example email rather than the full profile), and cut back the ones you already request.
- [ ] Check every redirect or return-to parameter in the auth flow, including after logout and in magic links, against an allowlist of your own domains. Parse it properly and reject `//`, backslash and URL-encoded tricks.
- [ ] Password reset tokens should expire after 15 minutes, work once, and be limited to 3 requests per email per hour.
- [ ] Magic-link tokens should expire after 10 minutes, work once, and be limited to 3 requests per email per hour, with a per-IP limit on top. Otherwise attackers can flood a victim's inbox and ruin your sender reputation.
- [ ] Rate-limit login, registration and password-reset endpoints per IP per minute, at the edge before requests reach your server.

### Enterprise SSO
- [ ] Google Sign-In plus email/password is not enterprise SSO. Add SAML 2.0 and/or OIDC before selling to enterprises, and build it before a procurement checklist asks for it.
- [ ] Build the complete SSO flow for Okta, Azure AD and Google Workspace: handshake, assertion handling, attribute mapping and session management.
- [ ] Make SSO configurable per tenant, with each customer's own identity provider and credentials.

## Web app security

### Baseline checks & scanning
- [ ] Before launch, cover at least: auth, encryption, session handling, error boundaries, rate limiting, input validation, logging, backups, monitoring and dependency scanning.
- [ ] Do a 30-minute self-audit: run `npm audit`, try reaching user B's records while logged in as user A by changing IDs in URLs and API calls, and check for secrets in code, committed .env files or frontend bundles.
- [ ] Scan staging with OWASP ZAP (free, one Docker command) before the first user signs up. Fix critical and high findings first (injection, auth bypass, data exposure), and treat mediums and lows as normal bugs.
- [ ] Use ZAP as your routine scanner (it covers about 80% of needs), and Burp Suite Pro for deep audits each quarter, before big launches or when enterprise customers ask for a third-party assessment.
- [ ] Use Burp to intercept your own requests and change the user ID, org_id or role claim. If you can then see another user's or tenant's data, or reach admin endpoints, authorization is broken. RLS doesn't help an API that passes unchecked parameters through.
- [ ] Add Snyk or GitHub code scanning to every push, and GitGuardian or TruffleHog to every PR.

### Headers, CSP, CORS & CSRF
- [ ] Set security headers on every response: Content-Security-Policy, X-Frame-Options and Strict-Transport-Security. Most frameworks leave them out, and they take about five minutes to add.
- [ ] Use a CSP that allows scripts only from approved domains and blocks unauthorized inline scripts, so even injected script tags won't run.
- [ ] Run the CSP in report-only mode for about a week, fix what would break, then enforce it.
- [ ] Keep a list of every external resource and third-party script (analytics, chat, pixels, A/B testing, fonts): what data it can reach and which pages load it. Remove what you can't justify.
- [ ] Don't load third-party scripts on login, checkout, account-settings or admin pages. Load scripts only on the pages that need them.
- [ ] Block clickjacking with `X-Frame-Options: DENY` plus `Content-Security-Policy: frame-ancestors 'self'`. If widgets or payment forms must be framed, allow it only on those routes.
- [ ] Never allow a wildcard or reflected CORS origin, especially when credentials are allowed. Hard-code a list of your own front-end origins.
- [ ] Allow only the HTTP methods and headers each endpoint's frontend uses, and reject the rest at preflight.
- [ ] Set SameSite on session cookies and require CSRF tokens on every endpoint that changes data.

### Input validation & injection
- [ ] Validate every endpoint, form, API parameter and query string on the server with a schema library such as Zod, reusing the schema from the form. Browser-only validation improves the experience but protects nothing.
- [ ] Check each input's type and maximum length (for example, search capped at about 200 characters) and reject bad input instead of trying to repair it.
- [ ] Test every API route with direct requests that skip the frontend (bad values, extra fields). If the server accepts what the form would reject, fix it.
- [ ] Sanitize string input so injected markup such as script tags never runs.
- [ ] When the AI falls back from Prisma or another ORM to raw SQL, use the ORM's parameterized raw-query method. Search the codebase for raw queries built from user input and fix them.
- [ ] Make sure inputs survive odd characters. A name with an apostrophe must not break a database write.

### Access control
- [ ] Treat login (who you are) and permissions (what you may do) as separate problems. Use roles, ownership checks on every route and row-level security.
- [ ] Enforce roles and permissions on the server in every API route and server action. Hiding a button is cosmetic, because anyone can call the endpoint.
- [ ] Keep every rule about what a user can see, do or pay (pricing, discounts, feature gating) on the backend. The frontend only displays results.
- [ ] Check ownership on every request: compare the logged-in user with the resource's owner and return 403 if they differ. Never trust an ID from the URL, query or body.
- [ ] Test for IDOR by changing an ID in the URL or an API call. List every endpoint that takes a resource ID and confirm each one checks ownership.
- [ ] Use UUIDs or other unguessable IDs instead of sequential integers, so records can't be listed by counting up.
- [ ] Never let the browser talk to the database directly (for example, frontend-only Supabase calls). Put a backend in between that owns business rules, validation, rate limiting and auth.
- [ ] Launch with three roles (admin, member, viewer), which cover about 90% of SaaS needs. Don't build a custom permission matrix before a paying customer needs one.
- [ ] Replace an `is_admin` boolean with granular permissions (create, delete, export), make roles bundles of those permissions, and add scope (own records or all, own team or all).
- [ ] Define clear permission levels for complex apps (for example super admin, org admin, staff, clinical team, patient portal).
- [ ] Require auth plus a role check on every admin route, so a normal session can't reach an admin page by editing the URL.
- [ ] Move the admin panel off guessable paths (/admin, /dashboard, /manage), rate-limit admin logins, and log every admin login attempt, action and data change.
- [ ] Beyond static roles, add attribute-based access control through a policy engine (time, location, device, IP reputation, data sensitivity). Require step-up auth, or deny the request, in risky contexts such as financial data at 3 AM from an unknown device.
- [ ] Use zero trust: re-check identity and authorization on every internal API call, database query and service-to-service request, not only at login.

### Next.js & React specifics
- [ ] Don't use Next.js middleware as your only auth check. Make sure its matcher covers every protected route, including API routes, and test variants such as trailing slashes, double-encoding and path prefixes.
- [ ] Check authorization on the server in every API route and Server Component too, because middleware may only confirm that a cookie exists.
- [ ] Add `import 'server-only'` to every file that touches the database or credentials, and keep server functions and client components in separate files with no shared exports.
- [ ] Search your production bundle for keys, database hostnames, connection strings, internal endpoints and env values. Know which env vars reach the browser (`NEXT_PUBLIC_`, `VITE_`).
- [ ] In React Server Components, select only the fields you display and pass small objects between components. Every prop reaches the browser, so check that no password hash, role flag or billing token leaks.

### Errors
- [ ] Show users short, generic messages and send the full details (stack trace, request context, session) to a private structured log. Detailed errors belong in development only.
- [ ] Catch errors at every boundary: API routes, background jobs, webhook receivers and payment callbacks.
- [ ] Build branded 404, 500 and timeout pages that tell users what to do next and reveal nothing about the server.
- [ ] Build the error paths too: clear messages and fallback states instead of white screens or bare 500s.
- [ ] Wrap every external call (payments, APIs, database) in try/catch, and tell the user plainly what went wrong, with a retry button.
- [ ] Retry with exponential backoff (1s, 2s, 4s), then fail gracefully with a clear message.
- [ ] Give every UI component four states: loading, error, empty and success. Use error boundaries so one broken component can't crash the app.

### Dependencies & supply chain
- [ ] Run `npm audit`. Fix critical findings, update moderate ones, and ignore findings that don't affect how you use the package.
- [ ] Commit your lockfile, never delete it, and pin versions so a compromised upstream release can't reach production automatically.
- [ ] Scan dependencies continuously with npm audit, Snyk, Socket or Dependabot, and book a recurring 10-minute check (for example the first Monday of each month).
- [ ] Check every package the AI adds before installing it: is it maintained, does it have CVEs, is its license OK? Replace anything deprecated or untouched for 2+ years.
- [ ] Audit transitive dependencies too. Flag packages with under 100 weekly downloads, a maintainer change in the last 90 days, postinstall scripts, or typosquatted names.
- [ ] Check lockfile integrity in CI and block the deploy on any checksum mismatch.
- [ ] If you could write it in about 20 lines, write it yourself rather than adding a package with dozens of transitive dependencies.
- [ ] Treat every package as untrusted code that can read your env, files and network, and don't hand the whole `process.env` to every module.

### Accessibility
- [ ] Make every button, form, dropdown and modal work with the keyboard alone. Test by tabbing through every core action.
- [ ] Support screen readers: alt text, button labels, ARIA attributes on forms, labeled navigation.
- [ ] Run a contrast audit and fix any text below the WCAG AA ratio of 4.5:1. Treat accessibility as a legal duty (ADA lawsuits), not polish.

## Multi-tenancy

### Isolation
- [ ] Build tenant isolation from day one: a `tenant_id` column on every table, and a tenant filter on every query.
- [ ] Enforce isolation in the database with Postgres RLS policies that match on tenant or user ID, not only with WHERE clauses in app code, where one missed filter leaks data. The app is the first line of defense and the database the last.
- [ ] In Supabase, enable RLS on every table with user or tenant data, and check Authentication > Policies. A table with no policies is open to anyone with a valid session.
- [ ] Give every user-data table at least SELECT and INSERT policies (`user_id = auth user`), and UPDATE and DELETE policies where the data is sensitive.
- [ ] Never use the Supabase service role key in routes that serve user requests, because it bypasses all RLS. Query with the user's session or scoped credentials, and secure every path to the data.
- [ ] Put the tenant ID in every cache key (cached queries, page fragments, API responses). The cache sits in front of the database, so RLS doesn't protect it.
- [ ] Check isolation on every shared layer: search indexes, job queues, file storage paths and logging pipelines. A layer that doesn't know which tenant it is serving should serve nothing.

### Testing & monitoring isolation
- [ ] Write a cross-tenant leak test: load a page as tenant A, log out, load the same page as tenant B, and confirm none of A's data appears. Keep tests that prove tenant boundaries hold.
- [ ] Use a proxy to change the org or tenant ID in request bodies. Reading any other tenant's records is a critical hole.
- [ ] Alert when one tenant's data is shown to another, and keep logs that tell you how long an exposure lasted and who could have seen it.

### Architecture
- [ ] Choose the isolation model on purpose, based on what customer contracts require, not the ORM's default.
- [ ] Shared schema with RLS is fine to start, but watch for one tenant generating most of the load and slowing everyone else.
- [ ] Schema-per-tenant isolates performance better, but every migration has to run once per tenant (100 tenants means 100 runs).
- [ ] Use a database per tenant for regulated customers (healthcare, finance, government, heavy PII).
- [ ] Build indexes around real access patterns (students by school, attendance by date), and decide on multi-tenancy before building, not after you scale.
- [ ] Never fork the repo per client. Use feature flags keyed by tenant ID, and a shared base config with per-tenant overrides merged at runtime.
- [ ] Add middleware that identifies the tenant (subdomain, header or JWT claim) on every request before any business logic runs.
- [ ] Put tenant-specific custom fields in a per-tenant extension table or JSONB column, never in new columns on the shared schema.
- [ ] Run heavy per-tenant jobs in tenant-scoped workers or queues, and keep core schema migrations backward compatible so no single tenant forces downtime for everyone.

## APIs & webhooks

### API design & exposure
- [ ] Write a schema for every endpoint (accepted input, output, rejected cases) and enforce it before any business logic runs: reject wrong types and missing fields, and strip unknown fields.
- [ ] Allow only a fixed list of writable fields on each write endpoint (for example name, email, avatar). Never accept role, plan, permissions or account status from the client (mass assignment).
- [ ] Write tests that send forbidden fields (isAdmin, accountType) to every update endpoint and confirm they are rejected or dropped.
- [ ] Keep user and admin endpoints separate. Admin routes get stronger authorization, and user handlers drop admin-level fields.
- [ ] Remove every response field the client doesn't need (email, phone, billing address, internal IDs), and check every endpoint for missing auth.
- [ ] Version the API from day one (`/v1/`). Put breaking changes in a new version and let clients move on their own schedule.
- [ ] Before removing an endpoint, send a `Sunset` header with the retirement date, track remaining usage, and remove it only when usage reaches zero.
- [ ] Keep a public changelog and docs page, and announce changes early with deprecation warnings, migration guides and timelines. Treat your API as a product that technical buyers judge and other businesses depend on.
- [ ] Require an HMAC signature of the body, made with a shared secret, on every POST/PUT/PATCH/DELETE. Check it in middleware.
- [ ] Design service-to-service auth separately from user auth, because one leaked service credential exposes every account. Use mutual TLS for high-trust internal calls.
- [ ] GraphQL: turn off introspection and field suggestions in production, return generic errors, and cap query depth and complexity.
- [ ] Consider exposing your service over MCP so AI assistants can find it and integrate with it.

### Rate limiting & abuse
- [ ] Rate-limit per user or IP per window (for example 50/min or 1,000/hr). Real users make about 2–5 requests a minute; bots make hundreds.
- [ ] Rate-limit these first: AI and paid-API endpoints (cost), auth endpoints (brute force) and public data endpoints (scraping). Use Vercel's built-in limiter, Upstash Redis or your framework's library.
- [ ] Use three layers of limits: hard caps per window that return 429, adaptive limits (token bucket or sliding window) that tighten under load, and per-plan quotas.
- [ ] Block bots before they reach the rate limiter, and track rates per user and endpoint so you catch probing (for example 50 hits a second on one endpoint).
- [ ] Configure and load-test CORS and rate limiting before launch.

### Third-party calls & resilience
- [ ] Put each external API behind one wrapper file, and every vendor behind an abstraction layer, so you can switch providers when pricing or volume changes.
- [ ] Plan for each API failing (cached response, retry queue, or at least a "back shortly" message), and keep your own copy of important data.
- [ ] Don't fire API calls on every page load or chain them one after another. Run independent calls in parallel, and add retries, queuing, batching and caching.
- [ ] Put a circuit breaker on every third-party, webhook and service-to-service call. Return a fallback immediately while it is open.
- [ ] Give each external dependency its own small connection pool (a bulkhead, for example 10 connections for the payment API) so one hung service can't starve the rest.
- [ ] Set one overall timeout per request and split it across downstream calls. With 5s total, if the first call takes 3s, the next gets 2s.

### Long-running requests
- [ ] Never do slow work (for example a 45-second PDF export) inside the request. Create a job, return its ID in about 200 ms with a "processing" status, and show progress.
- [ ] Run jobs on a worker (Inngest, Trigger.dev or BullMQ) with exponential-backoff retries, and notify the user when the job finishes.
- [ ] Check an idempotency key before creating a job, so a double click returns the existing job instead of creating a duplicate.

### Webhooks
- [ ] Verify the signature on every incoming webhook (Stripe, Clerk, GitHub, Resend) with the official SDK and your endpoint secret before doing anything, and check every webhook endpoint in the app.
- [ ] Make handlers idempotent: save the event ID before running the logic, and return success without doing anything when the ID has already been seen. Returning an error on duplicates only triggers more retries.
- [ ] Reject webhook events older than 24 hours. This blocks replays and keeps the stored-ID table from growing forever.
- [ ] Use a random, unguessable webhook URL (not /webhooks/stripe) and, where possible, accept only Stripe's published IP ranges.
- [ ] Never return 200 when the business logic failed. That tells Stripe not to retry, and it is a common hidden cause of failed-payment complaints.
- [ ] Send failed events to a dead-letter queue, then retry them or raise an alert.

## Payments

### Checkout integrity
- [ ] Use hosted Stripe Checkout (cards, Apple Pay, Google Pay, receipts) instead of your own payment form, which keeps you out of PCI scope.
- [ ] Create Checkout sessions on the server. The client sends only a product ID, the server looks up the price, and you charge with Stripe Price IDs, never an amount from the frontend.
- [ ] Grant access only after a verified webhook confirms the payment and the amount matches your price. Reaching the success page proves nothing, and no thank-you page or email should go out before the charge succeeds.
- [ ] Use Stripe's unique event IDs to reject replays, so one payment can never set up several accounts or record twice.
- [ ] On a successful charge, run the whole business workflow: mark the invoice paid, activate the subscription, grant access, send the receipt, update the CRM.
- [ ] Use the Stripe customer billing portal for card updates, cancellations, plan switches and invoices.
- [ ] Test every payment method end to end, PayPal as well as cards: charge, confirmation email and success screen. A broken confirmation causes retries, double charges and chargebacks.
- [ ] When a card is declined, show a clear message and a retry option, never a frozen or blank screen.

### Failed payments & dunning
- [ ] Monitor payment success counts directly. Sentry and PostHog won't notice payments failing quietly.
- [ ] Set up dunning: retry failed charges on a staggered schedule over 7–14 days. Stripe supports this, but you have to configure it.
- [ ] Send a three-email failed-payment sequence with an update-card link (recovers roughly 30–40%), and keep the account active through a 7–14 day grace period.
- [ ] Count the real cost of payment failures: refunds still cost the fee, support takes time, and about 1 in 4 customers who hit a payment problem never return.
- [ ] Send duplicate-charge complaints to a person who checks the payment dashboard. Don't let a bot close them based on your own records.

### Disputes & chargebacks
- [ ] Publish your own refund policy (not a template or Stripe's default) and link it at checkout. Without one, Stripe tends to side with the customer.
- [ ] Alert when your dispute rate starts rising, before it crosses Stripe's threshold and your account gets frozen.
- [ ] Prepare a dispute-response template in advance, with transaction logs, delivery confirmations and refund-policy screenshots, because you only have days to respond.

### Usage-based billing
- [ ] Pick one pricing model (flat, per call or usage-based) and launch. Don't spend months building billing.
- [ ] Use credits to hide complexity (for example 1,000 a month; API call = 1, AI generation = 10, export = 5), so you can change what each action costs without changing the price.
- [ ] From day one, write every billable action (user, action, timestamp) to a dedicated table that both billing and analytics read, and report usage to Stripe's metered billing API.

## Secrets & keys

### Keeping secrets out of code
- [ ] Never put secret keys (OpenAI, Stripe secret, database connection string) in frontend code, because every visitor can read it. Check by opening the live site, pressing F12, and searching Sources for "key".
- [ ] Keep secrets only in your host's server-side env settings (Vercel, Netlify, Railway, Render, Fly), and call third-party APIs through your own small proxy route.
- [ ] Search the code for hard-coded keys and move them to env vars. Make sure `.env` is in `.gitignore` and never committed, because bots scrape leaked keys within hours.
- [ ] Don't assume vibe-coding platforms such as Lovable isolate or audit your secrets. Check yourself.
- [ ] Keep secrets out of logs, error messages and stack traces, where env vars often leak.

### Scanning & leaks
- [ ] Scan your entire git history for secrets, not just the current code. Rotate and revoke any key that was ever committed or shipped to the frontend, even if it was deleted later.
- [ ] Block secrets before they land: turn on GitHub push protection (about 2 minutes) or a pre-commit hook, and run GitGuardian, Gitleaks and TruffleHog.
- [ ] If a key leaks, rotate it immediately and report fraud to the provider. A leaked Stripe secret key is an emergency, because attackers can charge through your account within hours.
- [ ] If an outside resource you used turns out to be compromised, rotate your secrets right away.

### Secrets managers & rotation
- [ ] In production, use a secrets manager (Doppler, Infisical, AWS Secrets Manager) with encryption, role-based access, versioning and audit logs, and load secrets at runtime instead of baking them in at deploy.
- [ ] Log every secret access (who, when, from which IP, for which service) so you have a trail after an incident.
- [ ] Rotate keys on a schedule, not only after incidents: at least every 6 months, or automatically (for example every 30 days by cron: generate, update the manager, redeploy, health-check, revoke).
- [ ] Rotate with two live keys for zero downtime: create the new key, deploy it, confirm traffic works, then revoke the old one.

### Least privilege
- [ ] Apply least privilege to every credential and service account.
- [ ] Limit each API key to what it needs (read-only, specific endpoints, IP or domain allowlist). For example, a Stripe key that can only create checkout sessions from your domain.
- [ ] Use dynamic database credentials that expire after about an hour (Vault, Infisical, cloud secrets manager), scoped per service: the API gets read/write on its tables, analytics gets read-only, the worker gets only the job queue.
- [ ] Give each AI agent its own short-lived credential scoped to its task, never your personal key or login.

## Servers & infrastructure

### Serverless platform limits
- [ ] Read each platform's limits page, not its marketing page, before launch.
- [ ] Know the concurrency cap (Vercel Hobby about 10, AWS Lambda 1,000 per region by default). With 3 functions per request, about 300 users hit the Lambda default. Ask for increases before launch day.
- [ ] Compare function run time with the timeout (Vercel Hobby 10s, Pro about 60s). Move slow work such as 12–30s AI pipelines, file processing or bulk email to a background job with a callback.
- [ ] Check the other caps: request payload (about 4.5 MB, so upload big files straight to storage), response size (about 1 MB on Hobby), bandwidth (100 GB free), bundled function size (50 MB, which can fail without a clear error) and 100 deploys a day.
- [ ] Expect cold-start pileups on Vercel or Netlify when traffic spikes after an idle period.

### Choosing hosting
- [ ] Choose hosting for your current business stage, not the latest tutorial. Start on managed services, and self-host only when the economics justify it, depending on whether you are shorter on time or money.
- [ ] Vercel suits frontends and content sites. Railway suits containers, databases and workers without server management (watch its costs as you scale). Render adds managed Postgres and Redis, and Fly.io runs apps close to users.
- [ ] Split the stack: frontend on Vercel, and backend, jobs and processing on Railway or Render, talking over HTTPS. Background jobs can run on a VPS at about $5/month or a managed service at about $20/month.
- [ ] Serverless suits early products, but compare its monthly cost with containers (including your weekly ops hours) as you grow. Keep request/response APIs serverless and move queue workers and scheduled jobs to containers.
- [ ] Choose a VPS only if you're willing to own uptime, patching and maintenance. Don't over-invest before you have traffic, and don't under-invest in reliability once users depend on you.
- [ ] Split an all-in-one platform like Supabase into separate services once one layer (auth, storage, functions, database) starts limiting the others.
- [ ] The default bundle (Vercel, Sentry, Supabase, Clerk) is fine for your first ~10 small customers. Enterprise deals usually need infrastructure you own: data residency, deploying into their VPC, SOC 2 and support contracts.

### VPS & panel hardening
- [ ] On a VPS: SSH keys only, no password or root login, SSH moved off port 22, and unattended security updates turned on.
- [ ] Deny all inbound traffic by default with UFW/iptables and open only SSH, HTTP and HTTPS. Never expose the database port.
- [ ] Nginx only routes traffic. Add ModSecurity with the OWASP Core Rule Set in blocking mode on every route that accepts input.
- [ ] Run Fail2ban on the Nginx/ModSecurity logs with escalating bans, and alert on spikes in blocks or bans, which mean someone is probing.
- [ ] CloudPanel: limit port 8443 to your IP or VPN, turn on 2FA right after install, and put the panel behind a reverse proxy on a non-default path. Guard management panels like root access.
- [ ] Pin infrastructure images such as Redis to specific versions and review updates on a schedule. When a license change causes a fork, follow where the security patches go (for example Redis to Valkey), and keep the cache client behind an adapter.

### Edge, WAF, DDoS & SSRF
- [ ] Put a WAF in front of the whole stack (your host's or Cloudflare's), with custom rules for OWASP Top 10 patterns such as SQLi in query strings, XSS in forms and path traversal.
- [ ] Use Cloudflare bot management on high-value pages (pricing, checkout, API docs).
- [ ] Add adaptive, IP-based anomaly detection that escalates from throttling to temporary bans, instead of relying only on fixed per-user caps.
- [ ] Write a DDoS playbook before any attack: who is told, what gets switched off, where traffic goes.
- [ ] Behind Cloudflare, check that your origin IP doesn't leak through DNS history, MX records, email headers or unproxied subdomains. Firewall the origin to Cloudflare's IP ranges only, and use SSL Full (strict) with an Origin certificate.
- [ ] For any feature or agent that fetches user-supplied URLs (SSRF), allow only approved domains and block private and internal targets, including cloud metadata endpoints.
- [ ] Resolve each fetched URL to one pinned IP and re-check it on every redirect. Return the same generic error for every failed fetch and log the details privately.

### Cloud costs
- [ ] Set billing alerts at 50%, 75% and 90% of budget on every provider (Vercel, Supabase, OpenAI, AWS), with hard caps where possible. One function looping on an Opus-class model can cost about $300 an hour.
- [ ] Regularly shut down idle resources (forgotten staging environments, load-test replicas, unused buckets), and switch always-on idle resources to scale-to-zero (Neon pauses after 5 minutes idle).
- [ ] Size instances and databases to real usage (many apps run at about 15% utilization) and scale up when metrics call for it.
- [ ] Track egress (API responses, images, webhooks, cross-region traffic), which adds up quietly.
- [ ] Watch infrastructure cost per user. $900/month for 200 users paying $10 sends half your revenue to hosting.

## Deployment & CI/CD

### Environments
- [ ] Keep dev, staging and production completely separate (databases, API keys, config). Never test features or run migrations on the live database, and never let test users share tables with paying customers.
- [ ] Make staging match production (schema, services, env vars), give every PR a preview deploy (Vercel and Netlify offer them), and click through the preview before promoting.
- [ ] Define the environment as infrastructure-as-code, and write down everything that affects a deploy (env vars, versions, build order, cache state) so every deploy gives the same result.

### Branching, review & CI gates
- [ ] Treat main as production: branch protection, no direct pushes, at least one approval. Start with GitHub Flow and move to Git Flow only if release complexity demands it.
- [ ] Run tests, lint, build and a security scan on every PR, block the merge if any fails, and deploy automatically only when everything passes. No manual deploys, FTP or edits on the live server.
- [ ] A broken pipeline must never mean code ships without tests or linting.
- [ ] Add an AI PR reviewer (CodeRabbit, Sourcery or a custom Action that calls Claude) as a required check. Point it at business logic, SQL injection, edge cases and N+1 queries rather than style, have it flag anything touching auth, payments or deletion, and let critical findings block the merge.
- [ ] Keep commits, PRs and deploys small and focused on one change, so a failure points straight at its cause. Avoid Friday deploys.
- [ ] Pre-deploy check: secrets come from a manager, the model fallback chain works, token limits and cost caps are set, output validation is on, errors go to monitoring, and CORS and rate limits are tested.

### Releases & rollback
- [ ] Put new features behind flags (LaunchDarkly, Flagsmith or a JSON config in the DB), roll them out internal → beta → 10% (or 5%) → everyone, and switch off the flag instead of redeploying when something breaks.
- [ ] Canary releases: send about 5% of traffic to the new version, watch error rate, latency and status codes for about 15 minutes, and roll back automatically if they get worse.
- [ ] Automate canary promotion with GitHub Actions and your error-monitoring API: go to 100% if errors stay under threshold for about 30 minutes, and roll back if they don't.
- [ ] For AI features, gate the canary on quality: 5% of traffic for about an hour, with automatic rollback if the quality score or latency gets worse.
- [ ] Know your rollback before you deploy. It should be one click, under 60 seconds to 2 minutes, runnable by anyone on the team, and automatic when a deploy fails. No SSHing into servers.
- [ ] Add health checks so not every push goes straight live.
- [ ] Ask one question of every deploy: if it broke, could you fix it remotely within 30 minutes? If yes, any day is a deploy day.
- [ ] Run the deploy in staging under production-like traffic first.

### CI speed, cost & flakiness
- [ ] Plan for GitHub Actions' 2,000 free minutes running out as the team and tests grow (macOS runners are especially expensive). Check usage weekly and alert at 75%.
- [ ] For heavy CI, consider a self-hosted runner on a server of about $20/month for unlimited minutes, which you then have to maintain.
- [ ] Use path-based triggers (a README change doesn't need integration tests), run stages in parallel, and cache dependencies between builds.
- [ ] If tests pass locally but fail in CI, look first for env vars missing in CI. Make each test create its own data, watch for timeouts and race conditions on slower runners, and trust the CI result.

## Databases & data

### Choosing a database
- [ ] Choose by the shape of your data. Relational data (users → orders → items) belongs in Postgres (Supabase, Neon); self-contained JSON documents may suit Firebase or Convex.
- [ ] Judge the ecosystem (auth, storage, realtime, branching, SDKs) more than the engine, and pick what needs the least custom code.
- [ ] Plan your exit before building. Standard Postgres or MySQL (Supabase, Neon, PlanetScale) is portable, while proprietary models (Firebase, Convex, an edge ecosystem) mean a rewrite to leave.
- [ ] Compare read and write volume. Read-heavy global apps suit Cloudflare D1 (SQLite at the edge); write-heavy concurrent apps suit PlanetScale (sharding) or Neon (autoscaling).
- [ ] Neon: scales to zero (cold starts after idle, pay for real use) and has branching for testing migrations. PlanetScale: HTTP connections, zero-downtime schema changes, no enforced foreign keys. D1: very fast edge reads, weak on concurrent writes, tied to Cloudflare. Turso: SQLite at the edge.
- [ ] Pick Neon if your team knows Postgres or PlanetScale if it knows MySQL, and judge production cost and performance at 10,000 users, not by the free tier (PlanetScale dropped its free tier).
- [ ] Ask how each platform handles schema changes under live traffic before you commit to it.
- [ ] Pick Supabase over Firebase when you want to own your data (export as SQL) and need joins. Firebase suits weekend prototypes.
- [ ] Consider Convex for real-time collaborative apps, but weigh the lock-in of its proprietary query language.
- [ ] Prisma gives guardrails (schema, generated migrations, types) at the cost of a heavier client and slower cold starts. Drizzle is lighter and faster for teams that know SQL, but you own every optimization.
- [ ] Try pgvector in your existing Postgres before adding a vector DB. Use Pinecone, Weaviate or Chroma only when billion-scale vector search is the product itself.
- [ ] Store uploads in object storage (S3, Cloudflare R2), serve them through a CDN, and keep only the URL or key in the database. Blobs in the DB can quadruple its cost.

### Schema & migrations
- [ ] Design the schema on purpose: normalized tables, indexes, migration files and a backup plan, instead of adding columns to one table per feature (a 47-column table led to 8-second queries).
- [ ] Run migrations that add before they remove: create the new column, copy the data, switch the app over, and drop the old column only after checking. Never drop and recreate a column on a live DB.
- [ ] Write the rollback script along with the migration. If you can't describe how to reverse it, it isn't ready.
- [ ] Test every migration on a current copy of production, or on a Neon or PlanetScale branch, never on a weeks-old copy.
- [ ] With Neon, keep dev, staging and prod branches: reset dev daily, keep staging mirroring prod, and never touch prod directly.

### Backups & recovery
- [ ] Turn on point-in-time recovery (continuous WAL archiving) instead of relying on nightly snapshots. Most managed databases offer it; Supabase has it on paid plans.
- [ ] Test restores into a clean environment at least quarterly (monthly is better): check the tables, check the app runs, and time and document the steps. A cron job can do the test restores automatically.
- [ ] Set RPO (how much data you can lose; daily backups can lose 24 hours) and RTO (how long you can be down) from business cost, and measure against them.
- [ ] Keep backups offsite or in another region, and define a backup schedule and retention policy, which the AI won't add unless asked.

### Sync, events & concurrent edits
- [ ] Use Change Data Capture so inserts, updates and deletes emit events in real time instead of polling. Route events by type to each consumer, and send failed deliveries to a dead-letter queue.
- [ ] Choose a conflict strategy before coding. Last-write-wins is fine for settings and profiles but silently drops changes on shared documents.
- [ ] Use CRDTs (such as Yjs with Supabase Realtime or WebSockets) or operational transforms for collaborative editing, and event sourcing (immutable timestamped events) for structured business data.

### When to migrate platforms
- [ ] Before migrating, ask whether the current platform can handle 10x the load with optimization. If it can, stay.
- [ ] Migrate when a feature you need is architecturally impossible, or when workaround hours times your rate exceed the migration cost, not out of frustration. Keep a running total of what poolers, caches and workarounds cost.
- [ ] Once you decide, migrate quickly, with milestones, rollback points and verification checklists. Slow migrations cost the most.

## Scaling & performance

### Diagnosing a slow database
- [ ] When the database slows down, check connections first, then queries, then reads versus writes, and only then add hardware or replicas.
- [ ] Add a connection pooler (PgBouncer, Supavisor or the platform's own) before any read replica. Postgres defaults to about 100 connections and runs out fast.
- [ ] On Supabase, connect through Supavisor on port 6543 (not 5432) in transaction mode for serverless. Set pool size to the connection limit divided by instances (for example 10 × 3 = 30 against a limit of 32).
- [ ] Use pg_stat_statements to fix the queries with the highest total time (calls × duration), not just the slowest single query, and run EXPLAIN ANALYZE before adding indexes.
- [ ] Index the columns you filter and look up on, measure speed on real data before and after, and check that indexes exist before building features like dashboards.
- [ ] Select only the columns you use (no `SELECT *`), and run independent queries in parallel rather than one after another.
- [ ] Turn on slow query logging with a threshold, and keep a dashboard of slow queries, how often they run and their cost.

### Replicas, partitioning & sharding
- [ ] Read replicas only help read load. For write contention, use queues, background jobs and batched writes.
- [ ] Put read replicas near users (reads are about 80% of traffic), and use edge middleware (Cloudflare Workers or Vercel Edge) to send reads to the nearest replica and writes to the primary.
- [ ] Send a user's reads to the primary for a short window after they write (read-your-writes), and fall back to the primary when replica lag passes a threshold (for example 10s).
- [ ] Scale big Postgres in this order: indexes, then native partitioning (by month or region), then sharding only when monitoring proves you need it.
- [ ] Shard by tenant: give big tenants their own database and pool small ones together, or use Citus with tenant_id as the distribution column.

### Background jobs & queues
- [ ] Return the response as soon as the critical step (such as payment) succeeds, and move emails, receipts and reports to a background queue.
- [ ] Retry failed jobs automatically so a failed email never undoes a checkout. Log every failure and alert on spikes.
- [ ] Push expensive AI calls through a queue so bursts don't hit OpenAI or Stripe rate limits and fail silently for some users.

### Caching
- [ ] Before caching, decide how stale each kind of data may be: static info for a long time, blog posts about an hour, and pricing, permissions, inventory and account status never.
- [ ] Invalidate cache entries when the data changes (event-driven), using TTLs mainly for static content.
- [ ] Guard against cache stampedes with request coalescing, locking or stale-while-revalidate.
- [ ] Choose the layer to fit the data (in-process memory, Redis or CDN, each with its own TTL), and plan how every layer is invalidated when prices or inventory change.
- [ ] Cache queries that run often but change rarely, and expensive ones (for example an 8-join dashboard), instead of recomputing them on every request.
- [ ] Measure hit rate and database query count after adding a cache. If neither improves, the cache isn't working.
- [ ] Cache static assets in the browser, and cache shared API responses at the CDN. Even a 60-second TTL can cut about 95% of DB hits in a spike, and Vercel and Cloudflare don't do it by default.
- [ ] Serve the frontend from Vercel or Cloudflare Pages (200+ edge locations), and keep hot data in Redis (Upstash, Memcached).
- [ ] Debounce input-driven requests by about 300 ms.

### Load & growth stages
- [ ] Load-test with simulated traffic before launch, turn on autoscaling where offered, and aim to survive at least 100 simultaneous users.
- [ ] Prioritize by stage: 0–1K users is about features and speed, 1K–10K about reliability (monitoring, tests, pooling), and 10K–100K about architecture (sharding, multi-region, cost). Canary deploys matter at about 50K users, not 50.

## Monitoring, incidents & support

### Error tracking, uptime & alerts
- [ ] Install Sentry on the frontend and backend (free tier, about 15 minutes) to get the exact line, user, browser and URL for each exception, grouped by how many users it affects. Better Stack also works.
- [ ] Monitor uptime from outside your infrastructure and from several regions (BetterStack pings every 30–60 seconds and texts you), not with a server checking its own health.
- [ ] Alert on business numbers (payments, sign-ups and checkouts per hour). A healthy server can sit next to zero revenue.
- [ ] Run an automated test of the critical path (sign up, add to cart, check out, pay, confirm) about every 5 minutes.
- [ ] Aim to spot incidents within about 60 seconds, not hours later from a customer email.
- [ ] Watch for silent error rates, third-party timeouts with no alerts, and slow endpoints (such as 18-second responses). Log latency, error rate and cost per request.
- [ ] Add PostHog (free up to 1M events) to see what users click and where they drop off.
- [ ] Add session replay linked to each Sentry error, and flag rage clicks (for example 7 clicks in 3 seconds) as UX failures.

### Logging & audit trails
- [ ] Set up logging before any other hardening: request logs, error tracking, performance metrics and an audit trail.
- [ ] Log structured objects (timestamp, severity, request ID, user ID, action), not `console.log` sentences, and use levels consistently (debug, info, warn, error).
- [ ] Send all logs to one searchable private place, including errors with session, route, inputs and stack trace.
- [ ] Pass a correlation/request ID through every service and tie logs, metrics and traces together (OpenTelemetry), so you can follow any failed request end to end in under 60 seconds.
- [ ] Keep an audit trail of sensitive actions (plan upgrades, email changes, deletions, permission changes) to settle "I didn't authorize that" disputes.

### SLOs & error budgets
- [ ] Do the downtime math: 99% allows about 3 days 15 hours a year, 99.9% about 8h 45m, and 99.99% about 52 minutes.
- [ ] Set error budgets per critical endpoint, starting with your top three revenue endpoints (for example 0.1% per 30 days for payments). Ship while within budget, and freeze deploys to fix reliability when it's burning.
- [ ] Alert on burn rate (for example 50% of the budget gone in 48 hours), not only when the budget runs out.
- [ ] Rank incident severity by business impact (users affected, failed transactions, lost revenue). A checkout outage outranks a docs outage.

### Incidents & communication
- [ ] Write a runbook for each failure scenario while things are calm (hosting dashboard, database, deploy logs, rollback), so nobody relies on memory at 3 AM, and make sure the system pages you before customers notice.
- [ ] Prepare a post-mortem template before your first incident: what happened, impact, root cause, the fix, prevention.
- [ ] Hold a blameless post-mortem within 48 hours of every incident (scheduled automatically), and keep a searchable incident library.
- [ ] Host the status page on a separate host or domain, announce planned maintenance in advance, and keep outage templates ready (regular updates, subscriber emails, a rough ETA).

### Support
- [ ] Have the AI write the support playbook along with each feature (password reset failures, expired tokens, locked accounts, failed charges, missed webhooks).
- [ ] Set up three support tiers before your first customer: automated fixes for known issues (60–70% of volume), assisted triage where an agent packages context and escalates to you, and an incident-response playbook for security or data-integrity events.
- [ ] Connect a support agent to production alerts so it fixes known issues and escalates unknown ones with context and a recommendation.
- [ ] Let AI handle routine support (resets, known fixes, config errors), and send billing disputes, disguised feature requests and repeatedly unhappy customers to a person.
- [ ] Tag tickets by root cause, not symptom, and sort them into user error (fix UX), platform error (add monitoring) or business-logic error (fix the spec). A cause that shows up 3 times in a week is an engineering bug.
- [ ] Spend 30 minutes a week grouping the past 7 days of tickets by root cause.

## SEO & discoverability

### Getting crawled & indexed
- [ ] Serve a `sitemap.xml` listing every public page (and only public pages), and point to it from `robots.txt`.
- [ ] Check `robots.txt` doesn't block pages you want found. AI builders often leave a blanket `Disallow: /` from development.
- [ ] Remove leftover `noindex` tags and `X-Robots-Tag` headers from public pages, and keep them on login, dashboard and other private pages.
- [ ] Add a `<link rel="canonical">` to every public page so duplicate URLs (www vs apex, `.html` vs clean, preview subdomains, query strings) count as one page.
- [ ] Verify the site in Google Search Console (and Bing Webmaster Tools), submit the sitemap, and check the indexing report for excluded pages.
- [ ] Publish an `llms.txt` at the site root: a short plain-language summary of what you do, with links to your key pages, for AI assistants and search agents.

### On-page basics
- [ ] Give every page a unique `<title>` (about 50-60 characters) and meta description (about 140-160 characters) that say what the page is for.
- [ ] Use exactly one `<h1>` per page.
- [ ] Keep headings in order (h1 → h2 → h3) without skipping levels; style with CSS instead of picking a heading tag for its size.
- [ ] Give every meaningful image descriptive alt text, and use `alt=""` for decorative ones.
- [ ] Add schema.org structured data (JSON-LD) that fits the page: Organization, WebSite, FAQPage, Product, Article, JobPosting, LocalBusiness.
- [ ] Use short, readable, lowercase URL slugs with hyphens (`/pricing`, not `/page?id=3` or `/Pricing.html`), and redirect old URLs with 301s when you change them.
- [ ] Add an Open Graph image (1200×630) plus `og:title`, `og:description` and Twitter card tags so shared links show a proper preview.

### Links
- [ ] Link related pages to each other with descriptive anchor text, so no public page is an orphan only reachable from the sitemap.
- [ ] Find and fix broken internal and external links, and return a real 404 status (not a 200) for missing pages.

### Speed, mobile & HTTPS
- [ ] Compress and resize images (WebP or AVIF, sized to how they're displayed), lazy-load below-the-fold images, and set width and height to avoid layout shift.
- [ ] Check Core Web Vitals (LCP, INP, CLS) with PageSpeed Insights on mobile, and fix render-blocking scripts, fonts and CSS in the `<head>`.
- [ ] Test every public page at phone width: no horizontal scroll, readable text, tap targets big enough.
- [ ] Force HTTPS everywhere: redirect HTTP to HTTPS, pick one of www or apex and redirect the other, and turn on HSTS.

## Email & domain
- [ ] Set up SPF and DKIM on your sending domain so Gmail and Outlook don't treat receipts as spam.
- [ ] Send transactional mail (receipts, resets) from a different domain than marketing mail.
- [ ] Monitor inbox placement and spam complaint rates. "Delivered" only means the server accepted the mail.
- [ ] Rate-limit everything that sends email (reset links, magic links) to protect your sender reputation.
- [ ] Strip HTML from and escape all user input before it goes into email templates (for example Resend), and render it as plain text, never through the raw-HTML path. Otherwise a name field can become a phishing link sent from your verified domain.
- [ ] Test every email template by typing angle brackets, link tags and script tags into every input. Any clickable result means it can be injected.

## Mobile apps

### Security
- [ ] In Capacitor or other wrapped web apps, move keys and tokens out of localStorage into iOS Keychain or Android Keystore with a secure-storage plugin.
- [ ] Use certificate pinning on all API calls.
- [ ] Treat incoming deep links (login callbacks, password resets, payment confirmations) as untrusted: confirm where they came from and that they are signed, since another app can claim the same URL scheme.
- [ ] Encrypt sensitive fields in API payloads on top of TLS.

### App Store submission
- [ ] Include a privacy manifest (Apple requires one) and, if the app uses AI, declare how it handles user data (enforced since November 2025).
- [ ] Run a security, privacy and edge-case crash check before submitting. About 25% of apps are rejected (about 15% for privacy).

### Deep links
- [ ] Make shared links open in the app: host `apple-app-site-association` (no redirects) for iOS Universal Links, and `assetlinks.json` plus manifest intent filters for Android App Links. In Expo, use expo-linking.
- [ ] When the app isn't installed, send the link to the same content on the web with a smart install banner (Expo Router can handle this).

## AI features & agents

### Agent governance
- [ ] Decide what each agent may do before it starts, and require human approval before it touches live data, customer records or production.
- [ ] Put automated guardrails around agents: scoped credentials, network egress controls, and gates that reject out-of-bounds actions. A person clicking "approve" isn't enough on its own.
- [ ] Log every agent tool call (what it read, changed or called, and when) to an append-only audit trail, with a separate log per agent session.
- [ ] After building an agent, plan how you'll secure, monitor and scale it and handle its failures. Build tutorials skip all of this.

### Multi-agent design & memory
- [ ] Start with the orchestrator pattern: one central agent calls sub-agents and combines their results.
- [ ] Move to the conductor pattern (agents handing off to each other) only for specific sub-workflows, and only when data shows the orchestrator is the bottleneck. Circular agent calls take weeks to debug.
- [ ] For short-term memory, keep a rolling buffer: recent turns verbatim, older turns summarized.
- [ ] For long-term memory, embed only facts that change behavior (preferences, decisions) in a vector store (pgvector, Pinecone, Weaviate) and retrieve what's relevant. If the agent invents memories, fix retrieval.

### Data access & RAG security
- [ ] Put an authorization layer between the LLM and your data, and limit the model's database access to the current user's records, never whole tables, whatever the prompt says.
- [ ] Tag every embedded document with owner and permissions, filter vector retrieval by the requesting user, and check every source chunk before sending the answer.
- [ ] Scan uploaded documents for prompt-injection patterns before they are embedded.
- [ ] Filter model output before users see it: strip system instructions, internal details and other customers' data.

### Output validation & fallbacks
- [ ] Validate every model response (schema, length, banned content) before it reaches the frontend.
- [ ] When validation fails, retry with the reason added to the prompt (for example "stay under 200 words"), at most 2–3 times, then fall back to a simpler model, a cached answer or a person. Never show a raw error or blank screen.
- [ ] Set up a model fallback chain that switches providers automatically if the primary goes down.
- [ ] Put a gateway (AWS API Gateway, Cloudflare API Shield) in front of AI endpoints to reject requests without a valid key, cap payload and context size, and check the schema before any tokens are spent.
- [ ] Tag each AI request with the user ID and enforce daily and monthly token caps per tier at the gateway (for example 500 tokens a day free, 50,000 pro).

## AI models, prompts & costs

### Estimating & tracking spend
- [ ] Estimate the bill before launch: cost per call × users × calls a day (2¢ × 1,000 × 10 ≈ $14K/month).
- [ ] Set hard spending limits with each AI provider: an alert at 70% of budget and an automatic cutoff at 90%.
- [ ] Track token use per feature, endpoint and user action so you know what eats your margins.
- [ ] Review AI spend weekly by model, workflow and token type (input, output, cache read, cache write, as in GitHub's usage report), with a budget per workflow and alerts on spikes.
- [ ] Add a token-cost estimate to CI and flag changes over budget, such as a prompt edit that doubles context size.
- [ ] Re-check model pricing whenever new frontier models come out, because token and cache prices change.

### Model routing
- [ ] Use the cheapest model that does the job in production. Route about 80% of calls (FAQs, formatting, parsing, classification, boilerplate) to cheap models (Haiku, GPT-4o mini), mid-level work to Sonnet-class, and hard reasoning, security review and architecture to frontier models. This cuts average cost by about 70%. AT&T moved 40–70% of traffic, saved 56% and lost about 2% quality.
- [ ] Route with a short classifier (about 10 lines, scoring token count, task type and reasoning depth) at the API gateway, so the app calls one endpoint.
- [ ] Run your eval suite against each tier weekly, and move traffic to the cheap tier where it matches quality (for example on 85% of requests).

### Caching & batching
- [ ] Turn on prompt caching for repeated system prompts and context. Without it you can pay about 4x more, and low cache-read numbers mean you're paying full price for repeats. Cloudflare AI Gateway users see about 40% hit rates.
- [ ] Add a semantic cache (Upstash Vector, Pinecone) that reuses answers to similar queries from the last 24 hours. Hit rates of 40–60% and savings of about 30–40% are typical.
- [ ] Batch prompts and documents (for example 10 docs per call), and send non-urgent work to provider batch APIs, which cost up to 50% less.
- [ ] Combine caching, batching and model tiering, in that order, for a 70–90% total cost reduction without losing quality.

### Evals
- [ ] Use a second model as judge to score accuracy, tone, safety and schema compliance against pass/fail thresholds, and run it in CI on every push instead of exact-match assertions.
- [ ] Build the regression suite from real failures: add every bad response a user reports as a test case.
- [ ] Run the same inputs across models and prompt versions to prove a change actually improved quality.

### Vendor independence & self-hosting
- [ ] Put vendor AI and agent frameworks behind your own interface (AWS Bedrock Agents became "Classic" and was replaced by AgentCore), and plan moves off deprecated services on a timeline, not in an emergency.
- [ ] Avoid depending on one AI vendor. Open-weight models on hardware you control protect you from price changes, terms-of-service changes and shutdowns.
- [ ] A cheap self-hosted stack: Ubuntu + Docker, Postgres, Redis, an open-weight model (such as Qwen) on vLLM behind FastAPI, and Cloudflare at the edge. Keep the model behind your own endpoint so it can be swapped.
- [ ] For local inference hardware: a Mac Studio (64–128 GB) handles small and mid-size models, one RTX 3090 (24 GB) about 13B, and two about 30B. Weigh a year of frontier API spend against buying hardware.
- [ ] Treat your proprietary data as your edge. Consider fine-tuning open-weight models on your own infrastructure instead of sending everything to outside providers.

### Protecting published prompts
- [ ] Hide a unique callback URL (an OAST canary) in each published prompt, so a copied prompt calls home when an AI runs it. Use the hit patterns to decide which prompts to keep paid or private. This detects copying but can't prevent it.

## Testing & quality
- [ ] Have the AI write tests in the same session as each feature, including failure cases (for login: wrong password, lockout, logout).
- [ ] Run tests on every commit and fail the build if coverage drops below 60%. Run fast unit tests on every push and full integration tests on merges.
- [ ] Use unit tests (such as Vitest) and end-to-end tests (such as Playwright) to find unhandled errors before users do.
- [ ] Break the app on purpose: blank required fields, apostrophes and emoji, 10,000 characters in a 50-character field, dozens of submit clicks (double submissions), going back mid-request, and uploads of about 200 MB.
- [ ] Test on the weakest hardware you can find: old Android phones, Safari, tiny screens, slow mobile data. BrowserStack's free tier or spare phones are enough.
- [ ] Test mobile layouts and mobile uploads specifically (about 40% of users are on mobile).
- [ ] Have 5 strangers from your target audience (not friends or family) use the app while you stay silent. If they're stuck after 30 seconds, it isn't ready.
- [ ] Ship the unexciting basics: error messages, loading and empty states, password reset and terms pages.
- [ ] Production frontends also need responsive layouts, accessibility, speed on slow connections, no memory leaks, offline handling and a small bundle.
- [ ] Treat testing as a money decision: compare the customers lost (acquisition cost × users hitting a broken flow) with the few hours a test suite takes.

## Compliance & legal

### Legal documents, entity & insurance
- [ ] Before taking payments, have terms of service, a privacy policy, a DPA (if any third party touches user data), a refund policy and an MSA. Have the AI draft them from your real data flows, not a template, then pay for a legal review (a few hundred dollars).
- [ ] At the very least, generate a privacy policy (Termly or privacypolicies.com, about 10 minutes) covering what you collect, store and delete, and adapt a ToS with liability limits, conduct rules and dispute resolution.
- [ ] Make sure the privacy policy matches what the app actually collects. Keep an inventory of all personal data (including IPs in logs, device fingerprints and location) and every third party it goes to.
- [ ] Have a DPA ready if you process data for other businesses, and put SLAs, uptime guarantees and response times in your MSA with business customers.
- [ ] Before taking revenue, form an entity (such as an LLC), get an EIN, open a business bank account and write an operating agreement.
- [ ] Get cyber liability insurance (about $200–600 a year for a small SaaS). Insurers will ask about vulnerability scans, access controls and encryption at rest.
- [ ] Read your providers' terms (Supabase, Vercel, Stripe). Their liability is often capped at about one month of fees, so plan for bigger losses yourself.

### Privacy & data deletion (GDPR / CCPA)
- [ ] Your obligations depend on where users live, not where you are. One EU or UK user brings in GDPR (and the EU AI Act), and one California user brings in CCPA. Geofencing and ToS exclusions don't reliably get you out.
- [ ] Get informed consent in plain language before sharing data. A banner with only an accept button isn't enough.
- [ ] Before the first request arrives, map every table linked to a user (orders, messages, uploads, payments, sessions, tickets) and build a deletion pipeline that also covers analytics tools, logs and backups.
- [ ] Offer an in-app delete-my-data button and an opt-out.
- [ ] Answer GDPR access and deletion requests within one month of receiving them (this can be extended by up to two more months for complex or numerous requests, but you must tell the user within the first month). Soft-deleting is fine: deactivate the account at once, then hard-delete automatically, making sure the full deletion finishes inside that one-month window. Be able to produce a report of everything you held and confirm removal.
- [ ] Separate data the user controls from records the law requires you to keep. On deletion, anonymize the first and move the second to a locked retention layer with an expiry date.
- [ ] Build a retention schedule per industry, state and record type (finance, health, tax and some legal records need up to 7 years), and log what was kept or anonymized and when its clock ends.
- [ ] Have a breach-notification plan. Under GDPR, report a personal-data breach to the data protection authority within 72 hours of becoming aware of it, and tell affected users without undue delay if it puts them at high risk. HIPAA and financial rules may also apply.
- [ ] Keep a 90-day compliance calendar with review dates and regulation sources for every place your users live (cookie consent, data residency, privacy law changes).


### AI regulation
- [ ] Label all AI-generated output clearly, in a way users can't remove: watermarks on media, disclaimers on text, metadata on images. The EU AI Act transparency rules apply from Aug 2, 2026.
- [ ] Get explicit opt-in (never a pre-checked box) before any user photo, text or other input is sent to an AI model.
- [ ] Keep a tamper-proof log of every AI generation (timestamp, model, triggering input) for audits.
- [ ] Follow other new AI laws too (such as California SB 942) and check your product against them before you are forced to retrofit.

### Healthcare (HIPAA)
- [ ] Encrypt PHI at rest and in transit everywhere, including backups, logs and exports, and keep PHI out of application logs.
- [ ] Log every access to PHI (who, when, from where, what they did).
- [ ] Use HIPAA-compliant hosting (and PIPEDA-compliant, if relevant), and sign a BAA with every third party that can see PHI, including hosting, email and analytics.
- [ ] After any provider migration, re-check that every service touching PHI is still covered by the BAA. Coverage is per named service.

### Sales tax
- [ ] Map your sales-tax nexus: group customers by state and compare with each state's thresholds (for example $100k in revenue, 200 transactions, or the first dollar for digital goods).
- [ ] Turn on tax collection at checkout (for example Stripe Tax) and keep a filing calendar for every state where you have nexus.

### SOC 2 & enterprise security reviews
- [ ] If you sell to enterprises, start collecting SOC 2 evidence (access controls, logging, incident response, vendor management) from day one. The audit takes 3–6 months, and your controls need a history.
- [ ] Expect SOC 2 demands once contracts pass about $50k. A platform like Vanta (about 1,400 automated tests) can get you ready in 4–8 weeks for about $10k, versus about $50k and 6–12 months the traditional way.
- [ ] Use a compliance platform (Vanta, Drata, Secureframe; from about $200/month rather than about $25k of consulting) connected to AWS, GitHub and Google Workspace to gather evidence and flag drift.
- [ ] Automate the evidence: quarterly access reviews, monthly vulnerability scans (Dependabot), restore tests by cron, and patching within 30 days.
- [ ] Get SOC 2 Type 1 first (about 60 days), then start the 6–12 month observation period for Type 2.
- [ ] Prepare for security questionnaires before sales meetings: encryption at rest and in transit, vulnerability scans, last pen-test date, incident response plan, data location.
- [ ] Audit production, fix the findings, get an external pen test (or Burp Pro), then audit again to confirm the fixes held. Keep both reports for procurement.
- [ ] Publish a security page covering encryption, audits, pen-test schedule, incident response and how to report vulnerabilities.
- [ ] Write down your "customer ceiling": the largest customer and the compliance level you can serve today, and what must change to go higher.

## Business, product & pricing

### Positioning & finding customers
- [ ] Find your first customers where people complain about the problem (niche Facebook groups, Reddit, industry events), not among your followers.
- [ ] Be able to state your product as one problem-and-solution sentence.
- [ ] Base your ideal customer profile on your own background and advantages, and have the AI find the market segment where they count most.
- [ ] Build a competitor-analysis framework, then join (and pay for, if needed) the communities where your customers gather, to learn rather than pitch.
- [ ] Before writing code, write a positioning document (who you serve, what you deliver, how you differ, where you compete) and base all content, landing pages and sales on it.
- [ ] Focus on one problem, one market and one channel. One product converting at 5% beats ten with no revenue.
- [ ] Execution, distribution and speed are the moat, not the idea. Get it into customers' hands fast.
- [ ] Launch in stages: open to the waitlist first, watch engagement, then announce publicly.
- [ ] Treat launch day as a hypothesis. Watch real usage for about 100 days and change course based on data.
- [ ] Let data on real behavior (community activity, traffic, purchases) decide what you build next, even if it changes what you sell.
- [ ] Build a standalone social-proof page with only your strongest reviews, each tied to a specific product.
- [ ] Investors (angels, VCs, PE, bankers) won't sign NDAs, so set up your entity, insurance and proven financials first, and choose carefully who sees your plan.

### Pricing
- [ ] Price from the value of the pain you remove. 3 hours a week saved at $50/hour supports about $150/month, and a $500 tool that stops a $3k/month loss sells itself. Buyers don't care about your stack.
- [ ] Pick a pricing metric that grows with customer value: per user if you save time, per unit if you process data, per generation if you produce output.
- [ ] Make the free tier generous enough to show value but limited enough to give a reason to upgrade (for example 100 vs 500 calls a day).
- [ ] If customers use your product in bursts, consider prepaid credit packs that never expire instead of a subscription, with a bonus on the first purchase in place of demos or free trials.
- [ ] Design the product as a loop (find the issue, fix it, re-check it), with deeper extras as optional add-ons.
- [ ] Offer metered pricing (per action, credits, per outcome) that AI agents can use, not only per-seat human subscriptions.
- [ ] Map the buyer journey from first impression to payment, A/B test the pricing page, and track checkout abandonment as closely as uptime. Version your funnel like code.
- [ ] Build a per-user cost model (including tokens) and a monthly P&L that updates itself from your payment and hosting dashboards, to find customers or tiers that cost more than they pay.

### Costs, vendors & complexity
- [ ] Go in order: validate, sell, turn on paid infrastructure, then scale. Don't start a payment processor with monthly minimums before you have customers; build, sandbox and demo first.
- [ ] Negotiate with vendors: ask for a 60–90 day ramp, waived minimums, usage or pilot pricing, or their unadvertised startup program.
- [ ] Plan for outgrowing free tiers (CI minutes, hosting, database) before it happens mid-sprint.
- [ ] Add complexity (Git Flow, conductor agents, extra databases, Burp) only when real pain calls for it, and be able to justify each piece.
- [ ] A production app has many layers beyond frontend and backend (auth, hosting, CI/CD, security, rate limiting, caching, load balancing, error tracking, disaster recovery). Once you take data or payments, you also carry a software company's duties.

### Deciding what to build
- [ ] Run a feature health audit before building anything new: fix broken features people use, remove broken features nobody uses, then build.
- [ ] Use the support inbox to set priorities. Customers want existing features to work more than they want new ones.
- [ ] Before building a client-specific feature, estimate its full cost (build, testing, maintenance each release, opportunity cost). If yearly maintenance exceeds the contract value, get it funded or rescope it.
- [ ] Handle customizations through configuration, not code forks, and build one properly as a platform feature once three or more clients ask for it.
- [ ] Get AI working inside your own business before deploying it for anyone else.

### Selling custom software
- [ ] Sit with a business owner and go through all the software they pay for. Build only the ~50 features they use, not the ~1,000 they pay for.
- [ ] Operators can prototype the 20% of SaaS features they actually use around their own workflow, then hand it to engineers to harden and deploy, which keeps the data and roadmap theirs and avoids renewal price hikes.
- [ ] Staff-built "shadow AI" prototypes get about 80% of the way. Send them through a production pipeline for security, compliance, error handling, deployment and monitoring.
- [ ] Make one in-house person responsible for auditing internal builds, directing the AI to harden them, and managing outside engineers.
- [ ] Sell with the fee math: $40k a year in third-party fees (for example a 22% delivery-app commission) against about $15k for an owned system. Pitch ownership of customer data, channel and brand.
- [ ] Build once for one business type (a bakery, a salon, an HVAC firm) around its exact rules, then resell to similar businesses nearby.
- [ ] Treat scheduling as a workflow-rules problem (staff skills, zones, drive time, insurance checks, walk-ins, confirmations), not a technology problem.
- [ ] Target small businesses not yet using AI (about 22% adoption versus 32% overall): break their needs into buildable modules and replace bloated SaaS (such as a $2k/month CRM) with the ~20 features they use.
- [ ] Solve problems people already pay for, not platforms nobody asked for. Build custom tools that give one company an edge, and never sell that tool to its competitors.

### Being visible to AI agents
- [ ] Make product pages readable by AI shopping agents (schema.org markup, plain pricing tables, product specs), and publish a spec page listing everything an agent might filter on: price, uptime guarantees, integrations, certifications, data export.
- [ ] Package developer tools, skills and MCPs so an agent can find them, read the docs, check the price and install them without a checkout page or a sales call.
- [ ] Ask ChatGPT, Claude and Gemini to recommend a product in your category, and check whether and how you appear.

### Going global
- [ ] Detect each user's locale and format dates, numbers, currencies and addresses to match. Don't hard-code US formats.
- [ ] Save each user's time zone at signup and send every automated message in their local time.
- [ ] Turn on Stripe multi-currency (about 135 currencies) with automatic conversion at checkout.
- [ ] Plan purchasing-power-parity pricing from the start (Stripe supports it) as a growth lever, not a discount.

## Retention & onboarding
- [ ] Get new users to the core value within 60 seconds of signup, with a guided first run using templates or smart defaults instead of tours or docs.
- [ ] Identify the one core action that predicts retention (such as the first project). Track whether each user does it within 24 hours, and nudge them if not.
- [ ] Measure time-to-"aha" in steps and minutes. If under 40% of new users get there during week one, onboarding needs work.
- [ ] Reveal features as users hit milestones: core workflow in the first session, customization in the second, integrations in the third.
- [ ] Follow up automatically with users who don't return within 48 hours (these emails reportedly convert at 10–15%).
- [ ] Make re-engagement messages about the user's own work (tasks due tomorrow, what's new since they left, features they haven't tried), sent on day 1 and day 7. Never send generic "we miss you" emails.
- [ ] Track weekly use of core features per user, not logins, and flag users who fall below their own baseline (for example 3 missed days in a row) rather than a global threshold.
- [ ] Build retention dashboards by signup-week cohort, and track events on every core feature. A widely unused feature is a discoverability problem.

## Content & audience
- [ ] Start building an audience about 100 days before launch (content, conversations, communities) so you arrive with proof of demand: comments, signups, a waitlist.
- [ ] About 200 days before launch, practice making videos on a separate throwaway account about an unrelated topic to learn framing, hooks and delivery.
- [ ] Build in public, and use early feedback and referrals as social proof at launch.
- [ ] Review content weekly with a dashboard of saves, shares and skips by topic, format and time. Do more of what works.
- [ ] Give away your best work (fixes, frameworks, scripts, prompts) so people save and share it.
- [ ] Reply to every comment and DM, using a priority queue so important ones aren't missed.
- [ ] Use a repeatable format: name a painful problem, give three numbered steps with specific tools, end with a catchphrase.
- [ ] End each video with a specific question for the audience, and ask viewers to share what they fixed.
- [ ] Turn questions from comments and DMs into new videos.

## Career
- [ ] Build skills in security, orchestration and production judgment. New AI security roles quoted: AI Supply Chain Security Engineer ($130–180k), AI SOC Orchestrator ($100–150k), AI Security Specialist ($130–200k), AI Incident Response Orchestrator ($120–180k).
- [ ] Keep building skills with AI coding tools as the industry consolidates.
- [ ] Build credibility with delivered client work and results, not a portfolio of personal projects.
- [ ] Specialize in one industry you know and sell a specific outcome backed by specific proof. Rescuing failed AI projects (being the second builder a client calls) is a strong niche.
- [ ] Domain experts (healthcare, finance, insurance, logistics; even doctors) can direct AI to build production software. Pick one rented system (CRM, scheduling, inventory) and build an owned replacement as a proof of concept and first sale.
