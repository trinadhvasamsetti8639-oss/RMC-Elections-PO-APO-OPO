# Polling Duty Attendance App (GitHub Pages + Supabase)

Static front end on GitHub Pages; all data (users, attendance, reserve assignments, distribution, geofence, employee data, audit trail) is stored in your **Supabase PostgreSQL** project. Works on desktop and mobile browsers.

## 1. Create the Supabase database
1. Sign up at supabase.com > **New project** (choose a region near you, e.g. Mumbai; save the database password).
2. Open **SQL Editor > New query**, paste the whole of `supabase_setup.sql`, click **Run**. ("Success. No rows returned" is correct.) It creates the tables, the secure `api()` function, three report views, the default login `admin` / `admin123`, and locks every table so the browser can only call `api()`.
3. **Project Settings > API**: copy the **Project URL** and the **anon / publishable key**.

## 2. Publish the front end
1. Put both values in `config.js` (`SUPABASE_URL`, `SUPABASE_ANON_KEY`).
2. Upload all files to a GitHub repo root; Settings > Pages > deploy from `main` / root.
3. Open the Pages link, log in as `admin` / `admin123`, and **change that password immediately** (Admin > Users). Create the other users there.
4. Admin > Database & reports > **Save employee data to database** (or upload `MPL_PO-APO-OPO.xlsx` under Employee database > Apply).

Leave the config values empty for demo mode (browser only): `admin/admin123`, `user/user123`, `ro/ro123` (stations 1-5), `sec/sec123`, `dist/dist123`, `display/led123`. A red banner shows while in demo mode.

## Reports and cross-checking
- Supabase > **Table Editor**: `attendance`, `assignments`, `distribution`, `audit_log` (every action: who, when, what). Views `report_team`, `report_summary`, `report_reserve` join names with attendance and update live; use **Export to CSV** on any of them.
- In the app: Reports tab > **⬇ Excel** downloads Summary / Team / Reserve sheets.

## Roles
| Role | Can do |
|---|---|
| admin | Everything: capture, assign reserve, distribution, geofence, users, clear attendance, replace/update employee database |
| user | Capture attendance (team / reserve), reports, dashboard, distribution (view) |
| ro | Reports, dashboard and distribution for the stations assigned by admin only |
| sec | Assign attended reserve staff to teams, update distribution, reports, dashboard |
| dist | Update distribution status, reports, dashboard |
| display | Dashboard / LED screens only |

## LED screens
`team-dashboard.html` (25 stations per slide) and `reserve-dashboard.html` (50 staff per slide). Create a user with role LED Display, open each page full-screen, log in once. Slides change every 2 minutes, data refreshes every 10 seconds. Keys: arrows, space = pause, `f` = fullscreen.

## Notes
- The anon key is meant to be public. Security comes from the locked tables: the only thing it can do is call `api()`, which checks the user name and bcrypt-hashed password on every request and enforces roles.
- Supabase free projects **pause after 1 week of inactivity**: open the app a few days before polling day (or use Pro for one month). Free database limit is 500 MB; this app uses a few MB.
- Export a backup (CSV from Table Editor) after polling.
- `data.json` / `reserve.json` are public on a public Pages site; after the admin saves the data to the database it is served from there. Use a private repo if the files are sensitive.
