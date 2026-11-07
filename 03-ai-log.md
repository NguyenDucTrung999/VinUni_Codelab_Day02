
# 03 - AI Interaction Log (Nhật ký tương tác AI)

## Phase 1: Brainstorming & Stress-test

| Nội dung | Chi tiết |
|---|---|
| **Prompt 1** | Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng Vinhomes. Hãy gợi ý 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất, kèm con số thống kê ước tính về tổn thất. |
| **Prompt 2** | Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Nội dung thẻ bài toán]. Hãy đóng vai CFO và Trưởng phòng Vận hành, chỉ ra 3 điểm yếu về logic, metric và giải thích vì sao Rule-based có thể tốt hơn AI. |
| **Prompt 3** | Hãy so sánh 5 pain point đã đề xuất theo các tiêu chí: mức độ ảnh hưởng đến cư dân, tần suất xảy ra, chi phí vận hành và khả năng ứng dụng AI. Xếp hạng các vấn đề từ cao xuống thấp. |
| **Prompt 4** | Nếu triển khai giải pháp AI cho bài toán phân loại ticket cư dân, những trường hợp nào không cần sử dụng LLM mà chỉ cần Rule-based? Hãy đưa ra ví dụ cụ thể. |
| **AI trả lời/viết gì** | AI đề xuất 5 pain point, trong đó có vấn đề phân loại và điều phối ticket cư dân. AI chỉ ra một số trường hợp có thể xử lý bằng Rule-based và đề xuất xếp hạng các pain point. |
| **Bạn đã sửa / kiểm chứng gì** | Không sử dụng số liệu ước tính như dữ liệu thực tế. Lựa chọn bài toán phân loại ticket và điều chỉnh kiến trúc theo hướng Hybrid (Rule-based + LLM). |

## Phase 2: Thiết lập Metric

| Nội dung | Chi tiết |
|---|---|
| **Prompt 1** | Hãy xây dựng Success Metric cho hệ thống AI tự động phân loại và điều phối ticket tại Vinhomes. Các chỉ số phải cụ thể, đo lường được và gắn với hiệu quả vận hành. |
| **Prompt 2** | Hãy đề xuất cách đo lường hiệu quả trước và sau khi triển khai AI. Cần xác định baseline, công thức tính và cách thu thập dữ liệu cho từng chỉ số. |
| **Prompt 3** | Nếu hệ thống đạt Accuracy 90% nhưng vẫn phân loại sai một số ticket khẩn cấp như rò rỉ nước nghiêm trọng hoặc sự cố điện, liệu có thể xem là thành công không? Hãy đề xuất metric bổ sung để đánh giá rủi ro này. |
| **AI trả lời/viết gì** | AI đề xuất Macro-F1, Critical Issue Recall, tỷ lệ Auto-routing và thời gian phản hồi P95. AI giải thích Accuracy cao chưa chắc đảm bảo xử lý tốt các ticket quan trọng. |
| **Bạn đã sửa / kiểm chứng gì** | Rút gọn thành hai mục tiêu: giảm thời gian phân loại và điều phối từ 5–10 phút xuống dưới 1 phút; hướng tới độ chính xác phân loại và đề xuất đúng tổ kỹ thuật đạt 90%. Xác định đây là mục tiêu kỳ vọng, chưa phải kết quả thực nghiệm. |

## Phase 3: Deep-dive

| Nội dung | Chi tiết |
|---|---|
| **Prompt 1** | Hãy phân tích quy trình hiện tại và thiết kế quy trình tương lai cho hệ thống tiếp nhận ticket tại Vinhomes. Xác định bottleneck, vai trò AI và những công đoạn cần con người kiểm soát. |
| **Prompt 2** | Hãy phân tích các nguyên nhân khiến quy trình phân loại ticket mất nhiều thời gian. Phân biệt nguyên nhân đến từ dữ liệu đầu vào, quy trình nghiệp vụ và khả năng phối hợp giữa các bộ phận. |
| **AI trả lời/viết gì** | AI đề xuất quy trình tiếp nhận ticket, kiểm tra dữ liệu, phân loại bằng Rule-based/LLM, Validation, Auto-routing và cập nhật trạng thái. |
| **Bạn đã sửa / kiểm chứng gì** | Gộp quy trình thành 5 bước chính. Bổ sung nhánh chuyển CSKH xác nhận khi dữ liệu thiếu, kết quả không đủ tin cậy hoặc ticket có mức độ ưu tiên cao. |

## Phase 5: Evaluate

| Nội dung | Chi tiết |
|---|---|
| **Prompt 1** | Hãy đánh giá mức độ sẵn sàng triển khai giải pháp AI phân loại và điều phối ticket tại Vinhomes dựa trên dữ liệu, rủi ro, khả năng tích hợp, chi phí và hiệu quả vận hành. Đề xuất quyết định GO, NOT YET hoặc NO-GO. |
| **Prompt 2** | Hãy so sánh ba phương án Rule-based, LLM-only và Hybrid theo độ chính xác, chi phí, tốc độ xử lý, khả năng mở rộng và mức độ kiểm soát rủi ro. |
| **AI trả lời/viết gì** | AI nhận định giải pháp có tiềm năng nhưng cần dữ liệu ticket thực tế, baseline và kết quả so sánh giữa Rule-based, LLM và Hybrid trước khi triển khai. |
| **Bạn đã sửa / kiểm chứng gì** | Chọn **NOT YET** vì chưa có dữ liệu gán nhãn, chưa thực hiện kiểm thử và chưa xác định chi phí vận hành thực tế. Ưu tiên thu thập dữ liệu và xác lập baseline trước khi xây dựng Prototype. |

---

## Nhận xét sau quá trình sử dụng AI

Qua các Phase, AI hỗ trợ tìm kiếm ý tưởng, phân tích quy trình, đề xuất metric và đánh giá giải pháp. Tuy nhiên, đôi khi AI đề xuất những pain point đã có giải pháp xử lý bằng quy trình hoặc hệ thống hiện tại, đưa ra số liệu ước tính chưa được xác thực hoặc đề xuất sử dụng LLM cho những công việc chỉ cần Rule-based.


