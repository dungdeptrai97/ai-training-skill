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

# VN Stock Screener v3 — Sàng lọc cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude sàng lọc cổ phiếu từ dữ liệu có sẵn của người dùng, áp dụng các bộ lọc đa chiều để tìm ra cổ phiếu phù hợp với tiêu chí đầu tư.

## Dữ liệu đầu vào

Người dùng đã có sẵn dữ liệu. Khi nhận yêu cầu sàng lọc:

1. Hỏi người dùng đường dẫn file dữ liệu nếu chưa rõ (CSV, Excel, database, hoặc API endpoint)
2. Đọc và hiểu cấu trúc dữ liệu trước khi xử lý
3. Xác nhận các cột/trường có sẵn để biết có thể lọc theo tiêu chí nào

---

## ⭐ Bước 0: Phân loại yêu cầu (BẮT BUỘC)

### Nhóm A — Yêu cầu RÕ RÀNG → Chạy ngay
Có ≥1 tiêu chí số cụ thể: "P/E < 10", "RSI < 30", "ngân hàng ROE > 15%"

### Nhóm B — Yêu cầu NỬA RÕ → Dịch NLP, chạy luôn, giải thích kèm
Có ý định nhưng thiếu con số: "cổ phiếu tăng trưởng mạnh", "nợ thấp lợi nhuận cao"

> **[v3 THAY ĐỔI]** Nhóm B: KHÔNG dừng lại hỏi xác nhận. Thay vào đó, dịch → chạy luôn → giải thích
> logic dịch NGAY TRONG kết quả. Nếu người dùng muốn điều chỉnh, họ sẽ nói.
> Lý do: 90% trường hợp người dùng đồng ý với cách dịch, hỏi trước chỉ tạo thêm 1 lượt chat thừa.

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
```

---

## Dịch ngôn ngữ tự nhiên → Bộ lọc

### Bảng mapping từ khóa → tiêu chí

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

### Bộ lọc kỹ thuật

| Chỉ báo | Chi tiết |
|---------|---------|
| RSI(14) | >70: quá mua, <30: quá bán |
| MACD(12,26,9) | Bullish/Bearish cross |
| Golden/Death cross | MA50 cắt MA200 (xem code trong references/) |
| Volume đột biến | > 2x TB 20 phiên |
| Bollinger(20,2) | Chạm band trên/dưới |
| Giá vs MA | So với MA20, MA50, MA200 |

---

## ⭐ Quy trình xử lý (v3 update)

### Bước 1: Phân loại → Bước 0 ở trên

### Bước 2: Xác định ngành → áp bộ chỉ số đặc thù nếu cần

### Bước 3: Load dữ liệu, kiểm tra cột

### Bước 4: Chạy lọc + hiển thị Compact Funnel

> **[v3 THAY ĐỔI]** Funnel dạng compact — chỉ 3 dòng bất kể bao nhiêu tiêu chí:

```
📊 1,652 mã → [5 bộ lọc] → 15 mã ✅
   Bước loại nhiều nhất: "P/E < 12" (loại 68% mã)
   Chi tiết funnel: [xem đầy đủ]
```

Nếu người dùng muốn xem full funnel, cung cấp khi được hỏi. Mặc định chỉ hiện dạng compact.

### Bước 5: Trình bày kết quả với Smart Sort + Benchmark ngành

> **[v3 THAY ĐỔI — SMART SORT]** Tự động sort theo tiêu chí ưu tiên:

| Loại yêu cầu | Sort theo | Hướng |
|---------------|-----------|-------|
| Lọc P/E thấp | P/E | ↑ tăng dần |
| Lọc cổ tức | Dividend yield | ↓ giảm dần |
| Lọc tăng trưởng | EPS growth | ↓ giảm dần |
| Lọc ROE cao | ROE | ↓ giảm dần |
| Lọc RSI quá bán | RSI | ↑ tăng dần |
| Lọc momentum | Composite (RSI + MACD + trend) | ↓ giảm dần |
| Multi-criteria | Composite rank trung bình | ↑ tăng dần |

**Composite rank cho multi-criteria:**
```python
# Rank từng tiêu chí (1 = tốt nhất), lấy trung bình rank
df['rank_pe'] = df['pe'].rank(ascending=True)  # PE thấp → rank cao
df['rank_roe'] = df['roe'].rank(ascending=False)  # ROE cao → rank cao
df['composite_rank'] = (df['rank_pe'] + df['rank_roe']) / 2
df = df.sort_values('composite_rank')
```

> **[v3 THAY ĐỔI — BENCHMARK NGÀNH]** Mỗi bảng kết quả thêm 1 dòng cuối hiển thị TB ngành:

```
| Mã  | Giá    | P/E  | ROE   | EPS Growth | Div  |
|-----|--------|------|-------|------------|------|
| HPG | 26,800 | 9.8  | 14.2% | +35.2%     | 2.1% |
| HSG | 18,500 | 7.2  | 11.5% | +28.1%     | 3.5% |
| NKG | 12,300 | 6.5  | 10.8% | +42.3%     | 1.8% |
|-----|--------|------|-------|------------|------|
| 📊 TB ngành Thép | | 11.2 | 12.5% | +22.5% | 2.3% |
```

Dòng benchmark giúp người dùng biết: HPG P/E=9.8 THẤP HƠN TB ngành 11.2 → hấp dẫn.

### Bước 6: Xử lý kết quả đặc biệt

**Quá nhiều (>30):** Hiện top 20 + "Còn X mã khác. Thêm bộ lọc để thu hẹp?"

**Kết quả rỗng:** Relaxation Waterfall:
```
⚠️ 0 kết quả. Nới lỏng từng tiêu chí:
• Bỏ "Div > 10%" → 5 mã | Nới "ROE > 30%" → ">20%" → 8 mã | Nới "P/E < 3" → "<8" → 12 mã
→ Đề xuất: nới Dividend yield từ 10% xuống 5%.
```

### Bước 7: Xuất kết quả
Lưu CSV/Excel nếu yêu cầu. Nhắc: sàng lọc chỉ là bước đầu.

---

## Ví dụ đầu vào / đầu ra hoàn chỉnh

### Ví dụ 1 — Nhóm A (Rõ ràng)

**Input:** "Lọc cổ phiếu P/E < 10, ROE > 15% trên HOSE"

**Output:**
```
📊 416 mã HOSE → [P/E 0-10, ROE > 15%] → 8 mã ✅
   Bước loại nhiều nhất: "P/E < 10" (loại 76%)

| #  | Mã  | Tên         | Ngành     | Giá    | P/E  | ROE   | D/E  |
|----|-----|-------------|-----------|--------|------|-------|------|
| 1  | ACB | ACB         | Ngân hàng | 25,800 | 7.8  | 20.5% | —*   |
| 2  | HPG | Hòa Phát    | Thép      | 26,800 | 9.8  | 14.2% | 0.85 |
| 3  | VHM | Vinhomes    | BĐS       | 42,500 | 8.5  | 18.5% | 1.20 |
| ...                                                                |
|----|----|-------------|-----------|--------|------|-------|------|
| 📊 | TB | HOSE        |           |        | 15.8 | 12.3% | 0.95 |

*D/E không áp dụng cho ngành ngân hàng

📁 Lưu file? Nói "xuất CSV" hoặc "xuất Excel".
⚠️ Đây là kết quả sàng lọc, không phải khuyến nghị đầu tư.
```

### Ví dụ 2 — Nhóm B (NLP)

**Input:** "Tìm cổ phiếu công nghệ tăng trưởng mạnh, chưa quá đắt"

**Output:**
```
💡 Dịch: "công nghệ" → CNTT/Viễn thông | "tăng trưởng mạnh" → EPS growth > 20% | "chưa quá đắt" → P/E < 18

📊 32 mã CNTT → [EPS growth > 20%, P/E < 18] → 3 mã ✅
   Bước loại nhiều nhất: "EPS growth > 20%" (loại 72%)

| #  | Mã  | Tên     | Giá     | P/E  | EPS Gr | ROE   | Vol TB  |
|----|-----|---------|---------|------|--------|-------|---------|
| 1  | CMG | CMC     | 58,200  | 14.5 | +32.1% | 18.2% | 850K    |
| 2  | FOX | FPT Tel | 82,100  | 16.2 | +25.8% | 22.5% | 1.2M    |
| 3  | ELC | Elcom   | 35,800  | 11.8 | +21.5% | 16.8% | 320K    |
|----|-----|---------|---------|------|--------|-------|---------|
| 📊 | TB ngành CNTT    |         | 22.5 | +15.3% | 14.8% |         |

↩️ Điều chỉnh? Ví dụ: nới P/E < 25, thêm thanh khoản > 500K/phiên.
```

### Ví dụ 3 — Nhóm C (Mơ hồ)

**Input:** "Cổ phiếu nào đáng mua?"

**Output:**
```
Tôi cần hiểu thêm về phong cách đầu tư của bạn. Bạn quan tâm nhất đến:
1. 💎 Giá trị — P/E thấp, tài chính vững, cổ tức ổn
2. 🚀 Tăng trưởng — Doanh nghiệp mở rộng nhanh, EPS tăng mạnh
3. 💰 Cổ tức — Thu nhập thụ động > 5%/năm
4. ⚡ Momentum — Đang có đà tăng giá, volume tốt
5. 🔄 Phục hồi — Cổ phiếu chất lượng đang bị bán quá mức

Chọn số (1-5) hoặc mô tả thêm, tôi sẽ thiết kế bộ lọc phù hợp.
```

---

## Chiến lược sàng lọc có sẵn

| Chiến lược | Tiêu chí |
|------------|---------|
| 💎 Value | P/E < 12, P/B < 1.5, ROE > 12%, D/E < 1, Div > 3% |
| 🚀 Growth | EPS gr > 20%, Rev gr > 15%, ROE > 15% |
| ⚡ Momentum | RSI 50-70, Giá > MA50, Vol > 500K, MACD Bullish |
| 💰 Dividend | Div > 5%, Payout < 70%, LN ổn định 3Y |
| 🔄 Turnaround | P/B < 1, RSI < 35, LN quý gần nhất cải thiện |

---

## Lưu ý

- Ghi rõ nguồn + thời điểm dữ liệu
- KHÔNG khuyến nghị mua/bán
- P/E < 0 = lỗ → tự loại khi lọc "P/E thấp"
- So sánh xuyên ngành cần ghi chú: "P/E ngân hàng thường < P/E công nghệ"
- Kết quả sàng lọc là bước đầu, cần phân tích sâu hơn
