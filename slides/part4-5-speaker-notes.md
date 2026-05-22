# Part 4 + Part 5 — 講者 notes（繁中 conversational）

**Total**: Part 4 = 8 min / Part 5 = 3 min / **合計 11 min**

定位：reuse 5/09 Taichung 內容簡化版。inspirational 不是 instructional —
讓學員「知道有這條路」，回家自己 explore，不在現場手把手教。

---

## Part 4 — 從個人 vibe coding 到 multi-agent production（8 min）

### Slide 1 — 我們在哪裡？（~45 sec）

**開場**：
> 「OK，前面兩段 hands-on / live demo 之後，現在我們收一下，看一個更大的圖。」

**重點**：
- 快速回顧 Part 2/3 學員學會了什麼（agy CLI + OpenAB dual-engine）
- 拋問題：「那 production 多 agent 協作呢？」

**轉場**：
> 「用 digest-agent 這個我 5/09 在台中講過的真實案例帶你看。」

⏱ 45 sec

---

### Slide 2 — Pain point（~1 min）

**開場**：
> 「我每天早上想自動拿到一份『個股研究 digest』— 抓新聞、做產業分析、看市場情緒、給投資建議。一個 prompt 全包？」

**重點**：
- 強調**單一 agent 的三個痛點**：context 爆炸、prompt 牽一髮動全身、debug 找不到誰錯
- 用「責任邊界」這個詞 — 工程師都聽得懂

**轉場**：
> 「拆成 4 個 specialist，每個只管一件事，串起來。問題是 — 怎麼串？」

⏱ 1 min

---

### Slide 3 — ADK 1-pager（~1.5 min）

**開場**：
> 「Google 有一套 framework 叫 ADK — Agent Development Kit。簡單講就是把 agent 當 class 寫。」

**重點 1（核心 4 個 class）**：LlmAgent / SequentialAgent / ParallelAgent / Session
- 不用每個都細講，重點：「declarative 的方式描述 agent 之間的關係」

**重點 2（為什麼用 framework）**：
- session state、streaming、tool calling、eval **不要自己刻**
- 同份 code 本地能跑、丟雲端也能跑 — 這個是 Part 5 的伏筆

**轉場**：
> 「實際長什麼樣？看 digest-agent。」

⏱ 1.5 min

---

### Slide 4 — digest-agent 架構圖（~2 min）

**開場**：
> 「這就是 digest-agent。一個 SequentialAgent，四個 sub-agent 串聯。」

**重點**：
- 依序點過 4 個 sub-agent **各自的職責**（不要念名字，講做什麼）
- 強調**「shared Session state」** — agent 之間怎麼傳資料
- 對應回 Slide 2 的痛點：每個 agent 責任清楚、可以單獨換模型、單獨 debug

**互動**：
> 「有沒有人寫過 LangGraph 或 CrewAI？舉手看看。」（看現場反應決定要不要做 framework 對比）

**轉場**：
> 「code 長什麼樣？放心，不用抄，看意思就好。」

⏱ 2 min

---

### Slide 5 — ADK code snippet（~2 min）

**開場**：
> 「這是真實會跑的 ADK Python code，我精簡掉一些 detail。」

**重點**：
- 指出 `LlmAgent(name=, model=, instruction=, tools=)` 四個參數
- 指出 `SequentialAgent(sub_agents=[...])` 就是「把 4 個串起來」
- 強調**每個 agent 獨立寫、獨立測** — 工程師的耳朵會豎起來

**金句**：
> 「prompt engineering 從 art 變 software engineering — 因為你可以 unit test 它了。」

**轉場**：
> 「等等 — 那 agy CLI 跟 ADK 是什麼關係？我同學可能有點亂。」

⏱ 2 min

---

### Slide 6 — CLI vs ADK 澄清（~45 sec）

**開場**：
> 「特別澄清一下，因為今天台南場我們 Part 2/3 用 agy，不是 5/09 的 gemini-cli。」

**重點**：
- **agy = 你個人 terminal 對話的 CLI**
- **ADK = 在 server 上跑 production 多 agent 的 framework**
- 兩個都吃 Gemini 家族 model，**不衝突，是不同層**

**金句**：
> 「CLI 是你跟 agent 講話的入口，ADK 是 agent 跟 agent 講話的骨架。」

**轉場**：
> 「OK，ADK code 寫好了。然後咧？放哪裡跑？這就接到 Part 5。」

⏱ 45 sec

---

### Slide 7 — Production deploy 怎辦（~30 sec）

**重點**：
- 列三條路：Cloud Run + FastAPI / Lambda 跨雲 / GEAP
- 賣關子，直接接 Part 5

⏱ 30 sec

**Part 4 合計**：45 + 60 + 90 + 120 + 120 + 45 + 30 = **510 sec ≈ 8.5 min** ✅

---

## Part 5 — GEAP pointer（3 min）

⚠️ **這段只有 3 分鐘，不要 deep dive，就是 pointer**。
講者如果發現自己想多講 → 停下來，丟回 5/09 deck。

---

### Slide 1 — GEAP 是什麼（~1 min）

**開場**：
> 「GEAP — Google Agent Engine Platform，Vertex AI 旗下的 managed agent hosting。」

**重點**：
- 比喻：「就像 Cloud Run 之於 container，GEAP 之於 ADK agent」
- 一句話 summary：**ADK code → `gcloud agent-engines deploy` → HTTPS endpoint**
- 5 個 bullet 點不要每個細講，掃過 auto-scaling / session / streaming / eval / IAM

⏱ 1 min

---

### Slide 2 — 3 條路線（~1 min）

**開場**：
> 「production deploy 沒有單一正確答案。」

**重點**：
- **強調「看需求選」** — 不要捧 GEAP 一個
- GEAP = enterprise / SLA / audit
- Cloud Run + Gemini API = 個人 / 小團隊（**多數學員會走這條**）
- 本地 Gemma = 醫療 / 政府 / 資料不能出去（呼應 HISP 我自己案例可以一句帶過）

**金句**：
> 「合規 / scale / 預算 — 三選二最舒服，三個都要就準備花錢。」

⏱ 1 min

---

### Slide 3 — Deep dive 不在今天 + 收尾（~1 min）

**開場**：
> 「3 分鐘真的講不完 GEAP，所以今天不深講。」

**重點**：
- **明確 pointer 給 5/09 deck URL**：https://bwai0509.web.app/
- 列 5/09 有什麼（GEAP walkthrough / repo / Lab A/B/C / cost comparison）
- 收尾 take-away 3 點：agy 接班 / OpenAB dual-engine / ADK + GEAP 存在

**轉場**：
> 「接下來 Q&A 時間，社群預告我順便講。」

⏱ 1 min

**Part 5 合計**：60 + 60 + 60 = **180 sec = 3 min** ✅

---

## 整段流程整合 cue

- **進 Part 4 之前**：Part 3 demo 剛收（學員可能還在興奮），給 10 sec 喝水靜一下
- **Part 4 → Part 5**：不要停，直接接「然後咧？放哪？」
- **Part 5 → Q&A**：時間壓力大，5/09 URL 直接打在 slide 上不用念
- **如果 Part 3 超時**：Part 4 可以砍 Slide 5 code snippet（省 2 min），用「我有 code 在 5/09 repo」帶過
- **如果 Part 3 提早結束**：Part 4 多花時間在 Slide 4 架構圖跟學員互動，問他們會怎麼拆 agent

---

## TODO / 待補

- [ ] Slide 4 架構圖建議補一張**實際 screenshot**（5/09 deck 應該有，可 reuse）
- [ ] Slide 5 ADK code snippet **請 Jimmy 從 digest-agent repo 核對一次**真實 API 名稱 / 版本
- [ ] Slide 3 ADK class table 確認 5/23 當下 ADK 最新 stable 版有沒有 class rename
- [ ] Part 5 Slide 2 cost table 數字（如果現場想丟）— 從 5/09 deck 抓
