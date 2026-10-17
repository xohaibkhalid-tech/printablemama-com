# ⚠️ DO NOT USE HOSTINGER'S GIT DEPLOY FOR THIS SITE — EVER.

On 16 Oct 2026 Hostinger's native Git auto-deploy wiped an entire WordPress installation TWICE on another site (it deploys the repo over `public_html`, deleting files the repo doesn't own). The Hostinger → Git connection must stay disconnected for ALL sites.

**The ONLY deploy path for printablemama.com is this repo:** push/merge to `main` → GitHub Actions uploads changed files via FTP to `/public_html/`. Nothing is ever deleted on the server (`dangerous-clean-slate` is OFF and must stay OFF).

> ⚠️ **CRITICAL — REPO OUT OF SYNC (as of 17 Oct 2026):** This repo currently holds only ~57 HTML files. The live site has 201 pages (v12.2). **DO NOT trigger a deploy until the repo is fully synced with the live v12.2 build** — otherwise 57 files will overwrite live pages with older/incomplete versions. Sync the full v12.2 ZIP to this repo first, then deploy.
>
> (Roman Urdu: Hostinger ka Git deploy kabhi ON mat karna — ye files ura dega. Sirf is repo se deploy hota hai. AUR — repo abhi adhura hai (57 files vs live 201 pages). Pehle pura v12.2 sync karo, PHIR deploy karna.)

---

## How deploys work

- **Trigger:** any push/merge to `main`.
- **Workflow:** `.github/workflows/ftp-deploy.yml` (SamKirkland/FTP-Deploy-Action@v4.3.5).
- **Secrets** (repo Settings → Secrets → Actions — names only, never values): `FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD` — verify the exact names against the live workflow file before renaming anything.
- **Scope:** uploads new/changed files only. `dangerous-clean-slate: false` — NEVER deletes server files. Excludes `.git*`, `.github/**`, `README.md`. Server dir: `/public_html/`.
- **Concurrency:** only one deploy runs at a time (`cancel-in-progress: true`) — a new push cancels a stale in-flight deploy.
- **After every deploy:** verify https://printablemama.com/ live (HTTP 200 + expected change visible). Green Actions tick alone is NOT proof. The workflow's smoke-test step does this automatically and FAILS the run if the site doesn't respond.

## What this repo is

Static HTML site — printablemama.com (free printable paper templates: coloring pages, calendars, planners, certificates, tools). No build step, no database. What you see in the repo is what goes live.

## Rules

- Never enable Hostinger Git deploy for this site.
- Never set `dangerous-clean-slate: true`.
- Never rename secrets without verifying the live workflow first.
- Keep the repo in sync with the live site — manual FileZilla uploads and repo pushes must not fight each other. Pick ONE deploy path per change.
