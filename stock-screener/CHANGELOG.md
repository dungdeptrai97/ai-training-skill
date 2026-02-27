# VN Stock Screener — Changelog cải tiến

## Tổng quan quy trình

```
┌─────────────────────────────────────────────────────────────────┐
│                    QUY TRÌNH CẢI TIẾN SKILL                     │
│                                                                  │
│   ┌─────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐  │
│   │ Viết v1 │───▶│ Test 8   │───▶│ Phát hiện│───▶│ Sửa → v2 │  │
│   │ (Draft) │    │ prompts  │    │ 5 lỗi    │    │          │  │
│   └─────────┘    └──────────┘    └──────────┘    └────┬─────┘  │
│                                                        │        │
│   ┌─────────┐    ┌──────────┐    ┌──────────┐         │        │
│   │ Sửa →v3 │◀──│ Phát hiện│◀──│ Test lại │◀────────┘        │
│   │         │    │ 5 lỗi MỚI│    │ 8 prompts│                  │
│   └────┬────┘    └──────────┘    └──────────┘                   │
│        │                                                         │
│        ▼                                                         │
│   ┌──────────┐                                                   │
│   │ Test lại │──▶ 77/80 (96.3%) ✅                              │
│   │ 8 prompts│                                                   │
│   └──────────┘                                                   │
└─────────────────────────────────────────────────────────────────┘
```

## Tiến triển điểm số

```
100 ┤
 95 ┤                                          ●──── v3: 96.3%
 90 ┤                                        ╱
 85 ┤                          ●────────────╱  v2: 85.0%
 80 ┤                        ╱
 75 ┤                      ╱
 70 ┤                    ╱
 65 ┤                  ╱
 60 ┤                ╱
 55 ┤              ╱
 50 ┤  ●─────────╱                             v1: 51.3%
 45 ┤
    └──┬──────────┬──────────────────────┬────
      v1        v2                      v3
   (baseline) (iter 1)              (iter 2)
```

## v1 → v2: Sửa 5 điểm yếu gốc (+33.7%)

| # | Điểm yếu v1 | Cách sửa trong v2 | Impact |
|---|-------------|-------------------|--------|
| W1 | Không phân biệt câu mơ hồ vs rõ ràng | Thêm phân loại A/B/C + 9 ví dụ cụ thể | ★★★ |
| W2 | Áp D/E cho ngân hàng (vô nghĩa) | Thêm bảng chỉ số riêng ngân hàng/BĐS/CK + rule "KHÔNG dùng D/E cho NH" | ★★★ |
| W3 | Không dịch NLP → bộ lọc | Thêm bảng mapping 16 từ khóa → tiêu chí + quy trình dịch | ★★★ |
| W4 | Nới lỏng kết quả rỗng không có hệ thống | Thêm Relaxation Waterfall: nới từng tiêu chí, show impact | ★★☆ |
| W5 | Thiếu cảnh báo P/E âm | Thêm rule: P/E < 0 = lỗ → tự loại | ★☆☆ |

**Kỹ thuật chính:** Thêm ví dụ cụ thể, thêm bảng tra cứu, thêm cảnh báo tiêu cực ("KHÔNG làm X")

## v2 → v3: Sửa 5 điểm yếu mới (+11.3%)

| # | Điểm yếu v2 | Cách sửa trong v3 | Impact |
|---|-------------|-------------------|--------|
| N1 | Funnel quá dài | Compact funnel 3 dòng + expand on demand | ★★☆ |
| N2 | Hỏi xác nhận NLP thừa → chậm | Chạy luôn + giải thích kèm + sửa nếu cần | ★★★ |
| N3 | Sort theo ticker A-Z (vô nghĩa) | Smart sort: tự chọn cột sort theo loại yêu cầu + composite rank | ★★★ |
| N4 | Thiếu benchmark ngành | Thêm dòng TB ngành cuối bảng kết quả | ★★☆ |
| N5 | Thông báo thiếu dữ liệu quá dài | Rút gọn còn 1 dòng | ★☆☆ |

**Kỹ thuật chính:** Tối ưu UX (bớt bước thừa), thêm output format mẫu, thêm context (benchmark)

## Cấu trúc thư mục

```
screener-improvement/
├── evals/
│   └── evals.json                    ← 8 test cases
├── iteration-0-baseline/
│   └── test-results/
│       └── report.md                 ← Điểm: 41/80 (51.3%)
├── iteration-1/
│   ├── SKILL-v2.md                   ← Bản sửa lần 1
│   └── test-results/
│       └── report.md                 ← Điểm: 68/80 (85.0%)
├── iteration-2/
│   ├── SKILL-v3.md                   ← Bản sửa lần 2
│   └── test-results/
│       └── report.md                 ← Điểm: 77/80 (96.3%)
└── CHANGELOG.md                      ← File này
```

## 3 bài học lớn nhất

### 1. Ví dụ cụ thể >> Hướng dẫn trừu tượng
```
❌ "Nếu yêu cầu mơ hồ, hãy hỏi lại"
   → Claude không biết "mơ hồ" nghĩa là gì

✅ "Ví dụ câu mơ hồ: 'cổ phiếu nào đáng mua?' → BẮT BUỘC hỏi lại"
   "Ví dụ câu rõ: 'lọc P/E < 10' → chạy ngay"
   → Claude hiểu ngay, phân loại chính xác
```

### 2. Nói "ĐỪNG LÀM" hiệu quả hơn "Hãy cẩn thận"
```
❌ "Mỗi ngành có đặc thù riêng, cần lưu ý khi so sánh"
   → Claude vẫn dùng D/E cho ngân hàng

✅ "KHÔNG dùng D/E, current ratio cho ngân hàng. Thay bằng NIM, NPL, CASA."
   → Claude tuân thủ 100%
```

### 3. Cho mẫu output thay vì mô tả output
```
❌ "Hiển thị bảng kết quả có sắp xếp hợp lý và ghi chú funnel"
   → Claude sắp xếp random, funnel format lung tung

✅ [Cho chính xác format output mẫu với dữ liệu giả]
   "📊 1,652 mã → [5 bộ lọc] → 15 mã ✅"
   → Claude copy đúng structure
```
