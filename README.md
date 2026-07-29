# Fleet Wolf Photo Capture — Beginner Setup Guide

This walks through every single click needed to get this working. Do the steps in order.

---

## Part 1 — Put your files on the internet (GitHub Pages)

Geotab needs to load your add-in from a real web address (https://...), so first we put these 3 files somewhere public and free.

1. Go to https://github.com and click **Sign up** (skip if you already have an account).
2. Once logged in, click the **+** icon top-right → **New repository**.
3. Name it `photo-addin`. Leave it Public. Click **Create repository**.
4. On the new repo page, click **uploading an existing file** (a blue link in the middle of the page).
5. Drag in all 3 files from this project: `index.html`, `configuration.json`, `icon.svg`.
6. Scroll down, click **Commit changes**.
7. Now turn on GitHub Pages: click **Settings** (top tab of the repo) → in the left sidebar click **Pages**.
8. Under "Branch," choose `main` and folder `/ (root)`, click **Save**.
9. Wait about 1 minute, then refresh. GitHub will show you a link like:
   `https://YOUR-USERNAME.github.io/photo-addin/`
   That's your live add-in address. Write it down.

---

## Part 2 — Point the config file at your real address

Right now `configuration.json` has placeholder text `YOUR-GITHUB-USERNAME`. You need to fix that.

1. In your GitHub repo, click on `configuration.json`.
2. Click the pencil (✏️) icon to edit it.
3. Replace both instances of `YOUR-GITHUB-USERNAME` with your actual GitHub username.
4. Click **Commit changes**.

---

## Part 3 — Generate your Solution ID

Every Geotab add-in that stores data needs its own unique ID so your photos don't mix with anyone else's.

1. Go to: https://geotab.github.io/sdk/software/api/runner.html#sample:generate-addin-guid
2. Run the generator on that page — it gives you a string of letters/numbers.
3. Copy it.
4. Back in GitHub, open `index.html`, click the pencil to edit.
5. Find this line near the top of the script:
   ```
   var SOLUTION_ID = "aXRQUEFDSElOR0lEXFJFUExBQ0VfTUU"; // <-- REPLACE THIS
   ```
6. Replace the placeholder text between the quotes with the ID you just generated.
7. Commit changes.

---

## Part 4 — Install the add-in in MyGeotab

1. Log into MyGeotab (my.geotab.com) as an Administrator.
2. Click **Administration** (top menu) → **System** → **System Settings**.
3. Click the **Add-Ins** tab.
4. Click **New Add-In** (top right).
5. Choose the option to add via **configuration text/JSON** (not "browse for zip").
6. Open your `configuration.json` on GitHub, click the **Raw** button to see the plain text, copy all of it.
7. Paste it into the box in MyGeotab.
8. Click **Save/Confirm**. You should see a green success message.

---

## Part 5 — Test it on a phone

1. On a phone, open the **Geotab Drive** app and log in as a driver.
2. Look for a new menu item called **"Take Photo"** (this may take a login/logout or app refresh to appear, since Add-Ins are loaded at login).
3. Tap it, tap **Take Photo**, take a picture, confirm the preview shows.
4. Tap **Upload to Geotab** and watch the status message.

---

## Part 6 — Check it landed in MyGeotab

There isn't a default "photo gallery" screen in MyGeotab — media files are retrieved via the API. The simplest way to verify for now:

1. In MyGeotab, go to **Administration → System → API Reference** (or use the SDK "API Runner" tool at https://geotab.github.io/sdk/software/api/runner.html).
2. Run a `Get` call with `typeName: "MediaFile"` — you should see your uploaded photo's metadata (name, status) listed, with `status` eventually showing `Ready`.

If you want an actual photo gallery page inside MyGeotab later, that's a second, separate MyGeotab-side add-in — happy to help build that once this capture side is working.

---

## Known rough edge (heads up)

The "upload the actual image bytes" step (`UploadMediaFile`) is a newer, still-beta part of Geotab's API, and the exact request format has changed between versions. If Part 5's upload fails with an error, that's the most likely spot — the fix is usually a small tweak to how the image data is sent (Geotab's own official sample for this lives in `Geotab/mg-media-files` → `addin-media-files` folder on GitHub). Send me the exact error message you get and I'll adjust the code with you.
