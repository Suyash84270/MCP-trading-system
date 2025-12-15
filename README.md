#  Autonomous Trading System using AI Agents + MCP Protocol  
### Built with Python, MCP Servers, Multi-Agent Architecture & Gradio Dashboard  
**Author:** Suyash Sharma  

---

##  Overview

This project implements a **fully autonomous multi-agent trading system** powered by:

- **AI Agents (OpenAI Agents Framework)**
- **MCP (Model Context Protocol) Servers**
- **Financial data ingestion**
- **Persistent account and trading logic**
- **Autonomous researcher agents**
- **A real-time Gradio dashboard UI**
- **A trading floor scheduler**

The system simulates a realistic trading environment where **four autonomous traders** and a **researcher agent** coordinate through multiple MCP servers to:

- fetch stock prices  
- perform web research  
- store and recall memory  
- manage portfolios  
- execute trades  
- update logs & timeline  
- display results on the UI  

This README explains **every part of the system**, so anyone can understand and run the project.

---

#  Features

###  1. **Autonomous Trading Agents**
Each trader:
- Reacts to market data  
- Researches news  
- Executes trades  
- Stores memory  
- Adjusts strategy over time  
- Evaluates profit/loss  

###  2. **Research Agent**
Searches the web using:
- Brave Search MCP server  
- Fetch MCP server  
- Memory MCP server  

Produces structured financial insights.

###  3. **Financial Market Data**
Supports:
- Polygon.io (real-time or free EOD API)  
- Local fallback simulation if API rate limits  

###  4. **Account & Portfolio System**
Includes:
- Buy/Sell logic  
- Balance management  
- Holdings tracking  
- P/L calculation  
- Time-series portfolio value history  

---

#  Project Architecture

```
project/
├── accounts.py
├── accounts_server.py
├── accounts_client.py
├── market.py
├── market_server.py
├── push_server.py
├── mcp_params.py
├── traders.py
├── trading_floor.py
├── reset.py
├── templates.py
├── tracers.py
├── util.py
├── app.py                # Gradio UI
│
├── 1.ipynb               # Intro MCP servers
├── 2.ipynb               # Custom MCP server + client
├── 3.ipynb               # Advanced MCP + Polygon.io
├── 4.ipynb               # Autonomous trading example
└── 5.ipynb               # Multi-agent trading simulation
```

---

 Architecture Diagram

                         ┌──────────────────────────┐
                         │     Gradio Dashboard     │
                         │  (UI for Real-Time View) │
                         └─────────────┬────────────┘
                                       │
                                       ▼
                     ┌────────────────────────────────────┐
                     │      Autonomous Trading Floor      │
                     │   4 Traders + 1 Research Agent     │
                     └─────────────┬─────────────┬────────┘
                                   │             │
                                   ▼             ▼
              ┌────────────────────────┐   ┌──────────────────────────┐
              │   Researcher Agent     │   │   Trader Agents (x4)     │
              │  - Web search          │   │  - Execute trades         │
              │  - Fetch websites      │   │  - Read market data       │
              │  - Save memory         │   │  - Change strategies      │
              └──────────┬─────────────┘   └──────────┬───────────────┘
                         │                            │
            ┌────────────┴────────────────────────────┴───────────────┐
            │                        MCP Servers                       │
            ├──────────────────────────────────────────────────────────┤
            │  Accounts Server        → Buy/Sell/Balance/Holdings      │
            │  Market Server          → Share Prices (Free/Paid API)   │
            │  Brave Search Server    → News + Research                 │
            │  Fetch Server           → Fetch webpages                   │
            │  Memory Server          → Persistent knowledge             │
            │  Push Server            → Notifications                    │
            └──────────────────────────────────────────────────────────┘


---

#  Core Components Explained

## 1️ MCP Servers

The system uses **five MCP servers**:

| Server | Purpose |
|--------|---------|
| **accounts_server** | Manage account balances, holdings, transactions |
| **market_server** | Provides stock prices via Polygon API or fallback |
| **push_server** | Sends push notifications |
| **memory server** | Persistent memory using SQLite (libsql) |
| **brave-search server** | Web research & market news |

These run **locally** but behave as **external services**, accessed through `MCPServerStdio`.

---

## 2️ Agents System

Agents use the **OpenAI Agents Framework**:

Each Agent has:
- instructions  
- model  
- tools (MCP tools or python functions)  
- a scratchpad and long-running reasoning  

Agents include:

###  Researcher Agent
- Performs web research  
- Summarizes insights  
- Stores memory in libsql  
- Provides a general-purpose research tool  

###  Trader Agents (Warren, George, Ray, Cathie)
Each trader has:
- A unique personality-driven strategy  
- Access to research tools  
- Access to market data MCP server  
- Access to accounts server (buy/sell)  

They autonomously:
- analyze market conditions  
- research stocks  
- execute trades  
- track P/L  

---

## 3️ Accounts System

In **accounts.py**, each account stores:

- balance  
- holdings  
- list of transactions  
- portfolio value history  
- profit/loss calculation  

Includes key functions:
- `buy_shares()`  
- `sell_shares()`  
- `calculate_portfolio_value()`  
- `change_strategy()`  
- `report()`  

---

## 4️ Market Data System

In `market.py`:

- If Polygon API key exists → fetch real EOD or real-time data  
- Else → fallback to random prices  

Caching avoids rate limits.

---

## 5️ Gradio Dashboard (Trading UI)

The dashboard displays:

- Portfolio values  
- Time-series charts  
- Holdings table  
- Recent trades  
- Trace logs  

Placeholder for screenshots:

[INSERT SCREENSHOT 1 HERE — Dashboard Overview]

[INSERT SCREENSHOT 2 HERE — Trader Portfolio View]

#  File Structure

```
MCP/
├── 1.ipynb               # Intro: basic MCP usage with files, playwright, fetch  
├── 2.ipynb               # Custom MCP server + account MCP client  
├── 3.ipynb               # Memory server, Brave search, Polygon API tests  
├── 4.ipynb               # Trader + Researcher orchestration  
├── 5.ipynb               # Multi-trader system setup  
│
├── accounts.py           # Account model + buy/sell logic  
├── accounts_server.py    # MCP server exposing account functions  
├── accounts_client.py    # MCP client wrapper  
│
├── market.py             # Price fetching + caching  
├── market_server.py      # MCP server exposing market price lookup  
│
├── traders.py            # Trader logic: decision-making & execution  
├── trading_floor.py      # Runs all traders every N minutes  
│
├── templates.py          # Dynamic prompt instructions for agents  
├── tracers.py            # Logging + trace tracking  
│
├── push_server.py        # Push notification server  
├── reset.py              # Reset trader portfolios & strategies  
│
├── app.py                # Gradio trading dashboard UI  
├── util.py               # CSS, JS & helpers  
│
└── memory/               # Persistent memory DBs for traders  
```

---

#  How the Autonomous System Works

##  1. The Scheduler (trading_floor.py)
Runs every N minutes (default 60):

- Checks market open/closed  
- Runs each trader agent  
- Alternates between trade / rebalance cycles  
- Logs activity  

---

##  2. When a Trader Runs

The trader:

1. Reads its account & strategy  
2. Performs research using the Researcher tool  
3. Gets stock data  
4. Makes decisions  
5. Executes trades  
6. Saves updated state  
7. Sends push notification  
8. Updates dashboard data  
9. Logs everything  

---

#  Gradio UI Explained

The UI (app.py) displays:

###  Trader Name & Strategy  
###  Real-time Portfolio Value  
###  Auto-updating Line Chart  
###  Holdings Table  
###  Recent Transactions  
###  Activity / Trace Logs  

The UI refreshes automatically using **Gradio timers**.

---

#  Notebooks Included

### `1.ipynb`  
Intro to MCP servers: Fetch, Playwright, Filesystem, Memory.

### `2.ipynb`  
Creating your own MCP server + client (Accounts MCP Server).

### `3.ipynb`  
Combining multiple MCP servers + Polygon financial MCP server.

### `4.ipynb`  
Single-trader autonomous trading example.

### `5.ipynb`  
Full multi-agent trading simulation (4 traders + researcher).

---

#  Requirements

- Python 3.10+  
- Node.js + NVM  
- uv package manager (for Python dependencies)  
- Polygon.io API key (optional but recommended)  
- Brave Search API key  

---


```

# Gradio Dashboard

Shows real-time:

Portfolio Value & PnL

Holdings table

Transactions table

Logs (highlighted by event type)

Time-series portfolio chart

---


# Placeholders for Screenshots



Screenshot 1 — Dashboard Overview
![Dashboard Overview](path/to/screenshot1.png)

Screenshot 2 — Trader Details View
![Trader Details](path/to/screenshot2.png)



---

# Future Improvements

Add LLM-based reinforcement learning

Add more diversified financial instruments

Add caching layer for market snapshots

Deploy as cloud-based trading simulation

---

---

#  Summary


- Real MCP server integration  
- Multi-agent autonomous reasoning  
- Financial decision-making with memory  
- Asset management using real API data  
- Interactive trading dashboards  
- Production-grade modular architecture  

---

#  Conclusion

This project is a complete demonstration of:

- Modern multi-agent systems  
- MCP protocol in real-world workflows  
- Financial data pipelines  
- Autonomous decision-making  
- Modular server-based AI architecture  

