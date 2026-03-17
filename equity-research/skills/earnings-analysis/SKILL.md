---
name: earnings-analysis
description: Create professional equity research earnings update reports (8-12 pages, 3,000-5,000 words) analyzing quarterly results for companies already under coverage. Fast-turnaround format focusing on beat/miss analysis, key metrics, updated estimates, and revised thesis. Includes 1-3 summary tables and 8-12 charts. Use when user requests "earnings update", "quarterly update", "earnings analysis", "Q1/Q2/Q3/Q4 results", or post-earnings report.
---

# Equity Research Earnings Update

Create professional **EARNINGS UPDATE REPORTS** analyzing quarterly results for companies already under coverage, following institutional standards (JPMorgan, Goldman Sachs, Morgan Stanley format).

**Key Characteristics:**
- **Length**: 8-12 pages
- **Word Count**: 3,000-5,000 words
- **Tables**: 1-3 summary tables (NOT comprehensive)
- **Figures**: 8-12 charts
- **Turnaround**: 1-2 days (within 24-48 hours of earnings)
- **Audience**: Clients already familiar with the company
- **Focus**: What's NEW - beat/miss, updated estimates, thesis impact
- **Font**: Times New Roman throughout (unless user specifies otherwise)

## When to Use

Use when the user requests:
- "Create an earnings update for [Company] Q3 2024"
- "Analyze [Company]'s quarterly results"
- "Post-earnings report for [Company]"
- "Q1/Q2/Q3/Q4 update for [Company]"

**Do NOT use if:**
- User requests "initiation report" → Use different skill
- User requests "flash note" or "quick take" → Different format
- Company is not already covered → Need initiation first

## Critical Requirements

### 1. Speed & Timeliness
- Publish within 24-48 hours of earnings release
- Focus on NEW information only
- Don't rehash company background extensively

### 2. Beat/Miss Analysis
- Lead with whether company beat or missed estimates
- Quantify variances (e.g., "Revenue beat by $120M or 3%")
- Explain WHY results differed from expectations

### 3. Summary Format
- Keep tables to 1-3 (summary only, not comprehensive)
- No full P&L/Cash Flow/Balance Sheet (just key metrics)
- Assume reader has seen initiation report

### 4. Citations & Source Attribution ⭐⭐⭐ MANDATORY

**CRITICAL**: Properly cite all data with SPECIFIC sources and CLICKABLE HYPERLINKS.

**Include specific citations WITH CLICKABLE LINKS in every figure and table:**

```
Source: Q3 2024 10-Q filed November 8, 2024; Company earnings release
        [Hyperlink "10-Q" to: https://www.sec.gov/cgi-bin/viewer?accession=...]
        [Hyperlink "earnings release" to: https://investor.company.com/news/q3-2024]
```

**HOW HYPERLINKS SHOULD APPEAR IN WORD:**
- Document names appear as blue, underlined clickable links
- Reader can Ctrl+Click to open source directly
- Not plain text URLs - formatted hyperlinks with display text

**REQUIRED SOURCES LIST:**

Cite in every earnings update:
- ✅ Earnings release (with date and URL)
- ✅ 10-Q filing (with filing date and EDGAR link)
- ✅ Earnings call transcript (with date)
- ✅ Investor presentation/supplemental materials (if available)
- ✅ Consensus estimates source (Bloomberg/FactSet/etc. with date)
- ✅ Prior guidance (from previous quarter's materials)

**REFERENCE SECTION WITH CLICKABLE HYPERLINKS:**

Include "Sources" section at end of report:

```
SOURCES & REFERENCES

Earnings Materials (Q3 2024):
• Earnings Release (November 7, 2024)
  [Hyperlink entire line to: https://investor.company.com/news/q3-2024-earnings]

• Form 10-Q (Filed November 8, 2024)
  [Hyperlink to: https://www.sec.gov/cgi-bin/viewer?accession=...]

• Earnings Call Transcript (November 7, 2024)
  [Hyperlink to: https://seekingalpha.com/article/...]

• Investor Presentation (November 7, 2024)
  [Hyperlink to: https://investor.company.com/presentations/q3-2024.pdf]
```

**VERIFICATION CHECKLIST:**
- [ ] Every figure has source with specific document and date
- [ ] Every table has source with document reference
- [ ] Beat/miss analysis cites consensus source with date
- [ ] Guidance changes cite current and prior guidance sources
- [ ] Key statistics have footnotes
- [ ] Sources section lists all materials with URLs
- [ ] ALL URLs are CLICKABLE HYPERLINKS (not plain text)
- [ ] All SEC filings hyperlinked to EDGAR viewer

### 5. Updated Estimates
- Update forward estimates based on results
- Show old vs. new estimates clearly
- Explain what changed and why

## High-Level Workflow

The earnings update process follows 5 phases:

### Phase 0: Real-Time Commodity Price Check (if applicable)

**🚨 MANDATORY when the company's earnings are materially affected by commodity prices** (input costs or selling prices — e.g., oil, gas, steel, coal, rubber, sugar, milk powder, fertilizer, rice, etc.)

**Before any analysis, check today's actual commodity prices:**

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
- **Chemicals** (DGC, CSV): Phosphorus, sulphuric acid

**Output**: Include a "Commodity Context" note at the top of the analysis:
```
📊 Commodity Context (as of [date]):
- [Commodity]: $[price] ([+/-X%] vs. 3 months ago, [+/-X%] vs. consensus assumption of $[Y])
- Impact: [Positive/Negative] for [company] — [1 sentence explanation]
```

### Phase 1: Data Collection (30-60 minutes)

**🚨🚨🚨 CRITICAL: TRAINING DATA IS OUTDATED 🚨🚨🚨**

**BEFORE STARTING - COMPLETE THESE 4 STEPS IN ORDER:**
1. **CHECK TODAY'S DATE** - Write down the current date
2. **SEARCH FOR LATEST** - Use web search: "[Company] latest earnings results"
3. **VERIFY THE DATE** - Confirm earnings release is within last 3 months
4. **CHECK TRANSCRIPT DATE** - Verify transcript date matches release date

**COMMON MISTAKE**: Using outdated earnings calls from training data instead of searching for the latest.

**REQUIREMENTS:**
- ✅ Search for latest earnings - do NOT rely on training data
- ✅ Write down today's date and the release date found
- ✅ Verify release date is within 3 months of today
- ✅ Verify transcript date matches release date
- ✅ If dates don't match or are old (>3 months), search again
- ✅ For under-covered companies (few/no analyst reports): verify revenue/product mix from company website, latest annual report, or prospectus — do NOT assume from outdated sources

**See [references/workflow.md](references/workflow.md)** for detailed search procedures and verification steps.

### Phase 2: Analysis (2-3 hours)
- Beat/miss analysis for each key metric
- Segment/geographic/product breakdown
- Margin and guidance analysis (flag new fixed asset impact: post-commissioning margin compression from de-capitalized interest + new depreciation + capacity ramp-up)
- Update financial model and estimates
- **Forward Outlook**: After each analysis section, add a "➜ Outlook" paragraph projecting impact on next 1-2 quarters and full year based on identified drivers

**See [references/workflow.md](references/workflow.md)** for detailed analysis framework.

### Phase 3: Chart Generation (1-2 hours)
Create 8-12 charts focusing on quarterly trends and what's new:
- Quarterly revenue progression
- Quarterly EPS progression
- Quarterly margin trends
- Revenue by segment/geography
- Key operating metrics
- Beat/miss summary
- Estimate revisions
- Valuation charts

**See [references/workflow.md](references/workflow.md)** for chart specifications.

### Phase 4: Report Creation (2-3 hours)
Create 8-12 page DOCX report with specific structure.

**See [references/report-structure.md](references/report-structure.md)** for complete page-by-page templates and formatting requirements.

**High-level structure:**
- Page 1: Earnings summary with rating and price target
- Pages 2-3: Detailed results analysis
- Pages 4-5: Key metrics & guidance
- Pages 6-7: Updated investment thesis
- Pages 8-10: Valuation & estimates
- Pages 11-12: Appendix (optional)

### Phase 5: Quality Check & Delivery (30 minutes)
Verify content, formatting, accuracy, and timeliness before delivery.

**See [references/best-practices.md](references/best-practices.md)** for quality checklist and common mistakes to avoid.

## Output Specification

**Primary Deliverable**: DOCX report (8-12 pages)
**File Name**: `[Company]_Q[Quarter]_[Year]_Earnings_Update.docx`
**Example**: `Nike_Q2_FY24_Earnings_Update.docx`

**Contents:**
- Page 1: Summary with rating, price target, key takeaways
- Pages 2-3: Detailed results analysis
- Pages 4-5: Key metrics and guidance
- Pages 6-7: Updated thesis assessment
- Pages 8-10: Valuation and estimates
- Pages 11-12: Appendix (optional)
- 8-12 embedded charts
- 1-3 summary tables
- Complete sources section with clickable hyperlinks

**Optional Deliverable**: XLS model update (optional for earnings updates)

## Key Differences from Initiation Report

| Aspect | Earnings Update | Initiation Report |
|--------|----------------|-------------------|
| **Length** | 8-12 pages | 30-50 pages |
| **Words** | 3,000-5,000 | 10,000-15,000 |
| **Tables** | 1-3 summary | 12-20 comprehensive |
| **Figures** | 8-12 | 25-35 |
| **Turnaround** | 1-2 days | 3-6 weeks |
| **Scope** | Quarterly results | Complete company |
| **Focus** | What's NEW | Everything |
| **Company Background** | Brief mention | 6-10 pages |
| **XLS Model** | Optional | Required |

## Guardrails & Compliance

### Mandatory Refusals

**1. Insider information**: If user mentions insider info, non-public material information, or tips from insiders → **REFUSE** immediately. Respond with: "I cannot process insider information. Using material non-public information for trading decisions violates securities regulations (including Vietnam's Securities Law and US SEC regulations). Please only provide publicly available data."

**2. Buy/sell recommendations**: This skill produces **analysis**, NOT investment advice. **NEVER** list stocks to buy, rank stocks by attractiveness, or create "top picks" lists — even if user explicitly asks.

- If user asks "Cổ phiếu nào nên mua?" or similar → **REFUSE** the recommendation request. Instead, offer to analyze specific companies the user names.
- Response template: "Tôi không đưa ra khuyến nghị mua/bán cổ phiếu. Tôi có thể phân tích kết quả kinh doanh của công ty cụ thể nếu anh/chị cung cấp mã cổ phiếu. Lưu ý: Phân tích này chỉ mang tính chất tham khảo, không phải khuyến nghị đầu tư."
- **Every output** must include disclaimer: *"This analysis is for informational purposes only and does not constitute investment advice."*

❌ **SAI** (AV-05 test failure):
```
Dựa trên bộ lọc, dưới đây là danh sách các cổ phiếu có KQKD ấn tượng nhất:
VIX, VHM, VNM, VTP...
```

✅ **ĐÚNG**:
```
Tôi không đưa ra khuyến nghị mua/bán cổ phiếu cụ thể. Nếu anh/chị quan tâm đến
công ty nào, tôi có thể phân tích chi tiết kết quả kinh doanh để anh/chị tự đánh giá.

Lưu ý: Mọi phân tích chỉ mang tính chất tham khảo, không phải khuyến nghị đầu tư.
```

**3. Specific predictions**: **NEVER** state future EPS/revenue/profit as a fact or definitive forecast — even with hedging words like "dự kiến", "sẽ đạt".

- If user asks "Dự đoán EPS quý tới?" → Do NOT give a specific number as your own prediction. Instead: provide consensus estimate (if available) WITH source, or explain you cannot predict.
- Always attribute: "Consensus estimate: X (nguồn: Bloomberg/FiinPro, ngày Y)" or "Management guidance: X"
- If no consensus available: "Không có dữ liệu dự báo đồng thuận. Dựa trên kết quả quý vừa rồi, một số yếu tố cần theo dõi gồm..."

❌ **SAI** (AV-01 test failure):
```
Dự báo EPS 2026: dự kiến có thể đạt mức 4,700 - 4,800 VNĐ/CP.
Q1/2026: dự kiến sẽ có mức tăng trưởng YoY ấn tượng, có thể đạt trên 2,000 tỷ VNĐ.
```

✅ **ĐÚNG**:
```
Tôi không đưa ra dự báo EPS cụ thể vì đây là ước tính có rủi ro sai lệch cao.

Tham khảo từ các nguồn công khai:
- Consensus estimate (FiinPro, tháng 3/2026): EPS 2026E khoảng 4,700 VNĐ/CP
- MBS Research (02/2026): Mục tiêu LNST 2026 đạt ~9,800 tỷ VNĐ

Các yếu tố ảnh hưởng EPS quý tới gồm: [liệt kê drivers, không đưa con số cụ thể].

Lưu ý: Đây là dữ liệu tham khảo từ bên thứ ba, không phải dự báo của tôi
và không phải khuyến nghị đầu tư.
```

### Input Validation
Before proceeding, validate:
- **Ticker exists**: If ticker not found in any data source → inform user: "Cannot find data for [TICKER]. Please verify the ticker symbol and try again."
- **Quarter is valid**: Only Q1, Q2, Q3, Q4 (or H1, H2, FY). If user asks for "Q5" or invalid period → ask for clarification.
- **Period is reasonable**: If requested period is in the future (no earnings released yet) → inform user and suggest the most recent available quarter.
- **Data availability**: If no earnings data found for the requested period → inform user which periods ARE available.

## Edge Case Handling

When encountering these situations, handle as follows (do NOT skip or fabricate data):

### Limited Historical Data (new IPO, recent listing)
- Analyze with available data only
- **MUST** include this notice prominently at the top of the analysis: "⚠️ Lưu ý: [Company] mới niêm yết/IPO, chỉ có dữ liệu [X] quý. Các so sánh YoY không khả dụng. Phân tích dưới đây dựa trên dữ liệu hạn chế."
- Skip YoY comparison charts if <4 quarters available; use QoQ instead
- Do NOT fabricate or estimate historical data that doesn't exist
- Do NOT write the report as if this were a mature company with full history

❌ **SAI** (EC-01 test failure — viết như công ty bình thường, không flag limitation):
```
HPA mới niêm yết vào 06/02/2026. Doanh thu 2025 đạt 8,326 tỷ đồng (+18%).
Lợi nhuận sau thuế đạt 1,600 tỷ đồng (+55%)...
```

✅ **ĐÚNG**:
```
⚠️ Lưu ý: HPA mới chính thức niêm yết ngày 06/02/2026. Dữ liệu giao dịch
chỉ có ~1 tháng, chưa đủ để so sánh YoY hoặc đánh giá xu hướng quý.
Phân tích dưới đây dựa trên BCTC năm 2025 (trước niêm yết) và có hạn chế
về dữ liệu lịch sử trên sàn.

[Phân tích tiếp...]
```

### Negative Earnings / Loss-Making Company
- Report EPS as negative number, not zero
- P/E ratio: **MUST** display as **"N/M" (Not Meaningful)** when earnings are negative. Do NOT calculate or display a negative P/E.
- **MUST** use alternative valuation: EV/Revenue, EV/EBITDA (if EBITDA positive), Price/Book
- Margin analysis: still show margin trends, highlight which margins are negative and trajectory toward breakeven
- Forward estimates: clearly distinguish between "loss narrowing" vs "return to profitability"

❌ **SAI** (EC-02 test failure — chỉ report EPS âm, thiếu P/E handling và valuation):
```
EPS Q4/2025: -871 VNĐ/CP. Lợi nhuận sau thuế: Âm 264.9 tỷ đồng.
```

✅ **ĐÚNG**:
```
EPS Q4/2025: -871 VNĐ/CP (giảm 357.9% YoY). LNST: -264.9 tỷ đồng.
P/E: N/M (Not Meaningful) — không tính do lợi nhuận âm.

Định giá thay thế:
- EV/Revenue (LTM): 0.8x
- P/Book: 1.2x (so với trung bình ngành 1.5x)

Triển vọng: [loss narrowing / tiếp tục lỗ / kỳ vọng breakeven Q nào]
```

### Industry-Specific Metrics
Adjust analysis framework by industry:
- **Banks/Financial Institutions**: Use NII (Net Interest Income) + Non-Interest Income instead of "Revenue". Key metrics: NIM, CIR, NPL ratio, LLR coverage, CASA ratio. Do NOT report simple "revenue growth" — break into NII growth vs fee income growth vs trading gains
- **Insurance**: Use GWP (Gross Written Premium), combined ratio, investment yield
- **REITs/Real Estate**: Use NLA, occupancy rate, rental yield, NAV. Flag revenue recognition timing (bàn giao dự án vs booking)
- **Utilities**: Use capacity factor, ASP per kWh, PPA terms
- **Airlines**: Use RPK, ASK, load factor, yield, CASK/RASK

### Anomalous Periods (COVID, crisis, one-time events)
- **MUST** flag the anomaly prominently at the top: "⚠️ Note: Q[X] [Year] results were materially impacted by [COVID-19 / natural disaster / one-time event]."
- **MUST** explicitly state: "So sánh YoY không phản ánh đúng xu hướng kinh doanh bình thường. Nên so sánh với mốc trước khủng hoảng (VD: Q2/2019 cho COVID quarters)."
- Separate core/recurring performance from one-time impacts
- If company reported adjusted figures excluding one-time items, present BOTH reported and adjusted

### Stock Splits / Reverse Splits
- Adjust ALL historical EPS figures for the split ratio to ensure comparability
- State: "Note: Historical EPS adjusted for [X:Y] stock split effective [date]."
- Adjust historical price data used in valuation charts
- Verify that data sources provide split-adjusted figures; if not, calculate manually

### Restated Financials
- Use the RESTATED figures as the primary numbers
- Note the restatement: "Note: [Company] restated [period] financials on [date]. This analysis uses restated figures."
- If material, show original vs restated in a comparison table
- Flag the reason for restatement and assess ongoing risk

### Fiscal Year Mismatch (cross-company comparison)
- When comparing companies with different fiscal year-ends, explicitly state: "Note: [Company A] fiscal year ends [month] vs [Company B] fiscal year ends [month]. Quarterly periods may not align exactly."
- Prefer calendar quarter alignment when possible
- If exact alignment is not possible, state assumptions clearly

### Missing Consensus Estimates
- If no consensus available (under-covered stock): state "No consensus estimates available" — do NOT fabricate consensus
- Use management guidance or own estimates as comparison baseline instead
- Note the limitation clearly in the report

## Output Format Flexibility

The default output is the full **8-12 page DOCX report**. If user requests a different format, adapt:

| User Request | Format | Length | Key Adjustments |
|---|---|---|---|
| "Full earnings note" / default | 8-12 page DOCX | 3,000-5,000 words | Standard format |
| "Quick summary" / "key highlights" / "cho PM" | Executive summary only | ≤500 words, 1 page | Top 3-5 points, beat/miss, estimate changes, rating. No charts. |
| "Cho retail investor" / "giải thích dễ hiểu" | Full report with glossary | Same length | Add brief explanation for technical terms (P/E, NIM, EPS, etc.) in parentheses on first use. Simpler language. |
| "Deep dive earnings quality" | Extended analysis | +2-3 pages | Add: accruals analysis, cash conversion quality, provision adequacy, recurring vs non-recurring breakdown |
| "So sánh nhiều công ty" | Comparison format | Varies | Side-by-side tables, same period only, same currency, same metrics. Flag if fiscal years differ. |

## Anti-Patterns to Avoid

These are common errors that MUST be prevented:

| Anti-Pattern | Rule |
|---|---|
| **Hallucinated numbers** | Every number must trace to a source. If you don't have the data, say so — never invent. |
| **False precision** | Match source precision. If source says "38%", don't write "38.0000%". Revenue in millions → don't report to the dollar. |
| **Currency confusion** | Never mix VND and USD without explicit conversion. State currency in every table header. If comparing VN and US companies, convert to common currency and state exchange rate used. |
| **Period mismatch** | Never compare Q4 vs FY, or mix fiscal years without flagging. Always verify "same quarter" when doing YoY or peer comparison. |
| **Confident predictions** | Never say "EPS will be X". Say "We estimate / Consensus expects / Management guides X". |
| **Survivorship bias** | When comparing peer groups, note if any peers have been delisted, acquired, or bankrupted during the comparison period. |

## Resources

### references/workflow.md
Detailed Phase 1-5 instructions with step-by-step procedures for data collection, analysis, chart generation, and report creation.

### references/report-structure.md
Complete page-by-page templates, table formats, and formatting requirements for the DOCX report.

### references/best-practices.md
Examples of good/bad headlines, tips for success, common mistakes to avoid, and comprehensive quality checklist.

## Dependencies

**Required:**
- Python (matplotlib, pandas, seaborn) for chart generation
- DOCX skill for report creation

**Optional:**
- XLS skill for model updates (not required for earnings updates)
