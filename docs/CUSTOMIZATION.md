# 🛠️ Customization Guide

This guide explains how to edit every part of the profile README. Start with
the [quick rules](../docs/README.md#quick-rules-of-thumb), then read the
section you care about.

---

## 1. Header, tagline and links

`README.md` starts with three centered blocks: the name (`<h1>`), the tagline
(`<h3>`) and the quick links. Edit the text between the tags — nothing else
needs to change.

## 2. Badge row (profile views, followers, stars)

The first `<p align="center">` after the links contains three badges:

| Badge | Service | Meaning |
|-------|---------|---------|
| `komarev.com/ghpvc` | Komarev | Profile view counter (per-user, global) |
| `img.shields.io/github/followers/...` | Shields.io | Follower count; renders as a *Follow* button |
| `img.shields.io/github/stars/...` | Shields.io | Star count of your public repos |

Shields.io badges are extremely configurable. Examples:

```md
<!-- custom label & color -->
![Followers](https://img.shields.io/github/followers/ketandutt?label=Followers&style=flat-square&color=orange)
<!-- one line, no label -->
![Stars](https://img.shields.io/github/stars/ketandutt?style=flat-square)
```

Browse all options at <https://shields.io> or in its
[badge specification](https://shields.io/badges).

## 3. About Me & fun facts

Plain markdown bullets — just edit them. When you list projects, keep claims
accurate and prefer linking something playable over a bare repo name.

## 4. Demo Projects table

Columns: **Project · Play it · Repo · Built with**.

- Every “Play it” link should point at a live GitHub Pages site
  (`https://<user>.github.io/<repo>/`) or another hosted demo.
- If the demo lives in a different repo than the source (e.g. Unity WebGL
  builds are often committed to a separate `<Name>_Built` repo), link the
  source repo in **Repo**.
- If a source repo is deleted but the demo still works, keep the row and
  explain it in the footnote, like the Smarty Ants row does.
- To add more rows to the collapsed “More experiments & tools” list, mirror
  the `<details>` block structure already in the file.

## 5. “My Toolbox” icons

Uses [skillicons.dev](https://skillicons.dev). Edit the comma-separated list:

```md
![My Skills](https://skillicons.dev/icons?i=unity,godot,unreal,cs,python,js,ts,nodejs,html,css,php,mysql,flutter,arduino,git&theme=dark)
```

- Icon names: see <https://skillicons.dev> (hover a tile or inspect its URL).
- Add `&perline=N` to force `N` icons per row.
- Remove `&theme=dark` for light icons.

## 6. GitHub Stats cards (theme & options)

The cards use one **theme value everywhere: `gruvbox`**, which gives the
profile a single coherent palette. To switch to another look, change the
`theme=` parameter on every card:

| Section | Source |
|---------|--------|
| Profile summary (wide card) | `github-profile-summary-cards.vercel.app` — themes: `default`, `dark`, `gruvbox`, `github_dark`, … |
| Stats & top-languages cards | `github-readme-stats-fast.vercel.app` (community mirror of anuraghazra/github-readme-stats) — see its [README](https://github.com/Pranesh-2005/github-readme-stats-fast) for every option (`show_icons`, `rank_icon`, `hide`, `count_private`, …) |
| Streak card | `streak-stats.demolab.com` (official host of DenverCoder1/github-readme-streak-stats) — themes + options in its [README](https://github.com/DenverCoder1/github-readme-streak-stats) |
| Trophies | `github-profile-trophy.screw-hand.vercel.app` (community mirror; the original project's Vercel deployment is currently paused) |

Notes:

- Keep the small disclaimer under the language card — repo metrics are not
  proficiency.
- If a card service is down, see the [service health table](MAINTENANCE.md#service-health)
  for the known-good alternatives before switching endpoints around.

## 7. Get in Touch buttons

Standard [Shields.io](https://shields.io) buttons — change the label text,
colors and the target URL. To add a channel (itch.io, YouTube, X, …), pick its
`logo=` slug from shields.io, e.g.:

```md
[![itch.io](https://img.shields.io/badge/-My%20itch.io-fa5c5c?style=flat-square&logo=itch.io&logoColor=white)](https://yourname.itch.io)
```

## 8. Layout tips that keep the profile fast & clean

- **Prefer one blank line** between blocks; GitHub renders consecutive
  blank lines inside HTML blocks unexpectedly.
- **Don’t hotlink big images** into the README; every image adds page weight.
  The cards above are generated SVGs and are fine.
- **Center wide elements** with `<p align="center">` instead of many spaces.
- **HTML `<details>`** is supported and great for “more” sections.
- **Emoji-in-heading anchors**: GitHub strips the emoji when generating
  section anchors (`## 🚀 Demo Projects` → `#demo-projects`), so links into
  the profile like `https://github.com/KetanDutt#demo-projects` work.
