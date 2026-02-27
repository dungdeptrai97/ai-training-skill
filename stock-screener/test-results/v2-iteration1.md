# Iteration 0 — Baseline Test Results (SKILL v1)

## Tổng quan

| Test | Tên | Độ khó | Kết quả | Điểm | Vấn đề chính |
|------|-----|--------|---------|------|---------------|
| #1 | basic-pe-filter | Easy | ✅ PASS | 9/10 | Thiếu cảnh báo P/E âm |
| #2 | multi-criteria-value | Medium | ⚠️ PARTIAL | 6/10 | "Nợ thấp" bị dịch sai |
| #3 | vague-request | Medium | ❌ FAIL | 3/10 | Trả kết quả luôn, không hỏi lại |
| #4 | sector-banking | Hard | ❌ FAIL | 2/10 | Áp D/E cho ngân hàng |
| #5 | golden-cross | Hard | ⚠️ PARTIAL | 5/10 | Không tính được cross timing |
| #6 | empty-result | Medium | ⚠️ PARTIAL | 5/10 | Nới lỏng nhưng không cho funnel |
| #7 | combined-fund-tech | Medium | ✅ PASS | 8/10 | OK nhưng thiếu giải thích ý nghĩa |
| #8 | natural-language | Hard | ❌ FAIL | 3/10 | Không dịch NLP → bộ lọc |

**Tổng: 41/80 (51.3%)**

---

## Phân tích chi tiết từng test case bị fail/partial

### ❌ Test #3: "Cổ phiếu nào đáng mua bây giờ?"

**Mong đợi:** Claude hỏi lại chiến lược đầu tư, đề xuất các option.

**Thực tế (v1):** Claude chạy thẳng chiến lược "Growth" và trả kết quả luôn, không hỏi lại.

**Nguyên nhân gốc:** SKILL.md nói "Nếu yêu cầu mơ hồ, hỏi lại" nhưng không cho ví dụ CỤ THỂ 
câu nào là mơ hồ, câu nào là rõ ràng. Claude không biết phân biệt.

**Cần sửa:**
- Thêm ví dụ phân loại: câu rõ ràng vs câu mơ hồ
- Thêm quy tắc rõ ràng: nếu thiếu ≥2 tiêu chí cụ thể → bắt buộc hỏi lại

---

### ❌ Test #4: Ngân hàng NIM > 3.5%, NPL < 2%

**Mong đợi:** Dùng chỉ số đặc thù ngành ngân hàng.

**Thực tế (v1):** Claude cố gắng dùng D/E ratio và current ratio cho ngân hàng → kết quả vô nghĩa 
(D/E ngân hàng luôn > 10 vì bản chất đòn bẩy cao). Không nhận ra cần chỉ số riêng.

**Nguyên nhân gốc:** SKILL.md có nói "mỗi ngành có đặc thù riêng" trong phần Lưu ý, nhưng:
1. Chỉ nói chung chung, không liệt kê CỤ THỂ ngành nào cần chỉ số gì
2. Không có logic: "NẾU ngành = ngân hàng THÌ dùng NIM, NPL, CASA, CIR"
3. Không cảnh báo: "KHÔNG dùng D/E, current ratio cho ngân hàng"

**Cần sửa:**
- Thêm section riêng: "Chỉ số đặc thù theo ngành"
- Liệt kê rõ: ngân hàng → NIM, NPL, CASA, CIR, CAR, tăng trưởng tín dụng
- Thêm rule: skip D/E, current ratio cho ngành tài chính

---

### ❌ Test #8: "Cổ phiếu công nghệ tăng trưởng mạnh, chưa quá đắt, thanh khoản tốt"

**Mong đợi:** Dịch ngôn ngữ tự nhiên → bộ lọc cụ thể, giải thích logic.

**Thực tế (v1):** Claude lọc toàn bộ mã có EPS growth > 0, không lọc ngành, 
không giải thích cách dịch. Trả 50+ mã không liên quan.

**Nguyên nhân gốc:** SKILL.md không có hướng dẫn "dịch" ngôn ngữ tự nhiên.
Không có mapping: "tăng trưởng mạnh" = gì? "chưa quá đắt" = gì? "thanh khoản tốt" = gì?

**Cần sửa:**
- Thêm section: "Dịch ngôn ngữ tự nhiên thành bộ lọc"
- Bảng mapping từ khóa → tiêu chí
- Quy tắc: luôn giải thích logic dịch trước khi chạy lọc

---

### ⚠️ Test #2: "P/E < 12, ROE > 15%, nợ thấp, có trả cổ tức"

**Vấn đề:** "Nợ thấp" bị dịch thành D/E < 0.5 (quá khắt khe) thay vì D/E < 1.
Không giải thích tại sao chọn ngưỡng 0.5.

**Cần sửa:** Thêm bảng mapping cho các cụm từ mơ hồ với ngưỡng mặc định.

---

### ⚠️ Test #6: "P/E < 3, ROE > 30%, div > 10%"

**Vấn đề:** Đúng: báo 0 kết quả. Sai: nới lỏng tất cả cùng lúc thay vì từng tiêu chí,
không cho thấy nếu chỉ bỏ 1 điều kiện thì được bao nhiêu mã.

**Cần sửa:** Thêm quy trình nới lỏng từng bước (relaxation waterfall).

---

## Tổng hợp 5 điểm yếu chính cần sửa

| # | Điểm yếu | Mức ảnh hưởng | Xuất hiện ở test |
|---|----------|---------------|------------------|
| W1 | Không phân biệt được câu mơ hồ vs rõ ràng | Cao | #3, #8 |
| W2 | Không xử lý ngành đặc thù (ngân hàng, BĐS...) | Cao | #4 |
| W3 | Không dịch được ngôn ngữ tự nhiên → bộ lọc | Cao | #8, #2 |
| W4 | Nới lỏng kết quả rỗng chưa có hệ thống | Trung bình | #6 |
| W5 | Thiếu cảnh báo edge case (P/E âm, ngành tài chính) | Trung bình | #1, #4 |
