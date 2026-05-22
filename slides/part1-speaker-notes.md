# Part 1 — 開場 + What's New 講者稿

**時長**：5 min（含 1 min buffer）
**目標**：建立 story arc，讓學員知道「今天要看的不是技術文檔，是 ecosystem 移動的故事」

---

## Slide 1 — 標題（10 sec）

「大家午安，我是 Jimmy。很高興回到台南。今天主題：Vibe Coding × Gemini 無限可能 — 但我們會聚焦在最新發生的事情，從 Gemini CLI 接班到 Antigravity ecosystem。」

→ 直接進自介，不停留

---

## Slide 2 — 自介（30 sec）

「我是 LeapDesign 的廖嘉禾，Google AI GDE。過去 12 個月做了幾個 case：digest-agent 大家可能在 5/9 看過、LeapAgent、HISP RAG、PetCircle。重點是我家裡有 11 台機器跑 Tailscale 串著當 home lab。」

「我的工作流是 **Claude + Gemini 雙引擎**——雙模型 second-opinion，不是 vendor lock-in。今天就是這個雙引擎要怎麼 migrate 到 agy 接班後。」

「最後一句：**今天我不教你寫 code，我帶你看 ecosystem 怎麼動。**」

→ 「不教 code」是預期管理，學員不會期待 deep tutorial

→ Transition：「先給大家全場 menu」

---

## Slide 3 — Agenda（30 sec）

「6 個 part 75 分鐘。重點在 Part 2 跟 Part 3——這是 5/9 那場沒有的新東西。」

「Part 4-5 是從 5/9 簡化，10 分鐘帶到。」

「Part 6 Q&A 留 5 min。」

→ Transition：「來看為什麼 Part 2-3 是新的」

---

## Slide 4 — Section divider「5/9 之後發生了什麼」(5 sec)

「5/9 我在台中講完 Gemini CLI workshop，那之後 4 天，整個 ecosystem 動得超快。」

→ 進下一張看 5/19 Google 公告

---

## Slide 5 — Google blog 5/19（30 sec）

「5/19 Google 公告：**Gemini CLI 6/18 停服 Pro 跟 Ultra free tier**。Enterprise tier 繼續，但個人玩家用的免費那條沒了。」

「接班的是 Antigravity CLI——指令叫 `agy`。terminal-first coding agent，VS Code fork 的 IDE 也同步出。」

「所以 5/9 大家學到的 `gemini` 指令，6/18 之後不能用了？**沒這麼慘，但你要知道怎麼接班。**」

→ Transition：「但這只是 Google 動，社群更瘋」

---

## Slide 6 — Story Arc 時間軸（2 min）⭐ 全場最重要

> 講這張慢一點，這是整場 hook。

「來看這個時間軸，4 天 ecosystem 連續移動。」

**逐條解說（每條 20 sec）：**

1. **5/19 Google 公告**「OK 大家知道。」
2. **5/20 社群 issue #31**「有個 Joseph 開了 Antigravity GitHub issue 列完整 ACP feature request——ACP 是 Agent Client Protocol，給 IDE 跟 broker 接 CLI 用的標準，Gemini CLI 有，agy 還沒。」
3. **5/21 OpenAB PR #863**「OpenAB——這是個 Discord / Slack ↔ AI agent 的 broker，今天 Part 3 demo 主角——有人發 PR 想加 agy 支援，用 Node.js wrapper。OpenAB 的 maintainer Pahud 一刀切『temporary hack』拒了。」
4. **5/22 02:00 我留 comment**「凌晨兩點我以 GDE 身份在 issue #31 留 comment：dual-engine use case + 6/18 deadline + 修一個社群的勘誤。」（不要強調個人功勞，講「community pressure」更安全）
5. **5/22 11:51 Pahud merge PR #896**「中午 Pahud 自己用 Rust 寫了 agy-acp adapter merge 進 OpenAB v0.8.4-beta.2。**從我留 comment 到 merge 不到 10 小時。**」
6. **5/23 13:00 我們現在**「就是今天我們在這。下午 demo 跑的是 5/22 早上才 merge 的程式碼。」

「⭐ **重點不是我推了多少——是這個 ecosystem 真的會聽。issue / PR / merge 4 天閉環。你也可以是這個 loop 的一份子。**」

「這就是我說的 Vibe Coding：你不是在追 ecosystem，你是 ecosystem 的一份子。」

→ Pause 2 sec 讓學員消化

---

## Slide 7 — 三個身分（45 sec）

「不同身分今天 take-away 不同：」

- **學生 / 新手**：「Part 2 跟著我做，回家就會用 agy。」
- **production dev**：「Part 3 看 OpenAB 怎麼接 agy，學完回家自己 fork。」
- **OSS contributor**：「看完今天，你會知道 ecosystem 移動可以很快——下次別只當 user，當 contributor。」

---

## Slide 8 — Demo 環境（30 sec）

「兩邊環境：」

「**學員**：開 Cloud Shell。只要 Google account，零本地安裝。**進門時你掃 QR code 填 email，我把你加進 workshop GCP project**，這樣等等 Part 4 ADK demo 你也可以跑。」

「**講師**：我家 M4 mac mini 跑 OpenAB——巧合的是這台今天早上才從 Colima 遷到 OrbStack。Discord channel 等等也會給 invite。」

→ Transition：「現在開 Cloud Shell」

---

## Slide 9 — Part 2 開場（10 sec）

「現在打開 shell.cloud.google.com，我們開始 Part 2。」

→ 第一張 Part 2 slide

---

## 時間總表

| Slide | 時長 | 累積 |
|-------|------|------|
| 1 標題 | 10 sec | 0:10 |
| 2 自介 | 30 sec | 0:40 |
| 3 Agenda | 30 sec | 1:10 |
| 4 Section divider | 5 sec | 1:15 |
| 5 5/19 Google blog | 30 sec | 1:45 |
| 6 ⭐ Story arc | 2 min | 3:45 |
| 7 三個身分 | 45 sec | 4:30 |
| 8 Demo 環境 | 30 sec | 5:00 |
| 9 進 Part 2 | 10 sec | 5:10 |

**目標 5 min，實際 5:10 略 over，可以從 slide 7「三個身分」壓到 30 sec。**

---

## 風險 / 備案

- **如果學員 5/9 沒來**：slide 4 不要說「大家可能 5/9 看過 digest-agent」——改說「過去 12 個月幾個 case」就好
- **如果時間到 4:30 還在 slide 5**：跳過 slide 7 三個身分，直接 slide 8
- **如果有人問 5/19 sunset 細節**：「我們 Part 2 進去看 agy 你就知道沒事」 — 不要 5 min 就陷進 Q&A
