# ⚙️ Maintenance Guide

How the automation works, how to run and debug it, and what to do when one of
the external badge services misbehaves.

---

## 1. The automation: recent-activity feed

The workflow `.github/workflows/update-activity.yml` keeps the block between
`<!--START_SECTION:activity-->` and `<!--END_SECTION:activity-->` in
`README.md` populated with recent public GitHub activity.

### One-time installation

A ready-to-use copy ships at
[`docs/examples/update-activity.workflow.yml`](examples/update-activity.workflow.yml).
To install it you need an account or token with GitHub's **Workflows**
permission (the file touches `.github/workflows/`, which some automation
tokens cannot modify):

1. Copy `docs/examples/update-activity.workflow.yml` →
   `.github/workflows/update-activity.yml`.
2. **Delete** the legacy `.github/workflows/update_readme_activity.yml`
   (it pins `jamesgeorge007/github-activity-readme@v0.1.8`, an action built on
   the retired node12 runtime that cannot run on today's runners).
3. Push, then run the workflow once from the **Actions** tab (*Update GitHub
   Activity* → *Run workflow*).

### Why this custom updater exists

The README previously pinned `jamesgeorge007/github-activity-readme@v0.1.8`.
That action could not run on modern GitHub runners (retired Node runtimes) and
never produced a successful run. The replacement:

- uses only the **official `actions/github-script@v9`** action (node24) and the
  REST API — no third-party runner code;
- also lists **push events** (the most common activity for this account) with
  consecutive pushes to the same repo merged into one line;
- **rewrites the whole region** between the markers, so it can never leave
  stale entries behind;
- skips commits when nothing changed, and otherwise creates a rare
  **keep-alive empty commit** (~every 55+ idle days) so GitHub never
  auto-disables the scheduled workflow (GitHub disables schedules after ~60
  days of repository inactivity);
- is fully testable — the logic was validated against real GitHub event
  payloads before shipping.

### Schedule

- **cron:** `0 */6 * * *` (every 6 hours, UTC)
- **manual:** Repository → **Actions** → *Update GitHub Activity* →
  **Run workflow**

### What it lists

Push events, opened/closed issues, comments, opened/merged PRs, releases,
stars (⭐) and forks — newest first, up to **5 lines**. Events are read from
the public events API (`GET /users/<username>/events/public`, ~90 days of
history). When nothing is available it shows a friendly placeholder line.

### Editing the behavior

All knobs live at the top of the workflow's `script` block:
`MAX_LINES`, `INCLUDED_EVENT_TYPES` (add/remove event types), and
`NO_ACTIVITY_LINE`. The commit identity is the standard
`github-actions[bot]`, so the feed's commits are never attributed to the
profile owner.

### Important constraints

- Keep the two marker comments in `README.md` **exactly** as they are. If they
  are removed the workflow fails with a clear message (and your profile just
  stops updating — nothing else breaks).
- Do not hand-edit entries between the markers; the next run overwrites them.
- Commits made by this workflow (via the default `GITHUB_TOKEN`) do **not**
  trigger further workflow runs, so there is no update loop.

### Troubleshooting

| Symptom | Fix |
|---------|-----|
| Workflow never ran / no runs listed | GitHub only runs schedules for repos with activity in the last 60 days. Push a commit (or any change) and use **Run workflow** to kick it off; it then runs on schedule. |
| “Activity markers not found” failure | Someone removed the `<!--START_SECTION:activity-->`/`<!--END_SECTION:activity-->` comments — restore them and re-run. |
| Run failed with an API error | GitHub API hiccup; re-run the workflow manually (Actions tab → workflow → Run workflow). |
| Section shows the placeholder line | There were no listable public events in the last ~90 days (e.g. everything was pushed to private repos). It resolves itself as soon as public activity happens. |
| Rate-limit errors | Very unlikely: the workflow makes ≤ 5 API calls per run. |

---

## 2. Service health

The README renders images from several free badge services. This table was
last verified **2026-09-04** (by requesting each endpoint).

| Service (endpoint in README) | Status | Fallback / notes |
|------------------------------|--------|------------------|
| `komarev.com/ghpvc` (profile views) | ✅ up | Widely used view counter |
| `img.shields.io` (social badges) | ✅ up | Official |
| `skillicons.dev` (toolbox icons) | ✅ up | Official |
| `github-readme-stats-fast.vercel.app` (stats / top-langs) | ✅ up | Community mirror of [anuraghazra/github-readme-stats](https://github.com/anuraghazra/github-readme-stats) with short caching |
| `streak-stats.demolab.com` (streak) | ✅ up | **Official** current host of [github-readme-streak-stats](https://github.com/DenverCoder1/github-readme-streak-stats) (the old `github-readme-streak-stats.herokuapp.com` host is retired) |
| `github-profile-trophy.screw-hand.vercel.app` (trophies) | ✅ up | Community mirror; the original `github-profile-trophy.vercel.app` deployment is **paused by its owner** and returns 503 |
| `github-profile-summary-cards.vercel.app` (profile summary) | ✅ up | Official host of [vn7n24fzkq/github-profile-summary-cards](https://github.com/vn7n24fzkq/github-profile-summary-cards) |

Also verified **down** (do not switch back to these):

- `github-readme-stats.vercel.app` — the original stats deployment is paused (503 `DEPLOYMENT_PAUSED`).
- `github-profile-trophy.vercel.app` — paused (503 `DEPLOYMENT_PAUSED`).
- `github-readme-activity-graph.vercel.app` — paused (503 `DEPLOYMENT_PAUSED`).

### Re-checking a service

```bash
# A healthy SVG endpoint returns 200 with an image/svg+xml body:
curl -sIL "https://streak-stats.demolab.com/?user=ketandutt&theme=gruvbox" | head -5

# Vercel apps that are paused by their owner return 503 "DEPLOYMENT_PAUSED".
```

If a mirror disappears, options are: switch the README back to whichever
official deployment is alive, self-host the tool (all three are easy Vercel
deploys), or drop the card. When changing endpoints, keep the theme parameter
consistent with the rest of the profile (currently `gruvbox`).

---

## 3. Keeping the README itself healthy

- **Links:** demo links point at GitHub Pages sites (`<user>.github.io`).
  Before adding one, confirm Pages is on for that repo (Settings → Pages).
- **Alt text:** every `<img>` should carry a descriptive `alt`.
- **Formatting:** GitHub flavored markdown only; HTML allowed for centering
  and `<details>` blocks. Avoid raw `<font>`/color HTML (stripped by GitHub).
- **Testing changes:** push to a branch and open a PR — the profile only
  renders the README on the default branch (`master`), so PRs let you preview
  nothing on the live profile until merge. Consider rendering the file locally
  (any markdown previewer) for a quick check.

---

## 4. Changing the default branch / renaming the repo

- The profile only renders when the repo is named exactly `KetanDutt` and the
  README sits on the default branch.
- If the default branch is ever renamed, the workflow keeps working (it
  resolves the branch from `context.ref`), but scheduled runs must be
  re-enabled from the Actions tab afterwards.
