# Agent Performance Tracker

A simple HTML/CSS/vanilla JavaScript agent performance tracker. No build step and no third-party JavaScript dependencies are required.

## Features

- Daily records per agent with Present / Weekly Off / CL / LOP attendance statuses
- Manual daily FRT miss count
- CVR calculation from total chats and verified orders (30% default target)
- Shift-adherence miss counts, login-hour shortfall, configurable AHT and FRT thresholds
- Dashboard, date filters, agent history and attendance summaries
- Coaching/action history with follow-up dates
- Agent roster and weekly-off schedule
- CSV export and full JSON backup/restore
- Local audit history for important changes
- Optional GitHub API auto-save to a private data repository

## Run locally

Open `index.html` in a modern browser. No build process is needed. For a local web server, run `python3 -m http.server 8000` in this folder and open `http://localhost:8000`.

## Publish the website on GitHub Pages

1. Create a repository for the website and upload `index.html`, `style.css`, `app.js`, and `README.md` to its root.
2. In that repository, open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select the `main` branch and `/ (root)`, then save.
5. Wait for GitHub Pages to publish the site.

The app uses relative file references, so it works from a GitHub Pages repository subpath.

## Enable GitHub auto-save

**Keep the data repository private. Never save employee records to a public repository.** The website repository can be public while the data lives in a separate private repository.

1. Create a separate **private** GitHub repository, for example `agent-tracker-data`. Create a branch named `main`. You do not need to add a data file; the app can create it.
2. Open GitHub **Settings → Developer settings → Personal access tokens → Fine-grained tokens** and create a token.
3. Limit the token's repository access to only the private data repository you created.
4. Grant **Repository permissions → Contents: Read and write**. Do not grant unrelated permissions. Set an expiration and rotate the token when needed.
5. Open the deployed tracker and go to **Backup & settings → GitHub auto-save**.
6. Enter your GitHub username/organization, private data repository name, branch (`main`), and data file path (default `data/agent-tracker-data.json`). Paste the token.
7. If starting with an empty data repository, choose **Connect & upload local data**. If the remote JSON already contains your tracker records, choose **Connect & load from GitHub** instead; this replaces local records after confirmation.
8. After connection, changes are saved to local browser storage immediately and queued to GitHub after a short delay. GitHub creates a commit when the data file is updated.

The token is kept in the current browser tab's `sessionStorage` and is not written into the repository or the app's source files. It generally remains available on refresh in the same tab, but you may need to paste it again in a new tab/session. Disconnect removes the session token. Anyone who can access the active browser session may be able to use that token; do not use this setup on a shared or untrusted computer.

GitHub auto-save requires internet access. If GitHub sync fails, the app still saves locally and displays an error; export a JSON backup and reconnect/retry. Avoid editing the same data from multiple devices simultaneously, because this simple version is designed for one active editor and does not merge concurrent changes.

**Privacy note:** GitHub Pages websites are often publicly accessible even when source-code visibility or repository settings differ. Keep employee data in the separate private repository. Do not store sensitive personal or medical information in this tracker. GitHub commits retain a history of data changes, so deleted data may remain in repository history.

## Business rules

- Attendance statuses are exactly `Present`, `Weekly Off`, `CL`, and `LOP`.
- CL means approved leave.
- Absence is recorded as LOP, not as a separate status.
- Weekly Off, CL, and LOP do not generate performance flags.
- A missing daily record is a missing-entry warning; it is not inferred to be absence or LOP.
- CVR is verified orders / total chats. Orders exceeding chats are rejected for manual review.
- FRT and AHT thresholds are unset by default; configure them before using those flags.
- Consequence actions are manually recorded. The app does not automatically issue disciplinary outcomes.

## Limitations

- Data is stored in browser `localStorage` as a local copy. Clearing browser data can erase that copy; export JSON backups regularly.
- GitHub sync writes the entire tracker dataset into one JSON file. It is intended for a small team and one active editor, not concurrent multi-user collaboration.
- The weekly-off setting is used as a scheduling hint when creating an entry; for variable schedules, select attendance manually.
- The Google Fonts import is optional; the site falls back to system fonts if unavailable.
