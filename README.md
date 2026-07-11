# stonedge-label-app

Two tools, one repo:

| Folder | What it is | Live URL after Pages is enabled |
|---|---|---|
| `label-studio/` | Design and print 152×101mm product labels (company logo, material details, barcode, QR) | `https://<your-username>.github.io/stonedge-label-app/label-studio/` |
| `gallery/` | The page each label's QR code opens — slab photos + wall/floor application shots + a WhatsApp enquiry button | `https://<your-username>.github.io/stonedge-label-app/gallery/product.html` |
| `gallery/admin.html` | Upload photos for a new material — auto-creates the image folder, resizes photos, and updates `materials.json` for you, committing straight to this repo | `https://<your-username>.github.io/stonedge-label-app/gallery/admin.html` |

## One-time setup
1. Push this folder's contents to the `stonedge-label-app` repo (root of the repo = this folder's contents).
2. **Settings → Pages** → Source: Deploy from a branch → Branch: `main`, folder: `/ (root)` → Save. GitHub gives you the base URL within a minute or two.
3. Open `label-studio/index.html` (via the GitHub Pages URL, or locally by double-clicking it) → in the **Gallery Page URL** field, replace `YOUR-GITHUB-USERNAME` with your actual GitHub username so it points at `.../stonedge-label-app/gallery/product.html`. This is saved automatically after you type it once.

## Day-to-day use
- **Printing labels:** open `label-studio/`, fill in the product fields, click Print. See `label-studio/README.md` for TSC printer setup.
- **Adding a new material's photos:** open `gallery/admin.html`, fill in the material details, drag in photos, click publish — it handles the folder, image resizing, and `materials.json` entry automatically. One-time GitHub token setup required; see `gallery/README.md`. Then set that same slug (shown after publishing) in the label app's "Material Slug" field when printing that material's label.

Each subfolder has its own more detailed README — this one is just the map.
