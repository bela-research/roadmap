# Roadmap website: setup guide

This folder contains:

| File | What it is |
|---|---|
| `index.html` | The whole website. You only ever edit its **SETTINGS** block. |
| `roadmap_template.csv` | 20 example goals to import into Google Sheets. |
| `README.md` | This guide. |

How it works: you edit a Google Sheet → Google publishes it as a CSV file → every time someone opens the website, it downloads that CSV and draws the roadmap. Nobody but you can change anything; members can only look.

---

## Step 1: Put the template into Google Sheets

1. Open <https://sheets.google.com> and create a **Blank spreadsheet**. Give it a name, for example *Ütemterv*.
2. Click **File → Import → Upload**, and choose `roadmap_template.csv`.
3. In the import window set:
   - **Import location:** *Replace current sheet*
   - **Separator type:** *Detect automatically*
   - **Untick** *Convert text to numbers, dates, and formulas*. This keeps the dates exactly as `2026-12-15`.
   - Click **Import data**.
4. Select columns **F and G** (`start_date`, `due_date`) by clicking their letters, then **Format → Number → Plain text**. Dates you type later then stay exactly as typed.
5. Optional but helpful:
   - **View → Freeze → 1 row** keeps the column names visible.
   - Select column **E** (`horizon`) → **Data → Data validation → Add rule → Dropdown** with the options `now`, `next`, `later`. Do the same for column **H** (`status`) with `planned`, `in progress`, `done`, `delayed`. This prevents typos.

Do not rename or reorder the first row (the column names). You can add an extra column for your own notes; the website ignores it.

### What goes in each column

| Column | Required? | What to write |
|---|---|---|
| `branch` | yes | Main area, e.g. *Pénzügyek*. Branches appear in the order they first appear in the sheet. |
| `sub_branch` | no | Project within the branch, e.g. *Pályázatok*. Leave empty to put the goal in the branch's general lane (shown at the top of the branch). |
| `goal` | yes | Short title of the goal. |
| `description` | no | Longer text shown when someone clicks the goal. |
| `horizon` | yes | `now`, `next` or `later`. If it's missing or misspelled, the goal is treated as `later`. |
| `start_date` | no | `YYYY-MM-DD`, e.g. `2027-03-01`. |
| `due_date` | no | `YYYY-MM-DD`. |
| `status` | no | `planned`, `in progress`, `done` or `delayed`. Empty means `planned`. |
| `owner` | no | Person or team responsible. Shown as initials on the card and as the full name in the details. |
| `link` | no | A web address (`https://…`) for more information. |

How dates decide what you see:

- **Both dates** → a bar from start to deadline.
- **Only `due_date`** → a diamond (milestone) on the deadline.
- **No dates** → a dashed "approximate" card placed inside its horizon band (*Most / Következő / Később*).
- **Only `start_date`** → a bar that fades out at the end of its horizon (no fixed deadline).

Empty optional cells leave no trace on the page: no empty labels or dashes.

> **Privacy:** "Publish to web" (next step) makes the sheet readable by anyone who has the link, and the link is visible in the website's code. Only put things in the sheet that may be public, including the names in `owner`. A team name (e.g. *Kommunikációs munkacsoport*) is a good alternative to personal names.

---

## Step 2: Publish the sheet as CSV and copy the link

1. In the sheet: **File → Share → Publish to web**.
2. Under **Link**, choose the tab that holds your data (e.g. *Sheet1*) instead of *Entire document*, and choose **Comma-separated values (.csv)**.
3. Open **Published content & settings** and make sure **Automatically republish when changes are made** is ticked.
4. Click **Publish** and confirm.
5. Copy the link that appears. It looks like
   `https://docs.google.com/spreadsheets/d/e/2PACX-…/pub?gid=0&single=true&output=csv`
   It must end in `output=csv`. The ordinary link from your browser's address bar (ending in `/edit`) will **not** work.

If your Google account belongs to a school or company, its administrator may have switched off "Publish to web". In that case use a personal Google account.

---

## Step 3: Paste the link into `index.html`

1. Right-click `index.html` → **Open with → Notepad** (any plain text editor works; don't use Word).
2. Press **Ctrl+F** and search for `CSV_URL`. You are now in the clearly marked **SETTINGS** block.
3. Paste your link between the two quotes:
   ```js
   CSV_URL: 'https://docs.google.com/spreadsheets/d/e/2PACX-…/pub?gid=0&single=true&output=csv',
   ```
4. In the same block, replace `Egyesületünk neve` in `ORG_NAME` with your association's name. You can also change `SUBTITLE`.
   Keep the quotes `'…'` and the comma at the end of each line.
5. Save the file (**Ctrl+S**).
6. Double-click `index.html` to open it in your browser. It should now show the data from your Google Sheet, and the yellow *Bemutató adatok* (demo data) badge should be gone. (If your browser refuses to load data from a file on your computer, continue anyway: it will work once the site is on GitHub Pages.)

Other settings in the same block:

- `HORIZONS`: how many months *Most*, *Következő* and *Később* cover (default 0–3, 3–9 and 9+ months from today).
- `DEFAULT_ZOOM`: `'auto'` (recommended), or `'month'`, `'quarter'`, `'year'`.
- `BRANCH_COLORS`: branch colours, in the order branches appear in the sheet. The default order is chosen so colour-blind readers can tell neighbouring branches apart.

---

## Step 4: Create the GitHub repository

1. Create a free account at <https://github.com/signup> if you don't have one. Your **username** becomes part of the website address, so choose it with that in mind (e.g. your association's short name).
2. Click the **+** in the top-right corner → **New repository**.
3. **Repository name:** e.g. `utemterv`. Select **Public**. Leave everything else as it is, then click **Create repository**.
4. On the next page, click the link **uploading an existing file**.
5. Drag `index.html` into the page. (You can also add `README.md`. `roadmap_template.csv` isn't needed once `CSV_URL` is set.)
6. Click **Commit changes**.

---

## Step 5: Turn on GitHub Pages

1. In your repository, click **Settings** (top menu), then **Pages** (left menu).
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Under **Branch**, choose **main** and **/ (root)**, then click **Save**.
4. Wait 1–2 minutes and refresh the page. A box appears at the top: **Your site is live at** `https://<username>.github.io/utemterv/`.

---

## Step 6: Share the link

Send `https://<username>.github.io/utemterv/` to your members. It works in any browser, on desktop and on phones, and nobody needs an account.

---

## Everyday use

- **Edit only the Google Sheet.** Changes reach the website within about 5 minutes (Google refreshes published sheets with a delay). Readers just reload the page.
- **Check your sheet for mistakes:** open the site with `?debug` at the end, e.g. `https://<username>.github.io/utemterv/?debug`. A panel lists every problem (misspelled horizon, invalid date, dates that contradict the horizon, broken link…) with its row number. If there are no problems, it says *nincs hiba ✓*.
- **Changing the website itself** (settings in `index.html`): in GitHub open `index.html` → click the pencil icon (**Edit**) → make the change → **Commit changes**. Or upload the edited file again with **Add file → Upload files**. The site updates within 1–2 minutes.
- The *Frissítve* (updated) note in the header shows when the sheet was last published, if Google provides that information. Otherwise it shows when the page loaded the data.

---

## Troubleshooting

| What you see | What to do |
|---|---|
| *Az ütemterv most nem tölthető be*, and *Technikai részletek* says the link doesn't return CSV | You pasted the normal sheet link. Use the **Publish to web → CSV** link (Step 2). |
| The same message with *HTTP 400/404* | The sheet was unpublished or the link is incomplete. Publish again and paste the full link. |
| The yellow *Bemutató adatok* badge is still visible on the live site | The uploaded `index.html` still has an empty `CSV_URL`. Edit it in GitHub (pencil icon). |
| Edits in the sheet don't show up | Wait 5 minutes and reload. Check that *Automatically republish when changes are made* is ticked. |
| A goal is in the wrong place | Open the site with `?debug`. Write dates as `YYYY-MM-DD`. |
| The page is blank or broken after you edited `index.html` | A quote or comma in the SETTINGS block was probably deleted. Compare with the original file. |
