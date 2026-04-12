# RELEASES

Each release lives in its own folder: `pg-[NNN]-[artist-slug]-[title-slug]/`

```
releases/
├── _template/          ← copy this for every new release
├── pg-001-[...]/
├── pg-002-[...]/
└── ...
```

## Starting a release

See `workflows/release-workflow.md` for the full step-by-step process.

Quick start:
1. Copy `_template/` — rename to `pg-[NNN]-[artist]-[title]`
2. Fill in `release.md` — this is the master record
3. Run `./pg generate <path>` to create `release.json`

Status moves: `DEVELOPMENT → APPROVED → IN_PRODUCTION → RELEASED`
