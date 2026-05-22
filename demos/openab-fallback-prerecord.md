# OpenAB Demo Fallback Plan

**Use when live demo fails.** Section budget 25 min must still close on time.

---

## When to switch to fallback

**Hard triggers (immediate switch)**:
- Discord channel `#bwai-tainan` shows 100+ unread / rate-limited banner
- `ssh ts-m4` fails on stage (M4 unreachable)
- Both bots offline (no green dot in Discord member list)
- > 15s no bot reaction to first prompt in Demo A

**Soft triggers (consider after 1 retry)**:
- Demo A works but Demo B bot 30s+ no response
- Edit-streaming stuck on "Working..." > 45s
- gemini quota error visible in tmux log

**Recovery decision**: 講師喊一聲「我們切預錄」— 助教按 keyboard shortcut 切影片來源。

---

## Pre-recorded video — capture plan (do this T-3 hr before talk)

**Length target**: 5 min total, narrated 繁中

**Recording setup**:
- OBS Studio on M4, 1080p 30fps
- Capture Discord desktop window + tmux log split-pane
- Mic: AirPods Pro 或 built-in
- Output: `~/.agents/personal/events/bwai0523-tainan/assets/openab-demo-fallback.mp4`

**Shot list (5 min)**:

| Time | Scene | Narration |
|------|-------|-----------|
| 0:00-0:30 | GitHub PR #896 頁面 scroll | 「今早 merge 的 agy-acp adapter」 |
| 0:30-1:30 | Demo A: Discord agy-bot fibonacci | 「agy backend，edit-streaming 出來」 |
| 1:30-2:30 | Demo B: Discord gemini-bot fibonacci | 「同 prompt，gemini --acp」 |
| 2:30-4:00 | Demo C: Multi-agent URL shortener 三回合 | 「agent-to-agent dialogue」 |
| 4:00-4:45 | Demo D: 手機操作 (iPhone screen recording) | 「Tailscale + Discord = remote home lab」 |
| 4:45-5:00 | Recap slide overlay | 「三個 takeaway」 |

**File handoff**: 影片放到 USB-C 隨身碟 + 上傳 Drive 備份連結
- USB: `BWAI-2023-05-23/openab-demo-fallback.mp4`
- Drive: shared with 主辦方 organizer email

---

## Screenshot sequence (8 shots, plan B for video failure)

存放路徑: `~/.agents/personal/events/bwai0523-tainan/assets/openab-screenshots/`

| # | File | Content |
|---|------|---------|
| 1 | `01-pr896-merge.png` | GitHub PR #896 merged banner + diff 顯示 `agy-acp` block |
| 2 | `02-architecture.png` | Slide deck 架構圖 |
| 3 | `03-demoA-agy-prompt.png` | Discord channel, agy-bot prompt 剛送出 + 👀 reaction |
| 4 | `04-demoA-agy-result.png` | agy-bot 完成 fibonacci code 訊息全貌 |
| 5 | `05-demoB-gemini-result.png` | gemini-bot 完成同 prompt 結果 |
| 6 | `06-demoC-three-rounds.png` | URL shortener 三回合 thread 全貌 (long screenshot) |
| 7 | `07-demoD-phone.png` | iPhone Discord app 顯示 bot reply 的畫面 (實機拍照) |
| 8 | `08-recap.png` | 三 takeaway slide |

---

## One-page narrative speaker can read while showing static shots

**(若連影片都壞掉，講師翻投影片講故事，5 min)**

---

> 「打開第一張圖 — 這是今早 9 小時前 merge 的 PR #896。OpenAB 是個 Rust harness，把 Discord 跟任何 ACP-compatible CLI 接起來。我凌晨在 antigravity-cli issue #31 留 comment，希望有人寫一個 agy 的 adapter — Pahud 中午就把 Rust 版 agy-acp merge 進 OpenAB main。九小時。」
>
> 「第二張圖你看架構 — Discord 是 control plane，下面是 OpenAB broker，再下面是 CLI subprocess。每個 thread 對應一個 process，stdio 走 JSON-RPC。」
>
> 「第三、第四張是 Demo A — agy-bot 寫 fibonacci。注意 edit-streaming：bot 不是 reply chain，是 patch 同一則訊息。你會看到 code 慢慢長出來，每 1.5 秒更新一次。」
>
> 「第五張是 Demo B — 我換 gemini --acp，同樣 prompt。Gemini 給的解釋更長、加了 unit test，agy 給的比較 minimal。沒有誰贏 — 是 task fit。今天想要教學風格還是工程風格，你自己選。」
>
> 「第六張這張 long screenshot 是這次 demo 的高潮 — Demo C，multi-agent showdown。我先讓 agy 設計一個 URL shortener，然後 @ gemini 問它『有什麼問題』。Gemini 沒問我 context — Discord thread 就是 shared memory，它自己 scroll 看。最後我再讓 agy 根據 gemini 的批評做 refactor。三個 round 全部在 Discord 裡完成，兩個不同 vendor 的 CLI 在對話。」
>
> 「第七張是 Demo D — 我手機 push。Tailscale 通到家裡 M4，OpenAB 在那邊跑，我蓋上筆電也可以工作。Discord 是隨身的 terminal。」
>
> 「最後一張 — 三個 takeaway：Vibe coding 不只是寫 code 是 ecosystem velocity；雙引擎不是二選一是 best-tool-per-task；Discord 是新的 control plane。Repo openabdev/openab，自己回家裝。」

---

## Recovery time budget

| Failure mode | Switch cost | Total time used |
|--------------|-------------|-----------------|
| Live → video | 30s | still fits 25 min |
| Live → screenshots + narrative | 1 min | needs trim Q&A buffer |
| Total blackout (no projector) | — | 講 narrative + 引導現場到 Part 4 hands-on 提早開始 |

---

## Post-talk checklist (regardless of fallback)

- [ ] Push demo logs to `~/.agents/personal/events/bwai0523-tainan/logs/`
- [ ] 上傳實際發生在 channel 的對話 export 到 assets/
- [ ] 紀錄哪一段觸發 fallback (若有) → feedback file
