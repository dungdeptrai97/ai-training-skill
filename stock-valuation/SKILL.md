---
name: vn-stock-valuation
description: >
  Định giá cổ phiếu trên thị trường chứng khoán Việt Nam bằng nhiều phương pháp khác nhau.
  Sử dụng skill này khi người dùng muốn: định giá một mã cổ phiếu, tính giá trị hợp lý (fair value/intrinsic value),
  xác định cổ phiếu đang đắt hay rẻ, ước tính upside/downside, xây dựng mô hình DCF,
  định giá theo P/E so sánh, hoặc bất kỳ yêu cầu nào liên quan đến
  "định giá", "valuation", "giá trị hợp lý", "giá mục tiêu", "target price", "fair value",
  "cổ phiếu này đắt không", "nên mua ở giá nào".
  Kích hoạt cả khi người dùng hỏi: "định giá VNM", "giá hợp lý của FPT là bao nhiêu",
  "HPG đang được định giá thế nào", "upside MWG còn bao nhiêu".
---

# VN Stock Valuation — Định giá cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude áp dụng các phương pháp định giá phù hợp để ước tính giá trị hợp lý (intrinsic value) của cổ phiếu, so sánh với giá thị trường hiện tại để đánh giá mức độ hấp dẫn.

## Các phương pháp định giá

### 1. Chiết khấu dòng tiền (DCF — Discounted Cash Flow)

Phương pháp phù hợp nhất cho doanh nghiệp có dòng tiền ổn định và dự báo được.

**Quy trình:**

a) **Dự phóng dòng tiền tự do (FCFF hoặc FCFE)** cho 5-10 năm tới:
   - FCFF = EBIT × (1 - Tax rate) + Depreciation - CapEx - ΔWorking Capital
   - FCFE = Net Income + Depreciation - CapEx - ΔWorking Capital + Net Borrowing
   - Dựa trên tốc độ tăng trưởng lịch sử, kế hoạch kinh doanh, triển vọng ngành

b) **Xác định tỷ lệ chiết khấu:**
   - WACC = E/(D+E) × Ke + D/(D+E) × Kd × (1-T)
   - Cost of equity (Ke): dùng CAPM = Rf + β × (Rm - Rf)
     - Rf: lãi suất trái phiếu chính phủ VN 10 năm
     - Rm - Rf: equity risk premium cho VN (thường 7-9%)
     - β: beta của cổ phiếu (tính từ dữ liệu lịch sử hoặc dùng beta ngành)
   - Cost of debt (Kd): lãi suất vay thực tế của doanh nghiệp

c) **Giá trị terminal (Terminal Value):**
   - Gordon Growth: TV = FCF_n+1 / (WACC - g)
   - g (tốc độ tăng trưởng vĩnh viễn): thường 2-4% cho VN (tương đương lạm phát dài hạn)
   - Exit multiple: TV = FCF_n × EV/EBITDA multiple của ngành

d) **Tính giá trị doanh nghiệp:**
   - Enterprise Value = PV(FCFs) + PV(Terminal Value)
   - Equity Value = EV - Net Debt + Cash
   - Giá trị mỗi cổ phiếu = Equity Value / Số cổ phiếu lưu hành

e) **Phân tích độ nhạy (Sensitivity Analysis):**
   - Lập bảng sensitivity 2 chiều: WACC × growth rate
   - Cho thấy khoảng giá trị hợp lý thay vì một con số duy nhất

### 2. Định giá so sánh (Relative Valuation)

Phương pháp nhanh, phù hợp khi có nhiều doanh nghiệp cùng ngành để so sánh.

**Các bội số phổ biến:**

| Bội số | Công thức | Phù hợp cho |
|--------|-----------|-------------|
| P/E | Giá / EPS | Hầu hết các ngành |
| P/B | Giá / Book value per share | Ngân hàng, BĐS, holding |
| EV/EBITDA | Enterprise Value / EBITDA | So sánh xuyên cấu trúc vốn |
| P/S | Giá / Doanh thu per share | DN thua lỗ, startup, công nghệ |
| PEG | P/E / EPS growth rate | DN tăng trưởng cao |

**Quy trình:**
1. Chọn nhóm so sánh (peer group) cùng ngành, cùng quy mô trên thị trường VN
2. Tính bội số trung bình/trung vị của peer group
3. Áp vào chỉ số tài chính của cổ phiếu mục tiêu
4. Điều chỉnh premium/discount nếu cổ phiếu có đặc thù riêng (tăng trưởng cao hơn, rủi ro thấp hơn...)

**Ví dụ:**
- Fair P/E = Median P/E ngành × EPS forward → Target Price
- Fair P/B = Median P/B ngành × BVPS → Target Price

### 3. Định giá theo tài sản ròng (NAV — Net Asset Value)

Phù hợp cho: BĐS, holding company, doanh nghiệp có nhiều tài sản cố định.

**Quy trình:**
1. Định giá lại từng tài sản theo giá thị trường (đặc biệt BĐS, đất đai)
2. Trừ đi tổng nợ phải trả
3. RNAV = Tổng giá trị tài sản định giá lại - Tổng nợ
4. RNAV per share = RNAV / Số cổ phiếu lưu hành
5. Áp discount 20-40% (NAV discount thường thấy ở VN)

### 4. Mô hình chiết khấu cổ tức (DDM — Dividend Discount Model)

Phù hợp cho: doanh nghiệp trả cổ tức đều đặn, tỷ lệ payout ổn định.

**Gordon Growth Model:**
- P = D₁ / (Ke - g)
- D₁ = Cổ tức kỳ vọng năm tới
- Ke = Cost of equity
- g = Tốc độ tăng trưởng cổ tức dài hạn

**Two-stage DDM:** cho doanh nghiệp có giai đoạn tăng trưởng cao ban đầu, sau đó ổn định.

### 5. Định giá theo Sum-of-the-Parts (SOTP)

Phù hợp cho: tập đoàn đa ngành, holding company.

**Quy trình:**
1. Tách riêng từng mảng kinh doanh
2. Định giá mỗi mảng bằng phương pháp phù hợp nhất
3. Cộng tổng giá trị các mảng
4. Trừ nợ ròng ở cấp công ty mẹ
5. Áp holding discount nếu cần (10-30%)

## Chọn phương pháp phù hợp

Khi nhận yêu cầu định giá, chọn phương pháp dựa trên đặc điểm doanh nghiệp:

| Đặc điểm | Phương pháp ưu tiên |
|-----------|---------------------|
| Dòng tiền ổn định, dự báo được | DCF |
| Nhiều peer cùng ngành | Relative (P/E, EV/EBITDA) |
| Ngân hàng, tài chính | P/B kết hợp ROE, Residual income |
| Bất động sản | RNAV + P/E forward |
| Trả cổ tức đều | DDM |
| Tập đoàn đa ngành | SOTP |
| DN thua lỗ, startup | P/S, EV/Revenue |

Luôn sử dụng ít nhất 2 phương pháp để cross-check, sau đó đưa ra khoảng định giá (valuation range) thay vì một con số chính xác.

## Cấu trúc báo cáo định giá

```
# Báo cáo định giá: [MÃ CỔ PHIẾU]
## 1. Tổng quan & Giả định chính
## 2. Phương pháp 1: [Tên phương pháp]
   - Các giả định và tham số
   - Kết quả
## 3. Phương pháp 2: [Tên phương pháp]
   - Các giả định và tham số
   - Kết quả
## 4. Phân tích độ nhạy
## 5. Tổng hợp định giá
   - Khoảng giá trị hợp lý: [X] - [Y] VND/cp
   - So với giá hiện tại: Upside/Downside [Z]%
## 6. Rủi ro đối với định giá
```

## Nguyên tắc quan trọng

- Mọi định giá đều dựa trên giả định — luôn trình bày rõ các giả định và cho phép người dùng điều chỉnh
- Sử dụng dữ liệu tài chính VN: lãi suất TPCP VN, equity risk premium VN, thuế suất VN (20%)
- Không đưa khuyến nghị mua/bán — chỉ cung cấp khoảng giá trị hợp lý và để người dùng tự quyết định
- Ghi rõ hạn chế của từng phương pháp
- Tạo bảng sensitivity analysis để người dùng thấy giá trị thay đổi theo các kịch bản khác nhau
- Khi tính beta, ưu tiên dùng dữ liệu ít nhất 2 năm, so với VN-Index
