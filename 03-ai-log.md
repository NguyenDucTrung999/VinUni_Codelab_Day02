# Phase 6: REFLECTION — Nhật ký tương tác và phản chiếu với AI (AI Log)

Dự án thực hiện: **Vincom Smart Concierge — Trợ lý AI lập lộ trình trải nghiệm TTTM cá nhân hóa**
Công ty thành viên: **Vincom Retail**
Mô hình AI sử dụng đồng hành: **Gemini / ChatGPT**

---

## 1. AI đã đồng hành và hỗ trợ được những gì?

Trong suốt quá trình scoping bài toán từ ý tưởng ban đầu đến khi hoàn thiện báo cáo phân tích, AI đóng vai trò như một người phản biện (thought-partner) và giúp tôi giải quyết các vấn đề sau:

1. **Cấu trúc hóa ý tưởng thô:**
   * Ban đầu, tôi chỉ có một quan sát thực tế từ cuộc sống hàng ngày: khi một người hoặc một nhóm bạn nam vào Vincom thì rất dễ bị rối, không biết ăn gì, chơi gì cho phù hợp với túi tiền và quỹ thời gian rảnh.
   * AI đã giúp tôi chuẩn hóa thành một đề tài rõ ràng cho **Vincom Retail**, lập thành khung Problem Statement 6 trường thông tin (Actor, Workflow, Bottleneck, Business Impact, Metric, Operational Boundary) rất mạch lạc.

2. **Định hình các chỉ số đo lường (Metrics) thực tế:**
   * AI giúp lượng hóa các mục tiêu cụ thể: rút ngắn thời gian lên kế hoạch và tìm đường từ 50 phút xuống dưới 2 phút, độ lệch ngân sách dưới 10%, và tỷ lệ hài lòng của khách hàng trên 90%.

3. **Phân tích kiến trúc AI-Fit:**
   * AI giúp phân tích rõ ràng giữa ba lựa chọn: Rule-based, LLM Feature, và Multi-Agent. Nhờ đó, tôi nhận thấy không cần thiết phải dùng Multi-Agent gây phức tạp và tốn kém chi phí token, mà chỉ cần dùng LLM Feature kết hợp với bộ lọc dữ liệu có sẵn (Database Query) là đã giải quyết triệt để bài toán.

---

## 2. AI đã đưa ra câu trả lời sai hoặc gặp ảo giác (Hallucination) ở điểm nào?

Trong quá trình trao đổi, tôi đã phát hiện ra một số điểm sai lệch và ảo giác quan trọng của AI:

1. **Ảo giác về phạm vi quyền hạn (Scope Creep & Boundary Violation):**
   * Ban đầu, AI tự ý đề xuất tính năng: "Tự động đặt bàn và thanh toán luôn qua thẻ ngân hàng của người dùng". Đây là một sai lầm nghiêm trọng về mặt an toàn, bởi vì hệ thống tư vấn trải nghiệm tuyệt đối không được tự ý can thiệp vào tài chính của người dùng khi chưa có bước xác nhận phê duyệt (Human-in-the-loop).

2. **Ảo giác về danh mục gian hàng thực tế:**
   * Khi được yêu cầu thử lên một lịch trình mẫu, AI đã tự bịa ra một số quán ăn và khu vui chơi không hề có mặt trong TTTM Vincom (ví dụ: gợi ý các quán ăn vỉa hè bên ngoài hoặc các thương hiệu chưa từng mở chi nhánh tại Vincom).

3. **Thiếu tính toán thực tế về không gian di chuyển giữa các tầng:**
   * AI ban đầu sắp xếp một lịch trình rất bất hợp lý về mặt vật lý: cho người dùng ăn ở Tầng 4, sau đó xuống Tầng 1 mua sắm, rồi lại lên Tầng 5 xem phim, rồi lại xuống Tầng B1 uống trà sữa. Điều này khiến khách hàng phải đi lại lòng vòng và mệt mỏi hơn cả khi không dùng AI.

---

## 3. Tôi đã sửa đổi Prompt và thiết lập ranh giới (Operational Boundary) như thế nào?

Để khắc phục hoàn toàn các sai sót và ảo giác trên của AI, tôi đã thực hiện các điều chỉnh sau:

1. **Siết chặt ranh giới an toàn (Operational Boundary):**
   * Yêu cầu AI chỉ đóng vai trò là **Trợ lý tư vấn và gợi ý (Copilot)**, tuyệt đối không có quyền thanh toán hoặc tự động giữ chỗ khi chưa có xác nhận từ khách hàng.
   * Bắt buộc quy trình phải có bước người dùng xem và bấm duyệt bản nháp lộ trình (Human-in-the-loop) trước khi bắt đầu.

2. **Ràng buộc nguồn dữ liệu (Grounding):**
   * Quy định rõ ràng trong prompt: AI chỉ được phép chọn lựa gian hàng từ danh mục có sẵn trong cơ sở dữ liệu của TTTM Vincom cụ thể, không được tự sáng tác thương hiệu ngoài danh mục.

3. **Bổ sung logic tối ưu hóa di chuyển (Spatial Optimization Prompt):**
   * Thêm chỉ thị rõ ràng vào prompt: Khi xếp lịch trình, phải sắp xếp các hoạt động theo thứ tự tầng liền kề (ví dụ: từ thấp lên cao hoặc từ cao xuống thấp) để khách hàng di chuyển thang cuốn một chiều thuận tiện, không được đảo tầng liên tục.

4. **Xây dựng cơ chế dự phòng (Fallback):**
   * Đặt ra quy tắc nếu ngân sách người dùng nhập vào quá thấp hoặc khung thời gian quá ngắn, AI phải thông báo lịch sự và đề xuất phương án thay thế thực tế thay vì cố tình bịa ra lộ trình không khả thi.

---

## 4. Bài học kinh nghiệm rút ra

* **AI không thay thế được tư duy thực tế của con người:** Các mô hình AI rất mạnh về ngôn ngữ nhưng lại dễ mắc lỗi thiếu logic thực địa (như khoảng cách giữa các tầng, giá cả thực tế).
* **Ranh giới an toàn (Boundary) là yếu tố sống còn:** Không bao giờ được trao quyền hạn quá lớn cho AI trong các khâu ra quyết định tài chính hoặc cam kết dịch vụ mà luôn cần giữ bước con người kiểm soát.
