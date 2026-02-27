---
name: vn-portfolio-allocation
description: >
  Phân bổ và tối ưu danh mục đầu tư cổ phiếu Việt Nam. Sử dụng skill này khi người dùng muốn:
  xây dựng danh mục đầu tư mới, phân bổ tỷ trọng giữa các cổ phiếu, tối ưu danh mục hiện có,
  đánh giá rủi ro danh mục, rebalance danh mục, phân tích tương quan giữa các cổ phiếu,
  tính Sharpe ratio, backtest chiến lược phân bổ, hoặc bất kỳ yêu cầu nào liên quan đến
  "danh mục", "portfolio", "phân bổ", "allocation", "tỷ trọng", "rebalance",
  "đa dạng hóa", "diversification", "rủi ro danh mục".
  Kích hoạt cả khi người dùng hỏi: "xây danh mục 500 triệu", "phân bổ thế nào cho hợp lý",
  "danh mục tôi có rủi ro cao không", "nên thêm/bớt mã nào", "tỷ trọng FPT bao nhiêu phần trăm",
  "tối ưu danh mục cho tôi".
---

# VN Portfolio Allocation — Phân bổ danh mục đầu tư Việt Nam

## Mục đích

Skill này giúp Claude xây dựng, phân bổ, tối ưu và quản lý danh mục đầu tư cổ phiếu Việt Nam, áp dụng các nguyên tắc quản trị danh mục hiện đại phù hợp với đặc thù thị trường VN.

## Thu thập thông tin đầu vào

Khi nhận yêu cầu về danh mục, cần xác định:

1. **Thông tin danh mục hiện có** (nếu đã có):
   - Danh sách mã, số lượng, giá mua, ngày mua
   - Tổng giá trị danh mục hiện tại

2. **Hồ sơ nhà đầu tư** (hỏi nếu chưa rõ):
   - Tổng vốn đầu tư
   - Khẩu vị rủi ro: bảo thủ / cân bằng / tích cực
   - Thời gian đầu tư: ngắn hạn (<6 tháng), trung hạn (6-24 tháng), dài hạn (>2 năm)
   - Mục tiêu: bảo toàn vốn, thu nhập cổ tức, tăng trưởng vốn
   - Ràng buộc đặc biệt: ngành muốn tránh, tỷ trọng tối đa mỗi mã...

3. **Dữ liệu thị trường**: file dữ liệu giá, chỉ số tài chính

## Các chiến lược phân bổ

### 1. Phân bổ theo trọng số tối ưu (Mean-Variance Optimization)

Dựa trên lý thuyết Markowitz, tìm danh mục tối ưu trên đường biên hiệu quả (Efficient Frontier).

**Quy trình:**
1. Tính ma trận lợi suất kỳ vọng (expected returns) cho mỗi cổ phiếu
2. Tính ma trận hiệp phương sai (covariance matrix) từ dữ liệu lịch sử
3. Tối ưu hóa để tìm:
   - Danh mục Sharpe tối đa (Maximum Sharpe Ratio)
   - Danh mục rủi ro tối thiểu (Minimum Variance)
   - Danh mục theo mức rủi ro mục tiêu
4. Áp các ràng buộc thực tế:
   - Tỷ trọng tối thiểu mỗi mã: 0% (hoặc theo yêu cầu)
   - Tỷ trọng tối đa mỗi mã: 20-30% (tránh tập trung)
   - Tỷ trọng tối đa mỗi ngành: 30-40%
5. Vẽ Efficient Frontier chart

**Code tham khảo:** xem `references/optimization_templates.md`

### 2. Phân bổ theo rủi ro cân bằng (Risk Parity)

Mỗi cổ phiếu đóng góp phần rủi ro bằng nhau vào danh mục.

**Quy trình:**
1. Tính volatility mỗi cổ phiếu
2. Tính ma trận tương quan
3. Tối ưu để risk contribution bằng nhau
4. Điều chỉnh tỷ trọng: cổ phiếu biến động mạnh → tỷ trọng thấp hơn

### 3. Phân bổ chiến lược (Strategic Allocation)

Phân bổ theo khung top-down, phù hợp cho nhà đầu tư dài hạn.

**Cấp 1 — Phân bổ theo vốn hóa:**
| Khẩu vị rủi ro | Large-cap | Mid-cap | Small-cap |
|-----------------|-----------|---------|-----------|
| Bảo thủ | 70% | 25% | 5% |
| Cân bằng | 50% | 35% | 15% |
| Tích cực | 30% | 40% | 30% |

**Cấp 2 — Phân bổ theo ngành:**
- Đa dạng tối thiểu 4-5 ngành
- Không quá 30% vào một ngành
- Xem xét chu kỳ kinh tế để overweight/underweight ngành

**Cấp 3 — Chọn cổ phiếu trong mỗi ngành:**
- Ưu tiên cổ phiếu đã qua sàng lọc (dùng kết quả từ skill Stock Screener)
- Ưu tiên cổ phiếu có điểm đánh giá cao (dùng kết quả từ skill Stock Evaluation)
- Mỗi mã chiếm 5-15% danh mục, tổng 8-15 mã

### 4. Phân bổ Core-Satellite

Kết hợp ổn định và cơ hội:
- **Core (60-80%)**: cổ phiếu blue-chip, VN30, lợi suất ổn định
- **Satellite (20-40%)**: cổ phiếu mid/small-cap tiềm năng, chiến lược momentum

## Phân tích danh mục

### Chỉ số hiệu suất

| Chỉ số | Công thức | Ý nghĩa |
|--------|-----------|---------|
| Tổng lợi nhuận | (NAV cuối - NAV đầu) / NAV đầu | Hiệu suất tổng |
| Annualized return | (1 + Total return)^(252/n) - 1 | Lợi suất quy năm |
| Volatility | σ hàng năm (annualized std) | Rủi ro tổng thể |
| Sharpe Ratio | (Rp - Rf) / σp | Lợi suất điều chỉnh rủi ro |
| Sortino Ratio | (Rp - Rf) / σ_downside | Chỉ tính rủi ro giảm giá |
| Max Drawdown | (Trough - Peak) / Peak | Mức sụt giảm tối đa |
| Beta | Cov(Rp, Rm) / Var(Rm) | Rủi ro hệ thống vs VN-Index |
| Alpha | Rp - [Rf + β × (Rm - Rf)] | Lợi suất vượt trội |
| Calmar Ratio | Annualized return / Max drawdown | Hiệu suất / rủi ro sụt giảm |

### Phân tích rủi ro

1. **Rủi ro tập trung:**
   - Tỷ trọng top 3 mã (nên < 50%)
   - Tỷ trọng ngành lớn nhất (nên < 35%)
   - HHI index (Herfindahl) đo mức tập trung

2. **Rủi ro tương quan:**
   - Ma trận tương quan giữa các mã
   - Cảnh báo nếu nhiều cặp tương quan > 0.7
   - Đề xuất thêm mã có tương quan thấp để đa dạng hóa

3. **Stress testing:**
   - Mô phỏng danh mục trong các kịch bản: VN-Index giảm 10%, 20%, 30%
   - Xem lại hiệu suất trong các giai đoạn thị trường giảm sâu lịch sử (2020 COVID, 2022...)

4. **Value at Risk (VaR):**
   - VaR 95%: mức lỗ tối đa trong 95% trường hợp
   - Tính bằng historical simulation hoặc parametric

## Rebalance danh mục

Khi người dùng yêu cầu rebalance:

1. So sánh tỷ trọng hiện tại vs tỷ trọng mục tiêu
2. Tính chênh lệch từng mã
3. Đề xuất giao dịch cụ thể: mua thêm mã nào, bán bớt mã nào, số lượng bao nhiêu
4. Tính phí giao dịch ước tính (phí mua bán cổ phiếu VN: ~0.15-0.35%)
5. Đánh giá tác động thuế (nếu có lãi thực hiện)

**Ngưỡng rebalance đề xuất:**
- Rebalance khi tỷ trọng chênh > 5% so với mục tiêu
- Hoặc rebalance định kỳ: hàng quý hoặc nửa năm

## Backtest

Khi người dùng muốn kiểm tra chiến lược phân bổ:

1. Áp tỷ trọng vào dữ liệu giá lịch sử
2. Tính NAV hàng ngày/tuần
3. Tính các chỉ số hiệu suất
4. So sánh với benchmark (VN-Index, VN30)
5. Vẽ biểu đồ hiệu suất tích lũy
6. Phân tích drawdown

## Cấu trúc báo cáo danh mục

```
# Báo cáo danh mục đầu tư
## 1. Tổng quan danh mục
   - Tổng giá trị, số mã, ngày lập/cập nhật
## 2. Cấu trúc phân bổ
   - Bảng tỷ trọng từng mã
   - Biểu đồ tròn phân bổ theo ngành
   - Biểu đồ phân bổ theo vốn hóa
## 3. Hiệu suất (nếu có dữ liệu lịch sử)
   - Các chỉ số hiệu suất
   - Biểu đồ NAV vs benchmark
## 4. Phân tích rủi ro
   - Ma trận tương quan (heatmap)
   - VaR, Max Drawdown
   - Rủi ro tập trung
## 5. Đề xuất
   - Rebalance cần thiết
   - Gợi ý đa dạng hóa
```

## Đặc thù thị trường Việt Nam

Khi phân bổ danh mục VN, lưu ý:

- **Biên độ dao động**: HOSE ±7%, HNX ±10%, UPCOM ±15% — ảnh hưởng đến ước tính volatility
- **Thanh khoản**: nhiều mid/small-cap thanh khoản thấp → khó rebalance → cần giới hạn tỷ trọng
- **Room ngoại**: một số mã hết room → ảnh hưởng khả năng mua
- **T+2.5**: chu kỳ thanh toán → ảnh hưởng đến khả năng rebalance nhanh
- **Phí giao dịch**: phí mua 0.15-0.25%, phí bán 0.15-0.25% + thuế bán 0.1%
- **Lô giao dịch**: HOSE 100cp/lô — cần làm tròn khi tính số lượng
- **Tương quan cao**: nhiều cổ phiếu VN có tương quan cao với VN-Index → đa dạng hóa thực sự khó hơn so với thị trường phát triển

## Nguyên tắc quan trọng

- Không đưa khuyến nghị mua/bán cụ thể — chỉ cung cấp phân tích và đề xuất phân bổ
- Luôn cảnh báo rằng hiệu suất quá khứ không đảm bảo kết quả tương lai
- Nhắc người dùng về chi phí giao dịch khi rebalance (đặc biệt danh mục nhỏ)
- Khuyến khích đầu tư dài hạn và kỷ luật rebalance thay vì giao dịch liên tục
