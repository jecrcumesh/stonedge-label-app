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

## 4. Printing multiple different materials together (batch printing)
If you need to print labels for several different materials in one go:
1. Fill in one material's details (Material Name, Size, Lot No, etc.) as usual.
2. Click **➕ Add This Material to Batch** — it's added to the queue below, and the product fields clear automatically so you can move straight to the next material.
3. Repeat for every material you want in this run. Each queued item shows in a list with a **×** to remove it if you change your mind.
4. Once everything's queued, click **🖨️ Print All (N)** — this sends every queued label as a single print job, one label per sheet, in the order you added them.
5. In the print dialog, make sure "Pages per sheet" is set to **1**, same as single-label printing.

The batch queue is saved automatically, so it survives a page reload if you need to step away mid-batch. Company-wide details (logo, contact info, QR caption, etc.) are shared across every label in the batch — only the per-material fields differ.


## 4. Printing on a TSC label printer
Browsers can't send raw printer commands (TSPL/ZPL) directly to a printer for security reasons — printing always goes through your operating system's print dialog and driver. Two ways to work with that on a TSC printer:

**A. Standard driver printing (works today, no extra setup beyond driver install)**
1. Install TSC's Windows driver for your printer model — TSC publishes this via the **Seagull Driver Wizard** on their support site.
2. In the printer's driver properties (Printing Preferences), create or select a label/paper size of **152 × 101 mm** matching this design, with gap/black-mark sensing set to match your label stock.
3. Click **Print Label** in the app, pick the TSC printer in the dialog, set margins to "None," and print. The driver converts the page to TSPL commands for you — this is exactly how BarTender itself prints to TSC printers.

**B. Silent/automatic printing without the dialog (e.g. triggered by a barcode scan, or a "click and it just prints" workflow)**
This needs a small local print-agent (the most common free option is **QZ Tray**) running on the print station's PC, which lets the web page send a print job straight to the printer over USB or network without any dialog box. This is a worthwhile next step if you're printing many labels a day and want to skip the dialog each time — happy to add QZ Tray integration to the app if you'd like it.

## 5. Strip labels (152 × 16 mm, multiple products on one 6×4 in sheet)
For cases where you just need to identify pieces quickly — Product Name, Lot No. and Quantity (Sqft) only — without the full 152×101mm label, use the **Label Type** switch at the top of the page:

1. Click **Strip Labels (152 × 16 mm, multi-up on a 6×4 in sheet)**. This swaps the whole page to a separate, independent form — it never touches or affects the big 152×101mm label or its batch queue, which are exactly as they were before.
2. Fill in **Product Name**, **Lot No.** and **Quantity (Sqft)**, then click **➕ Add Strip to Sheet**. Repeat for every piece/product you need a strip for — the fields clear automatically after each add.
3. The **Strip Sheet Preview** on the right shows exactly how the sheets will print: each strip is 152 mm wide × 16 mm tall, with a dashed cut-line and scissors mark between every strip so they cut apart cleanly. Five strips fit per 152×101mm (6×4 in) sheet — once a sheet is full, additional strips automatically continue onto the next sheet.
4. Click **🖨️ Print Strip Sheet(s)** to send every queued strip as one print job — set your printer's page size to 152×101mm (6×4 in), same as the big label, and "Pages per sheet" to 1.
5. Switch back to **Big Label (152 × 101 mm)** at any time — your strip queue is saved automatically and is still there when you return to strip mode, exactly like the big label's batch queue.

### Reusing a label later (Saved Labels)
Every strip you add is also remembered in a **Saved Labels** list, separate from the print queue — so it's still there even after you **Clear All** the queue or print a job. To reuse one later (e.g. the same product with a new quantity):
1. Find it in **Saved Labels** and click **Use** — this loads its Product Name, Lot No. and Quantity back into the form.
2. Change whatever's different (usually just the Quantity), then click **➕ Add Strip to Sheet** as normal.
3. Adding a strip with the same Product Name as an existing saved label updates that saved entry instead of creating a duplicate — so your saved list stays one entry per product.

## 6. Files in this delivery
- `index.html` — the app (open directly or deploy to GitHub Pages)
- `preview_screenshot.png` — a rendered sample of the printed label at true size, for quick reference
