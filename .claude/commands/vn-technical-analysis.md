---
description: "Phân tích kỹ thuật cổ phiếu / VNINDEX theo 3 khung thời gian — cho ra khuyến nghị hành động cụ thể với tỷ lệ Reward/Risk. Dùng khi user hỏi 'PTKT', 'phân tích kỹ thuật', 'technical analysis', 'vùng giá mua', 'nên mua/bán chưa', 'xu hướng', 'điểm entry'"
argument-hint: "[mã CK hoặc VNINDEX]"
---

# VN Technical Analysis — Phân tích Kỹ thuật Đa Khung Thời Gian

Skill phân tích kỹ thuật chuyên sâu, áp dụng cho **cả VNINDEX lẫn cổ phiếu cụ thể**. Kết quả cuối cùng PHẢI cho ra **hành động cụ thể** (MUA / BÁN / GIỮ / CHỜ) kèm **tỷ lệ Reward/Risk**.

---

## NGUYÊN TẮC CỐT LÕI

### 1. Xu hướng dài hạn là BỐI CẢNH, ngắn hạn là THỜI ĐIỂM
- **KHÔNG BAO GIỜ** đưa tín hiệu mua ngắn hạn khi xu hướng dài hạn đang giảm mạnh (downtrend rõ ràng) mà không có cảnh báo.
- Ngắn hạn thuận chiều dài hạn → tín hiệu MẠNH
- Ngắn hạn ngược chiều dài hạn → tín hiệu YẾU, cần caution

### 2. Reward/Risk là tiêu chí quyết định
- Chỉ khuyến nghị giải ngân khi R:R ≥ 2:1
- Vùng có R:R ≥ 3:1 → ưu tiên cao
- R:R < 1.5:1 → KHÔNG khuyến nghị entry, dù tín hiệu tốt

### 3. Xác suất = Đa biến xác nhận
- Tín hiệu kỹ thuật đơn lẻ → xác suất ~50-55%
- 3+ biến xác nhận đồng thuận → xác suất tăng lên 65-75%
- Luôn đếm số biến xác nhận và ghi rõ trong output

---

## WORKFLOW

### Phase 0: Xác định đối tượng & Thu thập dữ liệu

**Input**: Mã CK (VD: HPG, FPT, VNM) hoặc VNINDEX

**Thu thập dữ liệu** (ưu tiên MCP Simplize → WebSearch):
1. **Giá & khối lượng**: OHLCV daily ít nhất 1 năm (lý tưởng 2-3 năm)
2. **Thông tin cơ bản**: Ngành, vốn hóa, free-float, % sở hữu nước ngoài
3. **Sự kiện sắp tới**: KQKD, cổ tức, phát hành, ĐHCĐ (nếu có trong 30 ngày tới)

> Nếu là VNINDEX: thêm dữ liệu breadth (số mã tăng/giảm), dòng tiền khối ngoại, Put/Call ratio (nếu có)

---

### Phase 1: Phân tích Đa Khung Thời Gian (Multi-Timeframe Analysis)

Phân tích TUẦN TỰ từ dài hạn → ngắn hạn. Xu hướng dài hạn là BỐI CẢNH cho ngắn hạn.

---

#### 1A. KHUNG DÀI HẠN (6-12 tháng | Weekly chart)

**Mục tiêu**: Xác định xu hướng chính (Primary Trend) — đây là BỐI CẢNH cho mọi quyết định.

**Chỉ báo bắt buộc:**

| Chỉ báo | Cách đọc | Ý nghĩa |
|---------|----------|---------|
| **MA50W & MA200W** (tuần) | Giá trên/dưới MA, MA50 cắt MA200? | Golden/Death Cross dài hạn |
| **Cấu trúc đỉnh/đáy** | Higher High + Higher Low? | Uptrend / Downtrend / Sideway |
| **Fibonacci Retracement** | Từ đáy → đỉnh major gần nhất | Vùng hỗ trợ/kháng cự chiến lược |
| **Volume Profile** (nếu có) | Vùng giá có volume lớn nhất | Vùng cân bằng (Value Area) |
| **ADX (14) weekly** | >25 = trending, <20 = sideway | Sức mạnh xu hướng |

**Output 1A — Kết luận xu hướng dài hạn:**
```
XU HƯỚNG DÀI HẠN: [UPTREND / DOWNTREND / SIDEWAY / CHUYỂN GIAO]
Cấu trúc: [Mô tả đỉnh đáy]
MA200W: [Giá hiện tại vs MA200W — trên/dưới bao nhiêu %]
ADX weekly: [Giá trị] → [Trending mạnh / Trending yếu / Không xu hướng]
Vùng hỗ trợ chiến lược: [Giá 1] — [Giá 2]
Vùng kháng cự chiến lược: [Giá 1] — [Giá 2]
```

---

#### 1B. KHUNG TRUNG HẠN (1-3 tháng | Daily chart)

**Mục tiêu**: Xác định xu hướng trung hạn và vùng giá hành động.

**Chỉ báo bắt buộc:**

| Chỉ báo | Cách đọc | Ý nghĩa |
|---------|----------|---------|
| **EMA20 & EMA50** (ngày) | Vị trí giá, crossover | Xu hướng trung hạn |
| **MACD (12,26,9)** | Signal line cross, histogram | Momentum trung hạn |
| **RSI (14)** | >70 overbought, <30 oversold | Quá mua / quá bán |
| **Bollinger Bands (20,2)** | Giá ở band nào, bandwidth | Biến động & vùng giá cực trị |
| **Volume trung bình 20 ngày** | So sánh volume hiện tại vs TB | Xác nhận xu hướng |
| **Ichimoku Cloud** (nếu áp dụng) | Giá vs cloud, Tenkan/Kijun | Xu hướng + hỗ trợ/kháng cự động |

**Output 1B — Kết luận xu hướng trung hạn:**
```
XU HƯỚNG TRUNG HẠN: [TĂNG / GIẢM / TÍCH LŨY / PHÂN PHỐI]
⚡ Tương quan với dài hạn: [THUẬN CHIỀU ✅ / NGƯỢC CHIỀU ⚠️]
MACD: [Trạng thái + histogram tăng/giảm]
RSI (14): [Giá trị] — [Trạng thái]
Volume: [Nhận xét xu hướng volume]
Hỗ trợ gần: [Giá] | Kháng cự gần: [Giá]
```

---

#### 1C. KHUNG NGẮN HẠN (1-2 tuần | Daily + 4H nếu có)

**Mục tiêu**: Xác định thời điểm hành động (timing).

**Chỉ báo bắt buộc:**

| Chỉ báo | Cách đọc | Ý nghĩa |
|---------|----------|---------|
| **EMA5 & EMA10** | Crossover ngắn hạn | Timing signal |
| **RSI (7)** ngắn hạn | Nhạy hơn RSI 14 | Tín hiệu quá mua/bán nhanh |
| **Stochastic (14,3,3)** | %K cắt %D, vùng 20/80 | Timing reversal |
| **Volume spike** | Volume > 1.5x trung bình | Xác nhận breakout/breakdown |
| **Nến đảo chiều** | Hammer, Engulfing, Doji Star... | Tín hiệu đảo chiều |
| **VWAP** (nếu có intraday) | Giá trên/dưới VWAP | Fair value trong ngày |

**Output 1C — Kết luận ngắn hạn:**
```
TÍN HIỆU NGẮN HẠN: [MUA / BÁN / TRUNG LẬP / CHỜ XÁC NHẬN]
⚡ Tương quan trung hạn: [THUẬN CHIỀU ✅ / NGƯỢC CHIỀU ⚠️]
⚡ Tương quan dài hạn: [THUẬN CHIỀU ✅ / NGƯỢC CHIỀU ⚠️]
Pattern nến: [Tên pattern nếu có]
Stochastic: [Giá trị + trạng thái]
Volume: [Nhận xét]
```

---

### Phase 2: Biến Bổ Sung Tăng Xác Suất (Confluence Factors)

> Dựa trên backtest thống kê, các biến sau khi xuất hiện đồng thời giúp tăng xác suất đúng từ ~55% lên 65-75%.

**Đếm số biến xác nhận và ghi rõ điểm confluence:**

| # | Biến bổ sung | Điều kiện BULLISH (+1) | Điều kiện BEARISH (+1) |
|---|-------------|----------------------|----------------------|
| 1 | **Xu hướng ngành (sector)** | Ngành đang outperform VNINDEX | Ngành underperform |
| 2 | **Dòng tiền khối ngoại** | Mua ròng liên tục ≥5 phiên | Bán ròng liên tục ≥5 phiên |
| 3 | **Relative Strength vs VNINDEX** | RS line đang tăng (outperform) | RS line đang giảm |
| 4 | **Divergence (RSI/MACD)** | Bullish divergence (giá thấp hơn, RSI cao hơn) | Bearish divergence |
| 5 | **Volume-Price Confirmation** | Giá tăng + volume tăng | Giá tăng + volume giảm (phân kỳ) |
| 6 | **Support/Resistance Cluster** | Giá tại vùng hội tụ ≥2 hỗ trợ (MA + Fib + trendline) | Tại vùng hội tụ kháng cự |
| 7 | **Catalyst sắp tới** | KQKD tốt, cổ tức, sự kiện tích cực trong 30 ngày | Phát hành, KQKD xấu |
| 8 | **Thanh khoản & Spread** | Thanh khoản đủ (>50K CP/phiên), spread hẹp | Thanh khoản thấp, khó thoát hàng |
| 9 | **Tâm lý thị trường chung** | VNINDEX uptrend, sentiment tích cực | VNINDEX downtrend, panic |
| 10 | **Mùa vụ (Seasonality)** | Tháng/quý lịch sử tích cực cho ngành/CP này | Mùa vụ bất lợi |

**Cho VNINDEX thêm:**

| # | Biến bổ sung | Điều kiện BULLISH | Điều kiện BEARISH |
|---|-------------|-------------------|-------------------|
| 11 | **Breadth** (số mã tăng/giảm) | Advance/Decline ratio > 1.5 | A/D < 0.7 |
| 12 | **% mã trên MA50** | >60% mã trên MA50 | <30% mã trên MA50 |
| 13 | **Margin toàn thị trường** | Margin giảm / ổn định | Margin đang ở đỉnh lịch sử |
| 14 | **Dòng tiền ETF ngoại** | Inflow VNM ETF, FTSE ETF | Outflow liên tục |

**Bảng xác suất tham khảo (từ backtest thị trường VN):**

| Số biến xác nhận | Xác suất ước tính | Đánh giá |
|-------------------|-------------------|----------|
| 1-2 biến | ~50-55% | ❌ KHÔNG đủ tin cậy để hành động |
| 3-4 biến | ~58-63% | ⚠️ Có thể hành động nếu R:R ≥ 3:1 |
| 5-6 biến | ~65-70% | ✅ Tín hiệu đáng tin, hành động được |
| 7+ biến | ~72-78% | 🔥 Tín hiệu mạnh, ưu tiên giải ngân |

---

### Phase 3: Xác Định Vùng Giá & Tỷ Lệ Reward/Risk

**Đây là phần QUAN TRỌNG NHẤT — không có R:R thì không có khuyến nghị.**

#### 3A. Xác định 3 vùng giá:

```
📍 VÙNG ENTRY (giá vào):
   - Entry lý tưởng: [Giá] (tại hỗ trợ mạnh / pullback về EMA / test lại breakout)
   - Entry chấp nhận: [Giá] (nếu giá không về đúng vùng lý tưởng)
   - Lý do: [Tại sao vùng này? — hội tụ hỗ trợ nào?]

🛑 VÙNG CẮT LỖ (Stoploss):
   - Stoploss: [Giá] (dưới hỗ trợ mạnh nhất / dưới đáy gần nhất / dưới MA quan trọng)
   - % rủi ro từ entry: [X%]
   - Lý do: [Tại sao cắt ở đây? — phá vùng này = sai thesis]

🎯 MỤC TIÊU LỢI NHUẬN (Target):
   - Target 1 (T1): [Giá] — chốt 1/3 vị thế — [Kháng cự gần nhất / Fib extension]
   - Target 2 (T2): [Giá] — chốt 1/3 vị thế — [Kháng cự tiếp theo]
   - Target 3 (T3): [Giá] — trailing stop phần còn lại — [Kháng cự chiến lược]
```

#### 3B. Tính Reward/Risk:

```
📊 REWARD / RISK RATIO:
   - Rủi ro (Entry → Stoploss): [X]đ = [Y%]
   - Lợi nhuận T1: [X]đ = [Y%] → R:R = [Z]:1
   - Lợi nhuận T2: [X]đ = [Y%] → R:R = [Z]:1
   - Lợi nhuận T3: [X]đ = [Y%] → R:R = [Z]:1

   ⚡ R:R trung bình (T1+T2+T3): [Z]:1
```

#### 3C. Quy tắc R:R → Hành động:

| R:R trung bình | Hành động |
|----------------|-----------|
| ≥ 3:1 | 🔥 **ƯU TIÊN CAO** — Entry tích cực, có thể giải ngân 70-100% kế hoạch |
| 2:1 — 3:1 | ✅ **CHẤP NHẬN** — Entry thận trọng, giải ngân 50-70% |
| 1.5:1 — 2:1 | ⚠️ **THẬN TRỌNG** — Chỉ entry nếu confluence ≥ 6 biến |
| < 1.5:1 | ❌ **KHÔNG ENTRY** — Chờ giá về vùng tốt hơn |

---

### Phase 4: Tổng Hợp & Khuyến Nghị Hành Động

#### 4A. Ma trận quyết định (Xu hướng × R:R × Confluence):

```
                    R:R ≥ 3:1          R:R 2-3:1         R:R < 2:1
                 ┌─────────────────┬────────────────┬──────────────────┐
DÀI HẠN TĂNG +  │ 🔥 MUA MẠNH    │ ✅ MUA         │ ⚠️ CHỜ pullback  │
TRUNG HẠN TĂNG  │ Full size       │ 50-70% size    │ Chờ R:R tốt hơn │
                 ├─────────────────┼────────────────┼──────────────────┤
DÀI HẠN TĂNG +  │ ✅ MUA thận     │ ⚠️ MUA nhỏ     │ ❌ KHÔNG MUA     │
TRUNG HẠN GIẢM  │ trọng (30-50%) │ (20-30% size)  │ Chờ trung hạn    │
                 ├─────────────────┼────────────────┼──────────────────┤
DÀI HẠN GIẢM +  │ ⚠️ Trading only │ ❌ KHÔNG MUA   │ ❌ KHÔNG MUA     │
TRUNG HẠN TĂNG  │ (scalp, nhỏ)   │ Counter-trend  │ Tuyệt đối không  │
                 ├─────────────────┼────────────────┼──────────────────┤
DÀI HẠN GIẢM +  │ ❌ KHÔNG MUA    │ ❌ KHÔNG MUA   │ ❌ KHÔNG MUA     │
TRUNG HẠN GIẢM  │ Chờ đảo chiều  │ Tuyệt đối không│ XÉT BÁN / CẮT LỖ│
                 └─────────────────┴────────────────┴──────────────────┘
```

#### 4B. Output — KHUYẾN NGHỊ HÀNH ĐỘNG (BẮT BUỘC):

```markdown
## 🎯 KHUYẾN NGHỊ HÀNH ĐỘNG: [MÃ CK]

**Hành động: [MUA / BÁN / GIỮ / CHỜ / CẮT LỖ / CHỐT LỜI / TRADING BÁN]**

### Tóm tắt 1 câu:
> [VD: "HPG đang trong uptrend dài hạn, trung hạn đang pullback về vùng hỗ trợ EMA50 + Fib 38.2% tạo cơ hội entry với R:R 3.2:1 — KHUYẾN NGHỊ MUA tại vùng 28,500-29,000 với stoploss 27,000."]

### Bảng tổng hợp:

| Tiêu chí | Kết quả |
|----------|---------|
| Xu hướng dài hạn | [TĂNG/GIẢM/SIDEWAY] |
| Xu hướng trung hạn | [TĂNG/GIẢM/TÍCH LŨY] |
| Tín hiệu ngắn hạn | [MUA/BÁN/TRUNG LẬP] |
| Đồng thuận 3 khung | [CẢ 3 THUẬN ✅ / 2/3 ✅ / XUNG ĐỘT ⚠️] |
| Confluence score | [X/10 biến] — [Đánh giá] |
| Entry | [Giá] |
| Stoploss | [Giá] (−[X%]) |
| Target 1 / T2 / T3 | [Giá T1] / [T2] / [T3] |
| R:R (trung bình) | [X]:1 |
| Đánh giá R:R | [🔥/✅/⚠️/❌] |
| **HÀNH ĐỘNG** | **[Cụ thể: Mua X% tại giá Y, stoploss Z]** |

### Kế hoạch hành động chi tiết:

**Nếu MUA:**
1. Giải ngân [X%] danh mục tại vùng [Entry]
2. Stoploss cứng tại [Giá] — cắt nếu close dưới giá này
3. Chốt lời 1/3 tại T1 [Giá], dời stoploss lên entry
4. Chốt lời 1/3 tại T2 [Giá], dời stoploss lên T1
5. Trailing stop phần còn lại khi qua T2

**Nếu GIỮ (đang có vị thế):**
1. Dời stoploss lên [Giá mới] (bảo vệ vốn/lợi nhuận)
2. Chốt bớt nếu giá đạt [Target tiếp theo]
3. Thêm vị thế nếu pullback về [Vùng giá] với volume

**Nếu BÁN / CẮT LỖ:**
1. Cắt lỗ [X%] tại giá [Giá] vì [lý do kỹ thuật]
2. Giá cần recover lên trên [Giá] để xem xét lại

**Nếu CHỜ:**
1. Trigger MUA nếu: [Điều kiện cụ thể — VD: close trên 30,000 với volume > 5M]
2. Trigger BÁN nếu: [Điều kiện cụ thể — VD: break dưới 27,000]
3. Kiên nhẫn chờ, KHÔNG FOMO vào giá hiện tại vì R:R chưa đủ
```

---

### Phase 5: Kịch Bản & Theo Dõi

```markdown
## Kịch bản theo dõi

### Kịch bản TÍCH CỰC (xác suất [X%]):
- Điều kiện: [VD: Giá break trên 32,000 + volume > 8M]
- Hành động tiếp: [VD: Thêm 20% vị thế, nâng T3 lên 38,000]

### Kịch bản CƠ SỞ (xác suất [X%]):
- Điều kiện: [VD: Giá dao động 28,500-31,500]
- Hành động tiếp: [VD: Giữ, chờ break]

### Kịch bản TIÊU CỰC (xác suất [X%]):
- Điều kiện: [VD: Giá close dưới 27,000]
- Hành động tiếp: [VD: Cắt lỗ 100%, chờ tái tích lũy]

### Mốc cần theo dõi:
- [ ] KQKD Q1/2026 (dự kiến tháng 4)
- [ ] Ngưỡng kỹ thuật: [Giá X] (breakout) / [Giá Y] (breakdown)
- [ ] Dòng tiền khối ngoại tuần tới
```

---

## GUARDRAILS — QUY TẮC BẮT BUỘC

### ❌ KHÔNG BAO GIỜ:
1. **Không đưa khuyến nghị mà KHÔNG có R:R** — thiếu R:R = thiếu stoploss = vô trách nhiệm
2. **Không dùng chỉ báo đơn lẻ** — "RSI quá bán nên mua" là SAI. Cần confluence
3. **Không phớt lờ xu hướng dài hạn** — tín hiệu mua ngắn hạn trong downtrend dài hạn phải có WARNING lớn
4. **Không nói "có thể lên hoặc xuống"** — luôn cho bias rõ ràng + điều kiện trigger
5. **Không copy/paste công thức** — mỗi mã có context khác nhau, phân tích phải cụ thể

### ✅ LUÔN LUÔN:
1. **Kết luận bằng hành động cụ thể** — "MUA tại 28,500, SL 27,000, T1 31,500" KHÔNG phải "có thể cân nhắc"
2. **Ghi rõ số biến confluence** — "6/10 biến xác nhận BULLISH"
3. **Disclaimer kỹ thuật cuối bài**:

```
⚠️ Lưu ý: Phân tích kỹ thuật dựa trên xác suất thống kê, KHÔNG phải dự đoán chắc chắn.
Luôn tuân thủ stoploss và quản lý vốn. Không đầu tư quá 10-15% danh mục vào 1 mã.
Kết hợp với phân tích cơ bản để có góc nhìn toàn diện.
```

---

## VÍ DỤ MINH HỌA — ĐÚNG vs SAI

### ❌ SAI — Mơ hồ, không hành động được:
```
HPG đang có tín hiệu tích cực với RSI tăng và MACD cắt lên.
Nhà đầu tư có thể cân nhắc mua nếu thấy phù hợp.
Cần theo dõi thêm diễn biến thị trường.
```

### ✅ ĐÚNG — Cụ thể, hành động được:
```
## 🎯 KHUYẾN NGHỊ: HPG — MUA

> HPG uptrend dài hạn (MA200W hướng lên), trung hạn đang pullback về vùng
> hội tụ EMA50 + Fib 38.2% (28,800). Confluence 7/10 biến bullish.
> R:R = 3.2:1 → ƯU TIÊN GIẢI NGÂN.

| Tiêu chí | Kết quả |
|----------|---------|
| Xu hướng dài hạn | TĂNG ✅ (giá trên MA200W +18%) |
| Xu hướng trung hạn | PULLBACK trong uptrend |
| Tín hiệu ngắn hạn | MUA (Hammer + Stochastic oversold) |
| Đồng thuận 3 khung | 3/3 THUẬN ✅ |
| Confluence | 7/10 biến: ngành thép outperform, khối ngoại mua ròng 5 phiên, RS tăng, bullish divergence RSI, volume xác nhận, hỗ trợ cluster (EMA50+Fib+trendline), KQKD Q4 tốt |
| Entry | 28,500 — 29,000 |
| Stoploss | 27,000 (−5.3%) |
| T1 / T2 / T3 | 31,500 / 34,000 / 38,000 |
| R:R | 3.2:1 🔥 |
| **HÀNH ĐỘNG** | **Mua 50-70% kế hoạch tại 28,500-29,000. SL 27,000.** |

Kế hoạch:
1. Mua 50% tại 28,800 (giá hiện tại gần hỗ trợ)
2. Mua thêm 20% nếu test 28,500 (entry lý tưởng)
3. SL cứng: close dưới 27,000 → cắt 100%
4. Chốt 1/3 tại 31,500, dời SL lên 29,000
5. Chốt 1/3 tại 34,000, dời SL lên 31,500
6. Trailing stop 7% cho phần còn lại
```

### ❌ SAI — Mua ngắn hạn nhưng phớt lờ downtrend dài hạn:
```
RSI quá bán, Stochastic cắt lên → MUA ngay tại 15,200.
```

### ✅ ĐÚNG — Có cảnh báo bối cảnh dài hạn:
```
## 🎯 KHUYẾN NGHỊ: XYZ — CHỜ (KHÔNG MUA)

⚠️ CẢNH BÁO: Xu hướng dài hạn GIẢM (giá dưới MA200W −22%, cấu trúc Lower High + Lower Low).
Mặc dù RSI ngắn hạn quá bán và Stochastic cho tín hiệu mua, đây chỉ là BOUNCE KỸ THUẬT
trong downtrend — xác suất thành công thấp (~40%).

Hành động: KHÔNG MUA trung hạn. Nếu trading ngắn hạn (chấp nhận rủi ro cao):
- Entry: 15,200 | SL: 14,500 (−4.6%) | Target: 16,200 (+6.6%) | R:R = 1.4:1 ❌
→ R:R < 1.5:1 → KHÔNG ĐỦ HẤP DẪN ngay cả cho trading.
→ CHỜ giá về vùng 13,800-14,000 (hỗ trợ mạnh) để có R:R tốt hơn.
```

---

## ĐẶC BIỆT: KHI PHÂN TÍCH VNINDEX

Khi đối tượng là **VNINDEX**, bổ sung thêm:

1. **Breadth Analysis**: Số mã tăng/giảm, % mã trên MA50, % mã trên MA200
2. **Sector Rotation**: Nhóm ngành nào đang dẫn dắt / suy yếu
3. **Dòng tiền**:
   - Khối ngoại (mua/bán ròng xu hướng)
   - ETF flow (VNM ETF, FTSE Vietnam, Fubon)
   - Margin toàn thị trường
4. **Sentiment**: Put/Call ratio (nếu có), VIX VN (nếu có)
5. **Khuyến nghị VNINDEX** → gắn với hành động cho DANH MỤC:
   - VNINDEX tăng mạnh → tăng tỷ trọng cổ phiếu
   - VNINDEX sideway → giữ nguyên, trading ngắn hạn
   - VNINDEX giảm → giảm tỷ trọng, tăng tiền mặt, phòng thủ

---

## OUTPUT FORMAT

Kết quả trả về theo cấu trúc markdown, dùng `/govalue-format` để format nếu user yêu cầu phong cách Simplize.

**Thứ tự output:**
1. Phase 1A → 1B → 1C (3 khung thời gian)
2. Phase 2 (Bảng confluence)
3. Phase 3 (R:R)
4. **Phase 4 — KHUYẾN NGHỊ HÀNH ĐỘNG** (phần quan trọng nhất, in đậm)
5. Phase 5 (Kịch bản theo dõi)
6. Disclaimer

---

*Skill version: 1.0 | Created: 2026-03-17*
