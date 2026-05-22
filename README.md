# agy-cli-quickstart

> Hands-on materials for Google **Antigravity CLI** (`agy`) — the terminal-first coding agent that's succeeding `gemini-cli` from 2026-06-18.

**Maintainer**: [Jimmy Liao](https://github.com/jimmyliao) · Google AI GDE (Taiwan)
**Status**: living repo — first content is the 2026-05-23 BWAI Tainan workshop, more to come.

---

## What is `agy`?

`agy` is the standalone Antigravity CLI from Google — a terminal-first coding agent (think `claude` / `codex` for Google's ecosystem). Install once:

```bash
# macOS / Linux / Cloud Shell
curl -fsSL https://antigravity.google/cli/install.sh | bash

# Windows PowerShell
irm https://antigravity.google/cli/install.ps1 | iex
```

Official download page: <https://antigravity.google/download>

Key flags to know:
- `agy -p "prompt"` — non-interactive print mode (great for scripts & CI)
- `agy -i` — interactive with an initial prompt
- `agy -c` — continue the most recent conversation
- `agy --sandbox` — terminal restrictions (safer for experiments)
- `agy --dangerously-skip-permissions` — auto-approve all tool calls (unattended automation)

## Why this repo?

Three pain points it solves:

1. **Migration from `gemini-cli`** — Pro/Ultra free tier sunsets 2026-06-18; `agy` is the official successor. This repo shows side-by-side command translations.
2. **Cloud Shell quickstart** — get from zero to first `agy` chat in 5 minutes with no local install.
3. **Ecosystem integrations** — how `agy` fits into Discord/Slack brokers (OpenAB), IDEs (Zed via ACP), and multi-agent stacks (ADK).

## Repo Layout

```
.
├── README.md                              ← you are here
└── events/
    └── bwai-tainan-2026-05-23/            ← first workshop materials
        ├── outline.md                     ← 75-min agenda
        ├── slides/                        ← 5 parts, Marp-compatible markdown
        ├── labs/                          ← Cloud Shell student handout
        ├── demos/                         ← OpenAB live demo script + fallback
        └── assets/                        ← screenshots / diagrams
```

## Events

| Date | City | Event | Folder |
|------|------|-------|--------|
| 2026-05-23 | Tainan | [BWAI Tainan](https://gdg.community.dev/events/details/google-gdg-tainan-presents-build-with-ai-x-wu-yue-chang-tan-suo-vibe-coding-yu-google-gemini-de-wu-xian-ke-neng/) | [`events/bwai-tainan-2026-05-23/`](events/bwai-tainan-2026-05-23/) |

## Related links

- Google blog — [Gemini CLI → Antigravity CLI transition](https://developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli/)
- [`google-antigravity/antigravity-cli`](https://github.com/google-antigravity/antigravity-cli) — official source repo
- Community issue tracking ACP support — [issue #31](https://github.com/google-antigravity/antigravity-cli/issues/31)
- [OpenAB](https://github.com/openabdev/openab) — chat-platform agent broker; `agy-acp` adapter merged in [PR #896](https://github.com/openabdev/openab/pull/896) on 2026-05-22
- Predecessor talk — [BwAI Taichung 2026-05-09](https://bwai0509.web.app/)

## License

Content: **CC BY 4.0** — adapt freely, attribute Jimmy Liao.
Code snippets: **MIT** unless otherwise noted.
