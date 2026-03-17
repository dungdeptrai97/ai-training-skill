# Earnings Preview

description: Build pre-earnings analysis with estimate models, scenario frameworks, and key metrics to watch. Use before a company reports quarterly earnings to prepare positioning notes, set up bull/bear scenarios, and identify what will move the stock. Triggers on "earnings preview", "what to watch for [company] earnings", "pre-earnings", "earnings setup", or "preview Q[X] for [company]".

## Workflow

### Step 0: Real-Time Commodity Price Check (if applicable)

**🚨 MANDATORY when the company's earnings are materially affected by commodity prices** (input costs or selling prices — e.g., oil, gas, steel, coal, rubber, sugar, milk powder, fertilizer, rice, etc.)

**Before building scenarios, check today's actual commodity prices:**

1. **Priority 1 — MCP Servers (Simplize)**: Query Simplize MCP data connectors for real-time or latest available commodity prices. Use LSEG, FactSet, or S&P Global endpoints if configured.
2. **Priority 2 — WebSearch**: If MCP data unavailable, search: "[commodity name] price today [date]" (e.g., "Brent crude oil price today 2026-03-16", "HRC steel price Vietnam March 2026")
3. **Record & Compare**: Note the current price vs. the assumption used in consensus estimates. Flag if there is a significant divergence (>5%).

**What to check by industry:**
- **Oil & Gas** (GAS, PVD, PVS, PVT, BSR): Brent crude, WTI, Henry Hub gas, Singapore crack spread
- **Steel** (HPG, NKG, HSG): HRC price, iron ore, coking coal
- **Agriculture/Food** (VNM, DBC, HAG): Milk powder (GDT), corn, soybean meal, live hog price
- **Rubber** (CSM, DRC, SRC): Natural rubber (TSR20, RSS3)
- **Fertilizer** (DPM, DCM): Urea price, ammonia
- **Power/Utilities** (NT2, POW): Coal price (Newcastle), gas price

**Use this data to calibrate scenario assumptions** — e.g., if Brent is currently $62 but consensus assumes $75, the bear case becomes more likely.

### Step 1: Gather Context

- Identify the company and reporting quarter
- Pull consensus estimates via web search (revenue, EPS, key segment metrics)
- Find the earnings date and time (pre-market vs. after-hours)
- Review the company's prior quarter earnings call for any guidance or commentary

### Step 2: Key Metrics Framework

Build a "what to watch" framework specific to the company:

**Financial Metrics:**
- Revenue vs. consensus (total and by segment)
- EPS vs. consensus
- Margins (gross, operating, net) — expanding or contracting?
- Free cash flow
- Forward guidance vs. consensus

**Operational Metrics** (sector-specific):
- Tech/SaaS: ARR, net retention, RPO, customer count
- Retail: Same-store sales, traffic, basket size
- Industrials: Backlog, book-to-bill, price vs. volume
- Financials: NIM, credit quality, loan growth, fee income
- Healthcare: Scripts, patient volumes, pipeline updates

### Step 3: Scenario Analysis

Build 3 scenarios with stock price implications:

| Scenario | Revenue | EPS | Key Driver | Stock Reaction |
|----------|---------|-----|------------|----------------|
| Bull | | | | |
| Base | | | | |
| Bear | | | | |

For each scenario:
- What would need to happen operationally
- What management commentary would signal this
- Historical context — how has the stock moved on similar prints?

**REQUIRED: Probability-Weighted Assessment**
Based on all gathered data (recent industry trends, management track record, macro conditions, channel checks), assign probability to each scenario (e.g., Bull 25% / Base 55% / Bear 20%) and state which scenario is MOST LIKELY with 2-3 supporting reasons from current evidence.

### Step 4: Catalyst Checklist

Identify the 3-5 things that will determine the stock's reaction:

1. [Metric] vs. [consensus/whisper number] — why it matters
2. [Guidance item] — what the buy-side expects to hear
3. [Narrative shift] — any strategic changes, M&A, restructuring

### Step 5: Output

One-page earnings preview with:
- Company, quarter, earnings date
- Consensus estimates table
- Key metrics to watch (ranked by importance)
- Bull/base/bear scenario table
- Catalyst checklist
- **Our call**: Most likely scenario with probability weights and key evidence supporting the prediction
- Trading setup: recent stock performance, implied move from options

## Important Notes

- Consensus estimates change — always note the source and date of estimates
- "Whisper numbers" from buy-side surveys are often more relevant than published consensus
- Historical earnings reactions help calibrate expectations (search for "[company] earnings reaction history")
- Options-implied move tells you what the market expects — compare to your scenarios
