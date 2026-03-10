---
name: idea-generation
description: >
  Sàng lọc và tìm kiếm cổ phiếu, cơ hội đầu tư trên thị trường chứng khoán Việt Nam (HOSE, HNX, UPCOM).
  Sử dụng skill này khi người dùng muốn: lọc cổ phiếu theo tiêu chí tài chính (P/E, P/B, ROE, EPS...),
  tìm cổ phiếu theo ngành/nhóm ngành, sàng lọc theo chỉ báo kỹ thuật (RSI, MACD, volume đột biến,
  breakout, relative strength, new high 52 tuần...),
  lọc cổ phiếu theo sự kiện (chia cổ tức, cổ phiếu thưởng, insider buying, đối tác chiến lược, quỹ mua vào),
  tìm cổ phiếu hưởng lợi từ tin tức vĩ mô hoặc giá hàng hóa tăng,
  lọc cổ phiếu loại trừ rủi ro (exclude ngành, exclude nợ cao, exclude thanh khoản thấp),
  hoặc bất kỳ yêu cầu nào liên quan đến "tìm", "lọc", "sàng lọc", "screener", "scanner" cổ phiếu,
  "cơ hội đầu tư", "tin tốt", "sự kiện", "catalyst", "breakout", "new high", "CANSLIM".
  Kích hoạt cả khi người dùng hỏi dạng: "cổ phiếu nào có P/E thấp", "tìm cổ phiếu ngành ngân hàng",
  "cổ phiếu nào tăng mạnh hôm nay", "lọc cổ phiếu vốn hóa lớn",
  "cổ phiếu nào đáng mua", "tìm cổ phiếu tốt", "gợi ý cổ phiếu",
  "cổ phiếu nào sắp chia cổ tức", "insider đang mua mã nào",
  "quỹ ngoại mua gì", "giá thép tăng thì mua gì", "đầu tư công tăng hưởng lợi gì",
  "cổ phiếu nào đang breakout", "mã nào mạnh hơn VN-Index",
  "cổ phiếu nào đang rẻ so với lịch sử", "lọc nhưng loại trừ ngành BĐS".
---

# VN Stock Screener v5 — Sàng lọc cổ phiếu & Cơ hội đầu tư Việt Nam

## Mục đích

Skill này giúp Claude sàng lọc cổ phiếu từ dữ liệu có sẵn của người dùng, áp dụng các bộ lọc đa chiều (cơ bản, kỹ thuật nâng cao, sự kiện, vĩ mô, loại trừ rủi ro) để tìm ra cổ phiếu và cơ hội đầu tư phù hợp.

## Dữ liệu đầu vào

**Ưu tiên lấy dữ liệu theo thứ tự sau:**

1. **Simplize MCP Server** (ưu tiên cao nhất): Sử dụng MCP server để lấy dữ liệu tài chính cổ phiếu (P/E, P/B, ROE, EPS, revenue, margins...), dữ liệu giá/volume, và báo cáo phân tích của CTCK. Chỉ fall back sang nguồn khác khi MCP không có dữ liệu cần thiết.
2. **File dữ liệu người dùng**: Nếu user cung cấp file (CSV, Excel) thì dùng kết hợp với MCP data.
3. **Web search**: Chỉ dùng khi MCP và file đều không có — ví dụ tin tức vĩ mô, sự kiện mới nhất.

---

## ⭐ Bước 0: Phân loại yêu cầu (BẮT BUỘC)

> Đây là bước ĐẦU TIÊN. Trước khi làm bất cứ gì, xác định yêu cầu thuộc nhóm nào.

### Nhóm A — Yêu cầu RÕ RÀNG → Chạy ngay
Có ≥1 tiêu chí số cụ thể: "P/E < 10", "RSI < 30", "ngân hàng ROE > 15%"

### Nhóm B — Yêu cầu NỬA RÕ → Dịch NLP, chạy luôn, giải thích kèm
Có ý định nhưng thiếu con số: "cổ phiếu tăng trưởng mạnh", "nợ thấp lợi nhuận cao", "cổ phiếu đang có tin tốt"

> Nhóm B: KHÔNG dừng lại hỏi xác nhận. Dịch → chạy luôn → giải thích logic dịch NGAY TRONG kết quả.

**Format output cho Nhóm B:**
```
💡 Cách tôi dịch yêu cầu:
"công nghệ tăng trưởng mạnh, chưa quá đắt, thanh khoản tốt"
→ Ngành CNTT/Viễn thông | EPS growth > 20% | P/E < 18 | Volume > 500K/phiên

📊 Kết quả: 5 mã (trong 32 mã ngành CNTT)
[bảng kết quả]

↩️ Muốn điều chỉnh tiêu chí? Ví dụ: nới P/E lên < 25, hoặc thêm ROE > 15%.
```

### Nhóm C — Yêu cầu MƠ HỒ → BẮT BUỘC hỏi lại
Không có tiêu chí: "cổ phiếu nào đáng mua?", "gợi ý cổ phiếu tốt"

**Cách hỏi lại:**
```
Bạn quan tâm nhất đến điều gì?
1. 💎 Giá trị — Cổ phiếu đang rẻ hơn giá trị thực
2. 🚀 Tăng trưởng — Doanh nghiệp đang mở rộng nhanh
3. 💰 Cổ tức — Thu nhập thụ động ổn định
4. ⚡ Momentum — Đang có đà tăng giá mạnh
5. 🔄 Phục hồi — Cổ phiếu tốt đang bị bán quá mức
6. 📰 Sự kiện — Có catalyst tích cực (insider mua, quỹ mua, đối tác chiến lược)
7. 🌍 Vĩ mô — Hưởng lợi từ xu hướng kinh tế/giá hàng hóa
8. 🎯 CANSLIM — Kết hợp tăng trưởng + kỹ thuật (phương pháp William O'Neil)
9. ⚖️ Hybrid — Kết hợp 50-50 giá trị + tăng trưởng + kỹ thuật mới breakout

Chọn số (1-9) hoặc mô tả thêm, tôi sẽ thiết kế bộ lọc phù hợp.
```

### Nhóm D — Yêu cầu SỰ KIỆN / VĨ MÔ → Xác định catalyst, map sang ngành, chạy
Đề cập sự kiện cụ thể hoặc xu hướng vĩ mô: "giá thép tăng", "đầu tư công", "insider mua", "chia cổ tức"

> Có thể KẾT HỢP với lọc fundamental/technical.

---

## Dịch ngôn ngữ tự nhiên → Bộ lọc

### Bảng mapping từ khóa → tiêu chí (CƠ BẢN + KỸ THUẬT)

| Từ khóa | Tiêu chí | Ngưỡng mặc định |
|---------|---------|-----------------|
| "tăng trưởng mạnh" | EPS growth YoY | > 20% |
| "tăng trưởng" | EPS growth YoY | > 10% |
| "rẻ", "undervalued" | P/E vs ngành | < 0.8x TB ngành |
| "chưa quá đắt", "hợp lý" | P/E | < 18 |
| "nợ thấp", "tài chính lành mạnh" | D/E | < 0.8 (phi TC) |
| "lợi nhuận cao" | ROE | > 15% |
| "thanh khoản tốt" | Volume TB 20p | > 500K cp |
| "blue-chip", "an toàn" | VN30 hoặc cap > 20K tỷ | — |
| "phòng thủ" | Beta | < 0.8 |
| "oversold", "quá bán" | RSI | < 30 |
| "uptrend", "đang tăng" | Giá > MA50 > MA200 | — |
| "cổ tức cao" | Dividend yield | > 5% |

### ⭐ [v5 MỚI] Bảng mapping từ khóa → KỸ THUẬT NÂNG CAO

| Từ khóa | Tiêu chí | Cách tính |
|---------|---------|-----------|
| "breakout", "phá vỡ", "vượt kháng cự" | Breakout detection | Giá vượt đỉnh N phiên + Volume > 2x TB (xem section Kỹ thuật nâng cao) |
| "tích lũy", "accumulation", "sideway sắp bùng" | Squeeze/Accumulation | Bollinger Bands thu hẹp + Volume giảm dần → chuẩn bị breakout |
| "new high", "đỉnh mới", "cao nhất" | New High 52 tuần | Giá hiện tại ≥ Max(giá 252 phiên) |
| "new low", "đáy mới" | New Low 52 tuần | Giá hiện tại ≤ Min(giá 252 phiên) |
| "mạnh hơn thị trường", "outperform", "relative strength" | RS vs VN-Index | RS ratio = %Δ giá CP / %Δ VN-Index (N phiên) > 1 |
| "đáy cao hơn", "higher low" | Higher lows pattern | Low gần nhất > Low trước đó |

### Bảng mapping từ khóa → SỰ KIỆN

| Từ khóa | Loại sự kiện | Dữ liệu cần |
|---------|-------------|-------------|
| "chia cổ tức", "trả cổ tức" | Cổ tức tiền mặt | Ngày GDKHQ, tỷ lệ, giá trị/cp |
| "cổ phiếu thưởng", "chia thưởng", "phát hành thêm cho CĐHH" | Cổ phiếu thưởng | Tỷ lệ, ngày chốt quyền |
| "insider mua", "lãnh đạo mua", "nội bộ mua" | Insider buying | Ai mua, SL, % vốn |
| "insider bán", "lãnh đạo bán" | Insider selling | Ai bán, SL, % vốn |
| "đối tác chiến lược", "hợp tác", "góp vốn", "nhà đầu tư chiến lược" | Strategic partnership | Đối tác, % sở hữu, loại GD |
| "quỹ mua", "quỹ ngoại mua", "fund mua", "dòng tiền quỹ" | Fund accumulation | Foreign flow, ETF flow, fund ownership |
| "KQKD tốt", "lợi nhuận vượt kỳ vọng", "kết quả kinh doanh đột phá" | Earnings beat | EPS actual vs estimate, % beat |
| "tin tốt", "catalyst", "sự kiện tích cực" | Multi-event scan | Scan tất cả loại trên |

### Bảng mapping từ khóa → VĨ MÔ / HÀNG HÓA

| Từ khóa | Ngành hưởng lợi TRỰC TIẾP | Ngành hưởng lợi GIÁN TIẾP | Ngành BẤT LỢI |
|---------|--------------------------|--------------------------|---------------|
| "giá thép tăng", "HRC tăng" | Thép: HPG, HSG, NKG, TLH, POM | Khai khoáng sắt | Xây dựng, ô tô (chi phí tăng) |
| "giá dầu tăng", "dầu thô lên" | Dầu khí: PVD, PVS, PLX, OIL, BSR | Dịch vụ dầu khí: PVS, PVC | Vận tải, hàng không: VJC, HVN |
| "giá gạo tăng" | Gạo/Nông sản: LTG, AGM, TAR | Phân bón: DPM, DCM | — |
| "giá phân bón tăng" | Phân bón: DPM, DCM, BFC | Hóa chất: CSV, DGC | Nông nghiệp (chi phí tăng) |
| "giá cao su tăng" | Cao su: PHR, DPR, TRC | Lốp xe: DRC, SRC, CSM | Sản xuất đồ nhựa |
| "giá đường tăng" | Mía đường: SBT, QNS, LSS | — | F&B dùng đường làm đầu vào |
| "lãi suất giảm" | BĐS, CK, NH (tín dụng tăng) | Tiêu dùng (vay rẻ) | — |
| "lãi suất tăng" | Ngân hàng (NIM ngắn hạn) | — | BĐS, CK, DN đòn bẩy cao |
| "đầu tư công tăng" | Xây dựng: CTD, HHV, C69, LCG, FCN | Thép, Xi măng, VLXD, Logistics | — |
| "FDI tăng" | KCN: KBC, SZC, IDC, NTC, SIP | Xây dựng, logistics | — |
| "xuất khẩu tăng" | Dệt may: TCM, MSH, STK; Thủy sản: VHC, MPC | Logistics: GMD, HAH | — |
| "tiêu dùng phục hồi" | Bán lẻ: MWG, FRT, DGW; F&B: VNM, SAB | PNJ, MSN | — |
| "du lịch phục hồi" | Hàng không: VJC, HVN, ACV | Du lịch, dịch vụ | — |

### ⭐ [v5 MỚI] Bảng mapping từ khóa → LOẠI TRỪ (Exclusion)

| Từ khóa | Hành động | Ví dụ |
|---------|----------|-------|
| "loại trừ", "bỏ qua", "không lấy", "trừ" + [ngành] | Exclude ngành | "loại trừ BĐS" → bỏ tất cả mã ngành BĐS |
| "loại trừ", "bỏ qua" + [tiêu chí] | Exclude theo tiêu chí | "loại nợ cao" → D/E > 2 bị loại |
| "nhưng không", "trừ mã" + [mã cụ thể] | Exclude mã cụ thể | "trừ HPG" → loại HPG khỏi kết quả |
| "an toàn", "rủi ro thấp" | Auto-exclude mặc định | Tự loại: D/E > 2, Vol < 100K, P/E < 0, mã cảnh báo |

---

## Bộ lọc hỗ trợ

### Bộ lọc cơ bản (cho ngành PHI TÀI CHÍNH)

| Tiêu chí | Mô tả | Ví dụ |
|-----------|--------|-------|
| P/E (TTM) | Giá / EPS trailing | P/E < 15 |
| P/B | Giá / Book value | P/B < 1.5 |
| ROE | LN / Vốn CSH | ROE > 15% |
| ROA | LN / Tổng TS | ROA > 5% |
| EPS | LN trên CP | EPS > 3000 |
| Revenue growth | Tăng DT YoY | > 10% |
| EPS growth | Tăng EPS YoY | > 15% |
| Net margin | Biên LN ròng | > 10% |
| D/E | Nợ / Vốn CSH | D/E < 1 |
| Div yield | Tỷ suất cổ tức | > 5% |
| Market cap | Vốn hóa | > 1000 tỷ |

**⚠️ P/E < 0 = doanh nghiệp lỗ → tự động loại khi lọc "P/E thấp" (trừ khi user yêu cầu khác)**

### ⭐ Bộ lọc NGÀNH ĐẶC THÙ

#### Ngân hàng
**KHÔNG DÙNG:** D/E, current ratio, quick ratio

| Chỉ số | Ngưỡng tốt | Thay thế cho |
|--------|------------|-------------|
| NIM | > 3.5% | (đặc thù ngành) |
| NPL ratio | < 2% | "nợ thấp" → dùng NPL thay D/E |
| CASA | > 25% | (đặc thù ngành) |
| CIR | < 40% | (hiệu quả chi phí) |
| CAR | > 10% | (an toàn vốn) |
| Credit growth | > 10% | "tăng trưởng" → dùng cái này |
| P/B, ROE | Dùng bình thường | — |

#### Bất động sản
- Chỉ số bổ sung: NAV, landbank, backlog
- D/E chấp nhận < 2 (cao hơn ngành khác vì bản chất vốn lớn)

#### Chứng khoán
- Chỉ số bổ sung: thị phần môi giới, margin lending, P&L tự doanh

**Nếu thiếu dữ liệu đặc thù** → ghi gọn 1 dòng:
"⚠️ Thiếu NIM/NPL → dùng ROE, P/B thay thế. Bổ sung dữ liệu NIM/NPL để lọc chính xác hơn."

---

### Bộ lọc kỹ thuật cơ bản

| Chỉ báo | Chi tiết |
|---------|---------|
| RSI(14) | >70: quá mua, <30: quá bán |
| MACD(12,26,9) | Bullish/Bearish cross |
| Golden/Death cross | MA50 cắt MA200 (xem code trong references/) |
| Volume đột biến | > 2x TB 20 phiên |
| Bollinger(20,2) | Chạm band trên/dưới |
| Giá vs MA | So với MA20, MA50, MA200 |

### ⭐ [v5 MỚI] Bộ lọc kỹ thuật NÂNG CAO

#### 1. Breakout Detection — Phát hiện phá vỡ vùng tích lũy

**Định nghĩa:** Giá vượt lên trên đỉnh cao nhất N phiên gần nhất VÀ volume xác nhận.

**Tiêu chí breakout hợp lệ:**
- Giá đóng cửa hôm nay > Max(High của N phiên trước), N = 20 hoặc 50
- Volume hôm nay > 2x Volume TB 20 phiên
- Giá đóng cửa > MA20

**⚠️ Phân biệt breakout thật vs giả:**
- Breakout MẠNH: giá vượt + volume > 3x + đóng cửa gần đỉnh ngày (> 80% range) → ✅
- Breakout YẾU: giá vượt nhẹ + volume < 1.5x + đóng cửa gần giữa → ⚠️ "Cần xác nhận thêm 1-2 phiên"
- Breakout GIẢ: giá vượt rồi quay đầu đóng dưới kháng cự → ❌ Không tính

```python
def detect_breakout(df, lookback=20, vol_multiplier=2.0):
    resistance = df['high'].rolling(lookback).max().shift(1)
    avg_vol = df['volume'].rolling(20).mean()

    breakout = (
        (df['close'] > resistance) &
        (df['volume'] > avg_vol * vol_multiplier) &
        (df['close'] > df['close'].rolling(20).mean())  # trên MA20
    )

    # Strength: strong vs weak
    day_range = df['high'] - df['low']
    close_position = (df['close'] - df['low']) / day_range  # 0-1
    strength = np.where(close_position > 0.8, 'MẠNH', 'YẾU')

    return breakout, strength
```

#### 2. Relative Strength (RS) — Sức mạnh tương đối so với VN-Index

**Định nghĩa:** Cổ phiếu tăng/giảm mạnh hơn VN-Index bao nhiêu trong N phiên.

```python
def relative_strength(stock_prices, vnindex_prices, period=60):
    stock_return = stock_prices.pct_change(period).iloc[-1]
    index_return = vnindex_prices.pct_change(period).iloc[-1]
    rs_ratio = (1 + stock_return) / (1 + index_return)
    # RS > 1: outperform | RS < 1: underperform
    return rs_ratio
```

**Cách hiển thị:**
| RS | Ý nghĩa | Hiển thị |
|----|---------|---------|
| > 1.3 | Mạnh hơn VN-Index ≥30% | "🟢 RS=1.45 — outperform mạnh" |
| 1.05-1.3 | Mạnh hơn nhẹ | "🟢 RS=1.15" |
| 0.95-1.05 | Ngang VN-Index | "⚪ RS=1.02" |
| < 0.95 | Yếu hơn VN-Index | "🔴 RS=0.78 — underperform" |

#### 3. New High / New Low 52 tuần

```python
def new_high_low(df, period=252):
    high_52w = df['high'].rolling(period).max()
    low_52w = df['low'].rolling(period).min()
    is_new_high = df['close'] >= high_52w
    is_new_low = df['close'] <= low_52w
    # Percentile: giá hiện tại nằm ở đâu trong range 52 tuần
    percentile_52w = (df['close'] - low_52w) / (high_52w - low_52w) * 100
    return is_new_high, is_new_low, percentile_52w
```

#### 4. Accumulation / Squeeze — Tích lũy trước bùng nổ

**Phát hiện vùng tích lũy (sideway + volume co lại + BB thu hẹp):**
- Bollinger Bandwidth < percentile 20 (BB thu hẹp nhất 20% thời gian)
- Volume giảm dần: Volume TB 5 phiên < Volume TB 20 phiên × 0.7
- Giá nằm trong range hẹp: (High - Low) / Close < 3% trong 10 phiên

**Ý nghĩa:** Vùng tích lũy thường báo hiệu biến động lớn sắp tới (breakout HOẶC breakdown). KHÔNG dự đoán hướng — cần chờ tín hiệu phá vỡ.

---

### ⭐ [v5 MỚI] So sánh với lịch sử chính mã đó (Historical Percentile)

> Thay vì chỉ so P/E với TB ngành (v4), v5 thêm so sánh với chính lịch sử 3-5 năm của mã đó.

**Khi nào áp dụng:**
- Khi user hỏi "đắt hay rẻ so với lịch sử", "P/E đang ở đâu", "đang rẻ so với chính nó"
- Hoặc TỰ ĐỘNG thêm cột Historical Percentile khi kết quả ≤ 10 mã (đủ chỗ hiển thị)

**Cách tính:**
```python
def historical_percentile(current_value, historical_series):
    """current_value nằm ở percentile nào trong series lịch sử"""
    return (historical_series < current_value).sum() / len(historical_series) * 100
```

**Cách hiển thị:**
| Percentile | Ý nghĩa | Hiển thị |
|-----------|---------|---------|
| 0-20% | Rất rẻ so với lịch sử | "📉 P/E ở vùng thấp nhất 5 năm (percentile 12%)" |
| 20-40% | Rẻ | "📉 Dưới TB lịch sử" |
| 40-60% | Bình thường | "⚪ Quanh TB lịch sử" |
| 60-80% | Đắt | "📈 Trên TB lịch sử" |
| 80-100% | Rất đắt so với lịch sử | "📈 P/E ở vùng cao nhất 5 năm (percentile 92%)" |

**Ví dụ output khi có Historical Percentile:**
```
| Mã  | P/E  | P/E pctl 5Y | ROE   | ROE pctl 5Y | Nhận xét |
|-----|------|-------------|-------|-------------|----------|
| HPG | 9.8  | 📉 18%      | 14.2% | ⚪ 45%      | P/E ở vùng rẻ nhất 5 năm |
| FPT | 22.1 | 📈 75%      | 28.5% | 📈 88%      | P/E hơi cao, nhưng ROE cũng cao kỷ lục |
```

---

### Bộ lọc sự kiện (Event-based)

#### Sự kiện doanh nghiệp (Corporate Events)

| Loại sự kiện | Dữ liệu cần | Cột hiển thị | Ý nghĩa đầu tư |
|-------------|-------------|-------------|----------------|
| Cổ tức tiền mặt | Ngày GDKHQ, tỷ lệ, giá trị/cp | Mã, tên, tỷ lệ, giá trị/cp, ngày GDKHQ, div yield | Thu nhập thụ động, tín hiệu tài chính lành mạnh |
| Cổ phiếu thưởng | Tỷ lệ, ngày chốt quyền | Mã, tên, tỷ lệ thưởng, ngày chốt, % dilution | Tăng thanh khoản, nhưng dilute EPS |
| Insider buying | Ai mua, chức vụ, SL mua | Mã, tên, người mua, SL, % vốn, ngày GD | Tín hiệu tích cực — ban lãnh đạo tin vào triển vọng |
| Insider selling | Ai bán, chức vụ, SL bán | Mã, tên, người bán, SL, % vốn, lý do | Cần xem xét lý do |
| Đối tác chiến lược | Đối tác, % sở hữu, loại GD | Mã, tên, đối tác, % sở hữu, mục đích | Nâng uy tín, chuyển giao công nghệ |
| Quỹ đầu tư mua ròng | Foreign flow, ETF flow | Mã, tên, giá trị mua ròng, % room còn | Smart money tích lũy |
| KQKD vượt kỳ vọng | EPS actual vs estimate | Mã, tên, EPS actual, % beat | Kéo giá ngắn hạn nếu beat > 15% |

#### ⚠️ Cảnh báo sự kiện

- **Cổ tức:** Giá điều chỉnh giảm bằng giá trị cổ tức vào ngày GDKHQ.
- **Cổ phiếu thưởng:** Dilute EPS. Cần tính EPS adjusted.
- **Insider buying:** Chỉ có ý nghĩa khi: CEO/Chủ tịch mua, SL > 0.1% vốn, bằng tiền túi (không phải ESOP).
- **Đối tác chiến lược:** Đánh giá: đối tác lớn/uy tín? Mục đích cụ thể? Lock-up? Giá phát hành vs thị giá?
- **Quỹ mua ròng:** Phân biệt ETF rebalance (bị động) vs active fund (chủ động).

---

### Bộ lọc vĩ mô / hàng hóa

#### Quy trình: Map catalyst → 3 cấp hưởng lợi → áp fundamental filter → hiển thị

#### ⭐ [v5 MỚI] Bước Price Check — Catalyst đã phản ánh vào giá chưa?

> **BẮT BUỘC** khi lọc theo Nhóm D (Macro/Event). SAU khi list ngành hưởng lợi, TRƯỚC khi kết luận, kiểm tra giá CP đã phản ứng chưa.

**Cách kiểm tra:**
1. Xác định thời điểm catalyst bắt đầu (ngày tin ra, hoặc ngày giá hàng hóa bắt đầu tăng)
2. So sánh giá CP hiện tại vs giá tại thời điểm catalyst
3. Hiển thị kết quả:

```
📊 Price Check — Catalyst đã phản ánh vào giá chưa?

| Mã  | Giá khi catalyst | Giá hiện tại | %Δ giá | VN-Index %Δ | Đánh giá |
|-----|-----------------|-------------|--------|-------------|----------|
| HPG | 22,000 (01/01)  | 26,800      | +21.8% | +8.5%       | ⚠️ Đã phản ánh phần lớn |
| NKG | 10,500 (01/01)  | 12,300      | +17.1% | +8.5%       | ⚠️ Đã phản ánh một phần |
| TLH | 8,200 (01/01)   | 8,500       | +3.7%  | +8.5%       | ✅ Chưa phản ánh (thậm chí underperform VN-Index) |
```

**Rule đánh giá:**
| %Δ giá CP vs %Δ VN-Index | Đánh giá |
|--------------------------|----------|
| CP tăng > 2x VN-Index | "⚠️ Đã phản ánh phần lớn — rủi ro mua đỉnh" |
| CP tăng 1-2x VN-Index | "⚠️ Đã phản ánh một phần — upside hạn chế" |
| CP tăng < VN-Index hoặc giảm | "✅ Chưa phản ánh — có thể còn cơ hội nếu catalyst đúng" |

**Nếu không có dữ liệu thời điểm catalyst** → ghi gọn:
"⚠️ Không xác định được thời điểm catalyst bắt đầu. Kiểm tra giá CP trong 1-3 tháng gần nhất để đánh giá mức phản ánh."

---

## ⭐ [v5 MỚI] Bộ lọc loại trừ (Exclusion Filter)

> Khi người dùng muốn "tìm nhưng tránh X", "loại trừ Y", "không lấy Z".

### Cơ chế hoạt động

Exclusion filter chạy TRƯỚC tất cả bộ lọc khác (sau Data Quality Rules):

```
Tổng mã → [Data Quality: loại cảnh báo/kiểm soát] → [Exclusion: loại theo yêu cầu] → [Bộ lọc chính] → Kết quả
```

### Các loại loại trừ

| Loại | Ví dụ | Hành động |
|------|-------|----------|
| Exclude ngành | "loại trừ BĐS và ngân hàng" | Loại tất cả mã thuộc ngành BĐS, Ngân hàng |
| Exclude tiêu chí | "loại nợ cao" → D/E > 2 | Loại mã vượt ngưỡng |
| Exclude mã cụ thể | "trừ HPG, HSG" | Loại mã cụ thể |
| Auto-exclude (khi user nói "an toàn") | — | Tự loại: D/E > 2 (phi TC), Vol < 100K, P/E < 0, mã cảnh báo, free float < 15% |

### Hiển thị trong funnel

```
📊 1,652 mã → [loại trừ: BĐS, NH] → 1,180 mã → [P/E < 12, ROE > 15%] → 15 mã ✅
   Loại trừ: 472 mã (Ngân hàng: 28, BĐS: 85, các ngành khác bị exclude: 359)
```

---

## Data Quality Rules

### Rule 1: Loại mã bất thường (cảnh báo/kiểm soát/hủy niêm yết)
### Rule 2: Flag outlier tài chính (P/E < 3, P/E > 100, ROE > 50%, ROE < -100%, D/E > 5, EPS growth > 200%)
### Rule 3: Flag thanh khoản (Vol < 50K, Vol < 10K, IPO < 6 tháng, Free float < 20%)

---

## ⭐ Quy trình xử lý (v5 update)

### Bước 1: Phân loại yêu cầu (A / B / C / D)

### Bước 2: Xác định loại lọc + exclusion

| Nhóm | Loại lọc | Quy trình |
|------|---------|-----------|
| A, B | Fundamental + Technical | Quy trình v3 |
| D — Sự kiện | Event-based | Section sự kiện |
| D — Vĩ mô | Macro/Commodity + **Price Check** [v5] | Section vĩ mô |
| D + A/B | Kết hợp | Event/macro TRƯỚC → fundamental/technical SAU |
| C | Mơ hồ | Hỏi lại với **9 lựa chọn** [v5] |
| **Bất kỳ + "loại trừ"** | **Exclusion chạy đầu tiên** [v5] | **Section Exclusion** |

### Bước 3: Xác định ngành + áp bộ chỉ số đặc thù

### Bước 4: Load dữ liệu, kiểm tra cột + Data Quality Rules + **Exclusion Filter** [v5]

**⚠️ Data Loading Priority:**
1. Gọi Simplize MCP Server để lấy dữ liệu tài chính, giá, volume
2. Nếu user cung cấp file → merge/cross-check với MCP data
3. Web search chỉ dùng cho tin tức, sự kiện, macro context

### Bước 5: Chạy lọc + Compact Funnel

### Bước 6: Trình bày kết quả với Smart Sort + Benchmark + Risk Flags + **Historical Percentile** [v5]

> **SMART SORT:**

| Loại yêu cầu | Sort theo | Hướng |
|---------------|-----------|-------|
| Lọc P/E thấp | P/E | ↑ tăng dần |
| Lọc cổ tức | Dividend yield | ↓ giảm dần |
| Lọc tăng trưởng | EPS growth | ↓ giảm dần |
| Lọc ROE cao | ROE | ↓ giảm dần |
| Lọc RSI quá bán | RSI | ↑ tăng dần |
| Lọc momentum | Composite (RSI + MACD + trend) | ↓ giảm dần |
| Multi-criteria | Composite rank trung bình | ↑ tăng dần |
| Lọc sự kiện | Ngày sự kiện (gần nhất trước) | ↑ tăng dần |
| Lọc vĩ mô | % doanh thu từ mảng hưởng lợi | ↓ giảm dần |
| Kết hợp event + fund | Số catalyst × chất lượng tài chính | ↓ giảm dần |
| **[v5] Breakout** | **Volume ratio (cao nhất trước)** | **↓ giảm dần** |
| **[v5] Relative Strength** | **RS ratio** | **↓ giảm dần** |
| **[v5] Hybrid** | **Composite: Value 33% + Growth 33% + Technical 34%** | **↓ giảm dần** |

> **BENCHMARK NGÀNH** + **[v5] HISTORICAL PERCENTILE** (nếu ≤ 10 mã)

### Bước 7: **[v5] Price Check** cho Nhóm D (Macro/Event)

> Sau khi list kết quả, TRƯỚC khi kết luận, chạy Price Check để kiểm tra catalyst đã phản ánh vào giá chưa.

### Bước 8: Xử lý kết quả đặc biệt (>30 / rỗng / thiếu dữ liệu)

### Bước 9: Gợi ý bước tiếp theo (cross-skill)

### Bước 10: Xuất kết quả

---

## Chiến lược sàng lọc có sẵn

| Chiến lược | Tiêu chí |
|------------|---------|
| 💎 Value | P/E < 12, P/B < 1.5, ROE > 12%, D/E < 1, Div > 3% |
| 🚀 Growth | EPS gr > 20%, Rev gr > 15%, ROE > 15% |
| ⚡ Momentum | RSI 50-70, Giá > MA50, Vol > 500K, MACD Bullish |
| 💰 Dividend | Div > 5%, Payout < 70%, LN ổn định 3Y |
| 🔄 Turnaround | P/B < 1, RSI < 35, LN quý gần nhất cải thiện |
| 📰 Event-driven | Insider mua ròng + ROE > 12% + Vol > 200K |
| 🌍 Macro-play | Ngành hưởng lợi từ catalyst + P/E < TB ngành + EPS growth > 0% |
| **🎯 CANSLIM** | **Xem chi tiết bên dưới** |
| **⚖️ Hybrid** | **Xem chi tiết bên dưới** |

### ⭐ [v5 MỚI] Chiến lược CANSLIM (William O'Neil)

7 tiêu chí CANSLIM, áp dụng cho thị trường VN:

| Chữ | Tiêu chí gốc | Áp dụng VN | Bộ lọc cụ thể |
|-----|-------------|------------|---------------|
| **C** | Current quarterly earnings | EPS quý gần nhất tăng mạnh YoY | EPS Q gần nhất growth > 20% YoY |
| **A** | Annual earnings growth | EPS hàng năm tăng đều | EPS growth 3Y CAGR > 15% |
| **N** | New product/management/price high | Sản phẩm mới, lãnh đạo mới, hoặc giá đỉnh mới | Giá ở vùng New High 52 tuần (percentile > 80%) HOẶC có sự kiện N (sản phẩm mới, CEO mới) |
| **S** | Supply and demand | Cung cầu cổ phiếu | Volume tăng khi giá tăng, Volume giảm khi giá giảm. Free float hợp lý (20-70%) |
| **L** | Leader or laggard | Cổ phiếu dẫn đầu ngành | RS vs VN-Index > 1.1 (outperform ít nhất 10%) |
| **I** | Institutional sponsorship | Tổ chức/quỹ đang mua | Có ≥1 quỹ lớn nắm giữ HOẶC quỹ mua ròng 30D > 0 |
| **M** | Market direction | Xu hướng thị trường chung | VN-Index > MA50 (thị trường uptrend) — NẾU VN-Index < MA50 thì CẢNH BÁO |

**Lọc tối giản (khi dữ liệu hạn chế):**
```
CANSLIM tối giản = EPS Q growth > 20% + EPS 3Y CAGR > 15% + Giá > MA50 + RS > 1.1 + Vol > 500K
```

**⚠️ CANSLIM yêu cầu M (Market direction) thuận lợi.** Nếu VN-Index đang downtrend (< MA50), ghi cảnh báo:
"⚠️ Thị trường chung đang downtrend (VN-Index < MA50). CANSLIM khuyến nghị KHÔNG mua mới khi M không thuận lợi. Kết quả dưới đây chỉ là watchlist cho khi thị trường phục hồi."

### ⭐ [v5 MỚI] Chiến lược Hybrid — Kết hợp 50-50 (Value + Growth + Breakout)

> Đây là chiến lược kết hợp linh hoạt: vừa chọn cổ phiếu có nền tảng tốt (Value + Growth), vừa chờ tín hiệu kỹ thuật xác nhận (Breakout) trước khi hành động.

**Triết lý:** "Mua doanh nghiệp tốt, ở giá hợp lý, tại thời điểm đúng."

**3 tầng lọc:**

```
Tầng 1 — VALUE (33%): Không đắt
├── P/E < TB ngành (hoặc P/E < 18)
├── P/B < 2.5
└── D/E < 1.5 (phi TC)

Tầng 2 — GROWTH (33%): Đang tăng trưởng
├── EPS growth YoY > 10%
├── Revenue growth YoY > 5%
└── ROE > 12%

Tầng 3 — TECHNICAL BREAKOUT (34%): Xác nhận kỹ thuật
├── Giá > MA50 (uptrend trung hạn)
├── RSI 45-70 (không quá mua, không quá bán)
└── MỘT trong các tín hiệu:
    ├── Breakout khỏi vùng tích lũy 20 phiên + Volume > 1.5x TB
    ├── HOẶC Golden cross (MA50 cắt lên MA200) trong 10 phiên gần nhất
    ├── HOẶC MACD vừa cắt lên Signal
    └── HOẶC Giá vượt MA200 lần đầu sau >30 phiên dưới MA200
```

**Composite Score cho Hybrid:**
```python
# Mỗi tầng cho điểm 0-100, sau đó tính trung bình có trọng số
value_score = rank_percentile(pe, ascending=True) * 0.5 + rank_percentile(pb, ascending=True) * 0.3 + rank_percentile(de, ascending=True) * 0.2
growth_score = rank_percentile(eps_growth, ascending=False) * 0.4 + rank_percentile(rev_growth, ascending=False) * 0.3 + rank_percentile(roe, ascending=False) * 0.3
tech_score = breakout_signal * 50 + (1 if rsi_in_range else 0) * 25 + (1 if above_ma50 else 0) * 25

hybrid_score = value_score * 0.33 + growth_score * 0.33 + tech_score * 0.34
```

**Ví dụ output Hybrid:**
```
⚖️ Hybrid Scan: Value + Growth + Technical Breakout

📊 1,652 mã → [Value filter] → 320 mã → [Growth filter] → 85 mã → [Technical breakout] → 6 mã ✅

| #  | Mã  | V-Score | G-Score | T-Score | Hybrid | P/E  | EPS Gr | Tín hiệu KT          |
|----|-----|---------|---------|---------|--------|------|--------|-----------------------|
| 1  | FPT | 62      | 88      | 85      | 78.4   | 16.2 | +25%   | Breakout 20D + Vol 2.3x |
| 2  | DGC | 85      | 78      | 72      | 78.2   | 8.5  | +32%   | MACD cắt lên Signal    |
| 3  | MWG | 75      | 65      | 90      | 76.5   | 12.8 | +12%   | Vượt MA200 lần đầu     |

💡 FPT nổi bật: tài chính vững + tăng trưởng mạnh + vừa breakout với volume cao.
⚠️ Đây là sàng lọc kết hợp, không phải khuyến nghị. Phân tích sâu trước khi quyết định.
```

---

## Ví dụ đầu vào / đầu ra hoàn chỉnh

### Ví dụ 1 — Breakout + Exclusion

**Input:** "Tìm cổ phiếu vừa breakout, volume đột biến, loại trừ ngành BĐS và cổ phiếu thanh khoản thấp"

**Output:**
```
💡 Dịch: "breakout" → Giá vượt đỉnh 20 phiên + Vol > 2x TB | "loại trừ BĐS" → Exclude ngành BĐS | "thanh khoản thấp" → Exclude Vol < 200K

📊 1,652 mã → [Exclude: BĐS, Vol < 200K] → 1,280 mã → [Breakout 20D + Vol > 2x] → 4 mã ✅

| #  | Mã  | Ngành   | Giá    | Breakout | Vol/TB Vol | RS 60D | P/E  | ROE   | Flag |
|----|-----|---------|--------|----------|-----------|--------|------|-------|------|
| 1  | FPT | CNTT    | 128,000| MẠNH     | 3.2x      | 🟢 1.32| 22.1 | 28.5% | ✅ |
| 2  | DGC | Hóa chất| 88,000 | MẠNH     | 2.8x      | 🟢 1.18| 8.5  | 25.8% | ✅ |
| 3  | GMD | Logistics| 52,500| YẾU      | 2.1x      | ⚪ 1.05| 15.2 | 18.5% | ✅ |
| 4  | ABC | Thép    | 15,200 | YẾU      | 2.0x      | 🔴 0.85| 2.1  | 52.3% | ⚡ P/E đột biến |

⚠️ Breakout YẾU (GMD, ABC): cần xác nhận thêm 1-2 phiên. Nếu giá quay lại dưới kháng cự → breakout giả.
💡 Phân tích sâu? "Phân tích FPT" hoặc "So sánh FPT và DGC".
```

### Ví dụ 2 — Macro + Price Check

**Input:** "Giá thép tăng 20% trong 2 tháng qua, cổ phiếu thép có còn cơ hội không?"

**Output:**
```
🌍 Catalyst: "Giá thép tăng 20% (2 tháng)"

📊 Chuỗi tác động:
Giá HRC tăng 20% → Biên LN sản xuất thép mở rộng → EPS cải thiện

🟢 HƯỞNG LỢI TRỰC TIẾP:
| Mã  | % DT thép | P/E  | EPS Gr | ROE   |
|-----|-----------|------|--------|-------|
| HPG | 85%       | 9.8  | +35%   | 14.2% |
| NKG | 95%       | 6.5  | +42%   | 10.8% |
| TLH | 80%       | 8.1  | +20%   | 9.5%  |

📊 Price Check — Catalyst đã phản ánh chưa? (so với 2 tháng trước)
| Mã  | Giá 2T trước | Giá nay  | %Δ CP  | %Δ VN-Index | Đánh giá |
|-----|-------------|----------|--------|-------------|----------|
| HPG | 22,000      | 26,800   | +21.8% | +6.2%       | ⚠️ Đã phản ánh phần lớn (+21.8% vs giá thép +20%) |
| NKG | 10,500      | 12,300   | +17.1% | +6.2%       | ⚠️ Đã phản ánh một phần |
| TLH | 8,200       | 8,500    | +3.7%  | +6.2%       | ✅ Chưa phản ánh (underperform cả VN-Index) |

💡 Kết luận:
• HPG, NKG: giá đã tăng tương đương mức tăng giá thép → upside hạn chế trừ khi giá thép tiếp tục tăng
• TLH: chưa phản ánh catalyst → có thể còn cơ hội, NHƯNG cần kiểm tra tại sao TLH underperform (có vấn đề riêng?)

⚠️ Rủi ro: Giá thép có thể đảo chiều. Theo dõi giá HRC Trung Quốc.
```

### Ví dụ 3 — Hybrid Strategy

**Input:** "Lọc theo chiến lược Hybrid"

**Output:**
```
⚖️ Hybrid: Value (33%) + Growth (33%) + Technical Breakout (34%)

📊 1,652 mã → [Value: P/E < 18, P/B < 2.5, D/E < 1.5] → 420 mã
            → [Growth: EPS gr > 10%, Rev gr > 5%, ROE > 12%] → 95 mã
            → [Technical: Giá > MA50, RSI 45-70, có tín hiệu breakout] → 5 mã ✅

| #  | Mã  | Hybrid | Value  | Growth | Tech   | P/E  | EPS Gr | Tín hiệu KT             | P/E pctl 5Y |
|----|-----|--------|--------|--------|--------|------|--------|--------------------------|-------------|
| 1  | FPT | 78.4   | 62     | 88     | 85     | 16.2 | +25%   | Breakout 20D, Vol 2.3x   | ⚪ 52%      |
| 2  | DGC | 78.2   | 85     | 78     | 72     | 8.5  | +32%   | MACD cắt lên Signal      | 📉 22%      |
| 3  | MWG | 76.5   | 75     | 65     | 90     | 12.8 | +12%   | Vượt MA200 lần đầu 35D   | 📉 30%      |
| 4  | HPG | 72.1   | 82     | 70     | 65     | 9.8  | +35%   | Golden cross 8 phiên trước| 📉 18%      |
| 5  | GMD | 68.8   | 55     | 72     | 80     | 15.2 | +18%   | Breakout, Vol 2.1x       | ⚪ 45%      |
|----|-----|--------|--------|--------|--------|------|--------|--------------------------|-------------|
| 📊 TB thị trường |  |        |        |        | 15.8 | +10%   |                          |             |

💡 Nổi bật:
• DGC: Hybrid 78.2 — Value rất cao (P/E chỉ 8.5, percentile 22% lịch sử = rẻ nhất 5 năm) + tăng trưởng 32%
• MWG: Vừa vượt MA200 lần đầu sau 35 phiên — tín hiệu đảo chiều trung hạn

⚠️ Hybrid = sàng lọc kết hợp, không phải khuyến nghị. Phân tích sâu trước khi quyết định.
💡 "Phân tích DGC" hoặc "So sánh FPT và DGC" để đi sâu hơn.
```

### Ví dụ 4 — CANSLIM

**Input:** "Lọc cổ phiếu theo CANSLIM"

**Output:**
```
🎯 CANSLIM Scan (William O'Neil)

📋 Kiểm tra Market Direction (M):
VN-Index: 1,285 — MA50: 1,250 — ✅ VN-Index > MA50 → thị trường uptrend → OK để mua

📊 1,652 mã → [C: EPS Q > 20% YoY] → 180 mã → [A: EPS 3Y CAGR > 15%] → 62 mã
            → [N: Percentile giá > 80%] → 28 mã → [L: RS > 1.1] → 12 mã
            → [S: Vol pattern tốt] → 7 mã ✅

| #  | Mã  | C (EPS Q) | A (3Y CAGR) | N (Price) | L (RS)  | I (Quỹ) | Score |
|----|-----|-----------|-------------|-----------|---------|----------|-------|
| 1  | FPT | +28% ✅   | 22% ✅      | Pctl 88% ✅| 🟢 1.32 | 5 quỹ ✅ | 7/7  |
| 2  | MWG | +35% ✅   | 18% ✅      | Pctl 82% ✅| 🟢 1.25 | 3 quỹ ✅ | 7/7  |
| 3  | DGC | +42% ✅   | 25% ✅      | Pctl 75% ⚠️| 🟢 1.18| 2 quỹ ✅ | 6/7  |

DGC: N (price percentile) = 75% — gần nhưng chưa đạt ngưỡng 80%. Nếu giá tăng thêm 5-8% sẽ đạt.

⚠️ CANSLIM là chiến lược mua cổ phiếu dẫn đầu khi thị trường uptrend.
   Khi M đổi chiều (VN-Index < MA50) → cân nhắc chốt lời/giảm tỷ trọng.
💡 "Phân tích FPT" để xem chi tiết fundamental.
```

---

## Lưu ý

- Ghi rõ nguồn + thời điểm dữ liệu
- KHÔNG khuyến nghị mua/bán — chỉ cung cấp dữ liệu khách quan và phân tích cơ hội
- P/E < 0 = lỗ → tự loại khi lọc "P/E thấp"
- So sánh xuyên ngành cần ghi chú: "P/E ngân hàng thường < P/E công nghệ"
- Kết quả sàng lọc là bước đầu, cần phân tích sâu hơn
- Sự kiện tích cực không đảm bảo giá tăng — luôn kết hợp phân tích tài chính
- Phân tích vĩ mô/hàng hóa có tính thời điểm — **[v5] luôn chạy Price Check**
- **[v5] Breakout có thể giả** — ghi rõ breakout MẠNH vs YẾU, khuyên chờ xác nhận nếu yếu
- **[v5] Historical percentile là context quan trọng** — P/E=10 có thể rẻ với mã này nhưng bình thường với mã khác
- **[v5] CANSLIM yêu cầu Market direction thuận lợi** — KHÔNG dùng khi VN-Index downtrend
- **Ưu tiên Simplize MCP Server** cho tất cả dữ liệu tài chính và báo cáo phân tích — web search chỉ là fallback cho tin tức/sự kiện
