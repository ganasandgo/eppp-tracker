# EPPP Pass Rate Tracker — Setup Guide

A dashboard that tracks California EPPP pass rates, pulled straight from the
Board of Psychology's own published statistics. Once it's set up, **it
updates itself** — no copy-pasting numbers, ever.

**Important to know up front:** the Board only publishes new numbers in
monthly PDF updates (no live feed exists). So "automatic" here means *the
tracker checks for new numbers every week, and instantly grabs anything new
the moment the Board posts it* — not second-by-second real-time.

---

## What you're getting

- [ ] A nice-looking dashboard (`index.html`) — works as a standalone preview right now, and as a live site once deployed
- [ ] A **repeat test-taker transparency section** — the board never publishes the pass rate for people retaking the exam, but it can be derived from their own totals (all candidates − first-timers). The dashboard computes and displays it automatically, every update
- [ ] A script that re-reads the Board's PDFs and rebuilds the data
- [ ] A free, scheduled robot (GitHub Actions) that runs that script every Monday — and any time you want, via one click
- [ ] Instructions below to put the live version on your own website

---

## Part 1 — One-time GitHub setup (~10 minutes)

You only do this once. After this, everything runs by itself.

### Step 1: Create a free GitHub account
- [ ] Go to [github.com/signup](https://github.com/signup) if you don't already have an account
- [ ] Verify your email when it asks

### Step 2: Create a new repository
- [ ] Click the **+** icon (top right) → **New repository**
- [ ] Name it something like `eppp-pass-rate-tracker`
- [ ] Set it to **Public** ⚠️ *(this matters — public repos get unlimited free automation minutes; private repos have a monthly limit)*
- [ ] Click **Create repository**

### Step 3: Upload the project files
- [ ] Unzip the `eppp-tracker.zip` file I gave you, on your computer
- [ ] On your new repo's GitHub page, click **uploading an existing file**
- [ ] Drag in **everything** from the unzipped folder — including the hidden `.github` folder (your file manager may need "show hidden files" turned on; on Mac that's `Cmd+Shift+.`, on Windows it's in the View tab)
- [ ] Scroll down, click **Commit changes**

> 💡 If your browser/file manager won't let you drag a folder, ask me — I can give you the exact `git` command-line steps instead, which always work.

### Step 4: Turn on GitHub Pages (this gives you the live URL)
- [ ] In your repo, click **Settings** (top tab)
- [ ] Click **Pages** (left sidebar)
- [ ] Under **Branch**, choose `main` and `/ (root)`, then **Save**
- [ ] Wait ~1 minute, refresh the page — you'll see a green box with your live URL, like:
  `https://yourusername.github.io/eppp-pass-rate-tracker/`
- [ ] **Save that URL** — you'll need it for Part 2

### Step 5: Run the automation once, by hand, to confirm it works
- [ ] Click the **Actions** tab in your repo
- [ ] Click **Update EPPP pass rate data** (left sidebar)
- [ ] Click **Run workflow** → **Run workflow** (green button)
- [ ] Wait ~30 seconds, refresh — you should see a green checkmark ✅
- [ ] If it's red ❌, click into it to see the error, and feel free to paste it back to me

That's it — from here on, it checks for new data **every Monday automatically**, forever, for free.

---

## Part 2 — Put it on your website

Your dashboard lives at the GitHub Pages URL from Step 4. Now you just need
to embed that page on your site. The easiest way on almost any platform is
an `iframe` — a little window that shows another page inside yours.

```html
<iframe
  src="https://yourusername.github.io/eppp-pass-rate-tracker/"
  style="width:100%; height:1400px; border:none;"
  loading="lazy">
</iframe>
```

Swap in your real URL from Step 4, then use whichever applies to you:

- [ ] **WordPress:** Add a **Custom HTML** block, paste the code above
- [ ] **Squarespace:** Add a **Code Block**, paste the code above
- [ ] **Wix:** Add an **Embed → HTML iframe** element, paste the code above
- [ ] **Plain/custom HTML site:** Paste it directly into your page's HTML where you want it to appear

If the dashboard looks cut off, just increase the `height:1400px` number
until it fits — there's no harm in it being a little tall.

---

## Keeping it running

You don't need to do anything else. A few honest notes:

- [ ] The data refreshes **weekly**, not the instant the Board updates their PDF — usually within a few days of them posting it
- [ ] You can trigger an instant refresh anytime from the **Actions** tab → **Run workflow** (same as Step 5)
- [ ] Year-to-date numbers (the current year) are provisional and may shift slightly as the Board finalizes them — that's the Board's data behaving normally, not a bug
- [ ] If GitHub ever changes how the PDFs are formatted, the script might need a small update — let me know and I can fix it

---

## What's in the zip, if you're curious

| File | What it does |
|---|---|
| `index.html` | The dashboard itself |
| `data/eppp_stats.json` | The actual numbers, in a simple format |
| `scripts/update_data.py` | Re-reads the Board's PDFs and rebuilds the data |
| `.github/workflows/update-data.yml` | The scheduled robot that runs the script weekly |
| `requirements.txt` | The two small Python libraries the script needs |
