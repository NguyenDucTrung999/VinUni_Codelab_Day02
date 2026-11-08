# Phase 1: SCAN — Tìm kiếm cơ hội (Cá nhân)

| # | Công ty thành viên | Góc nhìn (Lens) | Mô tả ngắn bài toán |
|---|-------------------|----------------|---------------------|
| 1 | Xanh SM | Tốn thời gian | Điều phối viên tra thủ công trạm sạc có cổng phù hợp và soạn tin chỉ dẫn cho tài xế báo pin yếu (ước tính khoảng 9 phút; giả định 8-10 phút/lượt). |
| 2 | Vinmec | Pain từ người khác | Bác sĩ mất khoảng 20-30 phút/ca để viết tóm tắt xuất viện dễ hiểu cho bệnh nhân. |
| 3 | VinFast | Lặp lại | Đối chiếu giao dịch sạc điện hằng tuần giữa trạm đối tác và hệ thống tài chính. |
| 4 | Vinhomes | AI có thể tốt hơn | Chatbot cư dân trả lời thủ tục (thi công nội thất, vé gửi xe tháng) còn rập khuôn, cư dân phải ra văn phòng ban quản lý. |
| 5 | Vinpearl | Tốn thời gian | Đọc email đặt phòng theo đoàn để kiểm tra quỹ phòng và soạn lệnh giữ phòng. |
| 6 | VinFast | AI có thể tốt hơn | Cố vấn dịch vụ phải dịch mô tả lỗi tiếng Việt dân dã của khách sang nhóm hiện tượng kỹ thuật để mở phiếu. |

> Các con số thời gian là ước tính giả định, chưa đo thực tế.

---

# Phase 2: QUICK-ASSESS — 3 Quick Problem Cards (Cá nhân)

Top 3 chọn từ bảng SCAN: #1 (Xanh SM), #2 (Vinmec), #3 (VinFast).

## THẺ BÀI TOÁN SỐ 1: Xanh SM — Co-pilot xử lý xe sắp hết pin

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Co-pilot soạn nháp xử lý xe sắp hết pin:  │
│    gợi ý trạm sạc tương thích trong tầm an toàn, hoặc đề    │
│    xuất xe sạc di động khi pin dưới 5%.                     │
│                                                             │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên trung tâm điều vận (quá │
│    tải giờ cao điểm); tài xế đang chờ cứu trợ, mất doanh    │
│    thu.                                                     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi/nhắn báo pin yếu                            │
│   2. Điều phối viên xác định vị trí và % pin của xe         │
│   3. Tra thủ công trạm sạc trống có cổng phù hợp dòng xe    │
│   4. Soạn tin chỉ dẫn (đường đi, trạm) gửi qua app tài xế   │
│   5. Gọi xe cứu hộ/sạc di động nếu pin quá thấp             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (ước tính ~9     │
│    phút; khoảng 8-10 phút/lượt trên tổng ~12-15 phút).      │
│                                                             │
│ AI nhảy vào hỗ trợ ở bước nào? Bước 3-4 (đọc dữ liệu API đã │
│    có -> soạn nháp). Bước 5 do luật cứng quyết định.        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian từ lúc nhận báo -> tin được duyệt: ~12 phút   │
│     -> dưới 3 phút                                          │
│   - 100% tin gửi đi có điều phối viên duyệt (cờ             │
│     [DRAFT_ONLY])                                           │
│   - 0 lần đề xuất trạm > 5km khi pin < 5% (đo bằng bộ       │
│     adversarial test)                                       │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│    (LLM soạn nháp + Rule chặn ngưỡng pin)                   │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 2: Vinmec — Soạn nháp tóm tắt xuất viện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Soạn nháp bản tóm tắt xuất viện bằng ngôn │
│    ngữ dễ hiểu từ bệnh án điện tử để bác sĩ chỉnh sửa và ký │
│    duyệt.                                                   │
│                                                             │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị nội trú (quá tải giấy  │
│    tờ); bệnh nhân/người nhà (khó hiểu thuật ngữ).           │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bác sĩ mở bệnh án, đọc ghi chú diễn tiến và kết quả    │
│      xét nghiệm                                             │
│   2. Tổng hợp chẩn đoán, thuốc ra viện, dặn dò, lịch tái    │
│      khám                                                   │
│   3. Viết lại bằng ngôn ngữ bệnh nhân hiểu được             │
│   4. Rà soát, ký duyệt, in/gửi cho bệnh nhân                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (ước tính ~20     │
│    trên ~25 phút/ca).                                       │
│                                                             │
│ AI nhảy vào hỗ trợ ở bước nào? Bước 2-3 (trích xuất + soạn  │
│    nháp). Bước 4 bắt buộc do bác sĩ.                        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian/ca (gồm cả bước bác sĩ duyệt): ~25 phút ->    │
│     dưới 10 phút                                            │
│   - 0 bản tóm tắt tới bệnh nhân khi chưa có chữ ký bác sĩ   │
│   - Tên thuốc/liều khớp 100% y lệnh (đối chiếu tự động bằng │
│     rule)                                                   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│    (LLM Feature + HITL bắt buộc)                            │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 3: VinFast — Đối chiếu giao dịch sạc điện đối tác

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Đối chiếu giao dịch sạc điện hằng tuần    │
│    giữa trạm đối tác và hệ thống tài chính, nêu rõ các dòng │
│    lệch.                                                    │
│                                                             │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên vận hành mạng lưới sạc và    │
│    tài chính (làm lặp lại mỗi tuần).                        │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tải file giao dịch từ từng đối tác trạm sạc            │
│   2. Chuẩn hóa định dạng (cột, múi giờ, đơn vị kWh)         │
│   3. So khớp từng giao dịch với dữ liệu hệ thống nội bộ     │
│   4. Điều tra tay các dòng lệch và lập báo cáo              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (ước tính vài     │
│    giờ/tuần, cần đo thực tế).                               │
│                                                             │
│ AI nhảy vào hỗ trợ ở bước nào? Bước 2-3 làm bằng            │
│    script/rule. LLM chỉ tùy chọn để tóm tắt nguyên nhân     │
│    dòng lệch.                                               │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian đối chiếu: vài giờ/tuần -> dưới 30 phút       │
│   - 100% giao dịch được so khớp xác định (deterministic)    │
│   - Phát hiện >= 99% dòng lệch đã biết trên dữ liệu lịch sử │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
│    (rule/script, không cần LLM)                             │
└─────────────────────────────────────────────────────────────┘
```

## Tự phản biện nhanh (CFO + Vận hành)

| Thẻ | Điểm cần thận trọng | Cách kiểm chứng / hướng xử lý |
|---|---|---|
| Xanh SM — xe sắp hết pin | Thời gian 8-10 phút và mức giảm xuống dưới 3 phút chưa đo thực tế; lời nhắn không được làm người lái hiểu nhầm là cứu hộ đã được điều động. | Đo baseline trên log thật; giữ điều phối viên duyệt; kiểm tra cả action lẫn câu chữ và chỉ triển khai shadow mode trước. |
| Vinmec — tóm tắt xuất viện | Sai thuốc, liều hoặc dặn dò có thể gây hại; dữ liệu bệnh án cần được bảo vệ. | Chỉ tạo bản nháp trong môi trường được phê duyệt; bác sĩ đối chiếu y lệnh và ký trước khi gửi; chưa dùng dữ liệu bệnh nhân thật cho prototype. |
| VinFast — đối chiếu giao dịch | Đối chiếu là phép so khớp có cấu trúc; dùng LLM cho quyết định khớp có thể làm giảm tính xác định. | Ưu tiên script/rule và lưu vết các dòng lệch; chỉ cân nhắc LLM để tóm tắt nguyên nhân sau khi xác minh dữ liệu. |
