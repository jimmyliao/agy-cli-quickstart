---
marp: true
theme: default
paginate: true
---

# Part 4 — 從個人 vibe coding 到 multi-agent production

**8 min · talk + diagram only · 改裝自 5/09 Taichung deck**

Jimmy Liao · Google AI GDE · BWAI Tainan 2026-05-23

---

## 我們在哪裡？

Part 2 / 3 你已經會了：

- ✅ 用 **agy CLI** 在 Cloud Shell 對話 + headless `-p` 跑批次
- ✅ 透過 **OpenAB** 把 agy / gemini 兩顆 CLI 接到 Discord
- ✅ 同一個 channel，dual-engine showdown

**那 production 多 agent 協作呢？**
一個 agy 不夠用的時候怎辦？

---

## Pain point — 為什麼一個 agent 不夠

實際案例：digest-agent（5/09 demo 的同一個專案）

> 每天早上自動產出「個股研究 digest」：抓新聞 → 產業分析 → 市場情緒 → 投資建議

一個 prompt 塞所有任務？

- ❌ 上下文爆炸，回答又長又糊
- ❌ 換模型 / 改 prompt 牽一髮動全身
- ❌ 沒有清楚的責任邊界，debug 找不到誰錯

**拆成 4 個 specialist agent，sequential 串起來。**

---

## ADK 1-pager — Google Agent Development Kit

Python / TypeScript framework，把 agent 當「class」寫：

| 元件 | 角色 |
|------|------|
| `LlmAgent` | 單一 agent，綁 model + instruction + tools |
| `SequentialAgent` | 多 agent 串聯（A → B → C → D）|
| `ParallelAgent` | 多 agent 並行（fan-out / fan-in）|
| `Session` / `State` | agent 之間共享 context |

**為什麼用 framework 不自己寫？**

- session state / streaming / tool calling / eval 都已經做好
- 同一份 code 可以本地跑、也可以丟 Vertex AI 上 scale
- Gemini API / Vertex AI / 自架 Gemma 三種 backend 可切換

---

## digest-agent 架構圖

```
                 ┌──────────────────────────────────┐
                 │     SequentialAgent (orchestrator) │
                 └──────────────────────────────────┘
                                  │
        ┌─────────────┬───────────┴────────────┬─────────────┐
        ▼             ▼                        ▼             ▼
 ┌─────────────┐ ┌──────────────┐  ┌──────────────┐ ┌──────────────────┐
 │   news_     │ │  industry_   │  │   market_    │ │     stock_       │
 │ collector   │→│   analyst    │ →│   analyst    │→│  orchestrator    │
 └─────────────┘ └──────────────┘  └──────────────┘ └──────────────────┘
   抓新聞 RSS      產業趨勢解讀        市場情緒 / 籌碼        綜合 → 投資 digest
       │                │                   │                   │
       └────────────────┴───────────────────┴───────────────────┘
                          shared Session state
```

**每個 sub-agent 都是一個 `LlmAgent`**，各自綁 prompt + tools。

---

## ADK code 長這樣（不用抄，看意思就好）

```python
from google.adk.agents import LlmAgent, SequentialAgent

news_collector = LlmAgent(
    name="news_collector",
    model="gemini-2.5-pro",
    instruction="抓今日財經新聞，依公司 / 產業分類輸出 JSON。",
    tools=[fetch_rss, search_web],
)

industry_analyst = LlmAgent(
    name="industry_analyst",
    model="gemini-2.5-pro",
    instruction="基於 news_collector 輸出，產出產業趨勢分析。",
)

# ... market_analyst, stock_orchestrator 略

digest_pipeline = SequentialAgent(
    name="digest_pipeline",
    sub_agents=[news_collector, industry_analyst,
                market_analyst, stock_orchestrator],
)
```

**重點**：每個 agent 獨立寫、獨立測，pipeline 用 declarative 串起來。

---

## CLI 是入口，ADK 是骨架

**澄清一下 model 選擇**：

| 場景 | 用什麼 |
|------|--------|
| 你個人在 terminal vibe coding | **agy CLI**（Part 2/3 學的）|
| Production multi-agent service | **ADK + Gemini API / Vertex AI** |

ADK code 內你還是寫 `model="gemini-2.5-pro"`，
跟你在 agy CLI 對話的是**同一個 Gemini 家族**。

只是一個是「跟你聊」，一個是「在 server 上 24/7 跑」。

---

## Production deploy 怎辦？

ADK code 寫完了，session state、streaming、tool calling 都 OK。

**然後咧？放哪裡跑？**

- 自己 Cloud Run + FastAPI？可以但 session / scaling / observability 要自己接
- Lambda + DynamoDB？跨雲 + 自己刻 SSE
- **GEAP — Google Agent Engine / Agent Engine Platform**（Vertex AI managed）

➡️ **Part 5（3 min）：GEAP 一頁帶過**
