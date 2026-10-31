# ROAS Master

A simple admin workspace starter with a GitHub Pages frontend and a Node.js + PostgreSQL backend.

## Deploy backend on Render
1. Create a PostgreSQL database in Render.
2. In Render, choose **New → Blueprint** and connect this repository. It reads `render.yaml`.
3. Add the database connection string as `DATABASE_URL`; set `API_PUBLIC_URL` to your Render service URL. Keep generated `JWT_SECRET`, `SETUP_KEY` and `TELEGRAM_WEBHOOK_SECRET` private.
4. Run `schema.sql` once against the database.
5. Call `POST /api/auth/setup` once with JSON: {"setupKey":"YOUR_SETUP_KEY","email":"admin@example.com","password":"at-least-12-characters"}. Then sign in with `POST /api/auth/login`.

## API endpoints
- `GET /api/health`
- `POST /api/auth/setup` (one-time bootstrap)
- `POST /api/auth/login`
- Bearer-token endpoints: `GET /api/overview`, `GET/POST/DELETE /api/clients`, `GET/POST /api/pages`, `GET/POST /api/links`
- `GET /r/:slug` counts a click and redirects
- `POST /api/telegram/webhook` accepts Telegram join-request updates with the configured secret header

## Important
This is a backend starter, not a finished production SaaS. The current static frontend still needs its login and API calls wired to the deployed API. Client-specific user roles and strict per-client data access, campaign-specific Telegram invite attribution, and Meta Ads OAuth/Marketing API are not yet implemented. Do not enter real client data or bot tokens until these are completed and security-tested. Never commit secrets.