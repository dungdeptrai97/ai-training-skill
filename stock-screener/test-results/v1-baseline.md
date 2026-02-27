# Iteration 1 — Test Results (SKILL v2)

## So sánh v1 → v2

| Test | Tên | v1 | v2 | Thay đổi | Ghi chú |
|------|-----|-----|-----|----------|---------|
| #1 | basic-pe-filter | 9/10 | 10/10 | +1 ✅ | Đã cảnh báo P/E âm |
| #2 | multi-criteria-value | 6/10 | 8/10 | +2 ✅ | "Nợ thấp" → D/E < 0.8 đúng hơn |
| #3 | vague-request | 3/10 | 9/10 | +6 ✅ | Hỏi lại đúng, đề xuất 5 chiến lược |
| #4 | sector-banking | 2/10 | 8/10 | +6 ✅ | Dùng NIM, NPL thay vì D/E |
| #5 | golden-cross | 5/10 | 8/10 | +3 ✅ | Có code tính cross, check timing |
| #6 | empty-result | 5/10 | 9/10 | +4 ✅ | Relaxation waterfall hoạt động tốt |
| #7 | combined-fund-tech | 8/10 | 9/10 | +1 ✅ | Thêm giải thích ý nghĩa |
| #8 | natural-language | 3/10 | 7/10 | +4 ✅ | Dịch NLP tốt hơn, nhưng còn vấn đề |

**Tổng: 41/80 → 68/80 (51.3% → 85.0%) ↑ +33.7%**

---

## Phân tích: Những gì đã cải thiện tốt

### ✅ Test #3 (3→9): Xử lý câu hỏi mơ hồ
**Trước (v1):** Claude chạy thẳng, trả kết quả không ai yêu cầu.
**Sau (v2):** Claude hỏi lại đúng format, đề xuất 5 chiến lược với emoji, giải thích rõ.
**Tại sao cải thiện:** Thêm phân loại A/B/C với ví dụ CỤ THỂ giúp Claude biết khi nào cần hỏi lại.

### ✅ Test #4 (2→8): Ngành ngân hàng
**Trước (v1):** Dùng D/E = 12 cho ngân hàng → kết quả vô nghĩa.
**Sau (v2):** Tự động chuyển sang NIM, NPL, CASA. Ghi rõ "KHÔNG dùng D/E cho ngân hàng".
**Tại sao cải thiện:** Bảng chỉ số riêng theo ngành + cảnh báo rõ ràng "KHÔNG dùng X cho Y".

### ✅ Test #6 (5→9): Xử lý kết quả rỗng
**Trước (v1):** Nới lỏng hết cùng lúc, người dùng không biết nên bỏ điều kiện nào.
**Sau (v2):** Relaxation waterfall: thử bỏ/nới từng tiêu chí → cho thấy từng cái giải phóng bao nhiêu mã.
**Tại sao cải thiện:** Quy trình nới lỏng từng bước được mô tả chi tiết với ví dụ.

---

## Phân tích: Những vấn đề MỚI phát hiện trong v2

### ⚠️ Vấn đề mới N1: Filtering funnel quá dài cho multi-criteria

Khi yêu cầu có 5-6 tiêu chí, funnel hiển thị 6 dòng → choáng ngợp, 
mất focus. Người dùng chỉ cần biết: "bắt đầu bao nhiêu → cuối còn bao nhiêu → tiêu chí nào loại nhiều nhất".

**Xuất hiện ở:** Test #2 (funnel 5 bước), Test #7 (funnel 4 bước)

**Cần sửa:** Funnel compact: chỉ hiện dòng đầu, dòng cuối, và bước loại nhiều nhất.

---

### ⚠️ Vấn đề mới N2: Bước xác nhận NLP dịch quá chậm

Test #8: Claude dịch xong, hỏi "Bạn có muốn điều chỉnh không?" rồi DỪNG, CHỜ user.
Nhưng 90% trường hợp user sẽ nói "OK" → thêm 1 lượt chat không cần thiết.

**Cần sửa:** Dịch + chạy luôn + cho biết logic dịch. Nếu user không đồng ý → điều chỉnh sau.
Chuyển từ "xin phép trước" → "làm trước, giải thích kèm, sửa nếu sai".

---

### ⚠️ Vấn đề mới N3: Không sort kết quả theo tiêu chí ưu tiên

Khi lọc xong, bảng kết quả sắp xếp theo mã ticker (A-Z) → vô nghĩa.
Nên sort theo tiêu chí quan trọng nhất trong yêu cầu.

**Ví dụ:**
- "Lọc P/E thấp" → sort theo P/E tăng dần
- "Lọc cổ tức cao" → sort theo dividend yield giảm dần
- "Lọc tăng trưởng" → sort theo EPS growth giảm dần
- Multi-criteria → sort theo composite rank

**Xuất hiện ở:** Test #1 (sort A-Z thay vì P/E), Test #2 (không rõ sort logic)

**Cần sửa:** Thêm quy tắc sort thông minh.

---

### ⚠️ Vấn đề mới N4: Thiếu context khi trình bày kết quả

Kết quả chỉ có số liệu khô: "HPG P/E=9.8 ROE=14.2%". 
Không cho biết: so với ngành thì HPG đang ở đâu? P/E=9.8 là tốt hay bình thường cho ngành thép?

**Cần sửa:** Thêm cột benchmark ngành hoặc dòng ghi chú "Trung bình ngành Thép: P/E=11.2"

---

### ⚠️ Vấn đề nhỏ N5: Đối với test #4, dữ liệu không có NIM

Claude thông báo thiếu NIM, đề xuất thay thế — OK.
Nhưng CÁCH thông báo quá dài (4 dòng giải thích) → nên gọn hơn.

---

## Tổng hợp 5 điểm yếu v2 cần sửa trong iteration 2

| # | Điểm yếu | Mức ảnh hưởng | Test liên quan |
|---|----------|---------------|----------------|
| N1 | Funnel quá dài, cần compact | Trung bình | #2, #7 |
| N2 | Bước xác nhận NLP thừa, gây chậm | Cao | #8 |
| N3 | Không sort kết quả theo tiêu chí ưu tiên | Cao | #1, #2, #7 |
| N4 | Thiếu benchmark ngành trong kết quả | Trung bình | #1, #2, #4 |
| N5 | Thông báo thiếu dữ liệu quá dài | Thấp | #4 |
