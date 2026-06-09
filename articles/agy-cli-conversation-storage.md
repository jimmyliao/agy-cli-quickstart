# Where does the Antigravity CLI (`agy`) store your conversations?

> A hands-on look at how `agy` persists and resumes conversations — useful if you
> want to script around it, build tooling, or just understand the terminal agent
> you're trusting with your code.
>
> 探一下 Google Antigravity CLI (`agy`) 怎麼存/還原對話。逆向自 **agy 1.0.6**（macOS）。

---

## TL;DR

- Data home is **`~/.gemini/antigravity-cli/`** (not `~/.antigravity/`, which only holds IDE config).
- Each conversation is one file in **`conversations/<UUID>.db`** — a **SQLite** database (the `.db` format is new in recent builds; older conversations are `<UUID>.pb` protobuf).
- The filename UUID **is** the conversation id.
- Resume with **`agy --conversation <UUID>`** (or `agy -c` / `--continue` for the most recent).
- **`cache/last_conversations.json`** maps each working directory to its latest conversation id.

---

## Finding the resume mechanism

Start with the CLI's own help — it tells you the model:

```bash
agy --help
```

```
  -c, --continue        Continue the most recent conversation
  --conversation        Resume a previous conversation by ID
  -p, --print           Run a single prompt non-interactively and print the response
  -i, --prompt-interactive  Run an initial prompt interactively and continue the session
```

So conversations have **IDs**, and you resume by ID. The next question is: where do those IDs and their data live?

## The data home

`~/.antigravity/` is a red herring — it only contains `extensions/` and `argv.json` (IDE config). The CLI's real home is:

```bash
ls ~/.gemini/antigravity-cli/
# bin/  brain/  builtin/  cache/  conversations/  knowledge/  log/
# mcp/  plugins/  updater/  history.jsonl  settings.json  keybindings.json ...
```

## Conversations are SQLite databases

```bash
ls -lt ~/.gemini/antigravity-cli/conversations/
# ff3e72b0-...-75de8068c296.db   <- newest (SQLite, current format)
# 0f35c3c6-...-2e941b444ee9.pb   <- older (protobuf, legacy format)
# ...
```

Each file is one conversation; the UUID in the filename is the id you pass to
`--conversation`. Recent `agy` builds write **SQLite** (`.db`); older conversations
remain as **protobuf** (`.pb`). Both formats coexist — old conversations aren't
migrated.

Peek at the schema:

```bash
sqlite3 ~/.gemini/antigravity-cli/conversations/<UUID>.db ".tables"
# battle_mode_infos  executor_metadata  gen_metadata  parent_references
# steps  trajectory_meta  trajectory_metadata_blob
```

- `trajectory_meta` → `trajectory_id | conversation_id | count | timestamp` (handy for ordering by recency).
- `steps` → the turn-by-turn record. Columns like `step_payload` are **protobuf blobs**, so the message text isn't plain SQL-readable — you'd decode the protobuf (or fall back to grepping printable strings) to recover prompts.

## Mapping a conversation to its directory

`agy` remembers the latest conversation per working directory:

```bash
cat ~/.gemini/antigravity-cli/cache/last_conversations.json
```

```json
{
  "/Users/me/projects/web-app": "ff3e72b0-30af-4b76-94ed-75de8068c296",
  "/Users/me/projects/api":     "2e536394-ba41-4d9b-a18a-25f816d3b253"
}
```

Invert this map and you get **id → directory**, i.e. where to `cd` before resuming
(the same launch-dir convention you see in other coding CLIs). Note it only records
the *latest* conversation per directory.

## Putting it together

To list and resume your `agy` conversations you need three things, all available
without running the agent:

| Field | Source |
|-------|--------|
| **id** | filename stem in `conversations/` |
| **mtime** | file mtime (or `trajectory_meta.timestamp`) |
| **directory** | invert `cache/last_conversations.json` |
| **resume** | `agy --conversation <id>` |

The only hard part is a human-readable **title / last prompt** — that lives inside
the protobuf blobs in `steps`, so it needs decoding rather than a plain query.

---

## Why this matters

Once you know the storage model, you can build tooling around it — a session
browser, a "resume my last conversation in this repo" shortcut, backups, or
cross-machine sync. (That's exactly the adapter I'm adding to my own
[`ccs`](https://github.com/jimmyliao/agent-cli-sessions) session browser.)

*Reverse-engineered from agy 1.0.6 on macOS, 2026-06. Paths/IDs are illustrative.*
