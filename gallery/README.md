# Stonedge Material Gallery

The page your label QR codes link to — shows product photos (slab + wall/floor application shots) and a one-tap WhatsApp enquiry button. Free to host, no server or database needed.

---

## 1. What's in this folder
- `product.html` — the page itself. Reads `?slug=` from the URL to know which material to show.
- `materials.json` — your product data: names, descriptions, image paths, and your WhatsApp number. **This is the only file you edit day-to-day.**
- `images/<slug>/` — one folder per material, holding its photos.

A sample entry (`burberry-beige`) with placeholder images is included so you can see the structure — swap the placeholder JPGs for real photos before going live.

## 2. Where images are stored (free)
Store them right in this same GitHub repo, inside `images/<material-slug>/`. GitHub Pages serves them for free along with the HTML — no separate image host, no bandwidth bill, no expiry. This works well up to a few thousand photos; if your catalog gets huge later, a dedicated image host is a future upgrade, not something you need on day one.

**Tip before uploading:** resize photos to roughly 1600px on the long edge and save as JPG (quality ~80). Full-resolution camera photos (4000px+, several MB each) will work but load slowly on a customer's phone over mobile data.

## 3. Adding a new material
1. Create a folder `images/<slug>/` (slug = lowercase, hyphens instead of spaces — e.g. `alaska-grey`).
2. Add your photos there (any names — `slab-1.jpg`, `wall-application.jpg`, etc.).
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
4. Commit and push. GitHub Pages updates automatically within a minute or two.
5. In **Stonedge Label Studio**, set the label's "Material Slug" field to `alaska-grey` — the QR code updates instantly to point at this material's page.

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
