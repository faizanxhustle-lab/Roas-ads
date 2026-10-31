# ROAS Master

Multi-client dashboard starter: static GitHub Pages frontend + Node.js/Express + PostgreSQL API.

## Deploy the backend (Render Blueprint)
1. Open Render Dashboard → New → Blueprint and connect this repository. It reads `render.yaml` and creates a PostgreSQL database plus the API service.
2. After provisioning, open the API service → Environment. Set `API_PUBLIC_URL` to the service's HTTPS URL. Keep the generated `JWT_SECRET`, `SETUP_KEY`, and `TELEGRAM_WEBHOOK_SECRET` private. Do not put them in GitHub or the browser.
3. The server runs `schema.sql` at startup to create missing tables/indexes.
4. Open the API URL + `/api/health`; it should return `{"ok":true,...}`.
5. Create the first admin from the frontend login page's **First-time admin setup** section. Enter API URL, the private `SETUP_KEY`, email, and a 12+ character password. This endpoint is one-time only.
6. Sign in using the same API URL. The UI saves the API URL locally; the login token stays in session storage and expires after 8 hours.

## Frontend
GitHub Pages is configured for `main /(root)`. After deployment, use:
`https://faizanxhustle-lab.github.io/Roas-ads/`

## Implemented API
- `GET /api/health`
- `POST /api/auth/setup` (one-time bootstrap)
- `POST /api/auth/login`, `GET /api/me`
- Bearer-token: `GET /api/overview`
- Admin: `GET/POST/DELETE /api/clients` (optional client login creation)
- Scoped: `GET/POST /api/pages`, `PATCH /api/pages/:id/publish`
- Scoped: `GET/POST /api/links`, `PATCH /api/links/:id/telegram-invite`
- Admin: `POST /api/telegram/channels`; scoped `GET /api/telegram/channels`
- Public: `GET /r/:slug` click-counting redirect, `GET /p/:slug` published landing page
- `POST /api/telegram/webhook` with Telegram secret header; join request attribution matches exact registered invite-link URL.

## Not production-complete yet
- Meta Ads OAuth/Marketing API and real ROAS/spend reporting are **not connected**.
- Telegram bot webhook registration and campaign invite-link creation are not automatic; configure Telegram with a bot token held only in Render secrets, create a unique invite link per campaign, then save the exact invite URL against its tracking link.
- The current UI has basic management screens; Telegram channel mapping and campaign invite association still need UI workflows.
- Before handling real customers: add audit logs, backups, account recovery, invite-link deduplication, abuse monitoring, and independent security testing. Never commit credentials or bot tokens.
