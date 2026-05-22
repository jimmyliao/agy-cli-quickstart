# BWAI Tainan 2026-05-23 — 講者準備索引

**講者**：Jimmy Liao（Google AI GDE, 台灣）
**主題**：Vibe Coding × Gemini 無限可能 — 從 Gemini CLI 接班到 Antigravity Ecosystem
**日期**：2026-05-23（明天）
**地點**：台南 GDG Build with AI
**時長**：~75 min talk + Q&A
**事件頁**：https://gdg.community.dev/events/details/google-gdg-tainan-presents-build-with-ai-x-wu-yue-chang-tan-suo-vibe-coding-yu-google-gemini-de-wu-xian-ke-neng/

---

## 一句話定位

> 「今早我留 GitHub issue comment，中午整個 OpenAB 生態接上來 — 這就是 Vibe Coding：你不是在追 ecosystem，你是 ecosystem 的一份子。」

---

## Deck 結構（~75 min）

| # | Part | 時長 | 性質 | 檔案 |
|---|------|------|------|------|
| 1 | 開場 + What's New | 5 min | talk | `slides/part1-opening.md`（TBD）|
| 2 | **Antigravity CLI 入門** | 30 min | **學員 Cloud Shell follow-along** | `slides/part2-agy-intro.md` ⏳ agent B |
| 3 | **OpenAB × agy / gemini Live Demo** | 25 min | **講師 demo (M4 host)** | `slides/part3-openab-demo.md` ⏳ agent C |
| 4 | digest-agent + ADK 模式（reuse 5/09 簡化）| 8 min | talk + diagram | `slides/part4-adk-reuse.md`（TBD）|
| 5 | GEAP 一頁帶過（reuse 5/09 簡化）| 3 min | talk | `slides/part5-geap-pointer.md`（TBD）|
| 6 | Q&A + 社群預告 | 4 min | open | — |

---

## Story Arc

```
[2026-05-19] Google blog: Gemini CLI 6/18 sunset
        ↓
[2026-05-20] 社群開 GitHub issue #31 (Antigravity CLI ACP feature request)
        ↓
[2026-05-21] OpenAB PR #863 (Node.js wrapper) 被 Pahud 以「temporary hack」拒
        ↓
[2026-05-22 02:00] Jimmy 以 GDE 身份留 issue #31 comment：dual-engine use case + 6/18 deadline 推 native ACP
        ↓
[2026-05-22 11:51] Pahud merge PR #896 — agy-acp Rust adapter 正式收進 OpenAB v0.8.4-beta.2
        ↓
[2026-05-23] Jimmy 在台南現場示範用這個 ⭐
```

時間軸做成一張投影片，全場最強 hook。

---

## Hands-on / Demo Setup

### 學員端（只要 Cloud Shell）
- Part 2 全程 https://shell.cloud.google.com
- Google account 登入即用，**零本地安裝**
- 詳細指令在 `labs/cloudshell-agy-lab.md`

### 講師端（M4 + Discord）
- Host: M4 mac mini（jimmym4.local / ts-m4）— 今天剛遷 OrbStack，RAM headroom 充足
- Runtime: OpenAB v0.8.4-beta.2 via `docker run`
- Discord: Jimmy 私 server `1339233219182067744` channel `1339233219182067747`
- 兩個 bot：`@agy-bot`（backend = agy-acp）+ `@gemini-bot`（backend = gemini --acp）
- 課前 1 hour 跑 checklist 在 `demos/openab-demo-script.md`
- Fallback 預錄 plan 在 `demos/openab-fallback-prerecord.md`

---

## 改裝自 5/09 Taichung

5/09 deck (https://bwai0509.web.app/) 的可保留 / 必汰換：

| 內容 | 5/09 | 5/23 |
|------|------|------|
| Gemini CLI 五大功能（Skills/Hooks/MCP/Checkpointing/Headless）| ✅ 主軸 | ❌ 整段汰換為 agy |
| digest-agent 架構 | ✅ 講 30 min | ✅ 簡化講 8 min（Part 4）|
| ADK SequentialAgent 概念 | ✅ 中 | ✅ 帶過（Part 4 內）|
| GEAP deploy | ✅ 詳 | ✅ 一頁帶過（Part 5）|
| Lab A/B/C (Cloud Run / Slack publisher / prompt format) | ✅ 40-50 min | ❌ 移除（Tainan 場時長和定位不一樣）|

---

## 5/23 比 5/09 的優勢

1. **時效性**：6/18 倒數 + 早上 PR #896 merge story
2. **互動感**：Discord 學員加入即時看 bot 回應 vs 5/09 Slack publisher lab
3. **設備門檻低**：Cloud Shell 全 cover，5/09 需要 GCP 帳號 + Cloud Run
4. **Story arc 強**：issue → PR → 現場 demo 的閉環 5/09 沒有
5. **dual-engine 哲學**：Claude + Gemini 雙引擎，不是 Anthropic vs Google 二選一

---

## Pre-talk Checklist（5/22 21:00 → 5/23 13:00 = ~16 hours）

### 🌙 今晚（5/22 21:00-23:00, ~2h）— 核心 critical path
- [ ] **Workshop GCP 啟動**：套既有 SOP `~/.agents/methodology/gde-workshop-temp-access.md`
  ```bash
  mkdir -p ~/workspace/personal/workshops/0523-tainan && cd $_
  cp ~/.agents/scripts/workshop.mk Makefile
  cp ~/.agents/scripts/workshop.env.example .workshop.env
  $EDITOR .workshop.env  # PROJECT=gdg-ws-0523-tainan / BILLING=$GDE_REDEEM / SHEET_URL=... / EXPIRE=2026-05-26 / BUDGET=200
  make init && make bootstrap   # 建 project + APIs + budget cap + AR repo + GCS bucket
  ```
- [ ] **Cloud Shell 自驗 agy CLI**（jimmyliao.dev@gmail.com）：
  ```bash
  curl -fsSL https://antigravity.google/cli/install.sh | bash
  ~/.local/bin/agy --version    # expect: v1.0.0+
  ~/.local/bin/agy -p "hello"   # 確認 OAuth flow + 回應 OK
  ```
- [ ] **Email collection Sheet 開好**：Google Form / Sheet for QR code 收 email
- [ ] **準備 QR code A4** 印出（QRcode 指向 email form）
- [ ] **Discord bot tokens**：兩個 application + tokens（agy-bot / gemini-bot），加進你 server
- [ ] **OpenAB v0.8.4-beta.2 部 M4**（demo Part 3 用，可選做 — 沒做就 demo Part 3 改 prerecorded video）

### 🌅 明早（5/23 08:00-12:00, ~4h）
- [ ] 跑一次完整 demo flow（Part 2 lab + Part 3 demo），計時 + 修 script
- [ ] **錄 5min fallback 影片**（Part 3，per `demos/openab-fallback-prerecord.md` shot list）
- [ ] Slides 預載 local（場地網路可能爛）
- [ ] 帶: M4 power cord, QR A4 print, 手機 Discord 登好

### 🎤 現場（5/23 12:00, T-1h）
- [ ] M4 場地 WiFi 上 Tailscale 還通（ts-m4 ping check）
- [ ] 第一堂開始前：學員掃 QR code 填 email → 你跑 `make enable-users`（idempotent，可重跑）
- [ ] Cloud Shell 從場地 WiFi 測一次
- [ ] 兩個 Discord bot 都送 `ping` 確認活著

### 🛬 課後（5/25-5/26）
- [ ] `make disable-users`（per SOP — IAM bindings 撤掉，Group membership 留 audit）
- [ ] `make destroy` or `make pause-billing`（看你要不要砍 project）
- [ ] memory 寫 retrospective `project_bwai0523_tainan.md`

---

## 後續產出檔案

```
~/.agents/personal/events/bwai0523-tainan/
├── outline.md                          (this file)
├── slides/
│   ├── part1-opening.md                (TODO)
│   ├── part2-agy-intro.md              (agent B 寫中)
│   ├── part2-speaker-notes.md          (agent B 寫中)
│   ├── part3-openab-demo.md            (agent C 寫中)
│   ├── part3-speaker-notes.md          (agent C 寫中)
│   ├── part4-adk-reuse.md              (TODO, reuse 5/09)
│   └── part5-geap-pointer.md           (TODO, reuse 5/09)
├── labs/
│   └── cloudshell-agy-lab.md           (agent B 寫中)
├── demos/
│   ├── openab-demo-script.md           (agent C 寫中)
│   └── openab-fallback-prerecord.md    (agent C 寫中)
└── assets/
    ├── (TBD screenshots: agy --help, issue #31, PR #896)
    └── (TBD diagrams: ACP ecosystem, OpenAB architecture)
```

---

## 相關 memory

- `[[reference_antigravity_agy]]` — agy 兩個 binary + capabilities + ACP gap
- `[[feedback_gemini_cli_partner]]` — Jimmy dual-engine 哲學
- `[[project_openab_poc_paused]]` — OpenAB PoC 計畫
- `[[project_antigravity_book]]` — BwAI 社群 Antigravity 書 (5/23 場可宣傳)
- `[[project_fleet_ops_session_20260522]]` — 今天的 fleet ops + agy/OpenAB 來龍去脈
