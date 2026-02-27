---
name: vn-stock-screener
description: >
  Sàng lọc và tìm kiếm cổ phiếu trên thị trường chứng khoán Việt Nam (HOSE, HNX, UPCOM).
  Sử dụng skill này khi người dùng muốn: lọc cổ phiếu theo tiêu chí tài chính (P/E, P/B, ROE, EPS...),
  tìm cổ phiếu theo ngành/nhóm ngành, sàng lọc theo chỉ báo kỹ thuật (RSI, MACD, volume đột biến...),
  tìm cổ phiếu breakout/breakdown, lọc theo thanh khoản hoặc vốn hóa,
  hoặc bất kỳ yêu cầu nào liên quan đến "tìm", "lọc", "sàng lọc", "screener", "scanner" cổ phiếu.
  Kích hoạt cả khi người dùng hỏi dạng: "cổ phiếu nào có P/E thấp", "tìm cổ phiếu ngành ngân hàng",
  "cổ phiếu nào tăng mạnh hôm nay", "lọc cổ phiếu vốn hóa lớn".
---

# VN Stock Screener — Sàng lọc cổ phiếu Việt Nam

## Mục đích

Skill này giúp Claude sàng lọc cổ phiếu từ dữ liệu có sẵn của người dùng, áp dụng các bộ lọc đa chiều để tìm ra cổ phiếu phù hợp với tiêu chí đầu tư.

## Dữ liệu đầu vào

Người dùng đã có sẵn dữ liệu. Khi nhận yêu cầu sàng lọc:

1. Hỏi người dùng đường dẫn file dữ liệu nếu chưa rõ (CSV, Excel, database, hoặc API endpoint)
2. Đọc và hiểu cấu trúc dữ liệu trước khi xử lý
3. Xác nhận các cột/trường có sẵn để biết có thể lọc theo tiêu chí nào

## Các bộ lọc hỗ trợ

### Bộ lọc cơ bản (Fundamental)

| Tiêu chí | Mô tả | Ví dụ điều kiện |
|-----------|--------|-----------------|
| P/E (TTM) | Giá / Lợi nhuận trên cổ phiếu | P/E < 15 |
| P/B | Giá / Giá trị sổ sách | P/B < 1.5 |
| ROE | Lợi nhuận trên vốn chủ sở hữu | ROE > 15% |
| ROA | Lợi nhuận trên tổng tài sản | ROA > 5% |
| EPS | Lợi nhuận trên cổ phiếu | EPS > 3000 VND |
| Tăng trưởng doanh thu | YoY revenue growth | > 10% |
| Tăng trưởng lợi nhuận | YoY profit growth | > 15% |
| Biên lợi nhuận ròng | Net profit margin | > 10% |
| Nợ/Vốn chủ (D/E) | Debt to equity ratio | D/E < 1 |
| Cổ tức | Dividend yield | > 5% |
| Vốn hóa | Market cap | > 1000 tỷ VND |

### Bộ lọc kỹ thuật (Technical)

| Tiêu chí | Mô tả |
|-----------|--------|
| RSI | Quá mua (>70) / Quá bán (<30) |
| MACD | Cắt lên/cắt xuống đường signal |
| MA crossover | MA ngắn cắt MA dài (Golden/Death cross) |
| Volume | Đột biến khối lượng (> 2x trung bình 20 phiên) |
| Bollinger Bands | Giá chạm band trên/dưới |
| Giá vs MA | Giá trên/dưới MA20, MA50, MA200 |

### Bộ lọc phân loại

| Tiêu chí | Giá trị |
|-----------|---------|
| Sàn | HOSE, HNX, UPCOM |
| Ngành ICB | Ngân hàng, Bất động sản, Công nghệ, Thép, Dầu khí... |
| Vốn hóa | Large-cap (>10K tỷ), Mid-cap (1K-10K tỷ), Small-cap (<1K tỷ) |
| Rổ chỉ số | VN30, VN100, HNX30 |

## Quy trình xử lý

Khi nhận yêu cầu sàng lọc:

1. **Hiểu yêu cầu**: Phân tích câu hỏi để xác định các tiêu chí lọc. Nếu yêu cầu mơ hồ (vd: "cổ phiếu tốt"), hỏi lại để làm rõ chiến lược đầu tư (giá trị, tăng trưởng, cổ tức, momentum...).

2. **Load dữ liệu**: Đọc file dữ liệu, kiểm tra các cột có sẵn. Nếu thiếu cột cần thiết, thông báo cho người dùng và đề xuất tiêu chí thay thế.

3. **Áp dụng bộ lọc**: Viết code Python (pandas) để lọc dữ liệu. Áp dụng các điều kiện theo thứ tự từ quan trọng nhất đến ít quan trọng nhất.

4. **Trình bày kết quả**: 
   - Hiển thị bảng kết quả có sắp xếp hợp lý
   - Ghi chú số lượng cổ phiếu qua từng bước lọc (funnel)
   - Nếu kết quả quá nhiều (>30), đề xuất thêm bộ lọc để thu hẹp
   - Nếu kết quả rỗng, nới lỏng điều kiện và thông báo

5. **Xuất kết quả**: Lưu ra file CSV/Excel nếu người dùng yêu cầu.

## Các chiến lược sàng lọc có sẵn

Khi người dùng không chỉ rõ tiêu chí, đề xuất các chiến lược phổ biến:

**Cổ phiếu giá trị (Value):**
- P/E < 12, P/B < 1.5, ROE > 12%, D/E < 1, cổ tức > 3%

**Cổ phiếu tăng trưởng (Growth):**
- Tăng trưởng EPS > 20% YoY, tăng trưởng doanh thu > 15%, ROE > 15%

**Cổ phiếu momentum:**
- RSI 50-70, giá trên MA50, volume trung bình 20 phiên > 500K, MACD > 0

**Cổ phiếu cổ tức (Dividend):**
- Dividend yield > 5%, payout ratio < 70%, lợi nhuận ổn định 3 năm

**Cổ phiếu phục hồi (Turnaround):**
- P/B < 1, RSI < 35, lợi nhuận quý gần nhất cải thiện vs quý trước

## Lưu ý quan trọng

- Luôn ghi rõ nguồn dữ liệu và thời điểm dữ liệu được cập nhật
- Kết quả sàng lọc chỉ là bước đầu tiên — nhắc người dùng cần phân tích sâu hơn trước khi ra quyết định
- Không đưa ra khuyến nghị mua/bán cụ thể, chỉ cung cấp dữ liệu khách quan
- Khi so sánh tiêu chí tài chính, nên so trong cùng ngành vì mỗi ngành có đặc thù riêng (vd: P/E ngân hàng khác P/E công nghệ)
