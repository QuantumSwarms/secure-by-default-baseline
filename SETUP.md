# Setup Note (for builders / subcontractors)

> **Read this first.** This repo is a **public, client-facing offering page** for EpochCore LLC's consulting practice. It is **not** a code repo — no source ships here.

## What this repo *is*
- A polished landing page for a productized consulting offer.
- The single source of truth for scope, pricing, deliverables, intake, and timeline for this offering.
- A funnel: prospects open an **Engagement Inquiry** issue → we triage → kickoff.

## What this repo *is not*
- Not a place for client source code, credentials, NDA material, or proprietary engagement artifacts. Those live in private repos / Notion / Drive linked from internal docs only.

## Folder map
```
.
├── README.md                  # The pitch — what we sell, who it's for, how to engage
├── LICENSE                    # Proprietary, all rights reserved
├── SECURITY.md                # Vuln reporting policy
├── CONTRIBUTING.md            # Internal collab rules
├── CHANGELOG.md               # Versioned offering changes
├── SETUP.md                   # This file
├── docs/
│   ├── scope.md               # In-scope / out-of-scope per tier
│   ├── pricing.md             # Tiers, what's included, payment terms
│   ├── intake-checklist.md    # What we need from the client at kickoff
│   └── deliverables.md        # Concrete artifacts produced
├── assets/                    # Diagrams, logos, screenshots used by README
└── .github/
    ├── ISSUE_TEMPLATE/        # Engagement inquiry + feedback
    ├── PULL_REQUEST_TEMPLATE.md
    ├── workflows/ci.yml       # Doc lint + required-files check + link check
    └── dependabot.yml         # GH Actions version bumps
```

## Working in this repo
1. Branch: `feat/<short-name>` from `main`.
2. Edit Markdown only — keep tone direct, outcome-led, SMB-readable (avoid jargon).
3. Conventional Commits (`feat:`, `docs:`, `fix:`, `chore:`).
4. PR → CI must pass → squash-merge.

## Recommended first edits before going live
- [ ] Replace any `TODO` placeholders in `README.md` with finalized copy.
- [ ] Confirm pricing in `docs/pricing.md` matches your current rate card.
- [ ] Add 1–2 case-study teasers (sanitized) to `README.md` if available.
- [ ] Set repo description and topics on GitHub (e.g., `consulting`, `smb`, `huntersville-nc`).
- [ ] Enable branch protection on `main` (require PR + status checks).
- [ ] Add a custom social preview image to `assets/` and set it in repo settings.

## Contact
**john@epochcoreqcs.com** — EpochCore LLC, Huntersville, NC.
