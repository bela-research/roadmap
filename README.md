# Roadmap website: setup guide

This folder contains:

| File | What it is |
|---|---|
| `index.html` | The whole website. You only ever edit its **SETTINGS** block. |
| `roadmap_template.csv` | 20 example goals, shown as demo data when `SHEET_URL` is empty. |
| `README.md` | This guide. |

How it works: you edit a Google Sheet → Google publishes it as a CSV file → every time someone opens the website, it downloads that CSV and draws the roadmap. Nobody but you can change anything; members can only look.

---

## Step 1: Set up the Google Sheet

Each branch has its own tab, named after the branch.

1. Open <https://sheets.google.com> and create a **Blank spreadsheet**.
2. Create one tab per branch, and in row 1 of each tab enter these 9 column names, in this order:
   `sub_branch`, `goal`, `description`, `horizon`, `start_date`, `due_date`, `status`, `owner`, `link`
3. In each tab, select columns **E and F** (`start_date`, `due_date`), then **Format → Number → Plain text**. Dates you type later then stay exactly as typed.
4. Optional but helpful:
   - **View → Freeze → 1 row** keeps the column names visible.
   - Select column **D** (`horizon`) → **Data → Data validation → Add rule → Dropdown** with the options `now`, `next`, `later`. Do the same for column **G** (`status`) with `planned`, `in progress`, `done`, `delayed`. This prevents typos.

Do not rename or reorder the first row (the column names). You can add an extra column for your own notes; the website ignores it.

### What goes in each column

| Column | Required? | What to write |
|---|---|---|
| `sub_branch` | no | Project within the branch, e.g. *Pályázatok*. Leave empty to put the goal in the branch's general lane (shown at the top of the branch). |
| `goal` | yes | Short title of the goal. |
| `description` | no | Longer text shown when someone clicks the goal. |
| `horizon` | yes | `now`, `next` or `later`. If it's missing or misspelled, the goal is treated as `later`. |
| `start_date` | no | `YYYY-MM-DD`, e.g. `2027-03-01`. |
| `due_date` | no | `YYYY-MM-DD`. |
| `status` | no | `planned`, `in progress`, `done` or `delayed`. Empty means `planned`. |
| `owner` | no | Person or team responsible. Shown as initials on the card and as the full name in the details. |
| `link` | no | A web address (`https://…`) as plain text. |

How dates decide what you see:

- **Both dates** → a bar from start to deadline.
- **Only `due_date`** → a diamond (milestone) on the deadline.
- **No dates** → a dashed "approximate" card placed inside its horizon band (*Most / Következő / Később*).
- **Only `start_date`** → a bar that fades out at the end of its horizon (no fixed deadline).

Empty optional cells leave no trace on the page: no empty labels or dashes. A branch whose tab has no goals yet isn't shown.

> **Privacy:** Published tabs are readable by anyone who has the link, and the link is visible in the website's code. Only put things in them that may be public, including the names in `owner`. A team name (e.g. *Kommunikációs munkacsoport*) is a good alternative to personal names.

---

## Step 2: Publish the tabs

1. In the sheet: **File → Share → Publish to web**.
2. Open **Published content & settings**, and in its dropdown tick only the branch tabs.
3. Make sure **Automatically republish when changes are made** is ticked.
4. Click **Publish** (or **Start publishing**) and confirm.
5. Copy the link. You only need the part up to and including `/pub`:
   `https://docs.google.com/spreadsheets/d/e/2PACX-…/pub`

If your Google account belongs to a school or company, its administrator may have switched off "Publish to web". In that case use a personal Google account.

---

## Step 3: Enter the link and branches in `index.html`

1. Right-click `index.html` → **Open with → Notepad** (any plain text editor works; don't use Word).
2. Press **Ctrl+F** and search for `SHEET_URL`. You are now in the clearly marked **SETTINGS** block.
3. Paste your link between the quotes of `SHEET_URL`.
4. Under `BRANCHES`, keep one line per branch, in the order they should appear:
   ```js
   { name: 'Pályázatírás', gid: '710740198' },
   ```
   `gid` is the number after `gid=` in the browser's address bar while that tab is open.
5. In the same block, replace the text in `ORG_NAME` with your association's name.
   Keep the quotes `'…'` and the comma at the end of each line.
6. Save the file (**Ctrl+S**).

Other settings in the same block:

- `HORIZONS`: how many months *Most*, *Következő* and *Később* cover (default 0–3, 3–9 and 9+ months from today).
- `DEFAULT_ZOOM`: `'auto'` (recommended), or `'month'`, `'quarter'`, `'year'`.
- `BRANCH_COLORS`: branch colours, in the order of `BRANCHES`. The default is a soft purple-blue-green scale, so tell branches apart by their names rather than by colour alone.

**Adding a branch later:** create its tab, tick it under **Published content & settings**, and add its line to `BRANCHES`. **Renaming a branch:** change its `name` in `BRANCHES`; the site doesn't read the tab's own name.

---

## Step 4: Create the GitHub repository

1. Create a free account at <https://github.com/signup> if you don't have one. Your **username** becomes part of the website address, so choose it with that in mind (e.g. your association's short name).
2. Click the **+** in the top-right corner → **New repository**.
3. **Repository name:** e.g. `utemterv`. Select **Public**. Leave everything else as it is, then click **Create repository**.
4. On the next page, click the link **uploading an existing file**.
5. Drag `index.html` into the page. (You can also add `README.md`. `roadmap_template.csv` isn't needed once `SHEET_URL` is set.)
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

- **Sharing with workgroup leaders:** share the sheet by email as Editor, not with "anyone with the link can edit". Under **Data → Protect sheets and ranges**, limit each tab to its leader and yourself. Edits go live without any approval step; **File → Version history** can restore an earlier version.
- **Edit only the Google Sheet.** Changes reach the website within about 5 minutes (Google refreshes published sheets with a delay). Readers just reload the page.
- **Check your sheet for mistakes:** open the site with `?debug` at the end, e.g. `https://<username>.github.io/utemterv/?debug`. A panel lists every problem (misspelled horizon, invalid date, dates that contradict the horizon, broken link…) with its tab and row number. If there are no problems, it says *nincs hiba ✓*.
- **Changing the website itself** (settings in `index.html`): in GitHub open `index.html` → click the pencil icon (**Edit**) → make the change → **Commit changes**. Or upload the edited file again with **Add file → Upload files**. The site updates within 1–2 minutes.
- The *Frissítve* (updated) note in the header shows when the sheet was last published, if Google provides that information. Otherwise it shows when the page loaded the data.

---

## Troubleshooting

| What you see | What to do |
|---|---|
| *Az ütemterv most nem tölthető be*, and *Technikai részletek* says the link doesn't return CSV | `SHEET_URL` isn't the published link, or the tab isn't published. See Step 2. |
| The same message with *HTTP 400/404* | The sheet was unpublished or the link is incomplete. Publish again and paste the full link. |
| The yellow *Bemutató adatok* badge is still visible on the live site | The uploaded `index.html` still has an empty `SHEET_URL`. Edit it in GitHub (pencil icon). |
| *Nem sikerült betölteni: …* appears in the header | That tab isn't published, or its `gid` in `BRANCHES` is wrong. The other branches still work. |
| Edits in the sheet don't show up | Wait 5 minutes and reload. Check that *Automatically republish when changes are made* is ticked. |
| A goal is in the wrong place | Open the site with `?debug`. Write dates as `YYYY-MM-DD`. |
| The page is blank or broken after you edited `index.html` | A quote or comma in the SETTINGS block was probably deleted. Compare with the original file. |
