# OpenAB Live Demo Script — BWAI Tainan 5/23

**Section**: Part 3 (25 min)
**Host machine**: M4 mac mini (jimmym4.local / ts-m4 / 100.96.176.6)
**OpenAB version**: v0.8.4-beta.2 (released 2026-05-22 11:51 Taipei)
**Discord server**: `jimmy-agents` (ID `1339233219182067744`)
**Demo channel**: `#bwai-tainan` (ID `1339233219182067747`)

---

## Pre-flight checklist (T-60 min, before talk)

```bash
# 1. Confirm M4 reachable + OrbStack up
ssh ts-m4 'orbctl status && uptime'

# 2. Pull OpenAB release artifact
ssh ts-m4 'cd ~/workspace/openab && git fetch --tags && git checkout openab-0.8.4-beta.2'

# 3. Confirm two config files exist
ssh ts-m4 'ls -la ~/workspace/openab/configs/bwai-{agy,gemini}.toml'

# 4. Confirm Discord bot tokens in env
ssh ts-m4 'test -f ~/.config/openab/bwai.env && echo OK || echo MISSING'

# 5. Pre-warm agy + gemini CLI (avoid first-call cold start)
ssh ts-m4 'agy --version && gemini --version'

# 6. Start both openab instances (tmux)
ssh ts-m4 'tmux new -d -s bwai-agy "cd ~/workspace/openab && \
  source ~/.config/openab/bwai.env && \
  ./target/release/openab --config configs/bwai-agy.toml"'
ssh ts-m4 'tmux new -d -s bwai-gemini "cd ~/workspace/openab && \
  source ~/.config/openab/bwai.env && \
  ./target/release/openab --config configs/bwai-gemini.toml"'

# 7. Watch logs in two split panes
ssh ts-m4 'tmux a -t bwai-agy'  # left pane
ssh ts-m4 'tmux a -t bwai-gemini'  # right pane

# 8. Smoke test in Discord (private channel, NOT #bwai-tainan)
#    "@agy-bot hello" → expect reply within 5s
#    "@gemini-bot hello" → expect reply within 5s

# 9. Project Discord on screen, hide token env from terminal pane

# 10. Print Discord invite QR code on slide screen
```

---

## Config files (canonical reference)

### `configs/bwai-agy.toml`

```toml
[bot]
platform = "discord"
token_env = "DISCORD_AGY_TOKEN"
allowed_guilds = ["1339233219182067744"]
allowed_channels = ["1339233219182067747"]
mention_only = true
display_name = "agy-bot"

[agent]
command = "agy-acp"
args = []
working_dir = "/home/agent"

[session]
edit_stream_interval_ms = 1500
idle_timeout_s = 600
max_sessions = 8
```

### `configs/bwai-gemini.toml`

```toml
[bot]
platform = "discord"
token_env = "DISCORD_GEMINI_TOKEN"
allowed_guilds = ["1339233219182067744"]
allowed_channels = ["1339233219182067747"]
mention_only = true
display_name = "gemini-bot"

[agent]
command = "gemini"
args = ["--acp"]
working_dir = "/home/agent"

[session]
edit_stream_interval_ms = 1500
idle_timeout_s = 600
max_sessions = 8
```

---

## Live run-of-show

### [T+0:00] Opening narrative — the PR #896 story (2 min)

**Action**: Switch projector to browser tab
**URL**: https://github.com/openabdev/openab/pull/896

**Talk track**:
> 「打開來看 — 今天早上 11:51 merge 的 PR。我凌晨兩點在 antigravity-cli issue #31 留 comment，Pahud 中午就把 agy-acp Rust adapter merge 進 OpenAB。9 小時。這就是 community-driven ecosystem 的速度。」

**Scroll to**: the canonical `[agent] command = "agy-acp"` block

---

### [T+2:00] Architecture diagram (3 min)

**Action**: Switch to slide deck (slide "架構一張圖")

**Talk track**: 解釋 Discord ↔ OpenAB ↔ stdio JSON-RPC ↔ CLI subprocess

**Key phrase**: 「Discord 是 control plane，CLI 是 execution plane。」

---

### [T+5:00] Demo A — Single bot, agy backend (5 min)

**Action**: Switch to Discord app, channel `#bwai-tainan`

**Show invite QR**: 提醒學員掃 QR 進 channel 觀戰

**Speaker types in Discord**:
```
@agy-bot 寫一個 Python function 算 fibonacci(n) 並有 memoization，附 docstring 跟一個簡單測試
```

**Expected timeline**:
- T+5:10 → bot 加上 👀 reaction (received)
- T+5:12 → bot 加上 🤔 reaction (processing)
- T+5:15 → bot post 訊息 "Working..."，開始 edit-stream
- T+5:25 → 訊息逐漸長出 code block (每 1.5s update 一次)
- T+5:45 → 最終完成，bot 加 ✅ reaction

**Speaker commentary while streaming**:
> 「看 token 一段一段進來的感覺。edit-streaming 每 1.5 秒 patch 同一則 Discord 訊息 — 比 reply chain 乾淨。」

**🚨 RISK CALLOUT**:
- 若 T+5:25 仍無 reaction → switch tmux pane，show log `tail -f`
- 若 T+5:40 仍無 response → invoke fallback (see `openab-fallback-prerecord.md`)
- 若 agy 噴錯 (e.g., workspace 未初始化) → SSH M4 跑 `agy-acp --help` 確認 binary，必要時直接切 Demo B

---

### [T+10:00] Demo B — gemini --acp same prompt (5 min)

**Speaker types**:
```
@gemini-bot 寫一個 Python function 算 fibonacci(n) 並有 memoization，附 docstring 跟一個簡單測試
```

**Expected**: response within 10-15s, longer explanation than agy

**Speaker commentary (side-by-side scroll)**:
> 「同樣 prompt — agy 走 minimal，gemini 補了教學註解。不是誰贏，是你今天 task 想要哪一種協作風格。」

**Talking points (pick 2)**:
- Type hint 完整度
- 有沒有 unit test
- Edge case 處理 (n=0, n<0)
- 解釋深度

**🚨 RISK CALLOUT**:
- 若 gemini quota exhausted → 切 Demo C 直接做 agy single-thread followup demo

---

### [T+15:00] Demo C — Multi-agent showdown ⭐ (7 min)

**Speaker types — Round 1 (agy 提案)**:
```
@agy-bot 設計一個 minimal URL shortener service，用 SQLite 存 mapping，Python FastAPI，給我 schema 跟 endpoints
```

**Wait** ~30-45s for response

**Speaker types — Round 2 (gemini 反駁)**:
```
@gemini-bot 上面 @agy-bot 提的 URL shortener 設計有哪三個明顯問題？要 production-grade 觀點
```

**Speaker commentary while gemini 跑**:
> 「Discord thread 就是 shared memory。我沒有手動複製 agy 的輸出給 gemini — Discord scroll context 它自己看得到。」

**Wait** ~30s

**Speaker types — Round 3 (agy 改進)**:
```
@agy-bot 針對 @gemini-bot 剛剛提的三點，給我 refactor 版本
```

**Closing commentary**:
> 「這就是 agent-to-agent dialogue — 兩個不同 vendor 的 CLI，在同一個 Discord channel，看著彼此的輸出做 iterative refinement。」

**🚨 RISK CALLOUT**:
- 若 Round 2 gemini 沒抓到 context → 手動補一句 quote agy 輸出
- 若任一 round timeout → 直接做 narrative wrap，跳到 Demo D

---

### [T+22:00] Demo D — Mobile push demo (3 min)

**Action**:
1. 講師把筆電蓋上 (or 切到桌面隱藏 Discord)
2. 拿出手機，舉高給觀眾看
3. 等待 — Discord push notification 應該還在
4. 點開 Discord app on phone
5. Type on phone (大聲念出來):
   ```
   @agy-bot 把剛剛那個 fibonacci 改寫成 async def，加 asyncio.sleep(0) 讓出 event loop
   ```
6. Send
7. 把手機翻給觀眾看 bot reaction
8. 等 ~30s
9. 手機收到完成通知，showcase

**Speaker close**:
> 「我筆電蓋著，M4 在家裡。我在手機上 push 一行訊息 — 那邊 agent 就開工。Tailscale + OpenAB + Discord = 我隨身帶的 home lab。」

**🚨 RISK CALLOUT**:
- 若手機 Discord 沒通知 → 手動打開 Discord app，still works
- 若 M4 wifi 斷 → narrative recovery：「假設網路 OK 的話...」秀預錄影片 (fallback doc)

---

### [T+25:00] Hand-off

**Speaker close**:
> 「Repo openabdev/openab，tag 0.8.4-beta.2，自己回家裝。下一段我們讓你動手。」

切到 Part 4 (學員 hands-on lab)

---

## Discord channel housekeeping

**Before talk**:
- Pin invite link message
- Pin `#rules` reminder (請勿 @ 其他 bot / 請勿洗頻)
- Set slow-mode 5s on `#bwai-tainan` to prevent flood

**During talk**: 助教監控 channel，若有學員亂 @ bot 立刻提醒

**After talk**: 保留 channel 7 天供 follow-up 問題

---

## Total timing

| T | Section | Duration |
|---|---------|----------|
| 0:00 | PR #896 story | 2 min |
| 2:00 | Architecture | 3 min |
| 5:00 | Demo A (agy) | 5 min |
| 10:00 | Demo B (gemini) | 5 min |
| 15:00 | Demo C (showdown) | 7 min |
| 22:00 | Demo D (mobile) | 3 min |
| 25:00 | Hand-off | — |

**Buffer**: 0 min built-in. Demo C can compress to 5 min if running hot.
