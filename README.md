# Workbench

Track testing, feedback, features and ideas for all your projects. Everything lives in plain Markdown files that your project agents can read and write.

## Start
- **Installed app (recommended):** open `https://<your-username>.github.io/workbench/` in Chrome or Edge, then choose *Install Workbench* from the browser menu or the address bar. The installed app remembers folder access.
- **Local file:** double-click `Workbench.html`. It works offline, but asks for folder access each time you open it.

Then:
1. Choose this folder as your workspace. New ideas are saved here in `ideas/`.
2. Use **Add project folder** for each project. Workbench creates a `proj_management/` folder inside it.
3. Point each project's agent at `PROJ_MANAGEMENT_SPEC.md`, or use the *proj-management-testing* skill.

To try Workbench with sample data, add `?demo` to the end of the address.

## Publishing (GitHub Pages, via VS Code)
This folder is a **public** repo, but only the app files are tracked. `.gitignore` keeps `proj_management/`, `ideas/`, `examples/` and anything else private.

First time:
1. Open this folder in VS Code and go to Source Control.
2. Click *Publish to GitHub* and choose a **public** repo named `workbench`.
3. On github.com, open the repo and go to **Settings → Pages**. Set *Deploy from a branch* to `main` and `/ (root)`, then save. After about a minute the app is live at `https://<your-username>.github.io/workbench/`.

After that, whenever the app changes:
1. Commit in VS Code's Source Control panel and click *Sync*.
2. The installed app picks up the new version the next time it starts. Restart it once more if needed.

## What's in this folder
| Item | Published | What it is |
|---|---|---|
| `Workbench.html`, `index.html` | yes | The app, plus a redirect so the bare address opens the app |
| `manifest.webmanifest`, `sw.js`, `icons/` | yes | Lets the app be installed and work offline |
| `PROJ_MANAGEMENT_SPEC.md` | yes | The file format your agents follow |
| `tools/*.reg` | yes | One-time helper so **Open folder** opens File Explorer |
| `CLAUDE.md` | yes | Notes for Claude sessions working on Workbench |
| `proj_management/` | no | Workbench's own features and tests (tracked in Workbench) |
| `ideas/`, `examples/` | no | Your ideas and the example project |
