# Phase 6: REFLECTION — Nhật ký tương tác và phản chiếu với AI (AI Log)

Dự án thực hiện: **Vinpearl Guest Review Copilot — Trợ lý AI soạn bản nháp phản hồi đánh giá khách sạn trên OTA**
Công ty thành viên: **Vinpearl**
Mô hình AI sử dụng đồng hành: **Gemini / ChatGPT**

---

## 1. AI đã đồng hành và hỗ trợ được những gì?

Trong suốt quá trình scoping bài toán từ ý tưởng ban đầu đến khi hoàn thiện báo cáo phân tích, AI đóng vai trò như một người phản biện (thought-partner) và giúp tôi giải quyết các vấn đề sau:

1. **Cấu trúc hóa ý tưởng thô:**
   * Ban đầu, tôi chỉ có một quan sát thực tế: CSKH khách sạn phải đọc từng review 1–3 sao trên Booking/Agoda/Google rồi soạn thư xin lỗi thủ công, dễ lệch giọng giữa các ca trực và thường trễ SLA 24 giờ.
   * AI đã giúp tôi chuẩn hóa thành đề tài **Vinpearl Guest Review Copilot**, lập khung Problem Statement 6 trường (Actor, Workflow, Bottleneck, Business Impact, Metric, Operational Boundary) rất mạch lạc.

2. **Định hình các chỉ số đo lường (Metrics) thực tế:**
   * AI giúp lượng hóa mục tiêu: rút thời gian soạn phản hồi từ 15 phút xuống dưới 3 phút; SLA trả lời OTA > 95% trong 24 giờ; 100% bản nháp phải có HITL trước khi đăng.

3. **Phân tích kiến trúc AI-Fit:**
   * AI giúp so sánh Rule-based, LLM Feature và Multi-Agent. Kết luận phù hợp nhất là **LLM Feature + SOP/Voucher DB**: LLM đọc review và soạn nháp; Rule lo chính sách bồi thường; CSKH luôn là người bấm gửi.

---

## 2. AI đã đưa ra câu trả lời sai hoặc gặp ảo giác (Hallucination) ở điểm nào?

Trong quá trình trao đổi, tôi đã phát hiện một số điểm sai lệch và ảo giác quan trọng:

1. **Ảo giác về phạm vi quyền hạn (Scope Creep & Boundary Violation):**
   * AI tự đề xuất: "Tự động đăng phản hồi lên Booking/Agoda và tự gửi voucher hoàn tiền 50% cho khách." Đây là sai lầm nghiêm trọng: hệ thống chỉ được tạo `[DRAFT_ONLY]`, tuyệt đối không tự đăng OTA và không hứa bồi thường ngoài SOP.

2. **Ảo giác về chính sách bồi thường:**
   * Khi review đòi "hoàn 100% + nâng suite miễn phí", AI bịa ra chương trình bồi thường không có trong catalog SOP của Vinpearl.

3. **Xử lý sai case pháp lý / an toàn:**
   * Với review tố ngộ độc thực phẩm hoặc mất cắp, AI soạn luôn thư xin lỗi kèm cam kết bồi thường thay vì escalate lên Guest Experience (`ESCALATE_GX`).

---

## 3. Tôi đã sửa đổi Prompt và thiết lập ranh giới (Operational Boundary) như thế nào?

Để khắc phục các sai sót trên, tôi đã điều chỉnh như sau:

1. **Siết chặt ranh giới an toàn (Operational Boundary):**
   * AI chỉ là **Copilot soạn nháp**. Mọi output phải mở đầu bằng `[DRAFT_ONLY]`.
   * Cấm tự đăng OTA, cấm hứa hoàn tiền / nâng hạng / tặng đêm ngoài SOP.

2. **Ràng buộc nguồn dữ liệu (Grounding):**
   * Chỉ được map sang SOP và mã voucher có trong catalog nội bộ; không được bịa chính sách mới.

3. **Bổ sung quy tắc escalate:**
   * Review liên quan ngộ độc, tai nạn, mất cắp, quấy rối → trả `ESCALATE_GX`, không soạn lời bồi thường.

4. **Xây dựng cơ chế dự phòng (Fallback):**
   * Nếu độ tin cậy thấp hoặc ngôn ngữ lạ → fallback về template xin lỗi theo chủ đề để CSKH viết tay phần chi tiết.

---

## 4. Bài học kinh nghiệm rút ra

* **AI mạnh về ngôn ngữ nhưng yếu về kiểm soát cam kết thương mại:** dễ hứa bồi thường “cho có vẻ chu đáo” nếu không bị chặn bằng ranh giới rõ.
* **HITL là bắt buộc với nội dung công khai trên OTA:** một câu sai trên Booking/Agoda ảnh hưởng hạng và uy tín cả property, không thể để AI tự đăng.
* **Boundary phải được kiểm thử bằng adversarial prompt:** cố tình dụ AI bỏ `[DRAFT_ONLY]`, tự hoàn tiền, hoặc tự xử lý case pháp lý để xác nhận ranh giới thật sự vững.
