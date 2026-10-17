# 🤖 Autonomous Research Agent

> A ReAct-style AI research agent that searches the web and Wikipedia,
> then generates structured research reports — powered by **Phi-2**,
> **LangChain**, and **free tools only**.

---

## 📌 Overview

This project implements an **Autonomous Research Agent** using a
controlled ReAct-style pipeline. Given any research topic, the agent:

1. 🔍 Searches the web using **DuckDuckGo**
2. 📖 Fetches encyclopedic context from **Wikipedia**
3. 🧹 Cleans and combines the retrieved data
4. 🤖 Generates a **structured 5-section report** using **Microsoft Phi-2**
5. 💾 Saves the report as a `.txt` file

---

## 🧠 Architecture

```
User Input
   │
   ▼
┌─────────────────────────────────────┐
│         ReAct Agent Loop            │
│  Thought → Action → Observe         │
│                                     │
│  Step 1: search_web()               │
│  Step 2: search_wikipedia()         │
│  Step 3: generate_detailed_report() │
└─────────────────────────────────────┘
   │
   ▼
Structured Report (.txt)
```

---

## 🛠️ Tech Stack

| Component     | Technology                        |
|---------------|-----------------------------------|
| Language      | Python 3.14.3                     |
| Environment   | Google Colab                      |
| LLM           | Microsoft Phi-2 (HuggingFace)     |
| LLM Wrapper   | LangChain + HuggingFacePipeline   |
| Web Search    | DuckDuckGo (`ddgs`)               |
| Knowledge     | Wikipedia API                     |
| Cost          | 100% Free                         |

---

## 💻 Core Components (API Reference)

This project relies on robust Python functions that govern the agent's behavior and generation pipeline:

- `search_web(topic: str, max_results: int = 3) -> str`: Uses the `ddgs` library to execute internet searches and parse the top semantic results based on the provided topic.
- `search_wikipedia(topic: str, sentences: int = 5) -> str`: Interfaces with the Wikipedia API to retrieve encyclopedic context and resolve disambiguations for the target entity.
- `generate_detailed_report(topic: str, context: str, llm) -> str`: The main text generation coordinator. It implements a multi-step logic by delegating to `generate_section` to invoke the Microsoft Phi-2 model five separate times for each logical section, ensuring maximum quality and focused generations.
- `agent_decide(steps_done: list) -> Tuple[str, str]`: A deterministic, rule-based decision engine answering the agent's control flow logic. Evaluates completed steps (`search_web`, `search_wikipedia`, `generate_report`) and returns the next Thought and Action pairs.
- `run_react_agent()`: The primary entry point. Initializes the agent memory block and loops over the `agent_decide` state machine in a canonical Thought → Action → Observe pattern until completion.

---

## 📁 Repository Structure

```
autonomous-research-agent/
│
├── Automated_Research_and_Report_Generation_Agent.ipynb   ← Main Colab Notebook
│
├── output_samples/
│   ├── Impact_of_AI_in_Healthcare.txt
│   └── Future_of_Renewable_Energy.txt
│
└── README.md
```

---

## 🚀 Getting Started

### 1. Open in Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QegqJYWSaRYkvyUNZb3RtV79gO4_H9sE?usp=sharing)

### 2. Set Runtime to GPU
```
Runtime → Change Runtime Type → T4 GPU
```

### 3. Run All Cells in Order
```
Cell 1  → Install dependencies
Cell 2  → Import libraries
Cell 3  → Load Phi-2 model       ← takes ~5 mins on first run
Cell 4  → DuckDuckGo search tool
Cell 5  → Wikipedia tool
Cell 6  → Text cleaning utilities
Cell 7  → Detailed report generator
Cell 8  → Save report utility
Cell 9  → ReAct agent loop
Cell 10 → ▶️ RUN — enter your topic here
```

### 4. Enter Your Topic When Prompted
```
📌 Enter research topic: Impact of AI in Healthcare
```

---

### Optional: Research with Parallel Search MCP

Follow the [Parallel Search MCP Colab guide](docs/parallel-search.md) to gather
web excerpts and source URLs with free, keyless search, then pass the context to
the existing report generator. This is a separate manual workflow; the default
DuckDuckGo and Wikipedia agent stays the same.

---

## 📤 Sample Output

```
=================================================================
                    AUTONOMOUS RESEARCH AGENT
                         Research Report
=================================================================

Topic: Future of Renewable Energy
Date : March 26, 2026

=================================================================
                      AGENT EXECUTION TRACE
=================================================================
  Steps : search_web → search_wikipedia → generate_report
  Total : 3 step(s)

═════════════════════════════════════════════════════════════════
  1. INTRODUCTION
─────────────────────────────────────────────────────────────────
Renewable energy refers to power generated from naturally
replenished sources such as solar, wind, hydro, and geothermal.
...

  2. KEY FINDINGS
─────────────────────────────────────────────────────────────────
  • Solar energy costs dropped 89% over the last decade.
  • Wind capacity tripled globally since 2010.
  ...
```

> 📂 See full sample reports in the [`output_samples/`](./output_samples/) folder.

---

## 📊 Report Structure

Every generated report includes:

| # | Section | Description |
|---|---------|-------------|
| 1 | Introduction | What the topic is and why it matters |
| 2 | Key Findings | 5 specific facts or statistics |
| 3 | Challenges | 4 bullets — Technical, Ethical, Economic, Social |
| 4 | Future Scope | Near-term trends and long-term possibilities |
| 5 | Conclusion | Summary and closing thought |

---

## ⚙️ ReAct Agent Behaviour

The agent follows a **Thought → Action → Observe** loop:
```
💭 THOUGHT : I don't have web data yet. I should search the web.
⚡ ACTION  : search_web
👁  OBSERVE : Running web search... ✅ (980 chars retrieved)

💭 THOUGHT : I have web data but need encyclopedic context.
⚡ ACTION  : search_wikipedia
👁  OBSERVE : Running Wikipedia search... ✅ (574 chars retrieved)

💭 THOUGHT : I have all context needed. Time to write the report.
⚡ ACTION  : generate_report
👁  OBSERVE : Generating detailed report... ✅
```

---

## ⚠️ Known Limitations

- Phi-2 is a small model — section quality may vary by topic
- DuckDuckGo may rate-limit on rapid repeated searches
- Generation takes 3–8 minutes on Colab CPU (faster on T4 GPU)
- Report quality improves with specific, well-known topics

---

## 🔧 Requirements

All dependencies are installed inside the notebook. No local
setup required.
```
langchain
langchain-community
langchain-huggingface
transformers
accelerate
ddgs
wikipedia
huggingface_hub
torch
```

---

## 🙌 Acknowledgements

- [Microsoft Phi-2](https://huggingface.co/microsoft/phi-2)
- [LangChain](https://www.langchain.com/)
- [DuckDuckGo Search](https://pypi.org/project/ddgs/)
- [Wikipedia API](https://pypi.org/project/wikipedia/)

---

## 👤 Author

**Aditya Pandey**
- GitHub: [@ADP-1](https://github.com/ADP-1)

---

⭐ **If you found this useful, give it a star!**

---
