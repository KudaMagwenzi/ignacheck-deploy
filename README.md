# Ignacheck — DigitalOcean Deploy

## Option A — App Platform (recommended)
1. Create a **Spaces** bucket in region `fra1` (name: `ignacheck-prod`) and generate **Spaces access keys**.
2. Create a **Managed PostgreSQL** database (or let the spec create a dev DB for tests).
3. In GitHub, confirm both repos exist: `ignacheck-api` and `ignacheck-web` (branch: `main`).
4. In this repo, edit `do-app-spec.yaml`:
   - Replace `<YOUR_GH_ORG>` with your GitHub org/user.
   - If you already created a DB, set `cluster_name` to its name; otherwise remove that line to let DO create a dev DB.
   - Put placeholder values for secrets (you’ll change them in DO UI).
5. In the DigitalOcean Control Panel → **Apps** → **Create App** → **Specify App Spec**, paste the whole `do-app-spec.yaml`.
6. When the app appears:
   - Open **api** component → **Settings → Environment variables** and set secrets:
     - `S3_ACCESS_KEY`, `S3_SECRET_KEY`, `OPENAI_API_KEY`, `JWT_SECRET` (random 64 chars).
   - Deploy.
7. After deploy finishes, copy the **api** default URL (e.g. `https://api-xxxxx.ondigitalocean.app`).
8. Open the **web** component → **Environment variables** and set:
   - `API_BASE` and `NEXT_PUBLIC_API_BASE` to the API URL from step 7. Redeploy.
9. (Optional) Add your custom domains:
   - Web: `app.ignacheck.ai`
   - API: `api.ignacheck.ai`

## Option B — Droplet + docker compose (manual)
Only if you prefer a VPS:
- Provision a Droplet (Ubuntu), install Docker, and run `docker compose up -d` using images from GitHub Container Registry. We already build images in your repos’ Actions. (Skip unless needed.)
