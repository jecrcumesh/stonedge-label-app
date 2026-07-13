# Stonedge Material Gallery

The page your label QR codes link to — shows product photos (slab + wall/floor application shots) and a one-tap WhatsApp enquiry button. Free to host, no server or database needed.

---

## 1. What's in this folder
- `product.html` — the page itself. Reads `?slug=` from the URL to know which material to show.
- `materials.json` — your product data: names, descriptions, image paths, and your WhatsApp number. **This is the only file you edit day-to-day.**
- `images/<slug>/` — one folder per material, holding its photos.

A sample entry (`burberry-beige`) with placeholder images is included so you can see the structure — swap the placeholder JPGs for real photos before going live.

### Page features
- Swipeable photo gallery with a fade transition, thumbnail strip, and an image counter
- Tap any photo to open it full-screen; double-tap to zoom in for detail (grain, veining, finish)
- A "Get in touch" card showing phone (tap to call), email (tap to open mail app), and website — pulled from the company-level fields below
- Skeleton loading animation instead of a plain "Loading…" text
- A friendly "not found" message if a QR points to a slug that's since been removed

### Company-level fields (top of `materials.json`, set once)
```json
"companyName": "Stonedge Private Limited",
"logo": "data:image/png;base64,...",
"whatsappNumber": "919714944008",
"phone": "919714944008",
"phoneDisplay": "+91 9714944008",
"email": "info@thestonedge.com",
"website": "http://www.thestonedge.com/"
```
`logo` is a data URI (base64) so the page stays a single fetch with no extra image request — regenerate it from any PNG with `base64 -i logo.png` and paste the output after `data:image/png;base64,`. `phoneDisplay` is optional — if omitted, the raw `phone` digits are shown instead.

## 2. Where images are stored (free)
Store them right in this same GitHub repo, inside `images/<material-slug>/`. GitHub Pages serves them for free along with the HTML — no separate image host, no bandwidth bill, no expiry. This works well up to a few thousand photos; if your catalog gets huge later, a dedicated image host is a future upgrade, not something you need on day one.

**Tip before uploading:** resize photos to roughly 1600px on the long edge and save as JPG (quality ~80). Full-resolution camera photos (4000px+, several MB each) will work but load slowly on a customer's phone over mobile data.

## 3. Adding a new material (recommended: use the Admin tool)
Open `admin.html` — it's a form: enter the material name, category, description, drag in your photos, and click **Upload & Publish to GitHub**. It automatically:
- slugifies the material name into a folder name (e.g. "Alaska Grey Granite" → `alaska-grey-granite`), and adds `-1`, `-2`, etc. if that slug is already taken
- resizes and compresses each photo (max 1600px, so uploads stay fast even from a phone camera)
- creates `images/<slug>/` and uploads the photos there
- adds the matching entry to `materials.json`
- shows you the final slug and the live page link when done — paste that slug straight into Label Studio's "Material Slug" field

**One-time setup for the Admin tool:**
1. On GitHub: **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Scope it to **only this repository**, with repository permission **Contents: Read and write**. Nothing else needed.
3. Open `admin.html`, fill in your GitHub username, repo name (`stonedge-label-app`), and paste the token in. Click **Test Connection** to confirm.
4. These details (including the token) are saved in that browser's local storage only — they're sent only to `api.github.com`, never anywhere else. Don't share this page publicly or hand the token to anyone outside the team; treat `admin.html` as an internal tool, not something to link customers to.

Each publish creates a normal commit to your repo, so you always have full history and can revert from GitHub if needed.

### Adding a material by hand (no token needed)
If you'd rather not set up a token, you can still do it manually:
1. Create a folder `images/<slug>/` (slug = lowercase, hyphens instead of spaces — e.g. `alaska-grey`).
2. Add your photos there.
3. Open `materials.json` and add an entry:
   ```json
   "alaska-grey": {
     "name": "Alaska Grey Granite",
     "category": "Granite",
     "description": "Flamed-finish granite with a cool grey tone, ideal for exteriors and flooring.",
     "images": [
       "images/alaska-grey/slab-1.jpg",
       "images/alaska-grey/wall-application.jpg"
     ]
   }
   ```
4. Commit and push.

## 4. WhatsApp enquiry button
Set your number once in `materials.json`:
```json
"whatsappNumber": "919714944008"
```
(country code + number, digits only, no `+` or spaces). Every material page uses this number, with a pre-filled message like *"Hi, I'm interested in Alaska Grey Granite. Could you share more details and pricing?"* — the customer just taps Send.

If different materials should route to different salespeople/numbers later, that's a small extension — ask and I'll add a per-material override.

## 5. Deploy to GitHub Pages
1. Create a repo, e.g. `stonedge-gallery`.
2. Upload this whole folder (`product.html`, `materials.json`, `images/`) to the repo root.
3. **Settings → Pages** → Source: Deploy from a branch → Branch: `main`, folder: `/ (root)` → Save.
4. Your gallery is now live at:
   `https://<your-username>.github.io/stonedge-gallery/product.html`
5. In Stonedge Label Studio, set **Gallery Page URL** (in the Barcode & Contact section) to that exact link — it's saved automatically and reused for every label. Only the **Material Slug** changes per label.

## 6. Testing a page
Visit `product.html?slug=burberry-beige` (with your real deployed URL) to see the sample entry. Visiting a slug that doesn't exist in `materials.json` shows a friendly "not found" message instead of breaking.
