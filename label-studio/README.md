# Stonedge Label Studio

A self-contained, single-file web app for designing and printing Stonedge product labels (152 × 101 mm) — company logo, material details, barcode, QR code, and contact info, laid out to a professional stone-industry style.

Everything (fonts loaded from Google Fonts at runtime; barcode + QR generation libraries) is bundled into the one `index.html` file except the two Google Font requests, so it works offline once opened, and needs **zero build step**.

---

## 1. Try it right now
Just double-click `index.html` — it opens in your browser and works immediately. Your default Stonedge logo is already built in; product fields are blank and ready for entry, or click **Load Sample** to see it populated.

## 2. Deploy to GitHub Pages (free hosting, so any team member can open it from a browser)
1. Create a new GitHub repository, e.g. `stonedge-label-studio`.
2. Upload `index.html` to the repo root (drag-and-drop on GitHub's web UI works fine, or `git add / commit / push`).
3. In the repo, go to **Settings → Pages**.
4. Under "Build and deployment", set **Source: Deploy from a branch**, Branch: `main`, folder: `/ (root)`.
5. Save. GitHub gives you a URL like `https://<your-username>.github.io/stonedge-label-studio/` within a minute or two.
6. Bookmark that URL on every computer/tablet at the workshop that needs to print labels.

No server, database, or ongoing cost — it's a static file GitHub hosts for free.

## 3. How the app works
- **Company Name, Tagline, Logo, Gallery Page URL, QR Caption, Contact No., Email, Website** are treated as "standing" details — they're saved in the browser's local storage, so they stay filled in next time you open the app on that device.
- **QR Caption** is the short line printed next to the scan code (default: "Watch It Come Alive") — edit it any time like any other field; longer text wraps automatically without overflowing the label.
- **Material Name, Size, Qty, Lot No, Description, Remark, Barcode Value, Material Slug** are per-product — click **Clear Product Fields** after printing one label to move to the next, without having to retype the company info.
- Typing a **Material Name** auto-fills a matching **Material Slug** (e.g. "Alaska Grey Granite" → `alaska-grey-granite`) — edit it if you want a shorter/different slug.
- The QR code links to **Gallery Page URL + ?slug=<Material Slug>** — see the companion `stonedge-gallery` project for the page it points to (product photos + WhatsApp enquiry button). Set the Gallery Page URL once after deploying that site; only the slug changes per label.
- The **barcode** (Code 128) regenerates live from the Barcode Value field as you type.
- **Print Label** opens your browser's print dialog sized exactly to 152 × 101 mm via CSS `@page` — what you see in the preview is what prints, to scale.

## 4. Printing on a TSC label printer
Browsers can't send raw printer commands (TSPL/ZPL) directly to a printer for security reasons — printing always goes through your operating system's print dialog and driver. Two ways to work with that on a TSC printer:

**A. Standard driver printing (works today, no extra setup beyond driver install)**
1. Install TSC's Windows driver for your printer model — TSC publishes this via the **Seagull Driver Wizard** on their support site.
2. In the printer's driver properties (Printing Preferences), create or select a label/paper size of **152 × 101 mm** matching this design, with gap/black-mark sensing set to match your label stock.
3. Click **Print Label** in the app, pick the TSC printer in the dialog, set margins to "None," and print. The driver converts the page to TSPL commands for you — this is exactly how BarTender itself prints to TSC printers.

**B. Silent/automatic printing without the dialog (e.g. triggered by a barcode scan, or a "click and it just prints" workflow)**
This needs a small local print-agent (the most common free option is **QZ Tray**) running on the print station's PC, which lets the web page send a print job straight to the printer over USB or network without any dialog box. This is a worthwhile next step if you're printing many labels a day and want to skip the dialog each time — happy to add QZ Tray integration to the app if you'd like it.

## 5. Files in this delivery
- `index.html` — the app (open directly or deploy to GitHub Pages)
- `preview_screenshot.png` — a rendered sample of the printed label at true size, for quick reference
