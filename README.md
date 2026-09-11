# Frillz Ecommerce - Deployment Repo

Deployment artifacts for [frillz.lk](https://www.frillz.lk) (frontend) and [api.frillz.lk](https://api.frillz.lk) (backend), hosted on cPanel/LiteSpeed at register.lk.

## Structure

- `backend/` - Node.js/Express API server (bundled `server.js`, deployed to `~/backend` on the server, run via cPanel's Node.js App / Passenger)
- `frontend/` - Static frontend build (deployed to `~/public_html`)

## Deployment

Pushing to `main` triggers `.github/workflows/deploy.yml`, which FTP-syncs `backend/` and `frontend/` to the server. The backend deploy also touches `backend/tmp/restart.txt` with a fresh timestamp on every run, which tells Passenger (used internally by cPanel's Node.js App) to restart the app on its next request — no cPanel API access needed.

Environment-specific files are **not** managed by CI and must be configured directly on the server:

- `frontend/config.js` (see `config.js.example`) - runtime `window.ENV.API_URL`
- `frontend/.htaccess` (see `.htaccess.example`) - SPA routing + cache headers
- Backend environment variables - set via cPanel's "Setup Node.js App" UI (not a `.env` file)

## Required GitHub secrets

| Secret | Purpose |
|---|---|
| `FTP_SERVER` | FTP host |
| `FTP_USERNAME` | FTP account username |
| `FTP_PASSWORD` | FTP account password |
