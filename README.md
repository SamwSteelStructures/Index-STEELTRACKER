[README.md](https://github.com/user-attachments/files/29723593/README.md)
# SteelTrack — Netlify build (shared backend)

Same app as your live demo, but data is stored in **Netlify Blobs** through a
Netlify Function, so **everyone who opens the site shares one dataset** (not
per-browser anymore). QR codes stay live.

## What's in here
- `index.html` — the app (its storage now talks to `/api/kv`)
- `netlify/functions/kv.mjs` — the shared key-value backend (Netlify Blobs)
- `netlify.toml`, `package.json` — Netlify config

## Deploy it

Because there's now a server-side Function, this is **not** a drag-and-drop
deploy — Functions need a build step. Two ways:

### Option A — Netlify CLI (no GitHub needed)
In a terminal, from this folder:

```
npm install -g netlify-cli      # one time
npm install @netlify/blobs      # adds the storage library
netlify login                   # opens the browser once
netlify deploy --prod           # choose "link to an existing site" -> steeltracker
```

That uploads the app + the Function. When it finishes it prints your live URL.

### Option B — Connect a Git repo
Put this folder in a GitHub repo and, in Netlify, "Add new site → Import from
Git." Netlify installs dependencies and deploys automatically on every push.

## After deploying
- Open the site, add a package — then open it on another device or browser and
  you'll see the **same** data. That's the shared backend working.
- Blobs data persists across deploys, so future updates won't wipe it.
- Back up anytime with the app's **Export Data** button.

## IMPORTANT: no access control
The `/api/kv` endpoint is **open** — anyone with the site URL can read or write
all data. That's fine for an internal, unpublicized demo, but it is **not**
secure for real production data. For real access control (individual logins,
read-only roles, audit trail, file downloads), use the **self-hosted build** —
that version has proper authentication built in.

## Note on your existing demo data
The per-browser (localStorage) demo and this shared build use different storage,
so data doesn't carry over automatically. You're starting fresh here.
