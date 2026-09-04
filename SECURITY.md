# Security Policy

## Supported versions

This repository is a GitHub profile page — there are no software releases and
no supported-version matrix. Fixes are applied to the `master` branch.

## Reporting a vulnerability

This repository contains only profile content (a `README.md`), static
documentation and a small GitHub Actions workflow. It has no runtime code, no
dependencies to exploit and no data of its own. Risks are therefore limited to
the third-party badge/image services linked from the README.

If you still find something you believe is a security issue:

1. **Do not open a public issue** describing the exploit.
2. Report it privately on GitHub:
   **https://github.com/KetanDutt/KetanDutt/security/advisories/new**
   (Security tab → *Report a vulnerability*), or email the repository owner.

You can expect an acknowledgement within a few business days.

## Things we already do

- The automation commits with the `github-actions[bot]` identity and the
  minimal `contents: write` permission.
- `GITHUB_TOKEN` is used with no additional scopes.
- The README uses `https://` links only.
- Third-party actions are pinned to released majors (`actions/github-script@v9`)
  rather than floating branches.
