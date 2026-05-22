# Part 2 講者稿 · Antigravity CLI 入門

**Build with AI Tainan · 2026-05-23 · 30 min**
Speaker: Jimmy Liao (Google AI GDE)

格式：每張投影片對應一段「開場句 → 重點 → 過場 → 預算時間」。
語氣：對著 60 人現場講話，繁中口語，可以中英混。

總時間預算：**30 分鐘**（含 10–12 分鐘 lab）

---

## Slide 1 · Title

- **開場句**：「OK 我們進 Part 2，標題很簡單 —— Antigravity CLI 入門，從 Gemini CLI 接班。」
- **重點**：
  - 5/09 台中場我講 `gemini`，今天起整個換掉
  - 為什麼換？下一張說
- **過場**：「先給大家一個 spoiler，這個換 deadline 不是我選的，是 Google 給的。」
- **時間**：30 秒

---

## Slide 2 · 這 30 分鐘你會帶走什麼

- **開場句**：「30 分鐘我希望你帶走四件事。」
- **重點**：
  - 認清兩個 agy（避免裝錯）
  - 5 個 flag
  - migration 對照
  - Cloud Shell hands-on，**不用裝任何東西在自己電腦**
- **過場**：「重點是最後那個，等下我們會請大家打開 shell.cloud.google.com，一起跑。」
- **時間**：45 秒

---

## Slide 3 · Why now (6/18 sunset)

- **開場句**：「先講為什麼是現在。Google 上週發了一篇 blog —— Gemini CLI 的 Pro/Ultra free tier，6/18 sunset。」
- **重點**：
  - 不是 Gemini 模型沒了，是 **CLI runtime** 換代
  - Antigravity CLI 是官方接班
  - 我自己上個月一直在 investigate 這個 migration
- **過場**：「但是這裡有一個雷，**有兩個東西都叫 agy**。」
- **時間**：1 分鐘

---

## Slide 4 · ⚠️ 兩個 agy

- **開場句**：「這張是今天最重要的安全提示，請看清楚。」
- **重點**：
  - 左邊 IDE launcher：VS Code fork 那個 bundle，**已經壞了**
  - 右邊 standalone CLI v1.0.0：**今天的主角**
  - Path 不同、來源不同、用途不同
- **過場**：「我自己踩過坑，講一下。」
- **時間**：1.5 分鐘

---

## Slide 5 · IDE launcher 已壞

- **開場句**：「macOS 26 更新之後，我 M1 Air 上的 `/usr/local/bin/agy` 直接變 broken symlink。」
- **重點**：
  - launcher binary 被 app.asar 吃掉了
  - 連 IDE 內「Install command line tool」都救不回
  - 如果你之前裝過，建議 `sudo rm` 掉，免得搞混
- **過場**：「OK 那 IDE 那顆放一邊，今天全部講 standalone CLI。」
- **時間**：1 分鐘

---

## Slide 6 · Standalone agy 是什麼

- **開場句**：「standalone 這顆是單一 binary，不用裝整套 IDE。」
- **重點**：
  - 從 antigravity.google/docs/cli-using 拿
  - 裝在 `~/.local/bin/agy`
  - v1.0.0 五月剛釋出
  - 定位：**Gemini CLI 直接接班**
- **過場**：「來看 help。」
- **時間**：45 秒

---

## Slide 7 · agy --help

- **開場句**：「help 出來長這樣 —— 用過 Gemini CLI 的人應該很眼熟。」
- **重點**：
  - 五個 flag 撐 90% 用法
  - 命名邏輯跟 `gemini` 幾乎一致
  - 同團隊出品，心智模型沿用
- **過場**：「一個一個來看。」
- **時間**：45 秒

---

## Slide 8 · -p headless

- **開場句**：「`-p` 是 print mode，最重要的一個。」
- **重點**：
  - 非互動、跑完就退出
  - cron / CI / Telegram bot 都靠它
  - 預設 5 分鐘 timeout
  - **但是注意：`-p` 不是 ACP**（這個踩坑後面講）
- **過場**：「下一個是 `-c`。」
- **時間**：1 分鐘

---

## Slide 9 · -c / --conversation

- **開場句**：「`-c` 是 continue，接上一個 session。」
- **重點**：
  - 跟 `gemini -c` 一模一樣
  - session id 存本地
  - 跨機要自己同步（這個 Cloud Shell 反而幫你解了，home 是 persistent）
- **過場**：「再來看 workspace 跟 sandbox。」
- **時間**：45 秒

---

## Slide 10 · --add-dir / --sandbox / --dangerously-skip-permissions

- **開場句**：「這三個一起看 —— workspace、sandbox、自動同意。」
- **重點**：
  - `--add-dir` 多目錄掛進來
  - `--sandbox` best-effort 限制
  - `--dangerously-skip-permissions` 名字嚇人但 cron 沒它不能跑
  - 想像 unattended job 跑到一半卡在 yes/no prompt —— 整夜 stuck
- **過場**：「OK，那從 `gemini` migrate 過來怎麼改？」
- **時間**：1.5 分鐘

---

## Slide 11 · Migration table

- **開場句**：「大部分時候就是 sed `s/gemini/agy/g`。」
- **重點**：
  - 90% one-to-one mapping
  - `--yolo` → `--dangerously-skip-permissions`（名字嚴肅一點）
  - `@file` 語法沒了，改 stdin pipe（這個現場 lab 會踩到）
- **過場**：「講夠了，動手吧。打開 Cloud Shell。」
- **時間**：1 分鐘

---

## Slide 12 · 打開 Cloud Shell

- **開場句**：「請大家現在打開瀏覽器，shell 點 cloud 點 google 點 com。」
- **重點**：
  - 用 Google 帳號登入
  - 等 VM provisioning，可能 5–30 秒
  - 看到 prompt 舉手讓我知道
  - **不用裝東西在自己電腦**，這就是 lab
- **過場**：「都好了我們開始跑。Handout 在 [URL]，跟著貼就好。」
- **時間**：1.5 分鐘（含等大家準備好）

---

## Slide 13 · Lab 流程總覽

- **開場句**：「Lab 七個步驟，我們一起跑前六個，第七個是 stretch goal。」
- **重點**：
  - 跟著 handout 走
  - 卡住舉手，旁邊 GDE 朋友會幫忙
  - 重點是**看清楚 expected output**，知道自己有沒有跑對
- **過場**：（切到 lab，現場 demo + 跟學員一起跑）
- **時間**：**10–12 分鐘**（lab 主體）

---

## Slide 14 · 踩坑 #1 ：-p ≠ ACP

- **開場句**：「Lab 跑完，講四個我這個月踩的坑。第一個：**`agy -p` 不是 ACP**。」
- **重點**：
  - 我一度以為 `-p` 可以直接接 OpenAB / Zed
  - 結果不行 —— ACP 是 stdio JSON-RPC，`-p` 是純文字
  - 我朋友 Joseph 寫了 wrapper PR #863，被 OpenAB maintainer Pahud 拒了
  - 我去 Antigravity issue **#31** 留了 GDE comment 等 Google ship native
- **過場**：「下一個。」
- **時間**：1.5 分鐘

---

## Slide 15 · 踩坑 #2 ：沒 streaming JSON

- **開場句**：「想做 dashboard 觀察 agent 在幹嘛？目前不行。」
- **重點**：
  - `agy -p` 只給 final stdout
  - 沒有 tool call event stream
  - workaround：`script` 命令 log parse，醜但能用
- **過場**：「第三個。」
- **時間**：45 秒

---

## Slide 16 · 踩坑 #3 ：sandbox 是 best-effort

- **開場句**：「`--sandbox` 名字聽起來很安全，其實不是 container 等級。」
- **重點**：
  - network 還是出得去
  - 真要 isolate 套 Docker 或 Cloud Shell（今天就在示範）
- **過場**：「最後一個。」
- **時間**：30 秒

---

## Slide 17 · 踩坑 #4 ：別開公開 wrapper repo

- **開場句**：「給工程師朋友的真心建議：**stopgap 的價值在快，不在精**。」
- **重點**：
  - ACP 是熱題，但 Google 隨時可能 ship native
  - 你的 wrapper 一夜 obsolete
  - 自用 cron 包一層 OK，**別投入長期維護**
- **過場**：「收尾。」
- **時間**：1 分鐘

---

## Slide 18 · Section 收尾

- **開場句**：「OK 30 分鐘到了，回顧一下。」
- **重點**：
  - 認清兩個 agy ✅
  - 5 個 flag ✅
  - Cloud Shell lab 跑過第一支 agent ✅
  - Migration 心智模型 ✅
- **過場**：「等下 Part 3 我們把 `agy` 接進 OpenAB，做多 agent 編排，剛好接上踩坑 #1 那個 ACP 故事。」
- **時間**：45 秒

---

## Slide 19 · 休息 5 分鐘

- **開場句**：「休息 5 分鐘，9:55 回來進 Part 3。」
- **重點**：
  - 有問題現場找我或 GDE GenAI Circle
  - 廁所在出去右轉
- **時間**：（休息，不計）

---

## 時間總表

| Slides | 內容 | 預算 |
|---|---|---|
| 1–2 | Title + intro | 1:15 |
| 3 | Why now | 1:00 |
| 4–5 | 兩個 agy + IDE 已壞 | 2:30 |
| 6–10 | Standalone + 5 flags | 5:00 |
| 11 | Migration table | 1:00 |
| 12–13 | Cloud Shell + lab 啟動 | 1:30 |
| Lab | 步驟 1–6 (+stretch) | 10:00–12:00 |
| 14–17 | 4 個踩坑 | 4:00 |
| 18 | 收尾 | 0:45 |
| **合計** | | **27–29 min** |

留 1–3 分鐘 buffer 給 Q&A 或 lab 卡關。

---

## 講者備忘

- 我是 GDE，可以提一下 GDE GenAI Circle 是內部 channel，**不要在公開 issue 引用**
- Issue #31 是公開的，可以講
- 強調我這個月一直在 investigate agy，今天分享是第一手經驗
- 如果有人問「為什麼 Google 要換 CLI」→ 答：terminal-first agent runtime 是未來方向，IDE bundle 太重
- 如果有人問「Claude Code / Codex CLI 怎麼辦」→ 答：dual-engine 持續，agy 是 Google 這條線的接班，跨家的還是分開用
