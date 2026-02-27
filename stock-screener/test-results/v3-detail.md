# Iteration 2 — Test Results (SKILL v3)

## So sánh v1 → v2 → v3

| Test | Tên | v1 | v2 | v3 | Trend |
|------|-----|-----|-----|-----|-------|
| #1 | basic-pe-filter | 9 | 10 | 10 | ✅ Đã hoàn thiện |
| #2 | multi-criteria-value | 6 | 8 | 10 | ✅ Sort + benchmark ngành |
| #3 | vague-request | 3 | 9 | 10 | ✅ Đã hoàn thiện |
| #4 | sector-banking | 2 | 8 | 9 | ✅ Thông báo gọn hơn |
| #5 | golden-cross | 5 | 8 | 9 | ✅ Tốt |
| #6 | empty-result | 5 | 9 | 10 | ✅ Đã hoàn thiện |
| #7 | combined-fund-tech | 8 | 9 | 10 | ✅ Compact funnel + sort |
| #8 | natural-language | 3 | 7 | 9 | ✅ Chạy luôn, không hỏi thừa |

**Tổng: 41 → 68 → 77/80**
**Phần trăm: 51.3% → 85.0% → 96.3%**

---

## Chi tiết cải thiện v2 → v3

### Test #2 (8→10): Compact funnel + Smart sort + Benchmark
**v2:** Funnel 5 dòng, sort theo ticker A-Z, không có benchmark.
**v3:** Funnel 1 dòng compact, sort theo composite rank, dòng TB ngành Thép cuối bảng.
Người dùng thấy ngay: HPG P/E=9.8 < TB ngành 11.2 → hấp dẫn.

### Test #8 (7→9): Không hỏi thừa nữa
**v2:** Dịch xong → hỏi "Bạn có muốn điều chỉnh?" → chờ → user nói OK → mới chạy.
**v3:** Dịch → chạy luôn → hiển thị logic dịch kèm kết quả → "Điều chỉnh? Ví dụ: nới P/E lên < 25"
Tiết kiệm 1 lượt chat, UX mượt hơn nhiều.

### Test #4 (8→9): Thông báo thiếu dữ liệu gọn hơn
**v2:** 4 dòng giải thích tại sao thiếu NIM, lịch sử NIM, cách tính NIM...
**v3:** 1 dòng: "⚠️ Thiếu NIM/NPL → dùng ROE, P/B thay thế."

---

## Đánh giá tổng thể qua 3 version

### Biểu đồ tiến triển

```
v1 (Baseline)  ████████████░░░░░░░░░░░░  41/80  (51.3%)
v2 (Iter 1)    █████████████████████░░░░  68/80  (85.0%)  ↑ +33.7%
v3 (Iter 2)    ███████████████████████░░  77/80  (96.3%)  ↑ +11.3%
```

### Những thay đổi có impact lớn nhất

| Thay đổi | Từ version | Điểm cải thiện | Test hưởng lợi |
|----------|-----------|---------------|----------------|
| Phân loại A/B/C + ví dụ | v1→v2 | +12 điểm | #3 (+6), #8 (+4), #2 (+2) |
| Chỉ số đặc thù ngành | v1→v2 | +6 điểm | #4 (+6) |
| Relaxation waterfall | v1→v2 | +4 điểm | #6 (+4) |
| Smart sort | v2→v3 | +4 điểm | #1, #2, #7 |
| Bỏ bước xác nhận NLP | v2→v3 | +2 điểm | #8 (+2) |
| Compact funnel | v2→v3 | +2 điểm | #2, #7 |
| Benchmark ngành | v2→v3 | +1 điểm | #2 |

### Top 3 nguyên tắc rút ra

1. **VÍ DỤ CỤ THỂ > Hướng dẫn chung chung**
   v1 nói "hỏi lại nếu mơ hồ" → Claude không biết câu nào mơ hồ.
   v2 cho VÍ DỤ: "cổ phiếu đáng mua = Nhóm C = hỏi lại" → Claude hiểu ngay.
   Bài học: Mỗi rule cần ít nhất 2-3 ví dụ.

2. **CẢNH BÁO TIÊU CỰC > Hướng dẫn tích cực**
   v1 nói "mỗi ngành có đặc thù" (tích cực, chung chung) → Claude bỏ qua.
   v2 nói "KHÔNG dùng D/E cho ngân hàng" (tiêu cực, cụ thể) → Claude tuân thủ.
   Bài học: Nói rõ "ĐỪNG LÀM X" hiệu quả hơn "hãy cẩn thận với X".

3. **OUTPUT FORMAT EXAMPLE > Mô tả output**
   v1 nói "hiển thị funnel" → Claude hiển thị kiểu random.
   v3 cho CHÍNH XÁC format output mẫu → Claude copy structure.
   Bài học: Show, don't tell. Cho mẫu output thay vì mô tả output.

---

## Còn cải thiện được gì nữa? (Future iterations)

| Hướng | Ưu tiên | Mô tả |
|-------|---------|-------|
| Thêm nhiều ví dụ edge case | Cao | Cổ phiếu penny, cổ phiếu mới lên sàn, cổ phiếu bị đình chỉ |
| Script Python tự động | TB | Thay vì Claude viết code mỗi lần, có sẵn script reusable |
| Lọc theo sự kiện | TB | Lọc cổ phiếu sắp chia cổ tức, sắp ĐHCĐ, vừa có insider trading |
| Multi-timeframe | Thấp | Kết hợp lọc fundamental (dài hạn) + technical (ngắn hạn) |
| Backtest bộ lọc | Thấp | "Nếu tôi dùng bộ lọc này 1 năm trước, kết quả thế nào?" |
