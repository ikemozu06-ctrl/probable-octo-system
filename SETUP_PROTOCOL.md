# AGOI™ Platform — Setup Protocol (read this first)

This guide gets the **whole platform** — World Bank + AfDB + AfCFTA — live on
Streamlit Cloud **without** the folder-drop problem you hit before.

## Why the last uploads failed (so it doesn't happen again)

GitHub's **drag-and-drop web uploader silently drops files and subfolders.**
That is why `6_AfCFTA.py`, `7_Corridor_Simulator.py` and the `agoi/afcfta/` folder
never arrived. It is not your fault and it is not the instructions — the web
uploader is simply unreliable for multi-folder projects.

**The fix: don't use drag-and-drop. Use ONE of the two reliable methods below.**

---

## METHOD A — GitHub Desktop (recommended, no command line)

This is the most reliable option and needs no typing of commands.

1. **Install GitHub Desktop** → https://desktop.github.com (free).
2. Sign in with your GitHub account.
3. **File → New Repository.**
   - Name: `agoi-platform`
   - Local path: pick any folder on your computer.
   - Click **Create Repository**.
4. Open the folder it just created (GitHub Desktop shows a "Show in Explorer/Finder" button).
5. **Unzip `agoi-platform.zip`. Copy EVERYTHING from inside the unzipped
   `agoi-platform` folder into your new repository folder** — the `agoi/` folder,
   the `app/` folder, `requirements.txt`, all of it. (Copy the *contents*, not the
   outer folder.)
6. Back in GitHub Desktop, you'll see all the files listed as changes. In the
   bottom-left "Summary" box type `initial platform` and click **Commit to main**.
7. Click **Publish repository** (top). Uncheck "Keep this code private" if you want
   Streamlit's free tier. Click **Publish**.

Every file and folder is now in GitHub, guaranteed — no drops. Skip to **Deploy**.

---

## METHOD B — Git command line (if you prefer terminal)

```bash
# 1. Unzip agoi-platform.zip, then cd into the folder that contains
#    the `agoi/` and `app/` folders:
cd path/to/agoi-platform

# 2. Create the repo and commit everything at once:
git init
git add .
git commit -m "initial platform"
git branch -M main

# 3. Create an empty repo on github.com first (no README), copy its URL, then:
git remote add origin https://github.com/<your-username>/agoi-platform.git
git push -u origin main
```

`git add .` takes **every** file and folder in one shot — this is exactly why the
command line never drops files the way the web uploader does.

---

## Verify the repo is complete (30-second check)

On github.com, open your repo and confirm this structure exists. **If any folder
is missing, the app will break** — so check before deploying:

```
agoi-platform/  (or your repo root)
├── app/
│   ├── streamlit_app.py
│   ├── _shared.py
│   └── pages/
│       ├── 1_Country_Profile.py
│       ├── 2_Africa_Map.py
│       ├── 3_Pillar_Comparison.py
│       ├── 4_Scenario_Tool.py
│       ├── 5_Export_and_Methodology.py
│       ├── 6_AfCFTA.py                 ← must be here
│       └── 7_Corridor_Simulator.py     ← must be here
├── agoi/
│   ├── config.py  registry.py  pipeline.py
│   ├── data_sources/   (worldbank.py, afdb.py, demo.py)
│   ├── scoring/        (engine.py, normalization.py)
│   ├── geospatial/     (iso.py)
│   └── afcfta/         ← must be here (config, elasticity, engine, data)
├── requirements.txt
├── runtime.txt
└── .streamlit/config.toml
```

Click into `app/pages/` — you must see **7 files**. Click into `agoi/afcfta/` —
you must see **5 files**. If yes, you're good.

---

## Deploy on Streamlit Cloud

1. Go to **https://share.streamlit.io** → sign in with GitHub.
2. **New app** → **Deploy a public app from GitHub.**
3. The three inputs it asks for:
   - **Repository:** `<your-username>/agoi-platform`
   - **Branch:** `main`
   - **Main file path:** `app/streamlit_app.py`
     *(If your files sit under an extra `agoi-platform/` folder in the repo, use
     `agoi-platform/app/streamlit_app.py` instead. The verify step above tells you which.)*
4. Click **Deploy**. First build takes a few minutes.
5. When it loads, the sidebar should list **all 7 pages**, including **AfCFTA** and
   **Corridor Simulator**.

---

## Turn on live data (optional, after it's running)

The app works immediately in **Demo** mode. For real data:

- **World Bank** — works automatically in Live/Mixed mode once deployed (Streamlit
  Cloud has open internet; no key needed).
- **AfDB** — needs a free key. Register at **developer.iatistandard.org**, subscribe
  to the Datastore API, copy your Subscription Key. Then in your app:
  **Settings → Secrets** → paste:
  ```
  IATI_API_KEY = "your-key-here"
  ```
  Save. The app reboots and AfDB data flows. The home-page banner shows AfDB status
  either way, so you can always see what it's doing.

---

## If something still breaks

Open the app → bottom-right **Manage app** → the log panel shows the real error.
The two most common:

- **"ModuleNotFoundError: No module named 'agoi'"** → the `agoi/` folder didn't
  upload fully, or the Main file path is wrong. Re-check the verify step.
- **A page missing from the sidebar** → that page file isn't in `app/pages/`.
  Re-check `app/pages/` has all 7 files.

Copy the red error text from that log panel if you need help — that's the one
thing that makes any remaining issue quick to fix.
