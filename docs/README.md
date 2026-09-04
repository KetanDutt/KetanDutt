# 📖 Profile Docs

Everything you need to maintain this GitHub profile repository (the special
`KetanDutt/KetanDutt` repo that renders as the profile page).

| Document | What it covers |
|----------|----------------|
| [🛠️ Customization](CUSTOMIZATION.md) | Editing every section of the README: badges, cards, theme, skills icons, projects, links |
| [⚙️ Maintenance](MAINTENANCE.md) | How the GitHub Actions automation works, running & debugging it, and the health of the external badge services |
| [🛡️ Security](../SECURITY.md) | How to report a vulnerability in this repository |

## Repository layout

```
.
├── README.md                            # The profile page (edit freely)
├── SECURITY.md                          # Vulnerability reporting policy
├── docs/
│   ├── README.md                        # ← you are here
│   ├── CUSTOMIZATION.md                 # How to edit the profile content
│   ├── MAINTENANCE.md                   # Automation & service-health reference
│   └── examples/
│       └── update-activity.workflow.yml # Drop-in GitHub Actions workflow
└── .github/
    └── workflows/
        └── update_readme_activity.yml   # ⚠️ LEGACY — replace with the file in
                                         #    docs/examples (see MAINTENANCE.md §1)
```

## Quick rules of thumb

1. **The activity block is machine-managed.** The lines between
   `<!--START_SECTION:activity-->` and `<!--END_SECTION:activity-->` are
   rewritten by the GitHub Actions workflow described in
   [MAINTENANCE.md](MAINTENANCE.md). Do not hand-edit the entries between the
   markers — the next run overwrites them. (The rest of the file is yours.)
2. **Use https.** Every image/link in the README should use `https://` — GitHub
   renders `http://` images as broken in most contexts.
3. **Add an `alt` attribute** to every `<img>` you add; it keeps the profile
   readable when an image is slow or blocked.
4. **Test links before you merge.** Most demo links point at GitHub Pages
   sites; check the repo has Pages enabled (repo → Settings → Pages) before
   linking to it.
