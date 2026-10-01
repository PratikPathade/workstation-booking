# Workstation Booking Calendar

A static GitHub Pages site for booking days on a shared bioinformatics workstation. People book a range of days (a couple of days, a week, or longer) and say how many cores they will use, or all of them. The calendar shows how full the machine is each day and who runs next.

Bookings are stored as GitHub Issues, so there is no server to run.

## Setup

1. Create a **public** GitHub repo (e.g. `workstation-booking`).
2. Edit `CONFIG` near the bottom of `index.html`:
   - `repo`: `"your-username/workstation-booking"`
   - `workstation`: the display name
   - `totalCores`: logical cores on the machine. Find it with `nproc` (Linux) or
     `(Get-CimInstance Win32_Processor | Measure NumberOfLogicalProcessors -Sum).Sum` (Windows PowerShell).
3. Push the files:
   ```
   git init
   git add .
   git commit -m "Workstation booking calendar"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
4. In the repo: **Settings → Pages → Source: Deploy from a branch → `main` / root → Save**.
5. The site goes live at `https://<you>.github.io/<repo>/` after about a minute.

Until `repo` is set to a real repo, the page shows demo data so you can try it.

## How it works

- **Calendar:** each day shows the bookings (person and cores) and a bar for cores booked out of the total. Click a day for details.
- **Today card:** cores in use and free, who is running, and who is next.
- **Book days:** pick first and last day, then "All cores" or a number. The form checks every day in the range against the core limit. If it does not fit, it suggests the earliest slot that does.
- Submitting opens a pre-filled GitHub issue (a free GitHub account is needed). Clicking "Submit new issue" confirms the booking. Press Refresh afterwards.
- **Cancel or finish early:** close your issue.
- Issue titles must start with `[Booking]` and the body uses `Start:`, `End:`, `Cores:` lines. Edit them only if you keep that format.

## Limits

- GitHub's anonymous API allows 60 requests per hour per IP, so the page refreshes every 5 minutes and falls back to saved data if it is rate limited.
- The core limit is checked in the browser only. It prevents honest mistakes but cannot stop someone editing an issue by hand.
