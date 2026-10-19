# ✈️ MAWB Cargo Tracking Tool — Netlify Edition (zero installs)

The same bulk MAWB tracker, packaged so **Netlify hosts 100% of it for free**.
Nothing to install on your PC — no Node, no Python. You get a permanent link like
`https://my-mawb-tracker.netlify.app` that works on any computer or phone browser.

**How it works:** the web page runs on Netlify's CDN, and one small serverless
function (`netlify/functions/track.mjs`) makes the calls to
`6ecargo.goindigo.in` — the job your Python script / local server used to do.
The function has **zero dependencies**, so deploys take seconds and there's
nothing to break.

---

## 🚀 Deploy it (about 5 minutes, everything in the browser)

You need two free accounts: **GitHub** (to hold the files) and **Netlify** (to host).
GitHub is used because Netlify builds serverless functions reliably from a Git
repository — plain drag-and-drop deploys don't run a build step.

### Step 1 — Put these files on GitHub
1. Unzip `mawb-tracker-netlify.zip` and open the `mawb-tracker-netlify` folder.
2. Go to **github.com** → sign up / sign in → click **+** (top right) → **New repository**.
3. Repository name: `mawb-tracker` → **Create repository**.
4. On the next page click the **“uploading an existing file”** link.
5. Select **everything inside** the `mawb-tracker-netlify` folder
   (`public`, `netlify`, `netlify.toml`, `package.json`, `README.md`)
   and drag it into the GitHub upload area.
   ⚠️ Drag the *contents*, not the outer folder — `netlify.toml` must end up at
   the top level of the repository.
6. Click **Commit changes**.

Your repository should now look like:
```
mawb-tracker/
├── netlify.toml
├── package.json
├── README.md
├── netlify/functions/track.mjs
└── public/ (index.html, app.js, style.css, vendor/…)
```

### Step 2 — Connect Netlify
1. Go to **app.netlify.com** → **Sign up with GitHub** (easiest).
2. Click **Add new project** (or “Add new site”) → **Import an existing project**.
3. Choose **GitHub** → pick your `mawb-tracker` repository.
4. Leave every setting as-is — Netlify reads them from `netlify.toml`
   (no build command, publish folder `public`, functions `netlify/functions`).
5. Click **Deploy**. Wait ~30–60 seconds.

### Step 3 — Use it 🎉
- Netlify gives you a URL like `https://random-name-123.netlify.app` — open it.
- Click **Sample → 🚀 Start Tracking** to confirm it works, then use your own MAWBs.
- Nice name: **Site configuration → Site information → Change site name**
  (e.g. `my-mawb-tracker`), then bookmark/share the link with your team.
- No PC needs to stay switched on. It also works on your phone browser.

---

## 💰 Cost / free limits

- Each MAWB = one function call that runs ~1 second.
- Netlify's free plan (legacy) includes **125,000 function invocations/month**;
  newer free accounts use a monthly credit pool where function compute is
  billed per GB-hour — a few hundred MAWBs a day uses only a small slice of it.
- Either way: **300 MAWBs/day ≈ comfortably inside the free tier.**
  Check “Usage” in your Netlify dashboard anytime; no credit card is required.
- First request after idle may take ~1 extra second (serverless “cold start”) — normal.

## 🔄 Updating later
Edit any file on GitHub (pencil icon) → commit → Netlify redeploys automatically
in under a minute.

## ❓ Troubleshooting

- **Page loads but badge says “Server offline”** — open
  `https://<your-site>.netlify.app/api/health`; it should show `{"ok":true}`.
  If you get a 404, the function didn't deploy: check the repo structure
  matches the diagram above (especially `netlify.toml` at the top level),
  then **Deploys → Trigger deploy**.
- **A few MAWBs show errors/timeouts** — usually IndiGo being slow; use the
  **↻ Retry these** button. Details appear in **Netlify → Functions → track → logs**.
- **Every MAWB fails** — test one number manually on
  `6ecargo.goindigo.in`. If their site works but the tool doesn't, check the
  function log for the exact error.
- **Want a backup?** The local-PC version (`mawb-tracker`, with `START-TOOL.bat`)
  still works independently whenever you need it.

---

Files in this package:

| File | Purpose |
|---|---|
| `public/index.html`, `app.js`, `style.css` | The tool's web page (identical UI to the local version) |
| `public/vendor/xlsx.full.min.js` | Excel-download library, included so no build step is needed |
| `netlify/functions/track.mjs` | Serverless tracking engine (zero dependencies): GET form → POST → parse `GridViewAwbTracking` |
| `netlify.toml` | Tells Netlify where the page and function live |
| `package.json` | Project marker file (no dependencies to install) |
