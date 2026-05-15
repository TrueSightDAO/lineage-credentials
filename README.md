# lineage-credentials

The **data repo** for the TrueSight DAO credentialing platform.

This repo holds:
- Each program's manifest (lineage authority, attestation types, source pages).
- Every practitioner's signed events (practice events under `programs/<p>/pk-<hash>/practice/`, qualification + attestation events under `attestations/` once v2 lands).
- Pre-rendered per-person CVs under `_cache/cv/<slug>.{json,md,pdf}`.
- The directory index and pk-hash → slug alias map.

The matching **engine repo** is [`lineage-engine`](https://github.com/TrueSightDAO/lineage-engine) — Python scripts that aggregate events into CVs, Grok prompts for the human-readable summaries, and the WeasyPrint PDF templates.

Design doc: [`agentic_ai_context/CREDENTIALING_PLATFORM.md`](https://github.com/TrueSightDAO/agentic_ai_context/blob/main/CREDENTIALING_PLATFORM.md).

## Layout

```
lineage-credentials/
├── README.md
├── programs/
│   └── capoeira-tribo-mirim/
│       └── manifest.json
└── _cache/
    ├── index.json           # all members for the directory page
    ├── aliases.json         # pk-hash → slug
    └── cv/
        ├── <slug>.json      # pre-rendered CV (consumed by truesight.me)
        ├── <slug>.md        # human-readable testimonial
        └── <slug>.pdf       # job-application-grade PDF (added by build)
```

Per-person practice events under `programs/<p>/pk-<hash>/practice/` get added by the GAS event processor when the capoeira webapp posts a `[PRACTICE EVENT]` to Edgar. None exist yet — the MVP capoeira webapp work lands in a subsequent PR.

## Status

- **2026-05-14** — scaffolding committed: initial capoeira-tribo-mirim manifest + migrated Fatima + Emelin CVs from tokenomics.

The GitHub Action that auto-rebuilds the cache on push lands in a subsequent PR (it checks out `lineage-engine` and runs the cache builder).
