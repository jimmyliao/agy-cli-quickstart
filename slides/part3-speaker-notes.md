# Part 3 講者稿 — OpenAB × agy / gemini-cli Live Demo

**講師**: Jimmy Liao | **總長**: 25 min | **語言**: 繁中為主，技術名詞保留英文

---

## Slide 1 — 標題頁

**時長**: 30s
**開場**: 「Part 2 大家看 Gemini CLI 怎麼用，Part 3 我要把鏡頭拉遠一階 — 不是看一個 CLI，是看 CLI ecosystem 怎麼動。」
**Key points**:
- 預告等下是 live demo，講師演、學員 Discord 觀戰
- QR code 上來掃進 channel
**Transition**: 「先講今天早上發生的事。」

---

## Slide 2 — 今天早上的新聞

**時長**: 2 min
**開場**: 「我先說一個 9 小時前才發生的故事，不講這個今天的 demo 就沒意義。」
**Key points**:
- 凌晨 02:00 我在 antigravity-cli issue #31 留 comment，希望 ACP 支援
- 11:51 Pahud 把 agy-acp Rust adapter merge 進 OpenAB，發 v0.8.4-beta.2
- 從一個 GDE comment 到上游 binary，9 小時
- 「這不是我寫的 — 是 community PR。OpenAB 不是我的 repo。重點是 ecosystem 跑得有多快。」
**Transition**: 「OK，那 OpenAB 到底是什麼？」

---

## Slide 3 — What is OpenAB?

**時長**: 1 min
**開場**: 「一句話：把 Discord 變成你的 terminal。」
**Key points**:
- Rust 寫的 broker，把 chat platform 接到 ACP coding CLI
- 一個 Discord thread = 一個 CLI subprocess
- Supported backends 一整排：claude、gemini、codex、copilot、kiro、opencode、hermes、grok、現在還有 agy
**Transition**: 「我畫一張圖你比較容易看。」

---

## Slide 4 — 架構一張圖

**時長**: 2 min
**開場**: 「最上面 Discord，最下面 CLI process。中間 OpenAB 在做兩件事：routing 跟 streaming。」
**Key points**:
- Routing — 知道 @agy-bot vs @gemini-bot 該 fork 哪個 process
- Session pool — 同 thread 不重啟 CLI，保留 context
- Edit-streaming — 不洗版，patch 同一則 Discord 訊息
**金句**: 「Discord 是 control plane，CLI 是 execution plane。」
**Transition**: 「為什麼這件事重要？」

---

## Slide 5 — 為什麼這件事重要？

**時長**: 1.5 min
**開場**: 「四件事，我快速講。」
**Key points**:
- Ecosystem velocity — 9 小時 merge，沒有任何 vendor roadmap 跟得上
- Best-tool-per-task — 不是 Anthropic vs Google 二選一
- Remote home lab — 我手機就是 terminal
- Vendor-neutral — 你今天接 claude、明天接 gemini、後天接 grok，broker 不換
**Transition**: 「講夠了，開 demo。」

---

## Slide 6 — Demo A (agy 單 bot)

**時長**: 5 min
**開場**: 「先給你看單一 bot。我在 Discord 敲一個 prompt，看 agy backend 怎麼回。」
**Key points**:
- 切到 Discord，channel `#bwai-tainan`
- 提醒學員掃 QR 加入觀戰
- 送 prompt: `@agy-bot 寫一個 Python function 算 fibonacci(n) 並有 memoization`
- **看 reaction**: 👀 → 🤔 → ✅
- **看 edit-streaming**: 每 1.5s 訊息變長
**邊跑邊講**: 「token 進來的感覺。這是 ACP 的 partial result event，OpenAB 接到就 patch Discord 訊息。」
**Transition**: 「同一個 prompt，換引擎。」

---

## Slide 7 — Demo B (gemini --acp)

**時長**: 5 min
**開場**: 「換 gemini-bot。完全一樣 prompt。」
**Key points**:
- 送 prompt
- Side-by-side scroll 兩個 bot 結果
- 對比點：解釋深度、type hint、有沒有附 test、edge case
**金句**: 「不是誰贏，是 task fit。你今天想要教學風格還是工程風格？」
**Transition**: 「兩個 bot 各做各的不夠刺激 — 讓他們講話。」

---

## Slide 8 — Demo C (Multi-agent showdown) ⭐

**時長**: 7 min
**開場**: 「這段是我覺得 OpenAB 最 sexy 的部分。」
**Key points**:
- Round 1: @agy 設計 URL shortener
- Round 2: @gemini 批評上面那個設計（注意：我沒複製 agy 的輸出，Discord thread 就是 shared memory）
- Round 3: @agy 根據 gemini 的批評 refactor
**邊跑邊講**:
- Round 1 等的時候：「想像你跟 senior engineer pair，先丟初稿出來」
- Round 2 等的時候：「換 reviewer 角度，gemini 通常比較會挑 edge case」
- Round 3 結束：「這就是 agent-to-agent dialogue。兩個不同 vendor 的 CLI 在 Discord channel 裡對話，沒有人工搬運 context。」
**Transition**: 「最後一個 demo — 我把筆電蓋上。」

---

## Slide 9 — Demo D (手機 push)

**時長**: 3 min
**開場**: (動作：把筆電 lid 半合，拿出手機舉高)「現在 demo 機在這裡，但工作機在家裡 M4。」
**Key points**:
- 手機 Discord 收到剛剛的 push 通知
- 點開 channel，敲一行 followup
- M4 接到、agent 開工
- 30 秒後手機收到結果
**金句**: 「Tailscale 通到家，OpenAB 在 M4 跑，Discord 是我隨身帶的 terminal。」
**Transition**: 「Recap 三件事。」

---

## Slide 10 — Recap

**時長**: 1 min
**開場**: 「三個 takeaway 帶回家。」
**Key points**:
- Vibe Coding 不只寫 code — 是 ecosystem velocity
- 雙引擎不是二選一 — best-tool-per-task
- Discord 是新的 terminal — control plane 在哪工作就在哪
**收尾**: 「Repo openabdev/openab，tag 0.8.4-beta.2，docs/antigravity.md。Part 4 換你動手裝。」
**Transition**: 「短暫休息一下，2 分鐘後 Part 4。」

---

## 講者整體節奏提醒

- **不要 demo 中念 code** — code block 出來就停 2 秒讓觀眾自己看，講師看 bot reaction 為主
- **每個 demo 開頭先說「我要做什麼」再做** — 觀眾比較不會迷路
- **Bot 在跑等待時不要冷場** — 一定要有 talking point 填空 (見 demo script)
- **Demo C 是高潮** — 語速可以慢、語氣可以重，這段值得停留
- **若 fallback 觸發** — 不要解釋為什麼壞，直接「我們切預錄」一句話過，繼續講故事
- **時間紀律** — Demo A/B 各 5 min 是上限，提早結束就直接進 C；C 內建 buffer 從 7 壓到 5 min 都還能講完

---

## 緊急 plan B 一句話切換稿

「(看時鐘) OK 我們把後面壓一下，跳過 Demo X，直接進 Demo C — 因為 multi-agent 那段才是今天我要你帶回家的記憶點。」
