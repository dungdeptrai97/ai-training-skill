---
name: vn-stock-screener
description: >
  Sàng lọc và tìm kiếm cổ phiếu trên thị trường chứng khoán Việt Nam (HOSE, HNX, UPCOM).
  Sử dụng skill này khi người dùng muốn: lọc cổ phiếu theo tiêu chí tài chính (P/E, P/B, ROE, EPS...),
  tìm cổ phiếu theo ngành/nhóm ngành, sàng lọc theo chỉ báo kỹ thuật (RSI, MACD, volume đột biến...),
  tìm cổ phiếu breakout/breakdown, lọc theo thanh khoản hoặc vốn hóa,
  hoặc bất kỳ yêu cầu nào liên quan đến "tìm", "lọc", "sàng lọc", "screener", "scanner" cổ phiếu.
  Kích hoạt cả khi người dùng hỏi dạng: "cổ phiếu nào có P/E thấp", "tìm cổ phiếu ngành ngân hàng",
  "cổ phiếu nào tăng mạnh hôm nay", "lọc cổ phiếu vốn hóa lớn",
  "cổ phiếu nào đáng mua", "tìm cổ phiếu tốt", "gợi ý cổ phiếu".
---

# VN Stock Screener v2 — Sàng lọc cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude sàng lọc cổ phiếu từ dữ liệu có sẵn của người dùng, áp dụng các bộ lọc đa chiều để tìm ra cổ phiếu phù hợp với tiêu chí đầu tư.

## Dữ liệu đầu vào

Người dùng đã có sẵn dữ liệu. Khi nhận yêu cầu sàng lọc:

1. Hỏi người dùng đường dẫn file dữ liệu nếu chưa rõ (CSV, Excel, database, hoặc API endpoint)
2. Đọc và hiểu cấu trúc dữ liệu trước khi xử lý
3. Xác nhận các cột/trường có sẵn để biết có thể lọc theo tiêu chí nào

---

## ⭐ QUAN TRỌNG: Phân loại yêu cầu trước khi xử lý

> Đây là bước BẮT BUỘC đầu tiên. Trước khi làm bất cứ gì, phân loại yêu cầu vào 1 trong 3 nhóm:

### Nhóm A — Yêu cầu RÕ RÀNG (có ≥2 tiêu chí số cụ thể)
Xử lý ngay, không cần hỏi lại.

**Ví dụ:**
- "Lọc cổ phiếu P/E < 10" → Rõ: 1 tiêu chí số, đơn giản → chạy luôn
- "Lọc P/E < 12, ROE > 15%, cổ tức > 3%" → Rõ: 3 tiêu chí số → chạy luôn
- "Cổ phiếu ngân hàng RSI < 30" → Rõ: ngành + 1 tiêu chí số → chạy luôn

### Nhóm B — Yêu cầu NỬA RÕ (có ý định nhưng thiếu con số)
Dịch ngôn ngữ tự nhiên → bộ lọc, trình bày logic dịch, xin xác nhận trước khi chạy.

**Ví dụ:**
- "Tìm cổ phiếu công nghệ tăng trưởng mạnh, chưa quá đắt" → Nhóm B
- "Cổ phiếu nào nợ thấp mà lợi nhuận cao" → Nhóm B
- "Lọc cổ phiếu blue-chip phòng thủ" → Nhóm B

### Nhóm C — Yêu cầu MƠ HỒ (không có tiêu chí nào cụ thể)
BẮT BUỘC hỏi lại. Đề xuất chiến lược có sẵn để người dùng chọn.

**Ví dụ:**
- "Cổ phiếu nào đáng mua?" → Nhóm C → PHẢI hỏi lại
- "Gợi ý cổ phiếu tốt" → Nhóm C → PHẢI hỏi lại
- "Nên mua gì bây giờ?" → Nhóm C → PHẢI hỏi lại

**Cách hỏi lại cho Nhóm C:**
```
Tôi cần hiểu rõ hơn chiến lược đầu tư của bạn. Bạn quan tâm nhất đến điều gì?

1. 💎 **Giá trị (Value)** — Cổ phiếu đang rẻ hơn giá trị thực (P/E thấp, P/B thấp, ROE cao)
2. 🚀 **Tăng trưởng (Growth)** — Doanh nghiệp đang mở rộng nhanh (EPS tăng >20%, doanh thu tăng mạnh)
3. 💰 **Cổ tức (Dividend)** — Thu nhập thụ động ổn định (cổ tức >5%, trả đều đặn)
4. ⚡ **Momentum** — Đang có đà tăng giá mạnh (RSI tốt, volume tăng, uptrend)
5. 🔄 **Phục hồi (Turnaround)** — Cổ phiếu tốt đang bị bán quá mức

Hoặc bạn có thể mô tả phong cách đầu tư của mình, tôi sẽ thiết kế bộ lọc phù hợp.
```

---

## Dịch ngôn ngữ tự nhiên → Bộ lọc (cho Nhóm B)

Khi người dùng mô tả bằng từ ngữ chung chung, dịch theo bảng mapping sau:

### Bảng mapping từ khóa → tiêu chí

| Từ khóa người dùng | Dịch thành tiêu chí | Ngưỡng mặc định |
|---------------------|---------------------|-----------------|
| "tăng trưởng mạnh", "tăng nhanh" | EPS growth YoY | > 20% |
| "tăng trưởng", "đang phát triển" | EPS growth YoY | > 10% |
| "rẻ", "giá hời", "undervalued" | P/E so với trung bình ngành | < 0.8x trung bình ngành |
| "chưa quá đắt", "hợp lý" | P/E | < 18 (hoặc < trung bình ngành) |
| "đắt", "overvalued" | P/E | > 1.3x trung bình ngành |
| "nợ thấp", "tài chính lành mạnh" | D/E ratio | < 0.8 (phi tài chính) |
| "nợ cao", "đòn bẩy lớn" | D/E ratio | > 1.5 |
| "lợi nhuận cao", "sinh lời tốt" | ROE | > 15% |
| "thanh khoản tốt", "dễ mua bán" | Volume TB 20 phiên | > 500,000 cp/phiên |
| "thanh khoản thấp" | Volume TB 20 phiên | < 100,000 cp/phiên |
| "blue-chip", "an toàn" | VN30 + Vốn hóa | Thuộc VN30 hoặc > 20K tỷ |
| "penny", "nhỏ" | Vốn hóa | < 500 tỷ |
| "phòng thủ", "defensive" | Beta | < 0.8 |
| "đang bị bán quá mức", "oversold" | RSI | < 30 |
| "đang tăng", "uptrend" | Giá vs MA | Giá > MA50, MA50 > MA200 |
| "cổ tức cao", "thu nhập thụ động" | Dividend yield | > 5% |
| "ổn định", "consistent" | Std deviation lợi nhuận 3Y | Thấp |

**Quy trình dịch Nhóm B:**
1. Xác định từ khóa trong câu hỏi
2. Map sang tiêu chí cụ thể theo bảng trên
3. **TRƯỚC KHI CHẠY**: trình bày logic dịch cho người dùng xác nhận:
```
Tôi hiểu yêu cầu như sau:
- "công nghệ" → Lọc ngành: CNTT, Viễn thông
- "tăng trưởng mạnh" → EPS growth > 20% YoY
- "chưa quá đắt" → P/E < 18 (hoặc < trung bình ngành CNTT)
- "thanh khoản tốt" → Volume TB 20 phiên > 500K

Bạn có muốn điều chỉnh gì không? Nếu OK tôi sẽ chạy lọc.
```

---

## Bộ lọc hỗ trợ

### Bộ lọc cơ bản (Fundamental) — Áp dụng cho ngành PHI TÀI CHÍNH

| Tiêu chí | Mô tả | Ví dụ điều kiện |
|-----------|--------|-----------------|
| P/E (TTM) | Giá / EPS trailing 12 tháng | P/E < 15 |
| P/B | Giá / Book value per share | P/B < 1.5 |
| ROE | Lợi nhuận / Vốn chủ sở hữu | ROE > 15% |
| ROA | Lợi nhuận / Tổng tài sản | ROA > 5% |
| EPS | Lợi nhuận trên cổ phiếu | EPS > 3000 VND |
| Revenue growth | Tăng trưởng doanh thu YoY | > 10% |
| EPS growth | Tăng trưởng EPS YoY | > 15% |
| Net margin | Biên lợi nhuận ròng | > 10% |
| D/E | Nợ / Vốn chủ sở hữu | D/E < 1 |
| Dividend yield | Tỷ suất cổ tức | > 5% |
| Market cap | Vốn hóa thị trường | > 1000 tỷ VND |

**⚠️ Cảnh báo P/E:**
- P/E < 0 nghĩa là doanh nghiệp đang lỗ → loại khỏi kết quả khi lọc P/E thấp
- P/E rất thấp (< 3) có thể do lợi nhuận đột biến 1 lần → cần kiểm tra

### ⭐ Bộ lọc NGÀNH ĐẶC THÙ

Đây là phần quan trọng. Một số ngành có cấu trúc tài chính rất khác biệt, KHÔNG THỂ dùng chỉ số chung.

#### Ngân hàng (Banking)
**KHÔNG dùng:** D/E ratio, current ratio, quick ratio (vô nghĩa vì ngân hàng đòn bẩy cao là bản chất)

**DÙNG thay thế:**

| Chỉ số | Mô tả | Ngưỡng tốt |
|--------|--------|------------|
| NIM | Net Interest Margin — biên lãi ròng | > 3.5% |
| NPL ratio | Non-Performing Loan — tỷ lệ nợ xấu | < 2% |
| CASA | Tỷ lệ tiền gửi không kỳ hạn | > 25% |
| CIR | Cost-to-Income Ratio — hiệu quả chi phí | < 40% |
| CAR | Capital Adequacy Ratio — an toàn vốn | > 10% |
| Tăng trưởng tín dụng | Credit growth YoY | > 10% |
| P/B | Vẫn dùng được, là chỉ số chính cho ngân hàng | 1.0 - 2.5 |
| ROE | Vẫn dùng | > 15% |

**Ví dụ:** Khi nhận "lọc cổ phiếu ngân hàng nợ thấp" → dịch thành NPL < 2% (KHÔNG phải D/E < 1)

#### Bất động sản (Real Estate)
**Chỉ số bổ sung:**

| Chỉ số | Mô tả | Ghi chú |
|--------|--------|---------|
| NAV | Net Asset Value — giá trị tài sản ròng | So sánh P/NAV |
| Landbank | Quỹ đất (ha) | Quan trọng cho triển vọng |
| Backlog | Giá trị hợp đồng chưa ghi nhận | Đảm bảo doanh thu tương lai |
| D/E | Vẫn dùng nhưng ngưỡng khác | Chấp nhận D/E < 2 (cao hơn ngành khác) |

#### Chứng khoán (Securities)
**Chỉ số bổ sung:**

| Chỉ số | Mô tả |
|--------|--------|
| Thị phần môi giới | Market share (%) |
| Margin lending | Dư nợ cho vay margin |
| Proprietary trading P&L | Lãi/lỗ tự doanh |

**Quy tắc áp dụng:**
1. Khi phát hiện yêu cầu lọc cho ngành đặc thù → tự động chuyển sang bộ chỉ số riêng
2. Nếu dữ liệu không có chỉ số đặc thù (ví dụ không có NIM) → thông báo rõ:
   "Dữ liệu hiện tại không có cột NIM. Tôi sẽ dùng ROE và P/B thay thế cho ngân hàng. 
   Để lọc chính xác hơn, bạn có thể bổ sung dữ liệu NIM, NPL."

---

### Bộ lọc kỹ thuật (Technical)

| Tiêu chí | Mô tả | Chi tiết tính toán |
|-----------|--------|---------------------|
| RSI(14) | Relative Strength Index | Quá mua (>70), Quá bán (<30), Trung tính (30-70) |
| MACD(12,26,9) | Moving Average Convergence Divergence | Bullish: MACD cắt lên Signal. Bearish: ngược lại |
| MA crossover | Golden/Death cross | Golden: MA50 cắt lên MA200 (trong N phiên gần nhất). Death: ngược lại |
| Volume | Khối lượng giao dịch | Đột biến: > 2x trung bình 20 phiên |
| Bollinger Bands(20,2) | Biến động giá | Chạm band dưới → tiềm năng phục hồi |
| Giá vs MA | Vị trí giá so với trung bình động | Trên/dưới MA20, MA50, MA200 |

**Tính Golden/Death Cross gần đây:**
```python
# Golden cross trong N phiên gần nhất
def detect_recent_cross(df, short_period=50, long_period=200, lookback=5):
    ma_short = df['close'].rolling(short_period).mean()
    ma_long = df['close'].rolling(long_period).mean()
    
    # Cross xảy ra khi: hôm nay MA_short > MA_long VÀ N phiên trước MA_short < MA_long
    for i in range(1, lookback + 1):
        today_above = ma_short.iloc[-1] > ma_long.iloc[-1]
        past_below = ma_short.iloc[-1-i] < ma_long.iloc[-1-i]
        if today_above and past_below:
            return True, i  # Golden cross, xảy ra i phiên trước
    return False, None
```

---

### Bộ lọc phân loại

| Tiêu chí | Giá trị |
|-----------|---------|
| Sàn | HOSE, HNX, UPCOM |
| Ngành ICB | Ngân hàng, Bất động sản, Công nghệ, Thép, Dầu khí... |
| Vốn hóa | Large-cap (>10K tỷ), Mid-cap (1K-10K tỷ), Small-cap (<1K tỷ) |
| Rổ chỉ số | VN30, VN100, HNX30 |

---

## Quy trình xử lý

Khi nhận yêu cầu sàng lọc:

### Bước 1: Phân loại yêu cầu (A / B / C)
Xem section "Phân loại yêu cầu" ở trên.

### Bước 2: Xác định ngành
- Nếu yêu cầu chỉ rõ ngành → áp bộ chỉ số đặc thù (Ngân hàng, BĐS, Chứng khoán...)
- Nếu lọc chung → dùng bộ chỉ số cơ bản, nhưng cẩn thận khi trộn ngành tài chính và phi tài chính

### Bước 3: Dịch bộ lọc (nếu Nhóm B)
Dùng bảng mapping, trình bày logic, xin xác nhận.

### Bước 4: Load dữ liệu & Kiểm tra
- Đọc file dữ liệu
- Kiểm tra cột có sẵn
- Nếu thiếu cột → thông báo + đề xuất thay thế

### Bước 5: Áp dụng bộ lọc + Filtering Funnel
Viết code Python (pandas). **BẮT BUỘC** hiển thị funnel:
```
📊 Filtering Funnel:
Tổng số mã: 1,652
├─ Sàn HOSE: 416 mã (loại 1,236)
├─ P/E > 0 và < 12: 98 mã (loại 318)
├─ ROE > 15%: 42 mã (loại 56)
├─ D/E < 1: 28 mã (loại 14)
└─ Dividend yield > 3%: 15 mã ✅ KẾT QUẢ
```

### Bước 6: Xử lý kết quả

**Nếu kết quả bình thường (1-30 mã):**
- Hiển thị bảng sắp xếp hợp lý
- Ghi rõ thời điểm dữ liệu

**Nếu kết quả quá nhiều (>30 mã):**
- Hiển thị top 20 theo tiêu chí ưu tiên
- Đề xuất thêm bộ lọc để thu hẹp

**Nếu kết quả rỗng (0 mã):**
Áp dụng **Relaxation Waterfall** — nới lỏng từng tiêu chí một:
```
⚠️ 0 mã thỏa mãn tất cả điều kiện. Phân tích từng bước:

Nếu bỏ điều kiện "Dividend yield > 10%" → được 5 mã
Nếu bỏ điều kiện "ROE > 30%" → được 3 mã  
Nếu nới P/E < 3 thành P/E < 8 → được 12 mã

Đề xuất: Nới lỏng Dividend yield từ >10% xuống >5% sẽ có 18 mã phù hợp.
Bạn muốn điều chỉnh tiêu chí nào?
```

### Bước 7: Xuất kết quả
- Lưu ra CSV/Excel nếu yêu cầu
- Nhắc: "Kết quả sàng lọc chỉ là bước đầu — nên phân tích sâu hơn trước khi ra quyết định"

---

## Các chiến lược sàng lọc có sẵn

Khi người dùng chọn chiến lược (hoặc Claude gợi ý cho Nhóm C):

**💎 Cổ phiếu giá trị (Value):**
- P/E < 12, P/B < 1.5, ROE > 12%, D/E < 1, cổ tức > 3%

**🚀 Cổ phiếu tăng trưởng (Growth):**
- EPS growth > 20% YoY, revenue growth > 15%, ROE > 15%

**⚡ Cổ phiếu momentum:**
- RSI 50-70, giá trên MA50, volume TB 20 phiên > 500K, MACD Bullish

**💰 Cổ phiếu cổ tức (Dividend):**
- Dividend yield > 5%, payout ratio < 70%, lợi nhuận ổn định 3 năm

**🔄 Cổ phiếu phục hồi (Turnaround):**
- P/B < 1, RSI < 35, lợi nhuận quý gần nhất cải thiện vs quý trước

---

## Lưu ý quan trọng

- Luôn ghi rõ nguồn dữ liệu và thời điểm dữ liệu được cập nhật
- Kết quả sàng lọc chỉ là bước đầu tiên — nhắc người dùng cần phân tích sâu hơn
- KHÔNG đưa khuyến nghị mua/bán cụ thể, chỉ cung cấp dữ liệu khách quan
- Khi lọc P/E, luôn loại P/E âm (doanh nghiệp đang lỗ) trừ khi người dùng yêu cầu ngược lại
- Khi so sánh xuyên ngành, ghi chú rõ: "P/E ngân hàng thường thấp hơn P/E công nghệ — so sánh trong cùng ngành sẽ chính xác hơn"
