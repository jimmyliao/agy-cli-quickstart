# Antigravity Talk — 2026-06-12 (Fri)

> 這週五的分享：主題 **Antigravity**，其中會帶到一點 **agy CLI**。

## 範圍
- 主軸：Antigravity（Google 的 agentic coding 產品）
- 穿插：`agy` CLI（terminal-first coding agent，接棒 gemini-cli）小段介紹/demo

## Demo 素材

### agy 內建自我更新（self-update）
`agy update` 一鍵升級，無痛體驗——適合現場 demo「工具自己會長大」。

- 截圖：[`assets/screenshots/agy-self-update-1.0.4-to-1.0.6.png`](../assets/screenshots/agy-self-update-1.0.4-to-1.0.6.png)
- 內容：`agy --version` → `1.0.4`；`agy update` → Found new version **1.0.6** → Downloading → Extracting → Verification successful → Installed → *Please restart agy*
- demo 話術：版本迭代快（1.0.4 → 1.0.6 幾天內），show 出官方更新節奏 + 驗證(verification)步驟。

## 可選 demo 點子（個人 tooling 連動）
- `ccs`（[agent-cli-sessions](https://github.com/jimmyliao/agent-cli-sessions)）規劃中的 **agy adapter**：列出/還原 agy session。若週五前來得及逆向 `~/.antigravity/` session 格式，可當 one-more-thing。否則純口頭帶過「正在做」。

## TODO（talk 前）
- [ ] 確認 demo 機 agy 版本（restart 後應為 1.0.6）
- [ ] 串一條最短 `agy -p "..."` 非互動 demo
- [ ] 投影片補 self-update 截圖
