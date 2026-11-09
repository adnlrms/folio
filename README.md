# DocTrack — SDO Agusan del Norte

DocTrack is a document control system for official memoranda, letters and certificates on the Division letterhead. Each document gets a control number (for example `LRMS-2026-11-001`) and a QR code at the lower right of the footer. The PDF is filed in Google Drive and recorded in Google Sheets.

| Part | Where it runs |
|---|---|
| App (this repository) | GitHub Pages |
| Backend | Google Apps Script (`Code.gs`, `appsscript.json`) |
| Database | Google Sheets (created by setup) |
| PDF storage | Google Drive, in the `DocTrack Archive` folder (created by setup) |

## Files in this repository

| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `manifest.webmanifest` | Lets the app be installed on phones and computers |
| `sw.js` | Opens the app quickly and keeps drafting working offline |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`, `apple-touch-icon.png` | App icons |

## Setup

### 1. Backend (Google Apps Script)
1. Go to [script.google.com](https://script.google.com) and click **New project**.
2. Paste `Code.gs` into the editor.
3. Open **Project Settings**, tick **Show "appsscript.json"**, then paste `appsscript.json`.
4. Choose the `setup` function, click **Run**, and allow access. This creates the database sheet and the Drive folders.
5. Click **Deploy → New deployment → Web app** and set:
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Copy the **Web app URL**. It ends in `/exec`.

### 2. Connect the app
Open `index.html` and find this line near the top:

```html
<meta name="doctrack-api" content="PASTE_YOUR_APPS_SCRIPT_EXEC_URL_HERE">
```

Replace the placeholder with your `/exec` URL and keep the quotes. This is the only place where you paste it.

### 3. Publish on GitHub Pages
1. Upload all the files to the root of this repository.
2. Go to **Settings → Pages**. Set Source to **Deploy from a branch** and Branch to **main**, folder **/ (root)**, then click **Save**.
3. After about a minute the app is live at `https://<username>.github.io/<repository>/`.

### 4. First sign-in
1. Open the app and go to **Admin**.
2. Enter the default code `smrl`.
3. Set a new admin code (8 or more characters) and a staff code (6 or more characters). The app cannot be used until both are set.
4. QR codes are set to open this GitHub address automatically.

## Using DocTrack

- **Create**
  - Choose the office and document type, then type or paste from Word. Formatting is kept.
  - Filing reserves the control number, adds the QR code and saves a searchable PDF to Drive.
- **Certificates**
  - Portrait or landscape.
  - Paste or upload a list of recipients. Every name gets its own QR code, all in one PDF.
  - Signatories and their placement can be changed.
- **Records**
  - Shows 20 records per page to stay fast. Search, filter by Perpetual Index subject, reprint, and export reports to Excel.
- **Verify**
  - Scan a QR code with the camera, or type a control number, to confirm a document is genuine.
- **Admin**
  - App name, logo, colors, fonts (including Bookman Old Style), footer, privacy notice.
  - Offices: add, edit, disable or delete. Each has its own code format.
  - Files: add a PDF made outside the app (it gets a control number, and the QR code is stamped on every page), or delete any record. A deleted record's number can be reused for the office's next document in the same month or year.
  - Document types, certificate logos (12 placements including beside the QR code; each logo can have its own) and footers.
  - Access codes and the security checklist.

## Installing on devices

| Device | How to install |
|---|---|
| Windows, Mac, Android (Chrome or Edge) | Open the app and choose **Install**. |
| iPhone or iPad (Safari) | Tap **Share → Add to Home Screen**. |

## Updating

| What changed | What to do |
|---|---|
| `index.html` | Edit or re-upload it here. The site updates in about a minute. Users may need to refresh once. |
| `Code.gs` | In Apps Script go to **Deploy → Manage deployments**, click the pencil, set Version to **New version**, then **Deploy**. Do not make a new deployment: the `/exec` URL would change. |

## Troubleshooting

| Problem | Fix |
|---|---|
| Yellow **Preview mode** banner | The `/exec` URL was not pasted into `index.html`, or it was pasted incorrectly. |
| "The app reached Google but not DocTrack" | Redeploy with Execute as **Me** and access **Anyone**, as a new version. |
| Changes to `Code.gs` don't show | You saved the code but did not deploy a new version. |
| Old QR codes open the Apps Script address | They are forwarded to this app automatically. |

## Security notes

- Keep the admin and staff codes private, and change them if someone leaves the office.
- Repeated wrong codes lock sign-in for 15 minutes.
- Each QR code carries a secret key. Typing a control number without the key shows only the document's status.
- Leave **Allow the app inside other websites** off. GitHub hosting does not need it.
- The database is backed up weekly to `DocTrack Archive/_backups`, and the last 8 backups are kept.

- Text now fills each page down to the bottom margin, and pasted content can no longer cross the margins. I checked it with a 3-page test memo pasted the way Word sends it, comparing the editor and the PDF page by page.

Using the space:

Pages fill to the bottom margin. On the test memo, page 1 ended 3 px short of its margin.
A paragraph was sometimes pushed to the next page only because of its spacing, even though its text fit. That’s fixed.
Pages 2 and later no longer start with extra space above the first paragraph.
The editor now shows each page break in exactly the same place as the PDF. Before, a paragraph that had space above it could make the two differ by one line.

Neat placement (like Word’s defaults):

A paragraph never leaves just its first line at the bottom of a page, or just its last line at the top of the next one. Sometimes this leaves one empty line at the bottom of a page, as Word does.
Headings, such as a bold “II. GUIDELINES”, stay with the text below them instead of being stranded at the bottom of a page. So do lead-in lines like “…as follows:”. In 30 different test layouts, none of these rules was broken.

Margins kept safe when pasting from Word:

Paragraphs and lists can no longer be pushed outside the side margins.
Tables wider than the page are fitted to the text width, and long words inside table cells wrap.
Large pictures shrink to fit the page and keep their shape. A picture never runs past the margins or the footer.
