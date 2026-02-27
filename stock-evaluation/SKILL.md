---
name: vn-stock-evaluation
description: >
  Đánh giá và xếp hạng tổng hợp cổ phiếu Việt Nam dựa trên hệ thống chấm điểm đa tiêu chí.
  Sử dụng skill này khi người dùng muốn: đánh giá chất lượng một cổ phiếu, so sánh nhiều cổ phiếu,
  xếp hạng cổ phiếu trong ngành, cho điểm cổ phiếu, đánh giá rủi ro, review cổ phiếu,
  hoặc bất kỳ yêu cầu nào liên quan đến "đánh giá", "xếp hạng", "so sánh", "rating", "scoring",
  "cổ phiếu nào tốt hơn", "chất lượng doanh nghiệp".
  Kích hoạt khi người dùng hỏi: "đánh giá VNM", "so sánh FPT và CMG", "xếp hạng ngân hàng",
  "cổ phiếu nào tốt hơn giữa A và B", "cho điểm HPG", "review VCB".
---

# VN Stock Evaluation — Đánh giá & Xếp hạng cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude đánh giá chất lượng cổ phiếu một cách có hệ thống thông qua bộ tiêu chí chấm điểm, hỗ trợ so sánh nhiều cổ phiếu với nhau và xếp hạng trong ngành.

Khác biệt với các skill khác:
- **Stock Analysis**: đi sâu phân tích chi tiết từng khía cạnh
- **Stock Valuation**: tập trung ước tính giá trị hợp lý
- **Stock Evaluation (skill này)**: chấm điểm tổng hợp, so sánh, xếp hạng — giúp ra quyết định nhanh

## Hệ thống chấm điểm

### Khung đánh giá 5 trụ cột (tổng 100 điểm)

**Trụ cột 1: Chất lượng tài chính (30 điểm)**

| Tiêu chí | Thang điểm | Cách tính |
|-----------|------------|-----------|
| ROE | 0-8 | >20%: 8đ, 15-20%: 6đ, 10-15%: 4đ, 5-10%: 2đ, <5%: 0đ |
| Biên lợi nhuận ròng | 0-6 | Tăng đều 3 năm: 6đ, ổn định: 4đ, giảm nhẹ: 2đ, giảm mạnh: 0đ |
| D/E ratio | 0-6 | <0.5: 6đ, 0.5-1: 4đ, 1-2: 2đ, >2: 0đ |
| Free cash flow | 0-5 | Dương 3 năm liên tiếp: 5đ, dương 2/3 năm: 3đ, dương 1/3 năm: 1đ, âm 3 năm: 0đ |
| Current ratio | 0-5 | >2: 5đ, 1.5-2: 4đ, 1-1.5: 2đ, <1: 0đ |

**Trụ cột 2: Tăng trưởng (25 điểm)**

| Tiêu chí | Thang điểm | Cách tính |
|-----------|------------|-----------|
| Tăng trưởng doanh thu 3Y CAGR | 0-8 | >20%: 8đ, 10-20%: 6đ, 5-10%: 4đ, 0-5%: 2đ, <0%: 0đ |
| Tăng trưởng EPS 3Y CAGR | 0-8 | >25%: 8đ, 15-25%: 6đ, 5-15%: 4đ, 0-5%: 2đ, <0%: 0đ |
| Tính nhất quán tăng trưởng | 0-5 | Tăng đều mỗi năm: 5đ, tăng 2/3 năm: 3đ, biến động: 1đ |
| Triển vọng ngành | 0-4 | Ngành tăng trưởng cao: 4đ, ổn định: 2đ, suy giảm: 0đ |

**Trụ cột 3: Định giá (20 điểm)**

| Tiêu chí | Thang điểm | Cách tính |
|-----------|------------|-----------|
| P/E vs ngành | 0-7 | Thấp hơn >30%: 7đ, thấp hơn 10-30%: 5đ, ngang: 3đ, cao hơn: 1đ |
| P/B vs ngành | 0-5 | Thấp hơn >30%: 5đ, thấp hơn 10-30%: 3đ, ngang: 2đ, cao hơn: 1đ |
| PEG ratio | 0-5 | <0.8: 5đ, 0.8-1.2: 3đ, 1.2-2: 1đ, >2: 0đ |
| Dividend yield | 0-3 | >5%: 3đ, 3-5%: 2đ, 1-3%: 1đ, <1%: 0đ |

**Trụ cột 4: Kỹ thuật & Momentum (15 điểm)**

| Tiêu chí | Thang điểm | Cách tính |
|-----------|------------|-----------|
| Xu hướng giá | 0-5 | Uptrend rõ ràng: 5đ, sideway: 3đ, downtrend: 0đ |
| RSI position | 0-3 | 40-60 (trung tính): 3đ, oversold phục hồi: 2đ, overbought: 0đ |
| Volume trend | 0-4 | Volume tăng theo giá tăng: 4đ, volume bình thường: 2đ, volume giảm: 0đ |
| Vị trí vs MA200 | 0-3 | Trên MA200 & MA200 hướng lên: 3đ, gần MA200: 1đ, dưới MA200: 0đ |

**Trụ cột 5: Quản trị & Rủi ro (10 điểm)**

| Tiêu chí | Thang điểm | Cách tính |
|-----------|------------|-----------|
| Tính minh bạch | 0-3 | BCTC đúng hạn, kiểm toán sạch: 3đ, có ý kiến ngoại trừ: 1đ, trì hoãn: 0đ |
| Cổ đông nội bộ | 0-3 | Mua ròng: 3đ, không giao dịch: 2đ, bán ròng: 0đ |
| Lịch sử trả cổ tức | 0-2 | Đều đặn >3 năm: 2đ, không đều: 1đ, không trả: 0đ |
| Rủi ro đặc thù | 0-2 | Không có rủi ro lớn: 2đ, rủi ro trung bình: 1đ, rủi ro cao: 0đ |

### Xếp hạng tổng hợp

| Điểm | Xếp hạng | Ý nghĩa |
|------|----------|---------|
| 80-100 | ⭐⭐⭐⭐⭐ Xuất sắc | Cổ phiếu chất lượng cao, hấp dẫn ở nhiều khía cạnh |
| 65-79 | ⭐⭐⭐⭐ Tốt | Cổ phiếu chất lượng tốt, có một vài điểm cần lưu ý |
| 50-64 | ⭐⭐⭐ Trung bình | Cổ phiếu bình thường, cần xem xét kỹ trước khi đầu tư |
| 35-49 | ⭐⭐ Dưới trung bình | Nhiều điểm yếu, rủi ro cao |
| 0-34 | ⭐ Yếu | Không hấp dẫn, nhiều rủi ro tài chính |

## Quy trình đánh giá

### Đánh giá một cổ phiếu

1. Load dữ liệu và tính toán từng tiêu chí
2. Chấm điểm theo bảng tiêu chí
3. Tạo bảng scorecard chi tiết
4. Vẽ radar chart thể hiện 5 trụ cột
5. Nhận xét điểm mạnh và điểm yếu nổi bật
6. So sánh với trung bình ngành nếu có dữ liệu

### So sánh nhiều cổ phiếu

1. Chấm điểm tất cả các mã theo cùng tiêu chí
2. Lập bảng so sánh song song
3. Vẽ radar chart overlay
4. Xếp hạng từ cao đến thấp
5. Phân tích trade-off: mã nào mạnh ở khía cạnh nào

### Xếp hạng toàn ngành

1. Chấm điểm tất cả cổ phiếu trong ngành
2. Sắp xếp theo tổng điểm
3. Lập bảng xếp hạng với top 5-10
4. Highlight các outlier (cổ phiếu nổi bật tích cực/tiêu cực)

## Cấu trúc báo cáo đánh giá

```
# Đánh giá: [MÃ CỔ PHIẾU] — [ĐIỂM]/100 — [XẾP HẠNG]
## 1. Scorecard tổng hợp
   [Bảng điểm chi tiết + Radar chart]
## 2. Điểm mạnh chính
## 3. Điểm yếu / Rủi ro
## 4. So sánh với ngành
## 5. Tổng kết
```

## Lưu ý

- Hệ thống chấm điểm này là khung tham khảo — người dùng có thể yêu cầu điều chỉnh trọng số hoặc tiêu chí
- Điểm số có tính tương đối, phù hợp nhất khi so sánh cổ phiếu cùng ngành
- Một số tiêu chí trong Trụ cột 5 (Quản trị) có thể khó lượng hóa từ dữ liệu — ghi rõ khi nào dùng đánh giá định tính
- Luôn nhắc người dùng rằng điểm số là công cụ hỗ trợ, không thay thế phân tích toàn diện
