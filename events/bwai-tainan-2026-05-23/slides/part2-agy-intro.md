---
marp: true
theme: default
paginate: true
header: 'Build with AI Tainan 2026-05-23 · Part 2'
footer: 'Jimmy Liao · Google AI GDE'
---

# Antigravity CLI 入門
## 從 Gemini CLI 接班

Jimmy Liao / Google AI GDE
Build with AI · Tainan · 2026-05-23

`Part 2 / 30 min`

---

## 這 30 分鐘你會帶走什麼

1. 認清「兩個 agy」的差別（不踩坑）
2. Standalone `agy` v1.0.0 的 **5 個核心 flag**
3. 從 `gemini` → `agy` 的指令對照
4. **Cloud Shell hands-on lab**：不用裝任何東西

> 結束時你應該可以在 Cloud Shell 跑出 `agy -p` 第一支 headless agent。

---

## Why now？ — 6/18 sunset 時間軸

- **2026-06-18**：Gemini CLI Pro/Ultra free tier sunsets
- 官方公告：`developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli`
- 接班者：**Antigravity CLI (`agy`)** — terminal-first coding agent
- 5/09 台中場我們講 `gemini`，今天起一律改 `agy`

> 不是換 LLM，是換 **agent runtime**。Gemini 模型一樣在。

---

## ⚠️ 兩個 agy（看清楚再裝！）

| | **IDE launcher** | **Standalone CLI v1.0.0** |
|---|---|---|
| Path | `/usr/local/bin/agy` | `~/.local/bin/agy` |
| 來源 | Antigravity IDE bundle (VS Code fork) | `antigravity.google/docs/cli-using` |
| 用途 | 開 IDE 工作目錄（GUI launcher） | **terminal-first coding agent** |
| `-p` headless | ❌ | ✅ |
| 狀態 (2026-05) | broken on macOS 26 | ✅ 可用 |

> 今天主角是右邊那欄。左邊那個如果你已經裝了，建議 `rm` 掉。

---

## 我踩過的坑：IDE launcher 已壞

- macOS 26 update 後：
  - `/usr/local/bin/agy` → broken symlink
  - `Resources/app/` 被 `app.asar` 取代，**launcher binary 不見了**
  - IDE 內「Install command line tool」也救不回
- 結論：**不要再用 IDE 那顆 agy**，全部走 standalone CLI

```bash
# 清掉舊的（如果你有）
sudo rm -f /usr/local/bin/agy /usr/local/bin/antigravity
```

---

## Standalone `agy` 是什麼

- **單一 binary**，不用裝整套 IDE
- 來源：`https://antigravity.google/docs/cli-using`
- 安裝位置：`~/.local/bin/agy`
- v1.0.0（2026-05 釋出）
- 定位：**Gemini CLI 直接接班**

```bash
$ agy --version
1.0.0
```

---

## `agy --help` 重點節錄

```
Usage: agy [options] [prompt]

Options:
  -p, --print                       Non-interactive headless mode
  -i, --prompt-interactive          Initial prompt, then interactive
  -c, --continue                    Continue last conversation
      --conversation <id>           Resume specific session
      --add-dir <path>              Add workspace directory
      --sandbox                     Terminal restrictions
      --dangerously-skip-permissions   Auto-approve all actions
```

> 五個 flag 撐住 90% 的日常用法。

---

## 5 個核心 flag — `-p` headless

```bash
agy -p "say hello in 3 languages"
```

- 不互動、跑完就退出
- 預設 5 分鐘 timeout
- **cron / CI / Telegram bot 都靠它**
- stdout 是純文字（沒有 streaming JSON）

> 想像成 `curl` 等級的 building block。

---

## `-c` continue / `--conversation`

```bash
# 接上一個 session
agy -c "now translate that to Japanese"

# 指定 session id
agy --conversation 7a3f... "..."
```

- 跨多輪保留 context
- session id 存在本地，跨機要自己同步

---

## `--add-dir` / `--sandbox`

```bash
# 把多個目錄加進 workspace
agy --add-dir ./frontend --add-dir ./backend -p "找出共用型別"

# sandbox 限制 file/network access
agy --sandbox -p "..."

# unattended（cron / CI）必備
agy --dangerously-skip-permissions -p "..."
```

> `--dangerously-skip-permissions` 名字嚇人，但 cron 沒它不能跑。

---

## Migration table：`gemini` → `agy`

| 你以前打的 | 6/18 後改成 |
|---|---|
| `gemini -p "..."` | `agy -p "..."` |
| `gemini -c "..."` | `agy -c "..."` |
| `gemini --yolo` | `agy --dangerously-skip-permissions` |
| `gemini @file.py "..."` | `cat file.py \| agy -p "..."` |
| `gemini --sandbox` | `agy --sandbox` |
| `gemini -m gemini-2.5-pro` | （model 走後端選擇，flag 簡化） |

> 90% 是 sed `s/gemini/agy/g`，但 `@file` 改 stdin 要留意。

---

## Hands-on：打開 Cloud Shell

### 現在請打開：

# 👉 `shell.cloud.google.com`

- 用 Google 帳號登入
- 等 VM 起來（5–10 秒）
- 看到 `you@cloudshell:~$` 就 ready

> 不用裝任何東西在自己電腦，Cloud Shell 就是你的 lab。

---

## Lab 流程（10 分鐘）

1. 裝 `agy` 到 `~/.local/bin/`
2. `agy --version` 確認
3. `agy -p "hello"` 第一次對話
4. `agy -c` 接續
5. 把檔案餵進去 refactor
6. `--sandbox` 看擋了什麼
7. **Stretch**：寫個 mini cron job

> 詳細指令在 handout `cloudshell-agy-lab.md`，跟著貼就好。

---

## 我踩坑紀錄 #1：`-p` ≠ ACP

- 我一度以為 `agy -p` 可以直接接 OpenAB / Zed
- 結果：**ACP 是 stdio JSON-RPC，`-p` 是純文字**
- OpenAB maintainer Pahud 直接拒了我朋友的 wrapper PR #863
- 我去 Antigravity issue **#31** 留 GDE comment 等 Google ship native ACP

> headless ≠ ACP，這兩個是不同層的協定。

---

## 我踩坑紀錄 #2：沒有 streaming JSON

- 想做 dashboard / observability？
- `agy -p` 只給你 final stdout，**沒有 tool call event stream**
- 短期 workaround：`script` 命令 + log parse
- 長期：等 Google 出 streaming（issue #31 在追）

---

## 我踩坑紀錄 #3：sandbox 是 best-effort

- `--sandbox` 不是 container 等級隔離
- network 還是出得去（DNS / HTTPS）
- 真要 isolate：套 Docker 或 Cloud Shell（今天就在示範）

---

## 我踩坑紀錄 #4：別開公開 wrapper repo

- ACP 是熱題，但 Google 隨時可能 ship native 版
- 你的 wrapper 一夜 obsolete
- 自用 cron 包一層 OK，**別投入長期維護**

> 給工程師的提醒：**stopgap 的價值在快，不在精**。

---

## Section 收尾

- ✅ 認清兩個 agy
- ✅ 5 個 flag 撐住日常
- ✅ Cloud Shell lab 跑過第一支 agent
- ✅ Migration 心智模型建立

**下一段（Part 3）**：把 `agy` 接進 **OpenAB** 多 agent 編排
→ 為什麼今天的 wrapper PR 被拒，等 Google 出 ACP

---

# 休息 5 分鐘
## 回來進 Part 3：OpenAB × Antigravity

Questions? → 找我或 GDE GenAI Circle
