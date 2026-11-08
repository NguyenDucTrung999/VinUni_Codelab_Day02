# Lab 02 — Problem Scan: Vinpearl Guest Review Copilot (Vinpearl)

Bài nộp cá nhân theo form **Phase 1 — SCAN** và **Phase 2 — QUICK-ASSESS** trong [01-worksheet.md](01-worksheet.md).

**Dự án lựa chọn:** Vinpearl Guest Review Copilot — Trợ lý AI soạn bản nháp phản hồi đánh giá khách sạn trên OTA  
**Công ty thành viên:** Vinpearl

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:

1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 List bài toán của tôi:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Vinpearl** | Tốn thời gian (Time-consuming) | Nhân viên CSKH khách sạn phải đọc và soạn thủ công từng phản hồi đánh giá 1–3 sao trên Booking, Agoda, Google; mỗi bài mất 12–15 phút, giọng văn không thống nhất giữa các ca trực. |
| 2 | **VinFast** | Lặp lại (Repetitive) | Tư vấn viên showroom mỗi ngày soạn hàng chục bảng báo giá tùy biến (dòng xe, gói pin, phụ kiện, ưu đãi khu vực) bằng Excel, dễ lệch giá và mất khoảng 20 phút/khách. |
| 3 | **Xanh SM** | Lặp lại (Repetitive) | Nhân viên chất lượng phải đọc hàng trăm ghi chú hủy chuyến của khách và tài xế mỗi ca để gắn nhãn nguyên nhân gốc (tài xế muộn, khách đổi ý, lỗi app) phục vụ báo cáo tuần. |
| 4 | **Vinhomes** | AI có thể tốt hơn (AI-upgrade) | Bảo vệ/lễ tân xử lý đăng ký khách thăm bằng cách gọi điện xác nhận từng lượt với cư dân; giờ cao điểm mỗi lượt mất 10–15 phút, khách xếp hàng phàn nàn. |
| 5 | **Vinmec** | Pain từ người khác (Stakeholder Pain) | Điều dưỡng tiền khám phải đọc lại nhiều lần khám cũ để tóm tắt tiền sử, dị ứng và thuốc đang dùng; bác sĩ phàn nàn mất 8–10 phút đầu buổi vì hồ sơ chưa sẵn. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

## THẺ BÀI TOÁN SỐ 1: Vinpearl — Trợ lý AI soạn bản nháp phản hồi đánh giá OTA (Guest Review Copilot)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Xây dựng trợ lý AI soạn bản nháp phản hồi │
│ đánh giá 1–3 sao trên Booking/Agoda/Google theo đúng chính  │
│ sách thương hiệu Vinpearl, để CSKH chỉ việc duyệt và gửi.   │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [x] Khác: Vinpearl         │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH khách sạn Vinpearl và   │
│ quản lý trải nghiệm khách (Guest Experience).               │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Kéo đánh giá mới từ Booking, Agoda, Google về inbox    │
│   -> 2. Đọc review, xác định khiếu nại (phòng, F&B, spa...) │
│   -> 3. Tra cứu chính sách hoàn/đổi, voucher, SOP xin lỗi   │
│   -> 4. Soạn thư phản hồi tiếng Việt/Anh, gửi quản lý duyệt │
│   -> 5. Đăng lên OTA và ghi log vào hệ thống nội bộ         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3 và 4             │
│ (⏱ 12–15 phút/review; ca cuối tuần ~40 review/người).       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4        │
│ (Phân loại chủ đề + mức độ nghiêm trọng -> Gợi ý chính sách │
│ phù hợp -> Soạn bản nháp đa ngôn ngữ đúng giọng Vinpearl).  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Thời gian soạn phản hồi từ 15 phút ──> under 3 phút.      │
│ - 100% bản nháp phải có HITL (không tự đăng lên OTA).       │
│ - SLA phản hồi OTA đạt trên 95% trong 24 giờ.               │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM Feature soạn nháp + Rule tra cứu SOP/voucher; bắt buộc │
│ CSKH bấm duyệt trước khi gửi)                               │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 2: VinFast — Tự động soạn báo giá showroom theo nhu cầu khách

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động lập bảng báo giá VinFast từ mô tả │
│ nhu cầu khách (dòng xe, ngân sách, gói pin, phụ kiện) theo  │
│ bảng giá chính thức của khu vực.                            │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tư vấn viên showroom VinFast.          │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Hỏi khách nhu cầu (dòng xe, màu, pin, phụ kiện)        │
│   -> 2. Mở Excel/bảng giá khu vực, cộng từng dòng thủ công  │
│   -> 3. Áp ưu đãi theo chương trình tháng                   │
│   -> 4. Soạn file PDF gửi Zalo/email cho khách              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 và 3                │
│ (⏱ 20 phút/khách; dễ lệch giá khi đổi chương trình).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1, 2 và 3        │
│ (Trích xuất nhu cầu từ hội thoại -> Map SKU/bảng giá ->     │
│ Sinh bản nháp báo giá để tư vấn viên duyệt).                │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Giảm thời gian lập báo giá từ 20 phút ──> under 3 phút;     │
│ 100% đơn giá lấy từ bảng giá chính thức, không tự bịa giá.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ (LLM Feature + Rule/DB bảng giá; cấm AI tự đặt giá)         │
└─────────────────────────────────────────────────────────────┘
```

## THẺ BÀI TOÁN SỐ 3: Vinmec — Tóm tắt tiền sử khám trước giờ bác sĩ tiếp nhận

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động tóm tắt tiền sử, dị ứng và thuốc  │
│ đang dùng từ hồ sơ nhiều lần khám để điều dưỡng/bác sĩ đọc  │
│ trước khi tiếp nhận bệnh nhân.                              │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều dưỡng tiền khám và bác sĩ lâm     │
│ sàng tại phòng khám Vinmec.                                 │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Mở EMR, lướt nhiều lần khám cũ                         │
│   -> 2. Ghi chú tay tiền sử, dị ứng, thuốc đang dùng        │
│   -> 3. Hỏi lại bệnh nhân để bổ sung                        │
│   -> 4. Đưa tờ tóm tắt cho bác sĩ ngay trước giờ khám       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 và 2                │
│ (⏱ 8–10 phút/ca; dễ sót dị ứng thuốc).                      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 và 2           │
│ (Đọc EMR -> Sinh bản tóm tắt có cấu trúc; điều dưỡng duyệt  │
│ trước khi đưa bác sĩ).                                      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ Rút thời gian chuẩn bị hồ sơ từ 10 phút ──> under 2 phút;   │
│ không bỏ sót trường dị ứng/thuốc (recall 100% trên test     │
│ set vàng); AI không được tự kê đơn hay chẩn đoán.           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn bài toán cho Deep-Dive

Bài toán được lựa chọn để phân tích chuyên sâu (Phase 3 và Phase 5) là:  
**Thẻ bài toán số 1: Vinpearl — Trợ lý AI soạn bản nháp phản hồi đánh giá OTA (Guest Review Copilot).**

### Lý do lựa chọn:

1. **Tính thực tiễn cao:** CSKH khách sạn phải xử lý review mỗi ngày, đặc biệt cuối tuần và mùa cao điểm; đây là tác vụ lặp, tốn thời gian và dễ lệch giọng thương hiệu giữa các ca trực.
2. **Giá trị thương mại rõ ràng:** Phản hồi chậm hoặc lệch giọng trên OTA làm giảm điểm ranking Booking/Agoda và ảnh hưởng tỷ lệ đặt phòng. Rút SLA xuống dưới 24 giờ giúp giữ hạng và giảm rò rỉ booking.
3. **Phù hợp công nghệ:** LLM xử lý tốt ngôn ngữ cảm xúc trong review, phân loại chủ đề khiếu nại và soạn nháp đa ngôn ngữ. Rule/SOP lo phần chính sách hoàn-voucher. Bắt buộc HITL: AI chỉ được tạo `[DRAFT_ONLY]`, tuyệt đối không tự đăng lên OTA, không tự ý hứa hoàn tiền ngoài chính sách.
