---
description: "Format báo cáo phân tích cổ phiếu Việt Nam theo chuẩn Simplize — nhận dữ liệu đã có sẵn và trình bày thành báo cáo chuyên nghiệp, rõ ràng, dễ hiểu cho nhà đầu tư phổ thông"
argument-hint: "[mã cổ phiếu] [dữ liệu hoặc nguồn dữ liệu]"
---

# VN Stock Report — Phân tích Cổ phiếu Việt Nam theo phong cách Simplize

Skill này nhận dữ liệu đã có sẵn (từ context, file, hoặc user cung cấp) và trình bày thành báo cáo phân tích cổ phiếu chuyên nghiệp theo phong cách Simplize.

**KHÔNG** thực hiện: thu thập dữ liệu, tìm kiếm web. Nếu thiếu dữ liệu → hỏi user.

---

## WORKFLOW

### Step 1: Thu thập & kiểm tra dữ liệu

Parse input: **Mã cổ phiếu** + **Dữ liệu phân tích** (từ context, file đính kèm, hoặc text user cung cấp).

Dữ liệu cần có (hỏi user nếu thiếu):
- Tên công ty, ngành nghề, vị thế thị trường
- Số liệu tài chính (doanh thu, LNST, biên LN, ROE, PE...)
- Luận điểm đầu tư chính (tại sao nên/không nên mua)
- Định giá và kịch bản (base/conservative/worst)
- Khuyến nghị (BUY/OBSERVE/SELL) và mức giá

### Step 2: Xác định câu chuyện đầu tư

Trước khi viết, xác định:
1. **Góc nhìn chính** (thesis) — điều quan trọng nhất nhà đầu tư cần biết
2. **Tiêu đề hấp dẫn** — phản ánh thesis, không chỉ tên công ty
3. **Cấu trúc bài** — chọn 1 trong 4 kiểu cấu trúc ở mục "KIỂU CẤU TRÚC" bên dưới

### Step 3: Viết báo cáo

Viết theo cấu trúc đã chọn + quy tắc trình bày. Output file markdown tại: `docs/stock-analysis/{ticker}-analysis-{YYYYMMDD}.md`

---

## BÁO CÁO GỒM 7 PHẦN CỐT LÕI

Mọi báo cáo đều có 7 phần này. Tên section và nội dung bên trong linh hoạt theo từng cổ phiếu.

---

### PHẦN 1: YAML FRONTMATTER

````yaml
---
id: "STOCK-{TICKER}-{YYYYMMDD}"
type: "report"
title: "{TICKER}: {Tiêu đề phản ánh thesis chính}"
status: "active"
owner: "govalue"
created: "{YYYY-MM-DD}"
updated: "{YYYY-MM-DD}"
version: "1.0"
tags: [stock-analysis, {TICKER}, {ngành}, {khuyến nghị lowercase}]
ai_summary: |
  Báo cáo phân tích cổ phiếu {TICKER} ({tên công ty}) - {mô tả vị thế 1 dòng}.
  Khuyến nghị {BUY/OBSERVE/SELL} tại giá {X} đồng/CP. Định giá base case {Y} đồng/CP.
  {Luận điểm chính 1 dòng}. {Chỉ số nổi bật: PE, ROE, cổ tức...}.
---
````

---

### PHẦN 2: TIÊU ĐỀ + RECOMMENDATION

````markdown
# {TICKER}: {Tiêu đề hấp dẫn phản ánh thesis} ({DD/MM/YYYY})

## Recommendation

**{Tên công ty đầy đủ} (Mã: {TICKER})**

| Chỉ số | Giá trị/Trạng thái |
| :--- | :--- |
| **Khuyến nghị** | **Buy (Mua)** |
| **Giá hiện tại** | **17,x** (tại ngày 25/07/2019) |
| **Giá mua khuyến nghị** | **< 17,500** |
| **Định giá Base case** | **24,600** |
| **Tỷ trọng tối đa** | **16%** |
| **Tỷ suất cổ tức kỳ vọng** | **7%** |

---
````

**Quy tắc:**
- Chỉ hiện dòng nào CÓ data, bỏ dòng không có
- Các dòng tùy chọn: Tỷ trọng tối đa, Tỷ suất cổ tức, Win-loss ratio, Giá soát xét
- Khuyến nghị luôn ghi cả tiếng Anh + Việt: "Buy (Mua)", "Observe (Quan sát)", "Sell (Bán)"

Ví dụ tiêu đề tốt:
- "PVT: Giữ vững vị trí số 1 trong ngành vận tải biển tại Việt Nam"
- "HPG: Khi đoàn tàu giảm tốc"
- "VNM: Một doanh nghiệp tốt không hẳn đã là một cổ phiếu tốt"
- "FPT: Cổ phiếu tuyệt vời mà bạn không nên bỏ lỡ trong năm 2019"
- "CNG: Doanh nghiệp có hoạt động kinh doanh ổn định với tỷ suất cổ tức cao"

---

### PHẦN 3: BỐI CẢNH / CÂU CHUYỆN MỞ ĐẦU

Phần QUAN TRỌNG NHẤT tạo nên phong cách Simplize. KHÔNG bắt đầu bằng "Tổng quan công ty" nhàm chán. Chọn 1 trong 3 cách mở:

**Cách A — Câu chuyện/bài học đầu tư** (dùng khi có sự kiện đặc biệt, scandal, hoặc bài học hay):

````markdown
## Bối cảnh: {Câu hỏi hoặc chủ đề kích thích tư duy}

{Kể câu chuyện đầu tư liên quan — 2-3 đoạn văn. VD: Warren Buffett mua AmEx khi scandal,
"doanh nghiệp tốt ≠ cổ phiếu tốt", hoặc câu hỏi thách thức quan điểm phổ biến.}

{Kết nối câu chuyện với cổ phiếu đang phân tích — tại sao câu chuyện này relevant.}
````

Mẫu thực tế (trích PNJ):
> Khi 1 công ty gặp rủi ro về pháp lý, điển hình là lãnh đạo bị bắt, bất kỳ 1 nhà đầu tư cá nhân nào cũng muốn…bỏ chạy! [...] Warren Buffett tin rằng sự việc không ảnh hưởng đến hoạt động kinh doanh cốt lõi. Ông đến nhiều nhà hàng, sân bay [...] **Câu chuyện PNJ đang đi theo đúng "mô hình" này.**

Mẫu thực tế (trích VNM):
> Rất nhiều nhà đầu tư mua VNM vì đó là doanh nghiệp tốt đầu ngành. Tuy nhiên, mua cổ phiếu của một công ty tuyệt vời không đảm bảo khoản đầu tư thành công, vì 2 lý do: **Thứ nhất:** [...] **Thứ hai:** [...]

**Cách B — Bối cảnh ngành/thị trường** (dùng khi ngành đang có biến động lớn):

````markdown
## Bối cảnh {ngành/thị trường}

{Mô tả tình hình ngành — áp lực cạnh tranh, biến động giá, thay đổi chính sách, xu hướng mới.
2-3 đoạn, kèm số liệu in đậm.}

{Vị trí của công ty trong bối cảnh đó — bị ảnh hưởng thế nào, cơ hội/thách thức gì.}
````

Mẫu thực tế (trích HPG):
> Giá cổ phiếu HPG giảm gần **30%** từ đỉnh tháng 2/2018. HPG đang đối mặt tác động kép:
> - BĐS hạ nhiệt + chiến tranh thương mại Mỹ-Trung kìm hãm tăng trưởng
> - Vay nợ tăng cao tạo áp lực chi phí hoạt động

Mẫu thực tế (trích VRE):
> Hơn 1 năm trở lại đây, ngành bán lẻ Mỹ chứng kiến liên tiếp 2 thương vụ phá sản của Sears và Toy R Us. Số đông cho rằng cửa hàng vật lý sớm muộn cũng biến mất [...] Tuy nhiên, thực tế các nhà bán lẻ toàn cầu lại đang coi Đông Nam Á là điểm đến lý tưởng [...]

**Cách C — Tổng quan nhanh + Key Points** (dùng khi công ty ổn định, ít drama):

````markdown
## Tổng quan

{Tên công ty} – {mô tả vị thế 1 câu}.

- Doanh thu {năm}: **{X} tỷ đồng ({+/-X}% yoy)**
- LNST {năm}: **{X} tỷ đồng ({+/-X}% yoy)**
- {Chỉ số đặc biệt nếu có: CAGR, thị phần...}

**Keypoints:**
- {Luận điểm 1 — ngắn gọn, rõ ràng}
- {Luận điểm 2}
- {Luận điểm 3}
````

Mẫu thực tế (trích PVT):
> PVT – công ty con PVN, hoạt động trong lĩnh vực vận tải dầu khí đường biển, duy trì kinh doanh ổn định và khả năng sinh lời tốt kể cả khi giá dầu biến động.
> - Doanh thu 2018: **7,523 tỷ đồng (+22.4% yoy)**
> - LNST 2018: **780 tỷ đồng (+46.17% yoy)**
> - Tăng trưởng LNST kép **20%/năm** liên tục 6 năm (2013-2018)
>
> **Key points:**
> - Động lực tăng trưởng dài hạn từ dịch vụ vận tải
> - Dịch vụ thuê kho nổi FSO/FPSO tiếp tục ổn định

---

### PHẦN 4: THỊ TRƯỜNG ĐANG NGHĨ GÌ? (nếu có data sell-side)

````markdown
## Thị trường đang nghĩ gì?

{X} CTCK khuyến nghị MUA ({Y} Strong Buy, {Z} Buy), giá mục tiêu bình quân **{X} đồng/CP**.
Tăng trưởng doanh thu ước tính **{X}% CAGR**.

Tại giá {hiện tại}đ, thị trường phản ánh tăng trưởng doanh thu **{X}%**.
````

Hoặc dạng bảng chi tiết nếu có:

````markdown
| Công ty chứng khoán | Khuyến nghị | Giá mục tiêu (VNĐ/CP) |
| :--- | :--- | ---: |
| ACBS | BUY | 112,069 |
| VDSC | BUY | 126,000 |
| SSI | BUY | 128,000 |
| **Bình quân** | | **127,621** |
````

Nếu không có data sell-side → bỏ section này.

---

### PHẦN 5: PHÂN TÍCH KINH DOANH

Phần chiếm nhiều dung lượng nhất. Cấu trúc HOÀN TOÀN linh hoạt, chọn 1 trong 4 kiểu:

#### Kiểu 1: Chia theo mảng kinh doanh (phổ biến nhất)

Dùng khi công ty có 2+ mảng kinh doanh rõ ràng.

````markdown
## Mảng {tên mảng 1}: {X}% doanh thu, {Y}% LN gộp

{Mô tả hoạt động, vị thế thị trường — 1-2 đoạn văn narrative, in đậm số liệu quan trọng.}

| {Tiêu đề bảng} | {Cột 1} | {Cột 2} | {Cột 3} |
| :--- | ---: | ---: | ---: |
| {Dòng 1} | {val} | {val} | {val} |
| {Dòng 2} | {val} | {val} | {val} |

{Nhận xét bảng: xu hướng, so sánh, ước tính Simplize}

### {Sub-mảng hoặc sản phẩm nổi bật}

{Phân tích sâu hơn nếu cần — cơ hội, thách thức, ước tính tăng trưởng}

**Ước tính Simplize:** {doanh thu/sản lượng mảng này CAGR X%, biên LN Y%...}

## Mảng {tên mảng 2}: {X}% doanh thu, {Y}% LN gộp

{Tương tự — lặp cho mỗi mảng chính}
````

Mẫu thực tế (trích PVT — mảng vận tải):
> PVT là **đơn vị duy nhất** vận tải dầu thô tại Việt Nam, sở hữu 4 tàu chở dầu thô tổng trọng tải **417,929 DWT**.
> - 3 tàu vận chuyển toàn bộ dầu thô từ mỏ Bạch Hổ về NMLD Dung Quất
> - Khối lượng vận tải cho Dung Quất 2018: **7 triệu tấn (+15.5% yoy)**
>
> **Cơ hội từ NMLD Nghi Sơn** (công suất 10 triệu tấn/năm, PVN nắm 25%):
> - PVT đảm nhận tối thiểu **25% lượng dầu thô từ Kuwait** (~2.5 triệu tấn/năm)
> - Ước tính đóng góp **15-20% lợi nhuận** cho PVT
>
> Simplize ước tính: vận tải dầu thô CAGR **1.6%/năm**, dầu thành phẩm CAGR **20%/năm** trong 5 năm tới.

#### Kiểu 2: Chia theo luận điểm đầu tư (đánh số)

Dùng khi muốn build case đầu tư có hệ thống.

````markdown
## 1. {Luận điểm đầu tư 1 — câu khẳng định}

{Phân tích chi tiết — nhiều đoạn văn, xen kẽ bảng số liệu và narrative.
Bắt đầu bằng bối cảnh, sau đó đưa bằng chứng (số liệu, so sánh), kết bằng ước tính.}

| {Bảng minh họa} | {Cột} | {Cột} |
| :--- | ---: | ---: |
| {data} | {val} | {val} |

{Nhận xét và kết luận cho luận điểm này}

---

## 2. {Luận điểm đầu tư 2}

{Tương tự}

---

## 3. {Luận điểm đầu tư 3}

{Tương tự}
````

Mẫu thực tế (trích CNG — luận điểm 2 về cổ tức):
> Với lượng tiền mặt dồi dào, nợ vay không còn và dòng tiền ổn định…
>
> | Năm | FCF (Tỷ đồng) | CFO/Op Income (%) | FCF/Op Income (%) |
> | :--- | ---: | ---: | ---: |
> | 2016 | 148.7 | 86.4% | 70.8% |
> | 2017 | 89.4 | 91.2% | 49.3% |
> | 2018 | 63.9 | 80.6% | 37.1% |
>
> …CNG Việt Nam hiện đang duy trì 1 chính sách cổ tức khá ổn định, với tỷ lệ chi trả cổ tức hàng năm từ **15% – 30%/VĐL**.
>
> | Năm | Tỷ lệ cổ tức (%) | Tỷ suất cổ tức (%) |
> | :--- | ---: | ---: |
> | 2016 | 30.0% | 7.4% |
> | 2017 | 15.0% | 4.7% |
> | 2018 | 25.0% | 9.6% |
> | 2019-F | 25.0% | 10.9% |
>
> Simplize tin rằng CNG Việt Nam sẽ tiếp tục duy trì chính sách cổ tức cao [...] với mức tỷ suất cổ tức **10.9%** ở mức giá hiện tại.

#### Kiểu 3: Chia theo vấn đề/rủi ro

Dùng khi cổ phiếu đang đối mặt nhiều thách thức (thường cho OBSERVE/SELL).

````markdown
## {Vấn đề chính}: {mô tả ngắn gọn}

{Phân tích vấn đề — tại sao xảy ra, tác động thế nào, số liệu minh họa}

| {Bảng minh họa} | {Cột} | {Cột} |
| :--- | ---: | ---: |
| {data} | {val} | {val} |

### {Khía cạnh cụ thể của vấn đề}

{Phân tích sâu hơn}

## {Vấn đề/rủi ro tiếp theo}

{Tương tự}
````

Mẫu thực tế (trích HPG — mảng tôn mạ):
> Nhà máy tôn mạ màu Hưng Yên (400,000 tấn/năm) phải hoãn do cạnh tranh khốc liệt:
>
> **Cạnh tranh giá từ Trung Quốc** (thấp hơn ~7% dù bị áp thuế):
>
> | Sản phẩm | Tôn nội địa (VND/kg) | Tôn TQ (VND/kg) |
> | :--- | ---: | ---: |
> | Tôn mạ kẽm, mạ lạnh | 18,500–19,800 | 17,200–17,500 |
> | Tôn mạ màu | 24,800–25,200 | 21,000–21,200 |
>
> **Công suất dư thừa:** Tổng công suất tôn 2018: **8 triệu tấn**, tiêu thụ chỉ **~5 triệu tấn**.
>
> | Doanh nghiệp | Công suất (tấn) | Thị phần (%) |
> | :--- | ---: | ---: |
> | Hoa Sen | 2,280,000 | 34.7 |
> | Nam Kim | 1,200,000 | 16.5 |
> | Hòa Phát | 600,000 | 4.0 |

#### Kiểu 4: Chia theo xu hướng → cơ hội → năng lực

Dùng khi phân tích dựa trên macro/xu hướng ngành.

````markdown
## {Xu hướng ngành/macro}

{Phân tích xu hướng — quy mô, tốc độ, so sánh quốc tế}

| {Bảng so sánh quốc tế/khu vực} | {Cột} | {Cột} |
| :--- | ---: | ---: |
| {data} | {val} | {val} |

### {Yếu tố thúc đẩy 1}

{Chi tiết}

### {Yếu tố thúc đẩy 2}

{Chi tiết}

## {Khả năng tận dụng của công ty}

{Vị thế, năng lực, lợi thế cạnh tranh liên quan đến xu hướng}
````

Mẫu thực tế (trích VRE — xu hướng tiêu dùng):
> Simplize cho rằng tiêu dùng tại TTTM là xu hướng tất yếu của bán lẻ hiện đại.
>
> **1. Mật độ bán lẻ còn thấp:**
> - Hà Nội: 0.17 m²/người, TP.HCM: 0.13 m²/người
> - So sánh: Bangkok 0.89 m²/người, Singapore 0.75 m²/người
>
> **2. Cơ cấu dân số vàng:**
> - Dân số 96.49 triệu người (2018) – đông thứ 3 ĐNA
> - Độ tuổi trung bình 31.7 – tương đối trẻ trong khu vực
>
> **3. Tầng lớp trung lưu tăng mạnh:**
> - Tầng lớp trung lưu dự kiến tăng gấp đôi lên **33 triệu người** vào 2020
> - Tốc độ tăng trưởng **nhanh nhất ĐNA**

**NGUYÊN TẮC VIẾT PHẦN PHÂN TÍCH:**
- Mỗi section phải có **narrative** (giải thích TẠI SAO), không chỉ liệt kê số
- Xen kẽ **bảng số liệu** minh họa → nhận xét ngay sau bảng
- **In đậm** mọi số liệu quan trọng
- Đưa ước tính rõ ràng: "Simplize ước tính...", "Simplize cho rằng..."
- So sánh với ngành/đối thủ/khu vực khi có data
- Nêu cả tích cực VÀ rủi ro — không thiên lệch 1 chiều
- Có thể kết hợp nhiều kiểu trong 1 báo cáo (VD: mảng KD + rủi ro)

---

### PHẦN 6: ĐỊNH GIÁ VÀ KHUYẾN NGHỊ

````markdown
## Định giá và khuyến nghị Simplize

{Nhận xét PE, PB, EV/EBITDA hiện tại — so với lịch sử chính nó, trung bình ngành, khu vực.
Đây là đoạn narrative, không chỉ bảng.}

### 3 kịch bản định giá

| Kịch bản | Giá trị CP (VNĐ) | {Giả định chính 1} | {Giả định chính 2} |
| :--- | :--- | :--- | :--- |
| **#Base** | **{giá}** | {val} | {val} |
| #Conservative | {giá} | {val} | {val} |
| #Worst | {giá} | {val} | {val} |

**#Base case:** {Giả định chính — 1-2 câu}

**#Conservative case:** {Giả định thận trọng — 1-2 câu}

**#Worst case:** {Giả định xấu nhất — 1-2 câu}
````

Mẫu thực tế (trích FPT):
> P/E hiện tại: **7.9x** → gần mức -1SD, rẻ so với chính nó trong quá khứ. Hơn 50% lợi nhuận từ công nghệ mà P/E chỉ 7.9x → thị trường đang "phớt lờ" do lo ngại chiến tranh thương mại.
>
> | Kịch bản | Giá trị CP (VNĐ) | CAGR DT xuất khẩu PM | CAGR DT viễn thông | Cost of equity |
> | :--- | :--- | :--- | :--- | :--- |
> | **#Base** | **54,000** | 19.1% | 10.3% | 12.6% |
> | #Conservative | 43,000 | 13.8% | 7.2% | 13.2% |
> | #Worst | 34,700 | 9.0% | 5.1% | 13.8% |
>
> **Tỷ lệ win-loss:** Tỷ lệ giữa lợi nhuận (trường hợp đúng) và mức lỗ (trường hợp sai). Simplize luôn yêu cầu tối thiểu **2.5x** trước khi mua.

Thêm nếu có data:

````markdown
**Rủi ro đã tính đến:**
1. {Rủi ro 1 — kèm số liệu ảnh hưởng cụ thể}
2. {Rủi ro 2}
````

Hoặc section cổ tức nếu relevant:

````markdown
### Cổ tức

Kỳ vọng cổ tức {X} đồng/CP → tỷ suất **{Y}%** tại giá hiện tại.
````

---

### PHẦN 7: BOTTOM LINE

````markdown
## Bottom Line

{1-3 câu tóm tắt dứt khoát: kết luận đầu tư core, tỷ lệ win-loss, và/hoặc điểm nhấn chính.}

**Khuyến nghị: {BUY (Mua) / OBSERVE (Quan sát) / SELL (Bán)} tại giá {điều kiện}.**
````

Các biến thể Bottom Line đã thấy:

**BUY đơn giản:**
> Tỷ lệ win-loss **~2.3x** tại giá hiện tại. PVT có đầy đủ yếu tố: vị thế số 1, đội tàu trẻ, đa dạng nguồn thu, cổ tức cao, tăng trưởng ổn định.
>
> **Khuyến nghị: BUY (Mua) tại giá < 17,500 đồng/CP, tỷ trọng tối đa 16%.**

**BUY với bullet points:**
> Giá hiện tại 15,150 đồng/CP → thị trường đang phản ứng quá tiêu cực, bỏ qua triển vọng hồi phục.
>
> Những yếu tố tích cực:
> - Hợp đồng xuất khẩu lớn với TireCo (Bắc Mỹ)
> - Giá nguyên liệu duy trì ổn định ở mức thấp
> - Vinachem thoái vốn gỡ bỏ nút thắt nợ vay
>
> **Khuyến nghị: BUY (Mua) tại giá < 16,000 đồng/CP.**

**OBSERVE chờ BUY:**
> Simplize kỳ vọng PNJ sẽ tiếp tục biến động biên độ lớn và sẽ có những mức giá hấp dẫn dưới 86,000 đồng/CP.
>
> **Với tỷ lệ win-loss tối thiểu 2.x → Khuyến nghị BUY khi giá < 86,000 đồng/CP. Tại thời điểm hiện tại: OBSERVE (Quan sát).**

**OBSERVE/SELL kết hợp:**
> **Khuyến nghị:**
> - **Chưa nắm giữ VNM → OBSERVE (Quan sát)**
> - **Đang nắm giữ VNM → SELL (Bán)**
> - **BUY (Mua) ở trạng thái chờ** khi giá < 80,000 đồng/CP

**OBSERVE ngắn gọn:**
> Mua HPG khi dự án Dung Quất chưa có kết quả rõ ràng và thị trường đang bão hòa là rủi ro rất lớn.
>
> **Khuyến nghị: OBSERVE (Quan sát).**

---

## QUY TẮC TRÌNH BÀY

### Ngôn ngữ
- **Tiếng Việt** chủ đạo
- Giữ nguyên thuật ngữ tài chính: PE, PB, ROE, CAGR, EPS, LNST, FCF, LNTT, NLA, GFA, DWT, yoy...
- Giọng văn: chuyên nghiệp nhưng dễ hiểu, quan điểm rõ ràng, giải thích logic
- Xưng hô: "Simplize cho rằng...", "Simplize ước tính...", "Simplize tin rằng..."

### Số liệu
- **In đậm** mọi số liệu quan trọng
- Đơn vị: "tỷ đồng", "đồng/CP", "USD/thùng", "triệu tấn"
- Tỷ lệ % luôn kèm context: yoy, CAGR, vs ngành, vs lịch sử
- Giá cổ phiếu: **17,500** hoặc **17,x** (xấp xỉ)

### Bảng Markdown
- Ít nhất **3-5 bảng** mỗi báo cáo
- Align: `:---` text trái, `---:` số phải
- **In đậm** dòng công ty đang phân tích trong bảng so sánh
- *In nghiêng* dòng trung bình ngành/trung bình

### Cấu trúc Markdown
- `#` tiêu đề chính (1 lần duy nhất)
- `##` section chính
- `###` sub-section
- `---` phân cách giữa các phần lớn
- Bullet points cho danh sách, **bold** cho emphasis
- Xen kẽ narrative và bảng — KHÔNG liệt kê bảng liên tục không giải thích

### Độ dài
- **1,500 — 3,000 từ** tùy độ phức tạp
- Đủ sâu để thuyết phục, không dài lê thê

---

## QUALITY CHECKLIST

- [ ] YAML frontmatter đầy đủ (id, title, tags, ai_summary)
- [ ] Tiêu đề phản ánh thesis/insight, không chỉ tên công ty
- [ ] Bảng Recommendation chỉ hiện dòng có data
- [ ] Có phần bối cảnh/câu chuyện mở đầu hấp dẫn (Cách A/B/C)
- [ ] Phần phân tích theo 1 trong 4 kiểu cấu trúc (hoặc kết hợp)
- [ ] Ít nhất 3 bảng Markdown minh họa số liệu
- [ ] Mỗi section có narrative (giải thích TẠI SAO), không chỉ liệt kê
- [ ] Số liệu quan trọng **in đậm**
- [ ] 3 kịch bản định giá + giải thích từng kịch bản
- [ ] Bottom Line có khuyến nghị rõ ràng, dứt khoát
- [ ] **Không bịa số liệu** — chỉ dùng data được cung cấp
- [ ] File lưu đúng: `docs/stock-analysis/{ticker}-analysis-{YYYYMMDD}.md`
