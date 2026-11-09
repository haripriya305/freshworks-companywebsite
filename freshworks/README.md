# FreshWorks — Full-Stack Support Platform (Vercel-ready)

Static frontend (`frontend/`) + Express REST API (`backend/`) deployed together on Vercel.
Login, register, contact form, blog, dashboard and admin panel all work.

## Why login said "Request failed (404)" before
Vercel only hosted the HTML files. There was no `/api` function, so every call to `/api/auth/login`
returned 404. This version adds `api/index.js` + `vercel.json`, which turn the Express backend into a
Vercel serverless function at `/api/*`.

## Deploy on Vercel
1. Push this folder (the one containing `vercel.json`) to GitHub — **repo root must be this folder**.
2. Vercel → Add New Project → import the repo → Framework: **Other** → Deploy (no build command).
3. Project → Settings → Environment Variables → add `JWT_SECRET` (any long random text) → Redeploy.
4. Test: open `https://YOUR-SITE.vercel.app/api/health` → should show `{"status":"ok",...}`.

### Database
* **Demo mode (default)** – no setup. Uses built-in memory store with seeded demo users.
  Data (new registrations, contact messages) resets when Vercel restarts the function.
* **Permanent storage** – create a free MySQL (Aiven, TiDB Cloud, Railway) and add env vars
  `DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_NAME, DB_SSL=true`. Tables and demo data are created automatically.

Demo logins: `admin@freshworks-demo.com / Admin@123`, `dev@freshworks-demo.com / Agent@123`

## Run locally
```bash
npm install
cp .env.example .env      # optional; leave DB_HOST empty for memory mode
npm start                 # http://localhost:5000  (site + API on one port)
```

## Structure
```
api/index.js        Vercel serverless entry
vercel.json         routes /api/* -> Express, serves frontend/
backend/app.js      Express app (routes, auto DB init)
backend/config/db.js  MySQL or in-memory store (auto-selected)
frontend/           HTML/CSS/JS (login, register, dashboard, admin, contact, blog ...)
```
