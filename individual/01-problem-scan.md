# Phase 1: SCAN — Tìm kiếm cơ hội (Cá nhân)

Sử dụng 4 góc nhìn (Lenses) để quét qua hoạt động vận hành và trải nghiệm thực tế tại các công ty thành viên thuộc Vingroup:

| # | Công ty thành viên | Góc nhìn (Lens) | Mô tả ngắn bài toán |
|---|-------------------|----------------|---------------------|
| 1 | Vincom Retail | AI có thể làm tốt hơn | Khách hàng cá nhân hoặc nhóm bạn đến TTTM Vincom thường mất nhiều thời gian loay hoay không biết ăn gì, chơi gì, mua gì cho phù hợp với sở thích, tính cách, quỹ thời gian và ngân sách cá nhân. |
| 2 | Xanh SM | Lặp lại | Nhân viên vận hành phải tiếp nhận và phân bổ thủ công hàng trăm báo cáo sự cố trụ sạc hỏng, xe hết pin khẩn cấp từ tài xế về các trạm sạc còn trụ trống lân cận. |
| 3 | Vinhomes | Tốn thời gian | Ban quản lý tòa nhà phải đọc thủ công hàng trăm phản ánh sự cố kỹ thuật điện nước mỗi ngày để tạo phiếu công việc nội bộ cho từng tổ kỹ thuật chuyên trách. |
| 4 | VinFast | AI có thể làm tốt hơn | Cố vấn dịch vụ mất nhiều thời gian giải mã mô tả hiện tượng hỏng hóc bằng tiếng Việt dân dã của khách hàng để đối chiếu với danh mục mã lỗi kỹ thuật OBD-II. |
| 5 | Vinmec | Nỗi đau từ người khác | Bác sĩ tốn trung bình 25 phút mỗi ca xuất viện để tổng hợp thông tin lâm sàng và soạn bản tóm tắt hồ sơ bệnh án dễ hiểu cho bệnh nhân. |

---

# Phase 2: QUICK-ASSESS — 3 Thẻ bài toán nhanh (Cá nhân)

Chọn top 3 bài toán tiềm năng nhất để phân tích sơ bộ:

## THẺ BÀI TOÁN SỐ 1: Vincom Retail — Trợ lý AI lập lộ trình trải nghiệm TTTM cá nhân hóa (Vincom Smart Concierge)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Xây dựng trợ lý AI cá nhân hóa giúp một người     │
│ hoặc nhóm bạn lập lộ trình trải nghiệm (ăn uống, giải trí,  │
│ mua sắm) tại TTTM Vincom dựa trên ngân sách, thời gian      │
│ rảnh, sở thích và tính cách.                                │
│ Công ty thành viên: [x] Vincom Retail                       │
│                                                             │
│ Ai đang gặp khó khăn (Actor): Khách hàng cá nhân hoặc nhóm  │
│ bạn trẻ đến vui chơi tại TTTM Vincom.                       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Đến TTTM hoặc mở điện thoại tìm kiếm từng quán ăn,     │
│      rạp chiếu phim, khu vui chơi trên mạng xã hội          │
│   -> 2. Bàn bạc, tính toán tiền nong và so sánh khung giờ   │
│   -> 3. Đi lòng vòng tìm vị trí gian hàng giữa các tầng     │
│   -> 4. Thường xuyên bị trễ giờ chiếu phim hoặc chi tiêu    │
│      vượt mức ngân sách dự kiến                             │
│                                                             │
│ Bước tốn thời gian nhất: Bước 1, 2 và 3 (tốn 50 phút,       │
│ dao động 45 - 60 phút để bàn bạc, tìm quán và tìm đường).   │
│ AI có thể hỗ trợ ở bước nào: Bước 1, 2 và 3 (Tiếp nhận yêu  │
│ cầu ngôn ngữ tự nhiên -> Tự động lựa chọn gian hàng, tính   │
│ tổng chi phí, xếp thứ tự di chuyển tối ưu giữa các tầng).   │
│                                                             │
│ Đo thành công bằng gì (Metric):                             │
│ - Rút ngắn thời gian lên kế hoạch và tìm đường từ 50 phút   │
│   xuống dưới 2 phút.                                        │
│ - Độ lệch chi phí thực tế so với ngân sách đề ra dưới 10%.  │
│ - Mức độ hài lòng của khách hàng (CSAT) đạt trên 90%.       │
│                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Database danh  │
│ mục gian hàng, giá cả và sơ đồ tầng TTTM Vincom)            │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 2: Xanh SM — Điều phối sự cố trụ sạc cho đội xe taxi điện

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân tích tin nhắn báo sự cố trụ sạc từ   │
│ tài xế để gợi ý trạm sạc VinFast còn trụ trống tương thích. │
│ Công ty thành viên: [x] Xanh SM                             │
│                                                             │
│ Ai đang gặp khó khăn (Actor): Điều phối viên tổng đài Xanh  │
│ SM và Tài xế đang hoạt động thực địa.                       │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Tài xế gọi điện hoặc nhắn tin báo trụ sạc gặp lỗi      │
│   -> 2. Điều phối viên tra cứu GPS và tình trạng trụ sạc    │
│   -> 3. Xác định trạm sạc có cổng sạc tương thích còn trống │
│   -> 4. Soạn tin nhắn hướng dẫn tài xế di chuyển            │
│                                                             │
│ Bước tốn thời gian nhất: Bước 2 và 3 (8 đến 10 phút/lượt).  │
│ AI có thể hỗ trợ ở bước nào: Bước 2 và 3 (Gọi API tra cứu   │
│ trạm sạc theo vị trí và tự động tạo bản thảo hướng dẫn).    │
│                                                             │
│ Đo thành công bằng gì (Metric):                             │
│ Giảm thời gian phản hồi tài xế từ 10 phút xuống dưới 2 phút.│
│                                                             │
│ Quick Architecture: [x] LLM Feature (kết hợp Rule API)      │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 3: Vinhomes — Phân loại yêu cầu kỹ thuật tòa nhà từ cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động phân loại phản ánh hư hỏng thiết bị từ cư │
│ dân trên ứng dụng Vinhomes Resident để chuyển giao kỹ thuật.│
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang gặp khó khăn (Actor): Nhân viên chăm sóc khách hàng │
│ tại văn phòng ban quản lý tòa nhà.                          │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh trên ứng dụng                      │
│   -> 2. Nhân viên đọc nội dung và xác định lĩnh vực         │
│   -> 3. Tra cứu lịch trực của đội kỹ thuật phân khu         │
│   -> 4. Tạo phiếu công việc trên hệ thống nội bộ            │
│                                                             │
│ Bước tốn thời gian nhất: Bước 2 và 3 (10 phút/phiếu).       │
│ AI có thể hỗ trợ ở bước nào: Bước 2 và 3 (Phân loại ngành   │
│ kỹ thuật: điện, nước, thang máy, điều hòa và gợi ý điều vận)│
│                                                             │
│ Đo thành công bằng gì (Metric):                             │
│ Rút ngắn thời gian tiếp nhận từ 10 phút xuống dưới 1 phút;  │
│ độ chính xác phân loại đạt trên 90%.                        │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

# Quyết định lựa chọn bài toán cho Deep-Dive

Bài toán được lựa chọn để phân tích chuyên sâu (Phase 3 và Phase 5) là:
**Thẻ bài toán số 1: Vincom Retail — Trợ lý AI lập lộ trình trải nghiệm TTTM cá nhân hóa (Vincom Smart Concierge)**.

### Lý do lựa chọn:
1. **Tính thực tiễn cao:** Đây là vấn đề thực tế mà bất kỳ cá nhân hay nhóm bạn trẻ nào cũng gặp phải khi đến các TTTM quy mô lớn của Vincom. Việc giải quyết khâu đắn đo, tranh luận tìm chỗ ăn chơi giúp tiết kiệm thời gian đáng kể.
2. **Giá trị thương mại rõ ràng:** Giúp Vincom gia tăng tỷ lệ chi tiêu chéo (cross-spending) giữa các nhóm dịch vụ (ẩm thực, rạp chiếu phim, khu vui chơi và mua sắm), đồng thời giảm tình trạng khách hàng mệt mỏi bỏ về sớm.
3. **Phù hợp công nghệ:** Khả năng hiểu ngôn ngữ tự nhiên của LLM xử lý rất tốt các yêu cầu đa dạng, mang tính cảm xúc của người dùng ("nhóm con trai thích ăn nướng, thích chỗ yên tĩnh, ngân sách 250k/người, rảnh 3 tiếng"), sau đó kết hợp cơ sở dữ liệu gian hàng để đưa ra lịch trình chuẩn xác.
