# Part 3 — OpenAB × agy / gemini-cli Live Demo

**BWAI Tainan 2026-05-23 | Jimmy Liao, Google AI GDE**
**Section length**: 25 min | **Mode**: Instructor live demo (學員 Discord 觀戰)

---

## 今天早上的新聞 (Hot off the press)

**2026-05-22 02:00 Taipei** — 我在 `google-antigravity/antigravity-cli` issue #31 留下 GDE comment
**2026-05-22 11:51 Taipei** — Pahud merge PR #896，OpenAB v0.8.4-beta.2 released

**9 小時。** 一個 issue comment 變成上游 Rust adapter。

這就是今天要 demo 的主角：`agy-acp`

---

## What is OpenAB?

**Open Agent Broker** — https://github.com/openabdev/openab

- Rust harness：Discord / Slack / Telegram ↔ ACP-compatible coding CLIs
- 透過 stdio JSON-RPC 跟 CLI subprocess 講話
- 一個 thread = 一個 CLI process (session pool)
- 同一個 channel 可以多 bot → **dual-engine agent showdown**

**Supported backends (--acp mode)**:
claude · gemini · codex · kiro · copilot · opencode · hermes · grok · **agy** (NEW)

---

## 架構一張圖

```
   ┌──────────────┐
   │   Discord    │  ← 你的手機、瀏覽器、桌面
   │  / Slack /   │
   │  Telegram    │
   └──────┬───────┘
          │ Bot Gateway
          ▼
   ┌──────────────┐
   │    OpenAB    │  Rust harness (M4 mac mini)
   │   (broker)   │  Session pool · routing · streaming edit
   └──────┬───────┘
          │ stdio JSON-RPC (ACP)
          ▼
   ┌──────────────────────────────────┐
   │  agy-acp │ gemini --acp │ claude │
   └──────────────────────────────────┘
```

Discord = **control plane**. CLI = **execution plane**.

---

## 為什麼這件事重要？

1. **Ecosystem velocity** — 社群 PR 24 小時內 merge，比官方 roadmap 快
2. **Best-tool-per-task** — agy 給你 Antigravity workspace、gemini 給你 1M context、claude 給你精細 edit
3. **Remote home lab** — 我手機 push 一行訊息，M4 在家裡幫我跑 agent
4. **Vendor-neutral** — 不用 lock-in 任何一家

---

## Demo A — Single bot, agy backend

**Channel**: `#bwai-tainan` (邀請連結會發在現場 QR code)
**Bot**: `@agy-bot`

```
@agy-bot 寫一個 Python function 算 fibonacci(n) 並有 memoization
```

Watch for:
- 👀 → 🤔 reaction (bot picked up)
- Edit-streaming 每 ~1.5s 更新訊息
- 最終 code block

---

## Demo B — Same prompt, gemini --acp backend

```
@gemini-bot 寫一個 Python function 算 fibonacci(n) 並有 memoization
```

**對比觀察點**:
- 解釋語氣 (agy 偏簡 / gemini 偏教學)
- Type hint 風格
- 有沒有附 unit test

**Talking point**: 不是誰贏，是 task fit。

---

## Demo C — Multi-agent showdown ⭐

**同一個 channel，兩隻 bot 在 thread 內互嗆**

```
@agy-bot 幫我設計一個 URL shortener，用 SQLite 存
（agy 回完之後）
@gemini-bot 上面這個設計有什麼問題？提三個改進
（gemini 回完之後）
@agy-bot 針對 gemini 提的，給我 refactor 版本
```

**這就是 agent-to-agent dialogue**。Discord thread = 共享 memory。

---

## Demo D — 從手機 push

**我把筆電蓋上。**

- 拿出手機
- Discord push 通知亮起
- 點開、敲一行：「把剛剛那個 fibonacci 改成 async」
- M4 接到、agent 開工
- 30 秒後手機收到結果

**這就是 remote home lab via chat**。
你的 home server + Tailscale + OpenAB = 隨身 agent。

---

## Recap — 帶回家三件事

1. **Vibe Coding 不只是寫 code** — 是會跟 ecosystem 一起動
2. **雙引擎不是二選一** — 是 best-tool-per-task
3. **Discord 是新的 terminal** — control plane 在哪，工作就在哪

**Repo**: github.com/openabdev/openab
**Tag**: `openab-0.8.4-beta.2`
**Docs**: `docs/antigravity.md`

---

## Q & A → Part 4

下一段：學員自己動手裝 OpenAB local
