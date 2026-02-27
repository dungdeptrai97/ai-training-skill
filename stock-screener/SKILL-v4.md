---
name: vn-stock-screener
description: >
  Sàng lọc và tìm kiếm cổ phiếu, cơ hội đầu tư trên thị trường chứng khoán Việt Nam (HOSE, HNX, UPCOM).
  Sử dụng skill này khi người dùng muốn: lọc cổ phiếu theo tiêu chí tài chính (P/E, P/B, ROE, EPS...),
  tìm cổ phiếu theo ngành/nhóm ngành, sàng lọc theo chỉ báo kỹ thuật (RSI, MACD, volume đột biến...),
  tìm cổ phiếu breakout/breakdown, lọc theo thanh khoản hoặc vốn hóa,
  lọc cổ phiếu theo sự kiện (chia cổ tức, cổ phiếu thưởng, insider buying, đối tác chiến lược, quỹ mua vào),
  tìm cổ phiếu hưởng lợi từ tin tức vĩ mô hoặc giá hàng hóa tăng,
  hoặc bất kỳ yêu cầu nào liên quan đến "tìm", "lọc", "sàng lọc", "screener", "scanner" cổ phiếu,
  "cơ hội đầu tư", "tin tốt", "sự kiện", "catalyst".
  Kích hoạt cả khi người dùng hỏi dạng: "cổ phiếu nào có P/E thấp", "tìm cổ phiếu ngành ngân hàng",
  "cổ phiếu nào tăng mạnh hôm nay", "lọc cổ phiếu vốn hóa lớn",
  "cổ phiếu nào đáng mua", "tìm cổ phiếu tốt", "gợi ý cổ phiếu",
  "cổ phiếu nào sắp chia cổ tức", "insider đang mua mã nào",
  "quỹ ngoại mua gì", "giá thép tăng thì mua gì", "đầu tư công tăng hưởng lợi gì",
  "cổ phiếu nào đang có tin tốt".
---

# VN Stock Screener v4 — Sàng lọc cổ phiếu & Cơ hội đầu tư Việt Nam

## Mục đích

Skill này giúp Claude sàng lọc cổ phiếu từ dữ liệu có sẵn của người dùng, áp dụng các bộ lọc đa chiều (cơ bản, kỹ thuật, sự kiện, vĩ mô) để tìm ra cổ phiếu và cơ hội đầu tư phù hợp.

## Dữ liệu đầu vào

Người dùng đã có sẵn dữ liệu. Khi nhận yêu cầu sàng lọc:

1. Hỏi người dùng đường dẫn file dữ liệu nếu chưa rõ (CSV, Excel, database, hoặc API endpoint)
2. Đọc và hiểu cấu trúc dữ liệu trước khi xử lý
3. Xác nhận các cột/trường có sẵn để biết có thể lọc theo tiêu chí nào

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

**Cách hỏi lại (v4 — đã thêm Event-driven và Macro):**
```
Bạn quan tâm nhất đến điều gì?
1. 💎 Giá trị — Cổ phiếu đang rẻ hơn giá trị thực
2. 🚀 Tăng trưởng — Doanh nghiệp đang mở rộng nhanh
3. 💰 Cổ tức — Thu nhập thụ động ổn định
4. ⚡ Momentum — Đang có đà tăng giá mạnh
5. 🔄 Phục hồi — Cổ phiếu tốt đang bị bán quá mức
6. 📰 Sự kiện — Cổ phiếu có catalyst tích cực (insider mua, quỹ mua, đối tác chiến lược, cổ tức đặc biệt)
7. 🌍 Vĩ mô — Cổ phiếu hưởng lợi từ xu hướng kinh tế/giá hàng hóa

Chọn số (1-7) hoặc mô tả thêm, tôi sẽ thiết kế bộ lọc phù hợp.
```

### Nhóm D — Yêu cầu SỰ KIỆN / VĨ MÔ → Xác định catalyst, map sang ngành, chạy
Đề cập sự kiện cụ thể hoặc xu hướng vĩ mô: "giá thép tăng", "đầu tư công", "insider mua", "chia cổ tức"

> Nhóm D là NHÓM MỚI trong v4. Khi phát hiện từ khóa sự kiện/vĩ mô → dùng quy trình riêng (xem section Event-based và Macro bên dưới). Có thể KẾT HỢP với lọc fundamental/technical.

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

### ⭐ [v4 MỚI] Bảng mapping từ khóa → SỰ KIỆN

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

### ⭐ [v4 MỚI] Bảng mapping từ khóa → VĨ MÔ / HÀNG HÓA

| Từ khóa | Ngành hưởng lợi TRỰC TIẾP | Ngành hưởng lợi GIÁN TIẾP | Ngành BẤT LỢI |
|---------|--------------------------|--------------------------|---------------|
| "giá thép tăng", "HRC tăng" | Thép: HPG, HSG, NKG, TLH, POM | Khai khoáng sắt | Xây dựng, ô tô (chi phí tăng) |
| "giá dầu tăng", "dầu thô lên" | Dầu khí: PVD, PVS, PLX, OIL, BSR | Dịch vụ dầu khí: PVS, PVC | Vận tải, hàng không: VJC, HVN |
| "giá gạo tăng" | Gạo/Nông sản: LTG, AGM, TAR | Phân bón: DPM, DCM | — |
| "giá phân bón tăng" | Phân bón: DPM, DCM, BFC | Hóa chất: CSV, DGC | Nông nghiệp (chi phí tăng) |
| "giá cao su tăng" | Cao su: PHR, DPR, TRC | Lốp xe: DRC, SRC, CSM | Sản xuất đồ nhựa |
| "giá đường tăng" | Mía đường: SBT, QNS, LSS | — | F&B dùng đường làm đầu vào |
| "lãi suất giảm" | Ngân hàng (NIM có thể giảm nhưng tín dụng tăng), BĐS (chi phí vốn giảm), CK (margin rẻ hơn) | Tiêu dùng (vay tiêu dùng rẻ) | — |
| "lãi suất tăng" | Ngân hàng (NIM mở rộng ngắn hạn) | — | BĐS, CK, DN đòn bẩy cao |
| "đầu tư công tăng", "giải ngân đầu tư công" | Xây dựng hạ tầng: CTD, HHV, C69, LCG, FCN | Thép: HPG, HSG; Xi măng: HT1, BCC; VLXD | — |
| "FDI tăng", "vốn ngoại vào" | KCN: KBC, SZC, IDC, NTC, SIP | Xây dựng, logistics | — |
| "xuất khẩu tăng", "đơn hàng xuất khẩu" | Dệt may: TCM, MSH, STK; Gỗ: PTB, GDT; Thủy sản: VHC, MPC, FMC | Logistics: GMD, HAH | — |
| "tiêu dùng phục hồi" | Bán lẻ: MWG, FRT, DGW; F&B: VNM, SAB, MCH | Tiêu dùng: PNJ, MSN | — |
| "du lịch phục hồi", "khách quốc tế tăng" | Hàng không: VJC, HVN, ACV; Khách sạn | Du lịch, dịch vụ ăn uống | — |

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

### ⭐ [v4 MỚI] Bộ lọc sự kiện (Event-based)

Khi phát hiện yêu cầu liên quan đến sự kiện, dùng bộ lọc này:

#### Sự kiện doanh nghiệp (Corporate Events)

| Loại sự kiện | Dữ liệu cần | Cột hiển thị | Ý nghĩa đầu tư |
|-------------|-------------|-------------|----------------|
| Cổ tức tiền mặt | Ngày GDKHQ, tỷ lệ (%), giá trị (VND/cp) | Mã, tên, tỷ lệ, giá trị/cp, ngày GDKHQ, div yield | Thu nhập thụ động, tín hiệu tài chính lành mạnh |
| Cổ phiếu thưởng | Tỷ lệ (VD: 10:1), ngày chốt quyền | Mã, tên, tỷ lệ thưởng, ngày chốt, % dilution | Tăng thanh khoản, nhưng dilute EPS — cần đánh giá kỹ |
| Insider buying | Ai mua, chức vụ, SL mua, SL sau GD | Mã, tên, người mua, SL, % vốn, ngày GD | **Tín hiệu tích cực mạnh** — ban lãnh đạo bỏ tiền túi mua = tin vào triển vọng |
| Insider selling | Ai bán, chức vụ, SL bán, lý do | Mã, tên, người bán, SL, % vốn, lý do | Cần xem xét lý do — bán vì nhu cầu cá nhân hay mất niềm tin |
| Đối tác chiến lược | Đối tác, % sở hữu, loại GD (mua CP/phát hành riêng lẻ) | Mã, tên, đối tác, % sở hữu, mục đích | **Rất tích cực** — nâng uy tín, chuyển giao công nghệ, mở rộng thị trường |
| Quỹ đầu tư mua ròng | Foreign flow, ETF rebalance, fund ownership | Mã, tên, giá trị mua ròng 30D, số quỹ nắm giữ, % room còn | **Smart money tích lũy** — tín hiệu trung-dài hạn |
| KQKD vượt kỳ vọng | EPS actual vs consensus, % beat | Mã, tên, EPS actual, EPS estimate, % beat | Thường kéo giá tăng ngắn hạn, đặc biệt nếu beat > 15% |

#### Quy trình lọc theo sự kiện

**Bước 1:** Xác định loại sự kiện từ yêu cầu (dùng bảng mapping NLP ở trên)

**Bước 2:** Kiểm tra dữ liệu có cột sự kiện không
- NẾU CÓ: lọc trực tiếp
- NẾU KHÔNG: thông báo gọn + hướng dẫn nguồn dữ liệu:
```
⚠️ Dữ liệu hiện tại không có thông tin [loại sự kiện]. Nguồn bổ sung:
• Insider trading: cbtt.vsd.vn, SSI iBoard, VNDirect
• Cổ tức/thưởng: cafef.vn/su-kien, vietstock.vn/su-kien
• Foreign flow: SSI iBoard, FiinPro, VNDirect
• Đối tác chiến lược: CBTT trên HOSE/HNX, cafef.vn
```

**Bước 3:** Hiển thị kết quả với format riêng cho sự kiện:
```
📰 Sự kiện: [LOẠI SỰ KIỆN] — [X] mã có sự kiện trong [KHUNG THỜI GIAN]

| #  | Mã  | Tên        | Sự kiện              | Chi tiết              | Ngày    | P/E  | ROE   |
|----|-----|------------|----------------------|-----------------------|---------|------|-------|
| 1  | VNM | Vinamilk   | 💰 Cổ tức tiền mặt  | 1,500 VND/cp (3.2%)   | 15/03   | 18.5 | 35.2% |
| 2  | FPT | FPT Corp   | 🎁 Cổ phiếu thưởng  | 15% (10:1.5)          | 20/03   | 22.1 | 28.5% |
| 3  | MWG | MWG        | 👔 Insider mua       | Chủ tịch mua 500K cp  | 10/03   | 12.8 | 18.2% |

💡 Sự kiện tích cực không đảm bảo giá tăng — luôn kết hợp phân tích tài chính trước khi quyết định.
```

#### ⚠️ Cảnh báo quan trọng cho sự kiện

- **Cổ tức:** Giá thường điều chỉnh giảm bằng đúng giá trị cổ tức vào ngày GDKHQ. KHÔNG mua chỉ vì cổ tức cao mà quên kiểm tra tài chính.
- **Cổ phiếu thưởng:** Dilute EPS → P/E có thể tăng ảo nếu giá không điều chỉnh tương ứng. Cần tính EPS adjusted.
- **Insider buying:** Chỉ thực sự có ý nghĩa khi: (1) người mua là CEO/Chủ tịch, (2) số lượng đáng kể (> 0.1% vốn), (3) mua bằng tiền túi (không phải ESOP). Insider mua lô nhỏ, mang tính hình thức thì KHÔNG phải tín hiệu mạnh.
- **Đối tác chiến lược:** Cần đánh giá: đối tác có thực sự lớn/uy tín không? Mục đích hợp tác cụ thể? Có cam kết lock-up không? Phát hành riêng lẻ giá bao nhiêu so với thị giá?
- **Quỹ mua ròng:** Quỹ ETF mua vì rebalance (bị động) khác với quỹ active fund mua chủ động. Phân biệt rõ 2 loại.

---

### ⭐ [v4 MỚI] Bộ lọc vĩ mô / hàng hóa (Macro & Commodity)

Khi người dùng hỏi "cổ phiếu nào hưởng lợi khi [sự kiện vĩ mô/giá hàng hóa]":

#### Quy trình xử lý

**Bước 1:** Xác định catalyst vĩ mô từ yêu cầu (dùng bảng mapping Macro ở trên)

**Bước 2:** Map catalyst → ngành hưởng lợi theo 3 cấp độ:

```
🟢 Hưởng lợi TRỰC TIẾP — Doanh thu/lợi nhuận tăng trực tiếp từ catalyst
🟡 Hưởng lợi GIÁN TIẾP — Hưởng lợi theo chuỗi giá trị, hiệu ứng lan tỏa
🔴 BẤT LỢI — Chi phí đầu vào tăng, cầu giảm vì giá tăng
```

**Bước 3:** Trong mỗi nhóm hưởng lợi, áp thêm fundamental filter (nếu user yêu cầu hoặc mặc định)

**Bước 4:** Hiển thị kết quả với format riêng:

```
🌍 Catalyst: "Giá thép thế giới tăng mạnh"

📊 Chuỗi tác động:
Giá HRC tăng → Biên LN sản xuất thép mở rộng → EPS cải thiện → Rerating P/E

🟢 HƯỞNG LỢI TRỰC TIẾP (sản xuất thép):
| #  | Mã  | Tên      | % DT từ thép | P/E  | EPS Gr | ROE   | Tác động        |
|----|-----|----------|-------------|------|--------|-------|-----------------|
| 1  | HPG | Hòa Phát | 85%         | 9.8  | +35%   | 14.2% | Biên LN tăng mạnh nhất (tự chủ quặng) |
| 2  | HSG | Hoa Sen  | 90%         | 7.2  | +28%   | 11.5% | Hưởng lợi nhưng đòn bẩy cao |
| 3  | NKG | NKG      | 95%         | 6.5  | +42%   | 10.8% | XK nhiều → hưởng lợi kép |
|----|-----|----------|-------------|------|--------|-------|-----------------|
| 📊 TB ngành Thép |   |             | 11.2 | +22%   | 12.5% |                 |

🟡 HƯỞNG LỢI GIÁN TIẾP:
| Mã  | Ngành         | Lý do                          |
|-----|---------------|--------------------------------|
| KSB | Khoáng sản    | Cung cấp nguyên liệu cho thép  |

🔴 BẤT LỢI (chi phí thép tăng → ảnh hưởng tiêu cực):
| Ngành      | Lý do                                       |
|------------|---------------------------------------------|
| Xây dựng   | CTD, HBC — chi phí vật liệu tăng → biên LN giảm |
| Ô tô       | VEA, SVC — chi phí sản xuất tăng             |

⚠️ Rủi ro: Giá thép tăng có thể chỉ là ngắn hạn. Theo dõi: giá HRC Trung Quốc, sản lượng thép TQ, chính sách cắt giảm sản lượng.
💡 Gợi ý: Dùng /stock-analysis HPG để phân tích sâu mã quan tâm.
```

#### ⭐ Template chuỗi tác động cho các catalyst phổ biến

**Giá hàng hóa tăng:**
```
Giá [hàng hóa] tăng → DN sản xuất [hàng hóa]: biên LN mở rộng (giá bán tăng, giá vốn chậm hơn)
                     → DN thương mại [hàng hóa]: lãi tồn kho
                     → DN dùng [hàng hóa] làm đầu vào: chi phí tăng → biên LN thu hẹp
```

**Đầu tư công tăng:**
```
Giải ngân ĐTC tăng → Xây dựng/hạ tầng: backlog tăng → Thép, xi măng, VLXD: nhu cầu tăng
                    → Logistics/vận tải: khối lượng hàng tăng
                    → Ngân hàng: giải ngân vốn cho dự án
                    → BĐS: hạ tầng kết nối → giá trị quỹ đất tăng
```

**Lãi suất giảm:**
```
Lãi suất giảm → BĐS: chi phí vốn giảm, nhu cầu mua nhà tăng
              → Chứng khoán: margin rẻ hơn, dòng tiền vào thị trường
              → DN đòn bẩy cao: chi phí lãi vay giảm → LN cải thiện
              → Ngân hàng: NIM có thể giảm ngắn hạn nhưng tín dụng tăng
```

**FDI/Vốn ngoại vào:**
```
FDI tăng → KCN: tỷ lệ lấp đầy tăng, giá thuê tăng
         → Xây dựng: nhu cầu xây nhà xưởng
         → Logistics: hàng hóa XNK tăng
         → Nhân lực: nhu cầu lao động tăng
```

---

## ⭐ [v4 MỚI] Data Quality Rules — Kiểm soát chất lượng dữ liệu

> TRƯỚC khi trình bày kết quả, BẮT BUỘC chạy qua các rule sau:

### Rule 1: Loại mã bất thường

| Trạng thái | Hành động | Ghi chú |
|-----------|----------|---------|
| Bị cảnh báo (warning) | ⛔ Loại khỏi kết quả mặc định | Hiện nếu user yêu cầu: "⚠️ [MÃ] đang bị cảnh báo — rủi ro cao" |
| Bị kiểm soát (controlled) | ⛔ Loại khỏi kết quả | — |
| Bị hạn chế giao dịch | ⛔ Loại khỏi kết quả | — |
| Tạm ngừng giao dịch | ⛔ Loại khỏi kết quả | — |
| Hủy niêm yết (delisting) | ⛔ Loại khỏi kết quả | — |

> Nếu dữ liệu có cột trạng thái (status, warning, flag...) → tự động áp rule trên.
> Nếu KHÔNG có cột trạng thái → ghi 1 dòng: "⚠️ Dữ liệu không có cột trạng thái niêm yết — kết quả có thể chứa mã bị cảnh báo/kiểm soát. Kiểm tra trên HOSE/HNX trước khi giao dịch."

### Rule 2: Flag outlier chỉ số tài chính

| Chỉ số | Điều kiện bất thường | Flag hiển thị |
|--------|---------------------|---------------|
| P/E | < 3 (quá thấp) | "⚡ P/E đột biến — có thể do LN 1 lần (bán tài sản, thanh lý). Kiểm tra nguồn gốc LN." |
| P/E | > 100 (quá cao) | "⚡ P/E rất cao — DN đang ở đáy LN hoặc thị trường kỳ vọng tăng trưởng rất mạnh." |
| ROE | > 50% | "⚡ ROE đột biến — có thể do vốn chủ thấp hoặc LN 1 lần. Kiểm tra sustainability." |
| ROE | < -100% | "⚡ ROE âm lớn — vốn chủ có thể đang âm. Rủi ro tài chính rất cao." |
| D/E | > 5 (phi tài chính) | "⚡ Đòn bẩy rất cao — rủi ro tài chính. Kiểm tra khả năng trả nợ." |
| EPS growth | > 200% | "⚡ Tăng trưởng đột biến — có thể từ nền thấp hoặc LN bất thường. Kiểm tra tính bền vững." |

### Rule 3: Flag thanh khoản và đặc điểm giao dịch

| Điều kiện | Flag |
|-----------|------|
| Volume TB 20 phiên < 50K cp/phiên | "🔸 Thanh khoản rất thấp — khó mua/bán khối lượng lớn" |
| Volume TB 20 phiên < 10K cp/phiên | "🔸 Gần như không có thanh khoản — KHÔNG nên giao dịch" |
| Mã mới lên sàn < 6 tháng | "🔸 IPO gần đây — dữ liệu lịch sử hạn chế, biến động có thể cao" |
| Free float < 20% | "🔸 Free float thấp — giá dễ bị thao túng, spread mua/bán lớn" |

### Cách hiển thị flag trong bảng kết quả

Thêm cột "Flag" ở cuối bảng, hoặc ghi chú cuối bảng nếu có mã bị flag:

```
| #  | Mã  | Giá    | P/E  | ROE    | EPS Gr | Flag |
|----|-----|--------|------|--------|--------|------|
| 1  | ABC | 15,200 | 2.1  | 18.5%  | +180%  | ⚡ P/E đột biến, EPS tăng trưởng bất thường |
| 2  | DEF | 8,500  | 8.2  | 22.3%  | +15%   | 🔸 Thanh khoản thấp (Vol TB: 35K) |
| 3  | GHI | 42,100 | 9.5  | 16.8%  | +22%   | ✅ |

Ghi chú flag:
• ABC: P/E = 2.1 — kiểm tra nguồn gốc lợi nhuận. EPS tăng 180% có thể từ nền rất thấp.
• DEF: Volume TB chỉ 35K cp/phiên — khó giao dịch khối lượng > 100 triệu VND.
```

---

## ⭐ Quy trình xử lý (v4 update)

### Bước 1: Phân loại yêu cầu (A / B / C / D)
Xem section Bước 0 ở trên. **v4 thêm Nhóm D cho sự kiện/vĩ mô.**

### Bước 2: Xác định loại lọc

| Nhóm | Loại lọc | Quy trình |
|------|---------|-----------|
| A, B | Fundamental + Technical | Quy trình v3 (giữ nguyên) |
| D — Sự kiện | Event-based | Dùng section "Bộ lọc sự kiện" |
| D — Vĩ mô | Macro/Commodity | Dùng section "Bộ lọc vĩ mô" |
| D + A/B | Kết hợp | Lọc event/macro TRƯỚC → áp fundamental/technical SAU |
| C | Mơ hồ | Hỏi lại với 7 lựa chọn (đã thêm Event + Macro) |

### Bước 3: Xác định ngành + áp bộ chỉ số đặc thù nếu cần

### Bước 4: Load dữ liệu, kiểm tra cột + chạy Data Quality Rules

### Bước 5: Chạy lọc + hiển thị Compact Funnel

Funnel dạng compact — chỉ 3 dòng:
```
📊 1,652 mã → [5 bộ lọc] → 15 mã ✅
   Bước loại nhiều nhất: "P/E < 12" (loại 68% mã)
   Chi tiết funnel: [xem đầy đủ]
```

### Bước 6: Trình bày kết quả với Smart Sort + Benchmark + Risk Flags

> **SMART SORT** — tự động sort theo tiêu chí ưu tiên:

| Loại yêu cầu | Sort theo | Hướng |
|---------------|-----------|-------|
| Lọc P/E thấp | P/E | ↑ tăng dần |
| Lọc cổ tức | Dividend yield | ↓ giảm dần |
| Lọc tăng trưởng | EPS growth | ↓ giảm dần |
| Lọc ROE cao | ROE | ↓ giảm dần |
| Lọc RSI quá bán | RSI | ↑ tăng dần |
| Lọc momentum | Composite (RSI + MACD + trend) | ↓ giảm dần |
| Multi-criteria | Composite rank trung bình | ↑ tăng dần |
| **[v4] Lọc sự kiện** | **Ngày sự kiện (gần nhất trước)** | **↑ tăng dần** |
| **[v4] Lọc vĩ mô** | **% doanh thu từ mảng hưởng lợi** | **↓ giảm dần** |
| **[v4] Kết hợp event + fund** | **Số catalyst × chất lượng tài chính** | **↓ giảm dần** |

> **BENCHMARK NGÀNH** — dòng TB ngành cuối bảng (giữ từ v3)

> **[v4] RISK FLAGS** — cột flag hoặc ghi chú cuối bảng cho mã bất thường

### Bước 7: Xử lý kết quả đặc biệt

**Quá nhiều (>30):** Hiện top 20 + "Còn X mã khác. Thêm bộ lọc để thu hẹp?"

**Kết quả rỗng:** Relaxation Waterfall:
```
⚠️ 0 kết quả. Nới lỏng từng tiêu chí:
• Bỏ "Div > 10%" → 5 mã | Nới "ROE > 30%" → ">20%" → 8 mã | Nới "P/E < 3" → "<8" → 12 mã
→ Đề xuất: nới Dividend yield từ 10% xuống 5%.
```

**[v4] Thiếu dữ liệu sự kiện/vĩ mô:** Ghi gọn 1-2 dòng + hướng dẫn nguồn bổ sung

### Bước 8: Gợi ý bước tiếp theo (cross-skill)

> **[v4 MỚI]** Sau kết quả, luôn gợi ý:

```
💡 Bước tiếp:
• Phân tích sâu: "Phân tích [MÃ]" → chạy skill stock-analysis
• Định giá: "Định giá [MÃ]" → chạy skill stock-valuation
• So sánh: "So sánh [MÃ1] và [MÃ2]" → chạy skill stock-evaluation
• Thêm vào danh mục: "Phân bổ danh mục" → chạy skill portfolio-allocation
```

### Bước 9: Xuất kết quả
Lưu CSV/Excel nếu yêu cầu. Nhắc: sàng lọc chỉ là bước đầu.

---

## Ví dụ đầu vào / đầu ra hoàn chỉnh

### Ví dụ 1 — Nhóm A (Rõ ràng) + Data Quality Flags

**Input:** "Lọc cổ phiếu P/E < 10, ROE > 15% trên HOSE"

**Output:**
```
📊 416 mã HOSE → [P/E 0-10, ROE > 15%, loại mã cảnh báo/kiểm soát] → 7 mã ✅
   Bước loại nhiều nhất: "P/E < 10" (loại 76%)

| #  | Mã  | Tên         | Ngành     | Giá    | P/E  | ROE   | D/E  | Flag |
|----|-----|-------------|-----------|--------|------|-------|------|------|
| 1  | ACB | ACB         | Ngân hàng | 25,800 | 7.8  | 20.5% | —*   | ✅ |
| 2  | HPG | Hòa Phát    | Thép      | 26,800 | 9.8  | 14.2% | 0.85 | ✅ |
| 3  | VHM | Vinhomes    | BĐS       | 42,500 | 8.5  | 18.5% | 1.20 | ✅ |
| 4  | XYZ | Công ty XYZ | Hóa chất  | 5,200  | 2.1  | 52.3% | 0.40 | ⚡ P/E + ROE đột biến |
|----|-----|-------------|-----------|--------|------|-------|------|------|
| 📊 | TB  | HOSE        |           |        | 15.8 | 12.3% | 0.95 |      |

*D/E không áp dụng cho ngành ngân hàng

⚠️ Ghi chú flag:
• XYZ: P/E = 2.1 + ROE = 52.3% — có thể do lợi nhuận đột biến 1 lần. Kiểm tra BCTC trước khi quyết định.

📁 Lưu file? Nói "xuất CSV" hoặc "xuất Excel".
💡 Phân tích sâu? Nói "Phân tích HPG" hoặc "So sánh ACB và VHM".
⚠️ Đây là kết quả sàng lọc, không phải khuyến nghị đầu tư.
```

### Ví dụ 2 — Nhóm B (NLP) — giữ từ v3

**Input:** "Tìm cổ phiếu công nghệ tăng trưởng mạnh, chưa quá đắt"

**Output:**
```
💡 Dịch: "công nghệ" → CNTT/Viễn thông | "tăng trưởng mạnh" → EPS growth > 20% | "chưa quá đắt" → P/E < 18

📊 32 mã CNTT → [EPS growth > 20%, P/E < 18] → 3 mã ✅
   Bước loại nhiều nhất: "EPS growth > 20%" (loại 72%)

| #  | Mã  | Tên     | Giá     | P/E  | EPS Gr | ROE   | Vol TB  | Flag |
|----|-----|---------|---------|------|--------|-------|---------|------|
| 1  | CMG | CMC     | 58,200  | 14.5 | +32.1% | 18.2% | 850K    | ✅ |
| 2  | FOX | FPT Tel | 82,100  | 16.2 | +25.8% | 22.5% | 1.2M    | ✅ |
| 3  | ELC | Elcom   | 35,800  | 11.8 | +21.5% | 16.8% | 320K    | ✅ |
|----|-----|---------|---------|------|--------|-------|---------|------|
| 📊 | TB ngành CNTT    |         | 22.5 | +15.3% | 14.8% |         |      |

↩️ Điều chỉnh? Ví dụ: nới P/E < 25, thêm thanh khoản > 500K/phiên.
💡 Phân tích sâu? "Phân tích CMG" hoặc "Định giá FPT Telecom".
```

### Ví dụ 3 — Nhóm C (Mơ hồ) — v4 updated

**Input:** "Cổ phiếu nào đáng mua?"

**Output:**
```
Tôi cần hiểu thêm về phong cách đầu tư của bạn. Bạn quan tâm nhất đến:
1. 💎 Giá trị — P/E thấp, tài chính vững, cổ tức ổn
2. 🚀 Tăng trưởng — Doanh nghiệp mở rộng nhanh, EPS tăng mạnh
3. 💰 Cổ tức — Thu nhập thụ động > 5%/năm
4. ⚡ Momentum — Đang có đà tăng giá, volume tốt
5. 🔄 Phục hồi — Cổ phiếu chất lượng đang bị bán quá mức
6. 📰 Sự kiện — Có catalyst tích cực: insider mua, quỹ mua, đối tác chiến lược, cổ tức đặc biệt
7. 🌍 Vĩ mô — Hưởng lợi từ xu hướng kinh tế, giá hàng hóa, chính sách

Chọn số (1-7) hoặc mô tả thêm, tôi sẽ thiết kế bộ lọc phù hợp.
```

### Ví dụ 4 — [v4 MỚI] Nhóm D — Sự kiện (Insider + Đối tác chiến lược)

**Input:** "Tìm cổ phiếu có insider đang mua ròng mạnh, hoặc có đối tác chiến lược lớn vừa góp vốn"

**Output:**
```
📰 Lọc theo sự kiện: Insider buying + Đối tác chiến lược (30 ngày gần nhất)

📊 1,652 mã → [có sự kiện insider/đối tác 30D] → 12 mã ✅

👔 INSIDER MUA RÒNG:
| #  | Mã  | Tên      | Người mua           | SL mua  | % vốn | Giá mua  | P/E  | ROE   |
|----|-----|----------|---------------------|---------|-------|----------|------|-------|
| 1  | MWG | MWG      | Chủ tịch HĐQT       | 500,000 | 0.3%  | 52,000   | 12.8 | 18.2% |
| 2  | FPT | FPT Corp | CEO                  | 200,000 | 0.1%  | 128,000  | 22.1 | 28.5% |
| 3  | DGC | Hóa chất | Phó CT + 2 TV HĐQT  | 800,000 | 0.5%  | 85,000   | 8.5  | 25.8% |

🤝 ĐỐI TÁC CHIẾN LƯỢC GÓP VỐN:
| #  | Mã  | Tên      | Đối tác            | % sở hữu | Loại GD             | P/E  | ROE   |
|----|-----|----------|--------------------|-----------|---------------------|------|-------|
| 1  | TCB | Techcom  | MUFG (Nhật)        | 19.9%     | Mua cổ phần         | 10.2 | 18.5% |
| 2  | OCB | OCB      | Aozora Bank (Nhật) | 15.0%     | Phát hành riêng lẻ  | 7.8  | 14.2% |

💡 Ý nghĩa:
• Insider mua ròng = ban lãnh đạo bỏ tiền túi → tin vào triển vọng DN (đặc biệt khi SL lớn + nhiều người mua)
• Đối tác chiến lược lớn = nâng uy tín + chuyển giao công nghệ + mở rộng thị trường

⚠️ Lưu ý: Insider mua lô nhỏ (< 0.05% vốn) có thể chỉ mang tính hình thức. Ưu tiên mã có SL mua > 0.1% vốn.
💡 Phân tích sâu? "Phân tích DGC" hoặc "Định giá TCB".
```

### Ví dụ 5 — [v4 MỚI] Nhóm D — Vĩ mô / Hàng hóa

**Input:** "Giá thép thế giới đang tăng mạnh, cổ phiếu nào hưởng lợi?"

**Output:**
```
🌍 Catalyst: "Giá thép thế giới tăng mạnh"

📊 Chuỗi tác động:
Giá HRC tăng → Biên LN sản xuất thép mở rộng (giá bán tăng nhanh hơn giá vốn)
             → EPS cải thiện → P/E forward hấp dẫn hơn

🟢 HƯỞNG LỢI TRỰC TIẾP — Sản xuất thép (5 mã):
| #  | Mã  | Tên        | % DT thép | P/E  | EPS Gr | ROE   | Đặc điểm                    |
|----|-----|------------|-----------|------|--------|-------|------------------------------|
| 1  | HPG | Hòa Phát   | 85%       | 9.8  | +35%   | 14.2% | Tự chủ quặng → hưởng lợi lớn nhất |
| 2  | NKG | Nam Kim    | 95%       | 6.5  | +42%   | 10.8% | XK 40% → hưởng lợi giá quốc tế |
| 3  | HSG | Hoa Sen    | 90%       | 7.2  | +28%   | 11.5% | Đòn bẩy cao → lãi/lỗ khuếch đại |
| 4  | TLH | Thép Tiến Lên | 80%    | 8.1  | +20%   | 9.5%  | Quy mô nhỏ hơn               |
| 5  | POM | Pomina     | 88%       | 11.2 | +15%   | 7.2%  | 🔸 Đòn bẩy rất cao (D/E=2.8) |
|----|-----|------------|-----------|------|--------|-------|------------------------------|
| 📊 | TB ngành Thép    |           | 11.2 | +22%   | 12.5% |                              |

🟡 HƯỞNG LỢI GIÁN TIẾP:
• Khoáng sản (KSB): cung cấp nguyên liệu → nhu cầu tăng theo

🔴 BẤT LỢI — chi phí thép tăng làm giảm biên LN:
• Xây dựng (CTD, HBC): chi phí vật liệu ~30-40% giá trị công trình
• Ô tô (VEA): chi phí sản xuất tăng

⚠️ Rủi ro: Giá thép tăng có thể chỉ là chu kỳ ngắn hạn. Cần theo dõi:
   • Giá HRC Trung Quốc (Shanghai Futures)
   • Sản lượng thép Trung Quốc (chiếm 55% sản lượng thế giới)
   • Chính sách cắt giảm sản lượng / thuế XK Trung Quốc
   • Nhu cầu bất động sản Trung Quốc

💡 Phân tích sâu? "Phân tích HPG" hoặc "So sánh HPG và HSG".
⚠️ Đây là phân tích cơ hội, không phải khuyến nghị đầu tư.
```

### Ví dụ 6 — [v4 MỚI] Nhóm D — Kết hợp Vĩ mô + Fundamental

**Input:** "Chính phủ vừa tăng đầu tư công mạnh, cổ phiếu nào hưởng lợi? Lọc kèm ROE > 12% và thanh khoản tốt"

**Output:**
```
🌍 Catalyst: "Đầu tư công tăng mạnh"

📊 Chuỗi tác động:
Giải ngân ĐTC tăng → Xây dựng/hạ tầng (backlog tăng)
                    → Thép, xi măng, VLXD (nhu cầu tăng)
                    → Logistics (khối lượng vận chuyển tăng)
                    → Ngân hàng (giải ngân vốn cho dự án)
                    → BĐS (hạ tầng kết nối → giá trị quỹ đất tăng)

📊 Hưởng lợi 85 mã → [ROE > 12%, Vol > 500K] → 18 mã ✅
   Bước loại nhiều nhất: "Vol > 500K" (loại 55%)

🟢 HƯỞNG LỢI TRỰC TIẾP — Xây dựng & hạ tầng:
| #  | Mã  | Tên    | P/E  | ROE   | EPS Gr | Vol TB | Flag |
|----|-----|--------|------|-------|--------|--------|------|
| 1  | CTD | Cotec  | 11.5 | 15.8% | +18%   | 1.2M   | ✅ |
| 2  | HHV | HHV    | 9.2  | 14.2% | +25%   | 680K   | ✅ |
| 3  | FCN | FECON  | 8.8  | 12.5% | +30%   | 520K   | ✅ |

🟡 HƯỞNG LỢI GIÁN TIẾP — Vật liệu + Logistics:
| #  | Mã  | Tên       | Ngành    | P/E  | ROE   | Vol TB | Lý do hưởng lợi |
|----|-----|-----------|----------|------|-------|--------|------------------|
| 4  | HPG | Hòa Phát  | Thép     | 9.8  | 14.2% | 8.5M   | Nhu cầu thép xây dựng |
| 5  | HT1 | Xi măng   | Xi măng  | 10.5 | 13.8% | 1.1M   | Nhu cầu xi măng |
| 6  | GMD | Gemadept  | Logistics| 15.2 | 18.5% | 2.3M   | Vận chuyển hàng tăng |

⏰ Timeline: ĐTC thường tác động rõ nhất từ quý có giải ngân cao → theo dõi số liệu giải ngân hàng tháng (Bộ Tài chính).

💡 Phân tích sâu? "Phân tích CTD" hoặc "Phân bổ danh mục đầu tư công".
⚠️ Đây là phân tích cơ hội, không phải khuyến nghị đầu tư.
```

### Ví dụ 7 — [v4 MỚI] Kết hợp Event + Fundamental

**Input:** "Tìm cổ phiếu vừa có sự kiện tích cực (insider mua, quỹ mua) VÀ ROE > 15%"

**Output:**
```
📰+📊 Kết hợp: Sự kiện tích cực (30D) + ROE > 15%

📊 1,652 mã → [có event 30D] → 45 mã → [ROE > 15%] → 8 mã ✅

| #  | Mã  | Tên      | Catalyst                        | P/E  | ROE   | EPS Gr | Flag |
|----|-----|----------|--------------------------------|------|-------|--------|------|
| 1  | DGC | Hóa chất | 👔 3 insider mua + 📈 Quỹ mua  | 8.5  | 25.8% | +32%   | ✅ 2 catalyst |
| 2  | FPT | FPT      | 👔 CEO mua 200K cp              | 22.1 | 28.5% | +20%   | ✅ |
| 3  | TCB | Techcom  | 🤝 MUFG góp vốn 19.9%          | 10.2 | 18.5% | +15%   | ✅ |
| 4  | MWG | MWG      | 👔 Chủ tịch mua 500K cp         | 12.8 | 18.2% | +12%   | ✅ |
|----|-----|----------|--------------------------------|------|-------|--------|------|
| 📊 | TB  | Thị trường |                               | 15.8 | 12.3% | +10%   |      |

💡 DGC nổi bật: 2 catalyst cùng lúc (insider + quỹ) + ROE 25.8% + P/E chỉ 8.5 → xem xét phân tích sâu.

⚠️ Sự kiện tích cực + tài chính vững = xác suất thành công cao hơn, nhưng KHÔNG đảm bảo giá tăng.
💡 Phân tích sâu? "Phân tích DGC" hoặc "Định giá TCB".
```

### Ví dụ 8 — [v4 MỚI] NLP "tin tốt" (Nhóm B)

**Input:** "Cổ phiếu nào đang có tin tốt gần đây?"

**Output:**
```
💡 Dịch "tin tốt" → quét 5 loại sự kiện tích cực (30 ngày gần nhất):

📰 Tổng hợp sự kiện tích cực — 30 ngày qua:

👔 Insider mua ròng: 15 mã
| Mã  | Người mua        | SL      | P/E  | ROE   |
|-----|------------------|---------|------|-------|
| DGC | PCT + 2 TV HĐQT  | 800K cp | 8.5  | 25.8% |
| MWG | Chủ tịch         | 500K cp | 12.8 | 18.2% |
| ... (top 5)                                      |

🤝 Đối tác chiến lược: 3 mã
| Mã  | Đối tác         | % sở hữu | P/E  |
|-----|-----------------|-----------|------|
| TCB | MUFG (Nhật)     | 19.9%     | 10.2 |
| ... |                 |           |      |

📈 Quỹ mua ròng mạnh: 20 mã (top 5)
💰 Cổ tức cao sắp chốt: 8 mã (top 5)
🏆 KQKD vượt kỳ vọng: 12 mã (top 5)

↩️ Muốn xem chi tiết loại nào? Ví dụ: "xem đầy đủ insider mua" hoặc "lọc thêm ROE > 15%".
⚠️ Đây là tổng hợp sự kiện, không phải khuyến nghị đầu tư.
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
| **📰 Event-driven** | **Insider mua ròng + ROE > 12% + Vol > 200K** |
| **🌍 Macro-play** | **Ngành hưởng lợi từ catalyst cụ thể + P/E < TB ngành + EPS growth > 0%** |

---

## Lưu ý

- Ghi rõ nguồn + thời điểm dữ liệu
- KHÔNG khuyến nghị mua/bán — chỉ cung cấp dữ liệu khách quan và phân tích cơ hội
- P/E < 0 = lỗ → tự loại khi lọc "P/E thấp"
- So sánh xuyên ngành cần ghi chú: "P/E ngân hàng thường < P/E công nghệ"
- Kết quả sàng lọc là bước đầu, cần phân tích sâu hơn
- **[v4] Sự kiện tích cực không đảm bảo giá tăng** — luôn kết hợp với phân tích tài chính
- **[v4] Phân tích vĩ mô/hàng hóa có tính thời điểm** — catalyst có thể đã phản ánh vào giá, cần kiểm tra giá hiện tại vs giá trước tin
- **[v4] Luôn ghi chú rủi ro** cho mỗi catalyst: "Giá thép tăng có thể chỉ ngắn hạn", "ĐTC giải ngân chậm hơn kế hoạch"
