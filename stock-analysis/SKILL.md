---
name: vn-stock-analysis
description: >
  Phân tích chuyên sâu cổ phiếu trên thị trường chứng khoán Việt Nam, bao gồm phân tích cơ bản (fundamental)
  và phân tích kỹ thuật (technical). Sử dụng skill này khi người dùng muốn: phân tích một mã cổ phiếu cụ thể,
  xem báo cáo tài chính, phân tích xu hướng giá, đọc biểu đồ kỹ thuật, phân tích dòng tiền,
  đánh giá sức khỏe tài chính doanh nghiệp, phân tích ngành, hoặc bất kỳ yêu cầu nào liên quan đến
  "phân tích", "analysis", "đánh giá tài chính", "xem báo cáo", "biểu đồ" một mã cổ phiếu.
  Kích hoạt cả khi người dùng hỏi dạng: "phân tích VNM", "FPT đang thế nào", "tình hình tài chính HPG",
  "xu hướng giá MWG", "dòng tiền VCB ra sao".
---

# VN Stock Analysis — Phân tích cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude thực hiện phân tích toàn diện cho một hoặc nhiều mã cổ phiếu, kết hợp cả phân tích cơ bản và kỹ thuật, đưa ra bức tranh đầy đủ về tình trạng doanh nghiệp và cổ phiếu.

## Quy trình phân tích

Khi nhận yêu cầu phân tích một mã cổ phiếu:

### Bước 1: Xác định phạm vi phân tích

Hỏi người dùng nếu chưa rõ:
- Mã cổ phiếu cần phân tích
- Loại phân tích: cơ bản, kỹ thuật, hay toàn diện (cả hai)
- Khung thời gian quan tâm (ngắn hạn, trung hạn, dài hạn)
- Đường dẫn dữ liệu nếu chưa cung cấp

### Bước 2: Phân tích cơ bản (Fundamental Analysis)

Phân tích theo cấu trúc sau, sử dụng dữ liệu báo cáo tài chính:

**2a. Tổng quan doanh nghiệp:**
- Ngành nghề, mô hình kinh doanh, vị thế trong ngành
- Cơ cấu cổ đông, ban lãnh đạo (nếu có dữ liệu)

**2b. Phân tích kết quả kinh doanh:**
- Doanh thu: xu hướng, tốc độ tăng trưởng, tính mùa vụ
- Lợi nhuận gộp, lợi nhuận ròng, biên lợi nhuận
- So sánh YoY (cùng kỳ) và QoQ (liên quý)

**2c. Phân tích bảng cân đối kế toán:**
- Cơ cấu tài sản: tài sản ngắn hạn vs dài hạn
- Cơ cấu nguồn vốn: nợ vs vốn chủ sở hữu
- Chất lượng tài sản: hàng tồn kho, phải thu, tài sản cố định

**2d. Phân tích dòng tiền:**
- Dòng tiền từ hoạt động kinh doanh (CFO)
- Dòng tiền từ đầu tư (CFI)
- Dòng tiền từ tài chính (CFF)
- Free cash flow = CFO - CapEx
- So sánh lợi nhuận kế toán vs dòng tiền thực

**2e. Các chỉ số tài chính chính:**

| Nhóm | Chỉ số | Ý nghĩa |
|-------|--------|---------|
| Sinh lời | ROE, ROA, ROIC | Hiệu quả sử dụng vốn |
| Sinh lời | Gross margin, Net margin, EBITDA margin | Biên lợi nhuận |
| Tăng trưởng | Revenue growth, EPS growth | Tốc độ phát triển |
| Đòn bẩy | D/E, Net debt/EBITDA | Rủi ro tài chính |
| Thanh khoản | Current ratio, Quick ratio | Khả năng thanh toán |
| Hiệu quả | Vòng quay hàng tồn kho, Vòng quay phải thu | Hiệu quả vận hành |

**2f. Phân tích Dupont:**
Phân rã ROE = Net margin × Asset turnover × Equity multiplier để hiểu nguồn gốc lợi nhuận.

### Bước 3: Phân tích kỹ thuật (Technical Analysis)

**3a. Phân tích xu hướng (Trend):**
- Xác định xu hướng chính: tăng, giảm, sideway
- Các đường MA: MA20, MA50, MA100, MA200
- Vị trí giá so với các MA

**3b. Chỉ báo kỹ thuật:**
- RSI (14): quá mua/quá bán, phân kỳ
- MACD (12,26,9): tín hiệu mua/bán, histogram
- Bollinger Bands (20,2): biến động, squeeze
- Stochastic: vùng quá mua/quá bán
- Volume: xác nhận xu hướng, phân kỳ volume-giá

**3c. Hỗ trợ và kháng cự:**
- Xác định các mức hỗ trợ/kháng cự quan trọng
- Fibonacci retracement nếu có xu hướng rõ ràng
- Vùng tích lũy/phân phối

**3d. Mẫu hình giá (nếu nhận diện được):**
- Head & Shoulders, Double Top/Bottom
- Triangle, Channel, Flag
- Breakout/Breakdown

### Bước 4: Tạo biểu đồ

Sử dụng matplotlib/plotly để tạo biểu đồ trực quan:

- Biểu đồ giá candlestick với volume
- Overlay các đường MA và Bollinger Bands
- Sub-chart cho RSI, MACD
- Đánh dấu các mức hỗ trợ/kháng cự

Tham khảo file `references/chart_templates.md` để xem mẫu code biểu đồ.

### Bước 5: Tổng hợp và kết luận

Tổng hợp phân tích thành báo cáo có cấu trúc:

```
# Báo cáo phân tích: [MÃ CỔ PHIẾU]
## 1. Tổng quan
## 2. Phân tích cơ bản
   ### 2.1 Kết quả kinh doanh
   ### 2.2 Sức khỏe tài chính
   ### 2.3 Dòng tiền
## 3. Phân tích kỹ thuật
   ### 3.1 Xu hướng giá
   ### 3.2 Chỉ báo kỹ thuật
## 4. Điểm mạnh và rủi ro
## 5. Kết luận
```

## Nguyên tắc phân tích

- Luôn so sánh các chỉ số với trung bình ngành và đối thủ cùng ngành trên thị trường VN
- Xem xét bối cảnh vĩ mô Việt Nam: lãi suất, tỷ giá, chính sách, chu kỳ kinh tế
- Phân biệt rõ giữa dữ liệu (fact) và nhận định (opinion)
- Không đưa khuyến nghị mua/bán cụ thể — chỉ cung cấp phân tích khách quan
- Ghi rõ thời điểm dữ liệu, nguồn dữ liệu
- Khi phân tích ngành đặc thù (ngân hàng, bảo hiểm, chứng khoán), sử dụng các chỉ số riêng phù hợp:
  - Ngân hàng: NIM, NPL ratio, CIR, CASA, CAR, tín dụng tăng trưởng
  - Bất động sản: NAV, landbank, backlog
  - Chứng khoán: thị phần môi giới, margin lending

## Xử lý dữ liệu thiếu

Nếu thiếu dữ liệu cho một phần phân tích:
- Thông báo rõ phần nào không thể phân tích do thiếu dữ liệu
- Đề xuất nguồn dữ liệu bổ sung nếu biết
- Vẫn hoàn thành các phần có đủ dữ liệu thay vì dừng lại hoàn toàn
