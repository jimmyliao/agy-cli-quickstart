---
marp: true
theme: default
paginate: true
---

# Part 5 — GEAP：managed agent platform

**3 min · single-page pointer · 不深入，知道存在就好**

Jimmy Liao · Google AI GDE · BWAI Tainan 2026-05-23

---

## GEAP 是什麼

**Google Agent Engine Platform**（Vertex AI 旗下的 managed agent hosting）

把你寫好的 ADK agent 包成 service，丟上去就跑：

- ✅ Auto-scaling（含 scale-to-zero）
- ✅ Session / state 託管（不用自己接 DB）
- ✅ SSE streaming 內建（給前端用）
- ✅ Vertex AI eval / tracing / monitoring 一條龍
- ✅ IAM / VPC-SC / audit log 等企業治理直接套用

**簡單講**：ADK code → `gcloud agent-engines deploy` → 拿到 HTTPS endpoint。

---

## 3 條 production 路線怎麼選

| 路線 | 何時用 | 成本 | 心智負擔 |
|------|--------|------|----------|
| **GEAP**（Vertex AI managed）| Enterprise scale / 要 SLA / 要 audit | 高 | 低 |
| **Cloud Run + Gemini API** | 個人 / 小團隊 / 快速 ship | 中 | 中 |
| **本地 Gemma 4 / on-prem** | 資料不能出去 / 政府案 / 醫療 | 低（硬體高）| 高 |

5/09 digest-agent 走的是 **GEAP**，因為要 daily 排程 + 多人看。

但**沒有單一正確答案** — 看你的 scale、預算、合規需求。

---

## Deep dive 不在今天

3 分鐘真的講不完 GEAP — auto-scaling、streaming、auth、cost 都是大主題。

**想看完整版？**

➡️ **5/09 Taichung BWAI 場 deck**：https://bwai0509.web.app/

裡面有：

- GEAP deploy 完整 walkthrough
- digest-agent 4 sub-agent code（GitHub repo link）
- Lab A/B/C：Cloud Run / Slack publisher / prompt format
- 3 路線 cost comparison 試算

---

## 回到今天的主軸

今天台南場的 take-away：

1. **Gemini CLI 6/18 退場**，agy 接班 — 你已經會用了
2. **OpenAB**：把 CLI 變 multi-platform bot — 你看過 dual-engine demo 了
3. **ADK + GEAP**：production multi-agent 的路 — 你知道存在了

**今晚回家做什麼？**

- 把 Cloud Shell agy session 留著繼續玩
- 去 5/09 deck 看 digest-agent repo，clone 下來跑
- 加入 BwAI 社群 Antigravity 共筆計畫（Q&A 細講）

➡️ **Part 6：Q&A + 社群預告**
