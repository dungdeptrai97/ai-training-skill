---
id: "FB-USER-QUESTIONS-AGG"
type: "report"
title: "Tổng hợp câu hỏi người dùng"
status: "active"
owner: "trang"
created: "2026-03-13"
updated: "2026-03-13"
version: "1.0"
tags: [feedback, user-questions, aggregation, analytics]
ai_summary: |
  File tổng hợp câu hỏi người dùng từ nhiều nguồn khác nhau.
  Dùng để phân tích nhu cầu, hành vi và pain points của users.
  Hỗ trợ định hướng phát triển sản phẩm và nội dung giáo dục.
---

# Tổng hợp câu hỏi người dùng

> Aggregation point cho tất cả câu hỏi users từ nhiều nguồn.

---

## Nguồn dữ liệu

| # | Nguồn | File | Số câu hỏi | Cập nhật |
|---|--------|------|-------------|----------|
| 1 | Chatlog Users | [user-questions-chatlog.md](./user-questions-chatlog.md) | 35 | 2026-03-13 |
| 2 | Facebook | [user-questions-facebook.md](./user-questions-facebook.md) | 22 | 2026-03-13 |
| 3 | YouTube | [user-questions-youtube.md](./user-questions-youtube.md) | 26 | 2026-03-13 |
| 4 | Zoom Webinar | [user-questions-zoom.md](./user-questions-zoom.md) | 34 | 2026-03-13 |

---

## Thống kê tổng hợp

### Theo nguồn

| Nguồn | Số câu hỏi | Đặc điểm |
|-------|-------------|----------|
| Chatlog | 35 | Tra cứu CK, tin tức, phân tích thị trường |
| Facebook | 22 | Tư vấn tài chính cá nhân, phân bổ tài sản |
| YouTube | 26 | Nhận định thị trường, phân tích ngành, vĩ mô |
| Zoom | 34 | Cơ cấu danh mục, tư vấn mã cụ thể, chốt lời |
| **Tổng** | **117** | |

### Theo danh mục (tổng hợp cả 4 nguồn)

| Danh mục | Chatlog | Facebook | YouTube | Zoom | Tổng | Tỷ lệ |
|----------|---------|----------|---------|------|------|--------|
| Cơ cấu / Phân bổ danh mục CK | 0 | 0 | 0 | 12 | 12 | 10% |
| Phân bổ tài sản / Tư vấn tài chính | 0 | 18 | 0 | 0 | 18 | 15% |
| Phân tích ngành / Mã CP cụ thể | 10 | 0 | 8 | 10 | 28 | 24% |
| Nhận định thị trường / Xu hướng CK | 6 | 0 | 7 | 4 | 17 | 15% |
| Vĩ mô / Lãi suất / Lạm phát | 7 | 0 | 6 | 0 | 13 | 11% |
| Chiến lược mua / bán / chốt lời | 0 | 0 | 3 | 5 | 8 | 7% |
| Tin tức doanh nghiệp | 4 | 0 | 0 | 0 | 4 | 3% |
| Học tập / Giáo dục | 3 | 0 | 0 | 0 | 3 | 3% |
| Sử dụng sản phẩm Simplize | 0 | 0 | 0 | 2 | 2 | 2% |
| BĐS | 0 | 0 | 2 | 0 | 2 | 2% |
| Gửi tiết kiệm tối ưu | 0 | 2 | 0 | 0 | 2 | 2% |
| Tái cơ cấu tài sản | 0 | 2 | 0 | 0 | 2 | 2% |
| Khác | 4 | 0 | 0 | 1 | 5 | 4% |
| **Tổng** | **35** | **22** | **26** | **34** | **117** | **100%** |

---

## Insights chính

### Từ Chatlog (sản phẩm)
1. **Tra cứu & định giá cổ phiếu** (~29% chatlog) → cần công cụ tra cứu nhanh, định giá trực quan
2. **Phân tích thị trường** → nhu cầu hiểu xu hướng VNINDEX, tâm lý thị trường
3. **Tin kinh tế vĩ mô** → users quan tâm chính sách, GDP, hạ tầng
4. **Câu hỏi bằng tiếng Anh** xuất hiện → cần hỗ trợ đa ngôn ngữ

### Từ Facebook (tài chính cá nhân)
1. **Phân bổ tài sản** là nhu cầu #1 (82% câu hỏi FB) → cơ hội lớn cho robo-advisor
2. **Thiếu kiến thức tài chính** → nhiều người tự nhận, cần content giáo dục
3. **Mục tiêu mua nhà** chiếm tỷ lệ cao → áp lực BĐS với người trẻ
4. **CCQ/ETF** được quan tâm bởi nhóm trẻ → phù hợp người mới
5. **Kênh phổ biến**: Tiết kiệm (15), Vàng (12), CK (11), BĐS (10), CCQ (7), Crypto (5)

### Từ YouTube (nhận định chuyên gia)
1. **Phân tích ngành & mã cụ thể** (31%) → users muốn expert view cho HPG, VRE, SSI, TCM, PVS...
2. **Nhận định thị trường** (27%) → xu hướng VNINDEX, timing, outlook 2026
3. **Vĩ mô chuyên sâu** (23%) → FED, lạm phát, dầu, JPY, nâng hạng
4. **Users có kiến thức TB-Nâng cao** → hỏi về margin, NAV, định giá, downtrend
5. **Phụ thuộc KOL** → dựa vào nhận định chuyên gia thay vì tự phân tích

### Từ Zoom (tư vấn danh mục)
1. **Cơ cấu danh mục** (35%) → users chia sẻ danh mục thực, xin tư vấn tỷ trọng
2. **Nhận định mã cụ thể** (29%) → HPG (9 lần), MWG (11 lần) được hỏi nhiều nhất
3. **Chiến lược chốt lời** (15%) → không biết khi nào bán, cần framework
4. **Users trình độ cao nhất** → hiểu PB, DCA, sector rotation, margin
5. **Gắn bó với Simplize** → nhắc Special Report, góc nhìn Simplize
6. **2 câu hỏi về sản phẩm** → cách tìm CP trên Simplize, vì sao report không có vùng giá mua/bán

### Cross-source insights

| Đặc điểm | Chatlog | Facebook | YouTube | Zoom |
|-----------|---------|----------|---------|------|
| Trình độ | Cơ bản-TB | Người mới | TB-Nâng cao | Nâng cao |
| Chủ đề chính | Tra cứu, tin tức | Tài chính cá nhân | Nhận định TT | Danh mục cụ thể |
| Tone | Trung tính | Chia sẻ cá nhân | Hỏi KOL | Trao đổi chuyên gia |
| Nhu cầu SP | Công cụ tra cứu | Robo-advisor | Expert analysis | Portfolio advisor |
| Engagement | Thấp | Trung bình | Trung bình | Cao |

**User journey (funnel)**:
- **Facebook** (người mới) → thu hút bằng financial planning tool
- **Chatlog** (đang dùng SP) → nâng cấp tra cứu & định giá
- **YouTube** (nâng cao) → cung cấp expert analysis & macro dashboard
- **Zoom** (loyal users) → portfolio advisor, chốt lời framework, Special Report upgrades

---

## Hướng hành động

### Sản phẩm (Product)
- [ ] **Portfolio analyzer / optimizer** → phân tích danh mục, gợi ý tỷ trọng (Zoom #1 demand)
- [ ] **Fair value / vùng giá mua-bán** → users cần biết giá entry & chốt lời (Zoom + YouTube)
- [ ] **Chốt lời framework** → công cụ/hướng dẫn dựa trên PB, PE, target price (Zoom)
- [ ] **Diversification checker** → cảnh báo danh mục quá tập trung (Zoom)
- [ ] **Robo-advisor / Asset allocation** → tài chính cá nhân cho người mới (Facebook)
- [ ] **Macro dashboard** → FED, lãi suất, lạm phát, commodities, FX (YouTube)
- [ ] **Ngành analysis tool** → so sánh ngành, tìm ngành lead (YouTube + Zoom)
- [ ] **Margin risk calculator** → cảnh báo margin/NAV cao (YouTube)
- [ ] Cải thiện tra cứu / định giá cổ phiếu (Chatlog)
- [ ] Bổ sung CCQ/ETF cho người mới (Facebook)
- [ ] Hỗ trợ bilingual VN/EN (Chatlog)

### Nội dung (Content)
- [ ] **Special Report upgrades** → thêm vùng giá mua/bán, update khi sự kiện lớn (Zoom)
- [ ] **Expert market commentary** → phân tích chuyên sâu (YouTube)
- [ ] Content giáo dục: phân tích cơ bản, kỹ thuật, tài chính cá nhân (Chatlog + Facebook)
- [ ] Hướng dẫn sử dụng Simplize → cách tìm CP, đọc report (Zoom)

### Thu thập dữ liệu
- [ ] Thu thập thêm từ: support tickets, in-app search, Zalo, email

---

## Cách thêm nguồn mới

1. Tạo file `user-questions-{source-name}.md` trong folder này
2. Phân loại câu hỏi theo danh mục
3. Cập nhật bảng "Nguồn dữ liệu" ở trên
4. Cập nhật thống kê tổng hợp

---

## Liên kết

- [FAQ Database](../../education/faq-database.md)
- [Customer Feedback Index](./_INDEX.md)

---

*Updated: 2026-03-13 by Trang*
