# Cloud Shell × Antigravity CLI Hands-on Lab

**Build with AI Tainan · 2026-05-23 · Part 2**
講師：Jimmy Liao (Google AI GDE)
時間：約 10–12 分鐘

---

## ⚠️ 給 Jimmy 上場前確認的 TODO

- [x] **官方安裝指令確認**（5/22 21:00 antigravity.google/download）：`curl -fsSL https://antigravity.google/cli/install.sh | bash`
  目前已知頁面：<https://antigravity.google/docs/cli-using>
  上場前抓最新一行 curl 指令塞進 Step 1，並驗一次 Cloud Shell 能跑。
- [ ] 確認 6/18 之前 Cloud Shell 預裝的 `gemini` 還在，作為對照組
- [ ] 預先在自己帳號跑過一次 OAuth flow，現場才知道 device code UX

---

## Step 0 · 打開 Cloud Shell

打開瀏覽器：

```
https://shell.cloud.google.com
```

用你的 Google 帳號登入。等 VM 起來，提示符應該長這樣：

```
you@cloudshell:~ (your-project-id)$
```

**踩坑點**：第一次開可能要 30 秒，看到 `Provisioning your Cloud Shell machine...` 不要慌。

**想想看**：Cloud Shell 本身就是一台 Linux VM，5GB persistent `$HOME`，每個 session 都是同一台。

---

## Step 1 · 安裝 `agy` CLI

```bash
# 官方一鍵安裝（5/22 21:00 從 antigravity.google/download 確認）
curl -fsSL https://antigravity.google/cli/install.sh | bash

# 確認進 PATH
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

**Windows PowerShell 變體**（Cloud Shell 不會用到，但學員回家自學參考）：
```powershell
irm https://antigravity.google/cli/install.ps1 | iex
```

**預期輸出**：

```
Installed agy v1.0.0 to /home/you/.local/bin/agy
```

**踩坑點**：
- 如果 `agy: command not found` → PATH 沒進去，重跑 `export` 那行
- 如果 install script 404 → 換用 Jimmy 黑板上的備援 URL

---

## Step 2 · 確認版本 + 看 help

```bash
agy --version
agy --help | head -30
```

**預期輸出**：

```
1.0.0
```

```
Usage: agy [options] [prompt]

Options:
  -p, --print                       Non-interactive headless mode
  -i, --prompt-interactive          Initial prompt, then interactive
  -c, --continue                    Continue last conversation
      --conversation <id>           Resume specific session
  ...
```

**想想看**：跟你以前用的 `gemini --help` 排版有沒有像？這不是巧合 —— 同一個團隊。

---

## Step 3 · 第一次對話（headless）

```bash
agy -p "say hello in 3 languages, one per line"
```

**預期輸出**（內容會略不同）：

```
Hello!
你好！
こんにちは！
```

**踩坑點**：
- 第一次跑會跳 OAuth device flow，照螢幕指示貼 code
- 卡 5 分鐘沒回應 → Ctrl-C，網路或 OAuth 沒過

**想想看**：這就是 `agy` 最小單位的呼叫。可以塞進 cron、Telegram bot、CI pipeline。

---

## Step 4 · 接續對話 `-c`

```bash
agy -c "now translate the same to Korean and Vietnamese"
```

**預期輸出**：

```
안녕하세요!
Xin chào!
```

**踩坑點**：
- `-c` 不帶 prompt 會進 interactive mode，現場 demo 請務必帶 prompt
- session 存在 `~/.config/agy/` 底下，Cloud Shell home 是 persistent 所以 OK

**想想看**：跟 `gemini -c` 一模一樣 —— migration 心智模型成立。

---

## Step 5 · 餵檔案進去 refactor

```bash
cat > demo.py <<'EOF'
def hello(name):
    print("Hello, " + name + "! Today is " + str(__import__("datetime").date.today()))
EOF

cat demo.py | agy -p "refactor this Python to use f-strings and add a type hint"
```

**預期輸出**（範例）：

```python
from datetime import date

def hello(name: str) -> None:
    print(f"Hello, {name}! Today is {date.today()}")
```

**踩坑點**：
- 注意：`agy` 不吃 `@filename` 語法（Gemini CLI 有），要走 stdin pipe
- 大檔案（>50KB）建議 `--add-dir .` 把整個資料夾掛上去

**想想看**：CI 裡你可以用這個跑 auto-refactor PR。

---

## Step 6 · Sandbox 模式

```bash
agy --sandbox -p "list files in /etc and tell me what each one does"
```

**預期觀察**：
- agent 可能會問你「需要讀 `/etc/passwd`，允許嗎？」→ Cloud Shell 內 yes 沒差
- 加上 `--dangerously-skip-permissions` 就不會問

```bash
agy --sandbox --dangerously-skip-permissions -p "list files in /etc"
```

**踩坑點**：
- `--sandbox` 是 best-effort，不是 container 隔離
- 真要 isolate：套 Docker 或像現在這樣，整段跑在 Cloud Shell 拋棄式 VM 裡

**想想看**：unattended job 一定要 `--dangerously-skip-permissions`，不然會卡在 prompt 永遠不結束。

---

## Stretch · 寫一個 mini cron job

把以下存成 `daily-summary.sh`：

```bash
#!/usr/bin/env bash
set -euo pipefail

cd "$HOME"
TODAY=$(date +%Y-%m-%d)
LOG="$HOME/daily-${TODAY}.md"

agy --dangerously-skip-permissions -p "
Give me 3 trending topics in AI today, format as markdown bullets.
" > "$LOG"

echo "Wrote $LOG"
cat "$LOG"
```

跑一次：

```bash
chmod +x daily-summary.sh
./daily-summary.sh
```

如果 Cloud Shell 支援 cron（要 always-on VM 才實用），可以加：

```bash
crontab -e
# 加這行：每天早上 9 點跑
0 9 * * * /home/you/daily-summary.sh
```

**想想看**：把 `agy -p` 想成 `curl` 等級的 building block，30 行 bash 就是一個 agent。

---

## 完成！你應該已經會：

- [x] 安裝 standalone `agy` CLI
- [x] 用 5 個核心 flag（`-p` / `-c` / `--conversation` / `--add-dir` / `--sandbox` / `--dangerously-skip-permissions`）
- [x] 把檔案 pipe 進 agent
- [x] 寫一支 cron-able 自動化腳本

下一段 Part 3 我們會把 `agy` 接進 **OpenAB**，做多 agent 編排。

---

## 卡關備援

| 症狀 | 解法 |
|---|---|
| `agy: command not found` | `export PATH="$HOME/.local/bin:$PATH"` |
| OAuth device flow 卡住 | Ctrl-C 重跑，確認 Cloud Shell 沒被 popup blocker 擋 |
| `Error: timeout after 300s` | 預設 5 分鐘，重跑或拆小 prompt |
| Cloud Shell session 中斷 | home 是 persistent，重連繼續 |
| `agy -p` 沒輸出 | 加 `-v` / `--verbose`（如果有）；或檢查 stderr |

現場找 Jimmy 或 Cloud Shell 旁邊的 GDE 同伴。
