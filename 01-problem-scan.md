# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinmec** | Tốn thời gian | Bác sĩ mất nhiều thời gian (ước tính 20–30 phút/ca) để rà soát kết quả xét nghiệm, bệnh án cũ, đơn thuốc và quá trình điều trị nhằm viết bản tóm tắt hồ sơ xuất viện cho bệnh nhân. |
| 2 | **Vinhomes** | Lặp lại | Ban quản lý tòa nhà phải đọc, phân loại thủ công các ticket báo lỗi (điện, nước, vệ sinh) trên app cư dân, xác định mức độ ưu tiên và điều phối đến đúng tổ kỹ thuật. |
| 3 | **VinFast** | Pain từ người khác | Khách hàng phàn nàn vì phải chờ lâu khi gọi tổng đài hoặc nhận được câu trả lời chưa chính xác khi hỏi về các mã lỗi cảnh báo kỹ thuật hiển thị trên màn hình xe VF8/VF9. |
| 4 | **Xanh SM** | Lặp lại | Nhân viên vận hành mất nhiều thời gian nhập lại thông tin và đối chiếu thủ công từ ảnh chụp bằng lái, CCCD, đăng kiểm để xác minh hồ sơ đối tác tài xế mới. |
| 5 | **Vinpearl** | AI-upgrade | Khách du lịch gặp khó khăn khi tự lên lịch trình tối ưu tại VinWonders; chatbot hiện tại chủ yếu trả lời theo template, chưa cá nhân hóa gợi ý dựa trên thời tiết, độ tuổi, sở thích và thời gian vui chơi của gia đình khách. |
---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

**Top 3 bài toán là: Số 1, 2 và 5**

**Lý do lựa chọn Top 3:**

- **Vinmec:** Có Business Impact lớn vì giảm thời gian xử lý hồ sơ xuất viện cho bác sĩ. Công việc đòi hỏi tổng hợp lượng lớn dữ liệu phi cấu trúc, phù hợp với khả năng xử lý ngôn ngữ của LLM. Tuy nhiên, cần kiểm soát nghiêm ngặt độ chính xác và có bác sĩ phê duyệt.
- **Vinhomes:** Quy trình phân loại ticket có tính lặp lại cao, đầu vào chủ yếu là văn bản và hình ảnh, có thể áp dụng LLM kết hợp Rule-based để tự động hóa. Dễ xây dựng prototype và đo lường hiệu quả.
- **Vinpearl:** Có tiềm năng cải thiện trực tiếp trải nghiệm khách hàng, giảm tải cho tổng đài và tăng tương tác trên app. AI có thể kết hợp LLM với hệ thống gợi ý và thuật toán tối ưu lịch trình.

## Card #1: Vinmec - Tóm tắt bệnh án xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tự động tổng hợp dữ liệu bệnh án, kết quả xét     │
│ nghiệm, đơn thuốc và quá trình điều trị để tạo bản dự thảo  │
│ tóm tắt hồ sơ xuất viện cho bác sĩ kiểm tra và phê duyệt.   │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị.                       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khám/Điều trị ──> 2. Mở và rà soát kết quả EMR,        │
│   xét nghiệm, đơn thuốc ──> 3. Soạn thảo bản tóm tắt        │
│   xuất viện ──> 4. Bác sĩ kiểm tra và ký duyệt              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3                 │
│ (⏱ Ước tính 25 phút/ca)                                     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Trích xuất dữ liệu có cấu trúc từ EMR, tổng hợp thông tin  │
│ và sinh draft bản tóm tắt theo mẫu chuẩn y khoa)            │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian soạn thảo từ 25 phút xuống dưới 5 phút/ca│
│   - 100% bản tóm tắt được bác sĩ kiểm tra trước khi ký duyệt│
│   - Không tự suy diễn hoặc bổ sung dữ liệu y khoa không có  │
│     trong hồ sơ gốc.                                        │
│ Quick Architecture: [ ] No AI [ ] Rule [x] LLM [ ] Agent    │                    
└─────────────────────────────────────────────────────────────┘
```

## Card #2: Vinhomes - Phân loại ticket sửa chữa của cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân loại, trích xuất thông tin, xác định │
│ mức độ ưu tiên và điều hướng ticket yêu cầu kỹ thuật dựa    │
│ trên nội dung văn bản và hình ảnh cư dân gửi lên app.       │
│ Công ty thành viên:  [x] Vinhomes                           │
│                                                             │
│ Ai đang đau (Actor)? Ban quản lý (Nhân viên CSKH tòa nhà),  │
│ tổ kỹ thuật tiếp nhận và xử lý sự cố.                       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi ticket ──> 2. Lễ tân/CSKH đọc nội dung,     │
│   kiểm tra hình ảnh và xác minh thông tin ──>               │
│   3. Gắn tag loại sự cố, xác định mức độ ưu tiên ──>        │
│   4. Chuyển ticket đến tổ kỹ thuật phụ trách.               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3                 │
│ (⏱ Ước tính 5–10 phút/lượt)                                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (LLM/Vision phân tích nội dung text và ảnh, trích xuất      │
│ thông tin, phân loại sự cố và trả về JSON có cấu trúc.)     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Classification Accuracy đạt ít nhất 90%.                │
│   - Ít nhất 80% ticket đủ điều kiện được tự động phân tuyến.│
│   - P95 thời gian phân loại và định tuyến không quá 60 giây.│
│   - Ticket khẩn cấp không được tự động bỏ qua bước xác minh │
│     khi thông tin chưa đầy đủ hoặc không đủ tin cậy.        │
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent    │
└─────────────────────────────────────────────────────────────┘
```

## Card #5: Vinpearl - Lên lịch trình tối ưu

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán: Xây dựng trợ lý AI cá nhân hóa lịch trình vui     │
│ chơi tại VinWonders, gợi ý địa điểm và hoạt động phù hợp    │
│ theo độ tuổi, sở thích, thời tiết và thời gian của khách.   │
│ Công ty thành viên: [x] Vinpearl                            │
│                                                             │
│ Ai đang đau (Actor)? Khách du lịch (khó lựa chọn lịch       │
│ trình phù hợp), nhân viên CSKH (tốn thời gian tư vấn lặp lại│
│ các câu hỏi về địa điểm và hoạt động vui chơi).             │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách mở app và hỏi chatbot ──> 2. Chatbot rule-based  │
│   trả lời theo menu/template, chưa hiểu đầy đủ nhu cầu ──>  │
│   3. Khách không tìm được lịch trình phù hợp nên gọi hotline│
│   ──> 4. Nhân viên CSKH tư vấn thủ công và gợi ý địa điểm.  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4                 │
│ (⏱ Ước tính 10–15 phút/khách cần tư vấn trực tiếp)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2                │
│ (LLM hiểu yêu cầu tự nhiên, Recommendation Engine gợi ý     │
│ địa điểm, Constraint Optimizer xây dựng lịch trình phù hợp  │
│ với thời tiết, độ tuổi, thời gian và điều kiện hoạt động.)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm ít nhất 40% số cuộc gọi hotline liên quan đến      │
│     tư vấn lịch trình so với baseline trước triển khai.     │
│   - Tăng 20% Engagement Rate của trợ lý AI trên app so với  │
│     baseline trước triển khai.                              │
│   - Ít nhất 90% lịch trình đề xuất không vi phạm các        │
│     điều kiện về độ tuổi, thời gian và trạng thái hoạt động.│
│                                                             │
│ Quick Architecture: [ ] No AI [x] Rule [x] LLM [ ] Agent    │            
└─────────────────────────────────────────────────────────────┘
```
