# Postal Warrior Performance Dashboard

Web dashboard for **India Post – Ahmedabad HQ Region**. It reads the monthly BO Excel report and shows Postal Warrior status
(Gold / Diamond / Platinum) by division, sub division and BO, with shortfall details, KPI charts and Excel/CSV exports.
It is a static site (one `index.html`) – no server, database or build step.

## What is in this project

```
postal-warrior-dashboard/
├── index.html                  the whole dashboard (HTML, CSS, JavaScript, logo)
├── data/
│   └── BO_Dashboard.xlsx       the BO Excel report the dashboard loads automatically
├── config.json                  tells the dashboard your repository name (see step 0 below)
├── netlify.toml                 Netlify settings (keeps the data file from being cached)
├── .github/workflows/pages.yml GitHub Pages deployment (optional)
├── .nojekyll
├── .gitignore
└── README.md
```

## Tabs
Dashboard · Division Wise · Sub Division Wise (KPIs and charts) · Vertical Wise Summary (15 reports, sortable, Excel export) ·
Branch Office Wise (Office ID column, search by BO name/Office ID/sub division, division/sub division filters, Excel/CSV export,
click a BO for full business and shortfall detail) · Warrior Categories · Near Gold / Near Diamond / Near Platinum Warrior · Not Qualified BOs ·
Product Performance · KPI Charts · Data Management (upload and live sync) · Settings (clear all data). The layout works on mobile phones.

## Step 0 – one-time setup: edit config.json
Open **config.json** and set `githubRepo` to your GitHub username and repository name, for example:
```json
{
  "githubRepo": "myuser/postal-warrior-dashboard",
  "branch": "main",
  "dataFile": "data/BO_Dashboard.xlsx",
  "checkMinutes": 1
}
```
This lets the dashboard read the data file straight from GitHub even before Netlify or GitHub Pages finish redeploying,
so updates reach visitors sooner, and it is what the **Publish to GitHub** panel (see below) uses.
(On a `your-username.github.io` site the repository name is filled in automatically and this step is optional.)

## Put it on GitHub
1. On github.com click **New repository**. Choose **Private** (see the privacy note below) and create it.
2. Click **uploading an existing file**, drag in **everything inside this folder** (including the `data` and `.github` folders), then **Commit changes**.

## Publish it (choose one)

**A. Netlify (recommended)**
1. Netlify → **Add new site → Import an existing project → GitHub** and pick the repository.
2. Leave **Build command** empty and set **Publish directory** to `.` (a dot), then **Deploy**.
3. Netlify redeploys automatically whenever you commit a change.

**B. GitHub Pages**
1. Repository → **Settings → Pages → Build and deployment → Source: GitHub Actions**.
2. The included workflow publishes the site on every push to `main`. The address appears under **Actions** and in Settings → Pages.
   (Pages on a private repository needs a paid GitHub plan.)

## Update the data for every visitor
Open the repository's **data** folder on github.com → **Add file → Upload files** → drop the new Excel, keeping the name
**`BO_Dashboard.xlsx`** → **Commit changes**. The dashboard reads the file straight from GitHub, so most visitors see the new
data within about a minute – sooner than waiting for Netlify or GitHub Pages to redeploy. Anyone already on the dashboard sees
it appear **automatically**, with a green “Dashboard updated” message – no click needed. It checks every minute by default
(`checkMinutes` in `config.json`), and instantly whenever someone returns to the browser tab.

> **Important:** **Upload Excel** on the dashboard changes only the browser you use. Other visitors keep seeing the data from
> the website. To update the dashboard for everyone, use the GitHub upload above, or **Publish to GitHub** below.

### Or update from the dashboard itself (Publish to GitHub)
1. Create a token: GitHub → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
   Under *Repository access* pick **only this repository**; under *Permissions → Contents* choose **Read and write**. Copy the token.
2. Open the dashboard → **Data Management → Publish to GitHub**. Enter `your-username/your-repository`, the branch (`main`),
   the file (`data/BO_Dashboard.xlsx`) and the token. Tick *Remember* only on your own computer.
3. Click **Choose Excel & publish**. The dashboard checks the file, shows it, and saves it into the repository.
4. The site redeploys in 1–3 minutes; open dashboards then refresh by themselves.

## Live sync settings (Data Management tab)
- **Option 1 – Select a folder on this computer** (Chrome or Edge on a computer): pick the folder where the report is saved and choose the file.
  The dashboard re-reads it at the chosen interval and updates when it changes. The browser asks permission once.
- **Option 2 – Web address**: default `data/BO_Dashboard.xlsx` (the file in this project). You can also use another address on the same site.
- **Check every**: Off, 1, 5, 15, 30 minutes or 1 hour. Press **Save & sync now** after changing it.
- A typed local path such as `C:\Reports\file.xlsx` cannot be read by a web page – use Option 1.

## Run it on your own computer
Open a terminal in this folder and run `python -m http.server 8000`, then open <http://localhost:8000>.
(Opening `index.html` by double-click also works for uploading files, but live sync from `data/` needs a web address.)

## Excel columns the dashboard reads
Columns are matched by heading name, so column order and extra columns do not matter.

| Needed | Columns |
|---|---|
| Required | Division Name · Branch Office Name · Office_ID · POSB Account Open · Total Intital PLI/RPLI Collection · Sale of Postage Stamp · Actual Accountable Article booked · No. of Digital Txns · Total Business IPPB · Postal Warrior (Revised Parameter) · the three **Additional Free Point required for Gold / Diamond / Platinum Warrior** columns, each with its 10 shortfall columns just before it |
| Optional (treated as 0 if missing) | Sub Division Name · No. of PLI/RPLI Policies · Article Booking Revenue · Actual Parcel Booked · Parcel Revenue · Int. Mail Booked · Int. Mail Revenue · No. of IPPB Premium Account Open · No. of GI Policy · No. of Aadhaar Seeding · No. of POSA Linkage · No. of CELC · No. of DLC · Total Points (Revised Parameter) · Postal Warrior · Postal Warrior Without IPPB (Revised Parameter) |

## Privacy note
The Excel file contains real BO performance figures. On a **public** repository anyone can download it.
Keep the repository **private** unless the data may be shared publicly.

## Troubleshooting
| Message | What to do |
|---|---|
| "File not found at …" | The file must be at `data/BO_Dashboard.xlsx` in the published site. Check the folder and file name. |
| "This page was opened from a file on your computer" | Open the dashboard from its website address, or use Option 1 – Select folder. |
| "That is a path on your computer" | Web pages cannot read `C:\` paths. Use Option 1 – Select folder. |
| "The browser blocked that web address (CORS)" | Use a file on the same website. OneDrive and Google Drive share links do not work. |
| Public visitors still see old data | Check that the new Excel is at `data/BO_Dashboard.xlsx` with exactly that name (not `BO_Dashboard (1).xlsx`, not the root folder). Wait for the deploy to finish (Netlify → Deploys, or GitHub → Actions shows a green tick). Then reload the page, or open **Data Management → Save & sync now**. The yellow bar shows the file's last-modified time. |
| "Missing columns: …" | The Excel is missing a required column – see the table above. |

The Excel reader (SheetJS) and the fonts load from the internet (cdnjs / Google Fonts), so the dashboard needs an internet connection.
