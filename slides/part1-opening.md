---
marp: true
theme: default
paginate: true
header: "BwAI Tainan 2026-05-23 — Vibe Coding × 無限可能"
footer: "Jimmy Liao · Google AI GDE · @jimmyliao"
---

<!-- _class: lead -->
# Vibe Coding × Gemini 無限可能

## 從 Gemini CLI 接班到 Antigravity Ecosystem

Jimmy Liao（廖嘉禾）
Google AI GDE / LeapDesign.ai

🗓 2026-05-23 · Build with AI Tainan

---

# 自介 — 30 秒

- **Jimmy Liao** · LeapDesign.ai CEO
- **Google AI GDE** (台灣 · 2024–)
- 過去 12 個月走過：digest-agent / LeapAgent / HISP RAG / PetCircle
- **Fleet**: 11 台機器，Tailscale 串接，跨 macOS / Linux / Windows
- 工作流：**Claude + Gemini 雙引擎**，agy CLI 接班中

> 「我不教你寫 code，我帶你看 ecosystem 怎麼動。」

---

# 今天要講什麼

| Part | 內容 | 時間 |
|------|------|------|
| 1 | 開場 + 5/9 之後發生了什麼 | 5 min |
| 2 | **Antigravity CLI 入門**（Cloud Shell hands-on）| 30 min |
| 3 | **OpenAB × agy / gemini Live Demo** | 25 min |
| 4 | ADK 模式（從 5/9 簡化）| 8 min |
| 5 | GEAP 一頁帶過 | 3 min |
| 6 | Q&A + 社群預告 | 4 min |

⭐ **重點：Part 2 + Part 3 才是今天的新東西。**

---

<!-- _class: lead -->
# 5/9 之後，發生了什麼

## 一個 ecosystem 4 天加速版

---

# 5/19 — Google 公告

![bg right:40% 90%](https://placeholder.com/google-blog)

**`Gemini CLI` 6/18 停服 Pro / Ultra free tier**

- Enterprise tier 持續
- 接班 = **Antigravity CLI**（`agy`）
- 「terminal-first coding agent，VS Code fork IDE 同步出」

🔗 developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli

---

# Story Arc — 4 天 ecosystem 動了什麼

```
[5/19 Google]    Gemini CLI 6/18 sunset 公告
                 │
[5/20 Community] GitHub issue #31 (Antigravity CLI ACP feature request)
                 │   ↳ Joseph19820124 open，列完整需求
                 │
[5/21 Community] OpenAB PR #863 (Node.js wrapper)
                 │   ↳ 被 maintainer Pahud 以「temporary hack」拒絕
                 │
[5/22 02:00]     我以 GDE 身份留 issue #31 comment ⭐
                 │   ↳ dual-engine use case + 6/18 deadline + 修勘誤
                 │
[5/22 11:51]     Pahud merge PR #896 — agy-acp Rust adapter
                 │   ↳ OpenAB v0.8.4-beta.2 正式支援 Antigravity CLI
                 │
[5/23 13:00]     ⭐ 我們現在 ⭐
                 ↓
                 Live demo agy + OpenAB
```

⭐ **這就是 Vibe Coding：你不是在追 ecosystem，你是 ecosystem 的一份子。**

---

# 三個身分，三個角度看同件事

| 你是誰 | 6/18 之後 | 今天 take-away |
|--------|----------|----------------|
| **學生 / 新手** | gemini-cli 沒了怎辦？ | **agy CLI 完全接班 → Part 2 跟著做** |
| **production dev** | 我的 multi-agent 工具鏈會死嗎？ | **OpenAB v0.8.4-beta.2 已接 agy → Part 3 看現場** |
| **OSS contributor** | 社群移動有多快？ | **issue → PR → merge 10 小時內 → 你也可以推一把** |

---

# Demo 環境

- **學員端**：Cloud Shell（[shell.cloud.google.com](https://shell.cloud.google.com)）
  - 只要 Google account，**零本地安裝**
  - 進門掃 QR code 填 email → 我加進 workshop GCP project（給 ADK 用）
- **講師端**：home lab M4 mac mini（剛今天遷 OrbStack）
  - OpenAB v0.8.4-beta.2 + 兩個 agent backend（agy / gemini）
  - 學員 Discord 加入 channel 即時看 bot 回應

---

<!-- _class: lead -->
# 開始 Part 2

## Antigravity CLI 入門

打開 Cloud Shell：**shell.cloud.google.com**
