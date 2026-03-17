---
description: "Format bất kỳ nội dung phân tích tài chính nào theo chuẩn Simplize — nhận kết quả thô từ skill khác hoặc từ user, trình bày lại thành báo cáo chuyên nghiệp, dễ đọc, có câu chuyện"
argument-hint: "[nội dung cần format hoặc chủ đề phân tích]"
---

# Simplize Formatter — Format mọi phân tích tài chính

Skill này là **bước cuối cùng** trong pipeline: nhận output thô (từ skill phân tích khác, từ data user cung cấp, hoặc từ conversation context) và format lại theo phong cách Simplize.

**KHÔNG** phân tích thêm, không bịa số liệu, không tìm kiếm web. Chỉ **format lại** nội dung đã có.

---

## WORKFLOW

### Step 1: Nhận diện loại nội dung

Đọc input và xác định thuộc loại nào:

| Loại | Dấu hiệu nhận diện | Ví dụ |
| :--- | :--- | :--- |
| **Cổ phiếu đơn lẻ** | Có mã ticker, KQKD, định giá | "Phân tích FPT Q4/2025" |
| **So sánh cổ phiếu** | 2+ ticker, bảng so sánh | "So sánh VNM vs MCH vs QNS" |
| **Phân tích ngành** | Tên ngành, nhiều công ty, xu hướng | "Ngành thép Việt Nam 2025" |
| **Phân tích vĩ mô** | GDP, lãi suất, CPI, tỷ giá, chính sách | "Triển vọng kinh tế VN 2026" |
| **Phân tích chuyên đề** | Xu hướng, theme đầu tư | "AI và bán dẫn tại VN" |
| **Earnings update** | Kết quả quý, beat/miss, guidance | "Earnings Q4 FPT" |
| **Khác** | Không thuộc các loại trên | Format linh hoạt |

### Step 2: Chọn cấu trúc phù hợp

Dựa vào loại nội dung → chọn 1 trong các cấu trúc ở mục **CẤU TRÚC THEO LOẠI** bên dưới.

### Step 3: Viết theo Simplize Style Guide

Áp dụng **QUY TẮC GOVALUE** (cuối file) cho mọi loại nội dung.

### Step 4: Output

- Nếu là báo cáo dài (>500 từ) → lưu file: `docs/{loại}/{tên-file}-{YYYYMMDD}.md`
- Nếu là trả lời ngắn → output trực tiếp trong chat, vẫn giữ style

---

## CẤU TRÚC THEO LOẠI

---

### LOẠI 1: CỔ PHIẾU ĐƠN LẺ

→ Dùng đúng format `/vn-stock-report`. Gồm 7 phần:
1. YAML Frontmatter
2. Tiêu đề + Recommendation table
3. Bối cảnh / câu chuyện mở đầu
4. Thị trường đang nghĩ gì? (nếu có data sell-side)
5. Phân tích kinh doanh (linh hoạt theo 4 kiểu)
6. Định giá và khuyến nghị (3 kịch bản)
7. Bottom Line

---

### LOẠI 2: SO SÁNH CỔ PHIẾU

````markdown
# {Chủ đề so sánh}: {Insight chính} ({DD/MM/YYYY})

## Tại sao so sánh này?

{1-2 đoạn: bối cảnh tại sao cần so sánh, câu hỏi đầu tư cần trả lời}

## Bức tranh toàn cảnh

| Chỉ số | {Ticker 1} | {Ticker 2} | {Ticker 3} | Trung bình ngành |
| :--- | ---: | ---: | ---: | ---: |
| Vốn hóa (tỷ đồng) | **{val}** | {val} | {val} | *{val}* |
| Doanh thu LTM | **{val}** | {val} | {val} | *{val}* |
| Tăng trưởng DT (yoy) | **{val}** | {val} | {val} | *{val}* |
| LNST LTM | {val} | {val} | {val} | *{val}* |
| Biên LN ròng | {val} | {val} | {val} | *{val}* |
| ROE | {val} | {val} | {val} | *{val}* |
| P/E | {val} | {val} | {val} | *{val}* |
| P/B | {val} | {val} | {val} | *{val}* |

{Nhận xét bảng: ai dẫn đầu, ai rẻ nhất, ai tăng trưởng tốt nhất}

## {Góc so sánh 1}: {Tên — VD: Tăng trưởng doanh thu}

{Narrative phân tích — tại sao khác biệt, driver chính, bảng chi tiết nếu cần}

## {Góc so sánh 2}: {Tên — VD: Hiệu quả sinh lời}

{Tương tự}

## {Góc so sánh 3}: {Tên — VD: Định giá}

{Tương tự}

## Bottom Line

{So sánh tổng kết — ai hấp dẫn hơn ở góc nào, phù hợp nhà đầu tư nào}

**Lưu ý: Đây là phân tích so sánh, không phải khuyến nghị đầu tư.**
````

Mẫu thực tế (trích ý từ HP-07 — VNM vs MCH vs QNS):
> **Tại sao so sánh?** Cả 3 đều thuộc ngành FMCG/thực phẩm, cùng hưởng lợi từ xu hướng tiêu dùng phục hồi 2025. Nhà đầu tư cần biết ai tăng trưởng tốt nhất, ai rẻ nhất.
>
> | Chỉ số | **VNM** | MCH | QNS |
> | :--- | ---: | ---: | ---: |
> | LNST Q4/2025 (tỷ đ) | **2,840** | 2,079 | 598 |
> | Tăng trưởng QoQ | +12.4% | +24.1% | **+57.0%** |
> | Biên LN ròng | **14.8%** | 12.1% | 10.3% |
> | ROE (LTM) | **26.6%** | 22.1% | 18.4% |
>
> **Nhận xét:** QNS bứt phá mạnh nhất (+57% QoQ) nhưng quy mô nhỏ nhất. VNM vẫn dẫn đầu về biên lợi nhuận và ROE. MCH cân bằng giữa tăng trưởng và quy mô.

---

### LOẠI 3: PHÂN TÍCH NGÀNH

````markdown
# Ngành {tên ngành}: {Insight/thesis chính} ({DD/MM/YYYY})

## Bức tranh ngành

{Tổng quan ngành — quy mô, tốc độ tăng trưởng, vị thế trong nền kinh tế.
2-3 đoạn narrative, in đậm số liệu. Có thể mở bằng câu hỏi hoặc xu hướng gây chú ý.}

| Chỉ số ngành | {Năm N-2} | {Năm N-1} | {Năm N} | CAGR |
| :--- | ---: | ---: | ---: | ---: |
| Quy mô (tỷ đồng) | {val} | {val} | **{val}** | {val} |
| Tăng trưởng (%) | {val} | {val} | **{val}** | |
| {Chỉ số đặc thù} | {val} | {val} | **{val}** | |

## Động lực tăng trưởng

### {Driver 1}: {Mô tả}

{Phân tích — tại sao đây là driver, số liệu minh họa, so sánh quốc tế nếu có}

### {Driver 2}: {Mô tả}

{Tương tự}

## Cạnh tranh và vị thế

| Doanh nghiệp | Thị phần (%) | Doanh thu (tỷ đ) | Biên LN | ROE | P/E |
| :--- | ---: | ---: | ---: | ---: | ---: |
| **{Top 1}** | **{val}** | **{val}** | **{val}** | **{val}** | **{val}** |
| {Top 2} | {val} | {val} | {val} | {val} | {val} |
| {Top 3} | {val} | {val} | {val} | {val} | {val} |
| *Trung bình ngành* | | *{val}* | *{val}* | *{val}* | *{val}* |

{Nhận xét: ai dẫn đầu, ai đang lên, ai gặp khó}

## Rủi ro ngành

- {Rủi ro 1 — kèm đánh giá mức độ: cao/trung bình/thấp}
- {Rủi ro 2}
- {Rủi ro 3}

## Triển vọng và cơ hội đầu tư

{Dự báo ngành — tham chiếu nguồn cụ thể nếu có.
Không tự dự đoán — dùng "theo BMI Research...", "theo VDSC..."}

**Cổ phiếu đáng theo dõi trong ngành:** {Ticker 1}, {Ticker 2}, {Ticker 3}
(Đây là danh sách theo dõi, không phải khuyến nghị đầu tư.)

## Bottom Line

{2-3 câu tóm tắt: ngành đang ở đâu trong chu kỳ, điểm nhấn chính, cơ hội/rủi ro lớn nhất}
````

Mẫu thực tế (trích ý từ phân tích ngành thép):
> **Ngành thép Việt Nam** đạt quy mô sản lượng **~30 triệu tấn/năm** (2024), tăng trưởng CAGR **8.5%** giai đoạn 2019-2024. Tuy nhiên, ngành đang đối mặt 3 thách thức lớn:
>
> | Doanh nghiệp | Thị phần (%) | Công suất (triệu tấn) | Biên LN gộp | ROE |
> | :--- | ---: | ---: | ---: | ---: |
> | **HPG** | **36.2** | **14.0** | **18.4%** | **22.1%** |
> | NKG | 8.5 | 3.2 | 6.2% | 12.3% |
> | HSG | 7.8 | 2.8 | 5.1% | 8.7% |
>
> HPG thống trị với **36.2% thị phần**, biên lợi nhuận gộp gấp 3 lần đối thủ nhờ mô hình tích hợp dọc từ quặng sắt → thép xây dựng.

---

### LOẠI 4: PHÂN TÍCH VĨ MÔ

````markdown
# {Chủ đề vĩ mô}: {Insight chính} ({DD/MM/YYYY})

## Bức tranh hiện tại

{Tổng quan tình hình — GDP, CPI, lãi suất, tỷ giá, chính sách.
Mở bằng điểm nhấn lớn nhất hoặc thay đổi đáng chú ý nhất.}

| Chỉ số | {Quý/Năm N-2} | {Quý/Năm N-1} | {Quý/Năm N} | Xu hướng |
| :--- | ---: | ---: | ---: | :--- |
| GDP (% yoy) | {val} | {val} | **{val}** | {↑/↓/→} |
| CPI (% yoy) | {val} | {val} | **{val}** | {↑/↓/→} |
| Lãi suất điều hành | {val} | {val} | **{val}** | {↑/↓/→} |
| VND/USD | {val} | {val} | **{val}** | {↑/↓/→} |
| Tín dụng (% ytd) | {val} | {val} | **{val}** | {↑/↓/→} |

## {Chủ đề phân tích 1}: {VD: Chính sách tiền tệ}

{Narrative — phân tích sâu, bối cảnh, so sánh khu vực nếu có, bảng minh họa}

## {Chủ đề phân tích 2}: {VD: Dòng vốn FDI}

{Tương tự}

## {Chủ đề phân tích 3}: {VD: Rủi ro bên ngoài}

{Tương tự}

## Tác động đến thị trường chứng khoán

{Kết nối phân tích vĩ mô → ảnh hưởng cụ thể lên TTCK và các nhóm ngành.
Đây là phần nhà đầu tư quan tâm nhất.}

| Nhóm ngành | Tác động | Lý do |
| :--- | :--- | :--- |
| Ngân hàng | {Tích cực/Tiêu cực/Trung lập} | {Giải thích ngắn} |
| Bất động sản | {val} | {val} |
| Xuất khẩu | {val} | {val} |

## Bottom Line

{2-3 câu: kết luận vĩ mô, rủi ro lớn nhất, cơ hội lớn nhất cho nhà đầu tư}
````

Mẫu thực tế (trích ý):
> GDP quý 1/2026 ước đạt **6.8% yoy**, vượt kỳ vọng **6.2%** của consensus. Động lực chính đến từ sản xuất công nghiệp (+9.2%) và FDI giải ngân đạt **kỷ lục 5.8 tỷ USD** (+18% yoy).
>
> | Chỉ số | Q1/2025 | Q4/2025 | Q1/2026 | Xu hướng |
> | :--- | ---: | ---: | ---: | :--- |
> | GDP (% yoy) | 6.1% | 7.6% | **6.8%** | → ổn định |
> | CPI (% yoy) | 4.2% | 3.8% | **3.5%** | ↓ giảm nhiệt |
> | Lãi suất OMO | 4.5% | 4.5% | **4.5%** | → giữ nguyên |
>
> **Tác động TTCK:** Nhóm hưởng lợi rõ nhất là **xuất khẩu** (đơn hàng tăng) và **ngân hàng** (tín dụng tăng trưởng tốt). Nhóm BĐS chưa hưởng lợi nhiều do tín dụng bất động sản vẫn bị kiểm soát.

---

### LOẠI 5: PHÂN TÍCH CHUYÊN ĐỀ (THEMATIC)

````markdown
# {Chuyên đề}: {Insight chính} ({DD/MM/YYYY})

## Câu chuyện lớn

{Mở bằng xu hướng/sự kiện lớn — tại sao chuyên đề này quan trọng bây giờ.
Giống cách VRE mở bằng "Thương mại điện tử có giết chết bán lẻ?" hoặc
FPT mở bằng làn sóng chuyển đổi số. Kích thích tư duy.}

## {Phân tích góc 1}: {VD: Quy mô thị trường}

{Narrative + bảng — xu hướng toàn cầu, Việt Nam đang ở đâu, so sánh khu vực}

| Quốc gia | {Chỉ số 1} | {Chỉ số 2} | {Chỉ số 3} |
| :--- | ---: | ---: | ---: |
| Việt Nam | {val} | {val} | {val} |
| Thái Lan | {val} | {val} | {val} |
| Indonesia | {val} | {val} | {val} |

## {Phân tích góc 2}: {VD: Chuỗi giá trị}

{Ai hưởng lợi, ai bị disrupted, chuỗi cung ứng thay đổi thế nào}

## {Phân tích góc 3}: {VD: Doanh nghiệp Việt Nam trong cuộc chơi}

{Các công ty niêm yết liên quan — phân tích vị thế, không khuyến nghị mua/bán}

| Công ty | Ticker | Mức độ liên quan | Lý do |
| :--- | :--- | :--- | :--- |
| {Tên} | {Mã} | Cao/Trung bình | {Giải thích} |

## Rủi ro và hạn chế

{Những gì có thể đi sai — regulatory, cạnh tranh, timeline kéo dài hơn dự kiến}

## Bottom Line

{2-3 câu: chuyên đề này có phải cơ hội thật không, timeline, và cách nhà đầu tư nên tiếp cận}

**Lưu ý: Đây là phân tích xu hướng, không phải khuyến nghị đầu tư.**
````

Mẫu thực tế (trích ý — AI và bán dẫn):
> Khi Nvidia đạt vốn hóa **3 nghìn tỷ USD** và OpenAI định giá **150 tỷ USD**, câu hỏi tất yếu: Việt Nam có ai hưởng lợi từ làn sóng AI/bán dẫn?
>
> | Công ty | Ticker | Mức độ liên quan | Lý do |
> | :--- | :--- | :--- | :--- |
> | FPT | FPT | **Cao** | Mảng AI đạt 100 triệu USD doanh thu, đối tác Nvidia |
> | CMC | CMG | Trung bình | Cloud & data center, nhưng quy mô còn nhỏ |
> | Viettel | Chưa NY | Cao | Chip 5G tự phát triển, nhưng chưa niêm yết |
>
> Simplize cho rằng **FPT là cổ phiếu duy nhất trên sàn** có exposure trực tiếp vào AI với quy mô đáng kể. Tuy nhiên, P/E **20x** đã phản ánh phần lớn kỳ vọng.

---

### LOẠI 6: EARNINGS UPDATE (format nhanh cho output từ /er-earnings)

````markdown
# {TICKER} Q{X}/{Năm}: {Beat/Miss/In-line} — {Insight 1 dòng} ({DD/MM/YYYY})

## Kết quả nhanh

| Chỉ số | Thực tế | Consensus | Chênh lệch | YoY |
| :--- | ---: | ---: | ---: | ---: |
| Doanh thu (tỷ đ) | **{val}** | {val} | {+/-X%} | {+/-X%} |
| LNST (tỷ đ) | **{val}** | {val} | {+/-X%} | {+/-X%} |
| EPS (đ/CP) | **{val}** | {val} | {+/-X%} | {+/-X%} |
| Biên LN ròng | **{val}** | {val} | {+/-X bps} | |

**Verdict: {BEAT / MISS / IN-LINE}** — {1 câu giải thích ngắn}

## Điểm nhấn

- **{Điểm 1}:** {Chi tiết — số liệu in đậm}
- **{Điểm 2}:** {Chi tiết}
- **{Điểm 3}:** {Chi tiết}

## Phân tích mảng kinh doanh

| Mảng | Doanh thu | Tỷ trọng | YoY | Nhận xét |
| :--- | ---: | ---: | ---: | :--- |
| {Mảng 1} | **{val}** | {X}% | {+/-X%} | {1 câu} |
| {Mảng 2} | {val} | {X}% | {+/-X%} | {1 câu} |

## Thay đổi ước tính (nếu có)

| Chỉ số | Ước tính cũ | Ước tính mới | Thay đổi |
| :--- | ---: | ---: | ---: |
| LNST {năm} | {val} | **{val}** | {+/-X%} |
| EPS {năm} | {val} | **{val}** | {+/-X%} |

## Bottom Line

{2-3 câu: kết quả tốt/xấu hơn kỳ vọng ở điểm nào, outlook thay đổi gì}
````

---

### LOẠI 7: TRẢ LỜI NGẮN (câu hỏi đơn giản, không cần báo cáo dài)

Khi user hỏi câu ngắn (VD: "PE của FPT?", "ROE ngành ngân hàng?"), vẫn giữ Simplize style nhưng format gọn:

````markdown
**{Câu trả lời trực tiếp — in đậm, có số liệu}**

{1-2 câu context/so sánh nếu hữu ích}

| {Bảng nếu cần} | {Cột} |
| :--- | ---: |
| {data} | {val} |

*Nguồn: {nguồn dữ liệu, ngày}*
````

Mẫu:
> **P/E trailing của FPT hiện tại: 20.3x** (tại ngày 15/03/2026)
>
> So với:
> - P/E trung bình 5 năm FPT: **15.8x** → đang cao hơn +28%
> - P/E trung bình ngành IT Việt Nam: **14.2x**
> - P/E trung bình ngành IT khu vực ĐNA: **18.5x**
>
> Simplize nhận xét: Mức premium phản ánh kỳ vọng tăng trưởng từ mảng AI/DX.
>
> *Nguồn: FiinPro, ngày 15/03/2026*

---

## QUY TẮC GOVALUE (áp dụng cho MỌI loại nội dung)

### Giọng văn
- **Tiếng Việt** chủ đạo, giữ nguyên thuật ngữ tài chính: PE, PB, ROE, CAGR, EPS, LNST, NIM, CIR, NPL...
- Chuyên nghiệp nhưng dễ hiểu — giải thích logic, không chỉ liệt kê
- Có quan điểm rõ ràng: "Simplize cho rằng...", "Simplize ước tính...", "Simplize tin rằng..."
- **KHÔNG** dùng giọng quảng cáo, không dùng emoji, không dùng "chúng tôi" hay "tôi"

### Số liệu
- **In đậm** mọi số liệu quan trọng
- Đơn vị rõ ràng: "tỷ đồng", "đồng/CP", "USD/thùng", "triệu tấn"
- Tỷ lệ % luôn kèm context: yoy, QoQ, CAGR, vs ngành, vs lịch sử
- Giá cổ phiếu: **17,500** hoặc **17,x** (xấp xỉ)

### Bảng Markdown
- Ít nhất **3 bảng** cho báo cáo dài, **1 bảng** cho trả lời ngắn
- Align: `:---` text trái, `---:` số phải
- **In đậm** dòng công ty/chỉ số chính đang phân tích
- *In nghiêng* dòng trung bình ngành/trung bình

### Narrative + Bảng
- Xen kẽ narrative (giải thích TẠI SAO) và bảng (minh họa)
- KHÔNG liệt kê bảng liên tục không giải thích
- KHÔNG liệt kê số liệu không có narrative
- Mỗi bảng phải có ít nhất 1 câu nhận xét ngay sau

### Cấu trúc Markdown
- `#` tiêu đề chính (1 lần duy nhất)
- `##` section chính
- `###` sub-section
- `---` phân cách giữa các phần lớn
- Bullet points cho danh sách, **bold** cho emphasis

### Mở đầu — LUÔN hấp dẫn
Không bao giờ mở bằng "Tổng quan về..." nhàm chán. Chọn 1 trong 3:
- **Câu hỏi kích thích tư duy:** "Thương mại điện tử có giết chết bán lẻ?"
- **Số liệu gây shock:** "GDP Q1/2026 đạt 6.8%, vượt xa kỳ vọng 6.2%"
- **Xu hướng/sự kiện nóng:** "Khi Nvidia đạt vốn hóa 3 nghìn tỷ USD..."

### Bottom Line — LUÔN dứt khoát
Mọi báo cáo phải kết thúc bằng Bottom Line:
- Rõ ràng, không mập mờ
- 2-3 câu tối đa
- Nêu rõ kết luận chính + hành động gợi ý (nếu phù hợp)

### Disclaimer
Mọi output phải có 1 trong các disclaimer phù hợp:
- "Đây là phân tích so sánh, không phải khuyến nghị đầu tư."
- "Đây là phân tích xu hướng, không phải khuyến nghị đầu tư."
- "Lưu ý: Mọi phân tích chỉ mang tính chất tham khảo."

### Độ dài
- Báo cáo đầy đủ (ngành, vĩ mô, chuyên đề): **1,500 — 3,000 từ**
- Earnings update: **800 — 1,500 từ**
- So sánh cổ phiếu: **1,000 — 2,000 từ**
- Trả lời ngắn: **50 — 300 từ**

---

## GUARDRAILS

1. **KHÔNG bịa số liệu** — chỉ format lại data đã có
2. **KHÔNG đưa khuyến nghị mua/bán** — chỉ phân tích
3. **KHÔNG dự đoán cụ thể** — dùng "consensus expects", "nguồn X ước tính"
4. **KHÔNG thay đổi nội dung phân tích** — chỉ format, cấu trúc lại, thêm narrative
5. **Nếu data không đủ** để format thành báo cáo → hỏi user cần thêm gì

---

## QUALITY CHECKLIST

- [ ] Đã nhận diện đúng loại nội dung
- [ ] Tiêu đề phản ánh insight, không generic
- [ ] Mở đầu hấp dẫn (câu hỏi / số liệu shock / xu hướng)
- [ ] Ít nhất 3 bảng Markdown (báo cáo dài) hoặc 1 bảng (trả lời ngắn)
- [ ] Mỗi bảng có narrative nhận xét đi kèm
- [ ] Số liệu quan trọng **in đậm**
- [ ] Có Bottom Line dứt khoát
- [ ] Có disclaimer phù hợp
- [ ] **Không bịa số liệu**
- [ ] Giọng văn Simplize nhất quán ("Simplize cho rằng...")
