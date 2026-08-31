# Competitor Update Tracker

Weekly job that checks TikTok and Bigo Live's App Store / Google Play
release notes, plus TikTok's newsroom RSS feed, and writes a Markdown
digest of anything new since the last run.

## What it checks out of the box
- App Store version + release notes (official iTunes Lookup API — no key needed)
- Google Play version + "What's new" notes
- TikTok Newsroom RSS feed (verify this URL still resolves — see note below)

## Setup (GitHub Actions — free, no server needed)

1. Create a new **public or private** GitHub repo (e.g. `competitor-tracker`).
2. Upload these files to the repo, keeping the folder structure:
   - `competitor_tracker.py`
   - `requirements.txt`
   - `.github/workflows/weekly-digest.yml`
3. Go to the repo's **Settings → Actions → General → Workflow permissions**
   and set it to **"Read and write permissions"** (needed so the job can
   commit the digest back to the repo).
4. Go to the **Actions** tab, select "Weekly Competitor Digest", and click
   **Run workflow** once to test it.
5. Check the `digests/` folder in your repo (or the Actions run's
   **Artifacts** section) for `digest-<date>.md` — that's your downloadable
   weekly file.

It will then run automatically every Monday at 06:00 UTC (edit the `cron`
line in the workflow file to change this).

## Adding more competitors or sources
- **App Store**: find the app's numeric ID in its App Store URL
  (`apps.apple.com/.../id123456789`) and add it to `ITUNES_APPS`.
- **Google Play**: find the package ID in the Play Store URL's `id=`
  parameter and add it to `PLAY_STORE_APPS`.
- **RSS**: add any official blog/newsroom feed URL to `RSS_FEEDS`. Not
  every competitor has one — check their newsroom/press page footer for
  an RSS icon, or use a feed-generator service for pages that don't
  publish one natively.

## Known limitations (be aware of these)
- **Monetization/pricing changes and promo campaigns are NOT auto-tracked.**
  These are usually in-app, sometimes region-locked, and not reliably
  exposed via any public API — this still needs a periodic manual check
  inside the apps.
- I could not test-run this script live (no network access while building
  it), so **please run it once manually first** to confirm the RSS feed
  URL and both app IDs resolve correctly before relying on the schedule.
- `google-play-scraper` is an unofficial (but actively maintained) library
  reading public Play Store pages — Google could change its page structure
  and break it; if a run starts erroring, check for a library update.
