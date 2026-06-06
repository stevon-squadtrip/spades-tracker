# spades-tracker

A spades score tracker. **The entire app is a single `index.html`** — HTML, CSS, and
JS all inline (~300 KB). No build step, no dependencies, no framework.

- **Live:** https://spades-tracker.vercel.app
- **Repo (private):** https://github.com/stevon-squadtrip/spades-tracker
- **Vercel project:** `annon18s-projects/spades-tracker` — linked, and **auto-deploys on every push to `main`**
- **Local:** `C:\Users\stevo\projects\spades-tracker`

## Updating the app — the main task

The user supplies the newest page one of two ways (handle **both**):
- **A)** Saved directly over the repo file: `C:\Users\stevo\projects\spades-tracker\index.html`
  (in that case `git status` already shows `index.html` modified — skip the copy in step 1).
- **B)** Dropped in Downloads: `C:\Users\stevo\Downloads\index.html` (the default for browser saves).

When the user says **"update it"**, **"update spades"**, **"deploy the new version"**,
**"drop the new index"**, **"push it live"**, or anything similar, do this:

1. Make sure the newest file is the repo root `index.html`. If they used Downloads (B),
   copy it in (skip this if they already saved over the repo file, case A):
   ```powershell
   Copy-Item "C:\Users\stevo\Downloads\index.html" "C:\Users\stevo\projects\spades-tracker\index.html" -Force
   ```
   If unsure which is newer, compare `Get-Item` LastWriteTime of both and use the newer one.
2. Commit and push — this **auto-deploys to production** via the Vercel↔GitHub integration:
   ```powershell
   cd C:\Users\stevo\projects\spades-tracker
   git add -A
   git commit -m "Update spades-tracker page"
   git push
   ```
3. Verify the live site serves the new version (compare the length to the new file):
   ```powershell
   (Invoke-WebRequest "https://spades-tracker.vercel.app/" -UseBasicParsing).Content.Length
   ```
4. **Fallback** (if the GitHub auto-deploy ever doesn't fire): deploy straight from the folder:
   ```powershell
   vercel deploy --prod
   ```

## Important notes

- `index.html` lives at the **repo root** and is served at `/`. Keep it there — do not move it into a subfolder.
- The file intentionally has **no `<!DOCTYPE>` / `<html>` wrapper** — it starts with `<style>` and ends with `</script>`. That's how it was authored; don't "fix" it.
- `.vercel/` holds the linked-project config and env secrets — it's gitignored. **Never commit it.**
- Git push auth is cached on this machine, so `git push` works without a login prompt. (`gh` CLI is installed but not logged in — not needed for this workflow.)
