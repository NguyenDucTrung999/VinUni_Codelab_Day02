# Phase 3: DEEP-DIVE — Báo cáo phân tích chuyên sâu

Dự án lựa chọn: **Vincom Smart Concierge — Trợ lý AI lập lộ trình trải nghiệm TTTM cá nhân hóa**
Công ty thành viên: **Vincom Retail**

---

## 3.1. Quy trình vận hành hiện tại (Current-State Workflow)

Quy trình hiện tại khi một cá nhân hoặc một nhóm bạn trẻ đến vui chơi tại TTTM Vincom:

```text
┌──────────────────────────────────────────────┐
│ Bước 1: Gặp gỡ & Bàn bạc ý kiến              │
│ Ai: Nhóm bạn | ⏱ Thời gian: 15 phút 🔴        │
│ In: Ý kiến, sở thích, ngân sách từng người   │
│ Out: Thống nhất ý định chung sơ bộ           │
│ Bottleneck: Bất đồng quan điểm, khác gu ăn   │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 1: Ý kiến cá nhân -> Ý định chung)
┌──────────────────────────────────────────────┐
│ Bước 2: Tìm kiếm thông tin thủ công          │
│ Ai: Nhóm bạn | ⏱ Thời gian: 20 phút 🔴        │
│ In: Ý định sơ bộ, mạng xã hội, sơ đồ sảnh    │
│ Out: Danh sách quán ăn và suất chiếu phim    │
│ Bottleneck: Lướt mạng xã hội, xem sơ đồ sảnh │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 2: Thông tin phân tán -> Danh sách chọn)
┌──────────────────────────────────────────────┐
│ Bước 3: Di chuyển tìm gian hàng các tầng     │
│ Ai: Nhóm bạn | ⏱ Thời gian: 15 phút 🔴        │
│ In: Vị trí sảnh, biển chỉ dẫn tầng TTTM      │
│ Out: Tìm đến đúng gian hàng thực tế          │
│ Bottleneck: Đi thang cuốn lên xuống tốn sức  │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 3: Vị trí thực tế -> Gian hàng cụ thể)
┌──────────────────────────────────────────────┐
│ Bước 4: Trải nghiệm & Thanh toán             │
│ Ai: Nhóm bạn | ⏱ Thời gian: 10 phút          │
│ In: Hóa đơn (Bill), voucher giảm giá         │
│ Out: Hoàn tất buổi đi chơi, chi phí phát sinh│
│ Bottleneck: Trễ giờ chiếu phim, vượt tiền    │
└──────────────────────────────────────────────┘
```

### Các điểm nút quan trọng:
* **Tổng thời gian tiêu tốn cho việc lên kế hoạch và tìm đường (3 bước đầu):** 50 phút (dao động trong khoảng 45 - 60 phút tuỳ độ đông đúc).
* **Điểm nghẽn cổ chai (🔴 Bottlenecks):**
  * Bước 1: Bất đồng ý kiến giữa các thành viên về sở thích, khẩu vị ăn uống, người muốn tiết kiệm, người muốn trải nghiệm mới (15 phút).
  * Bước 2: Thông tin menu và giá cả bị phân tán trên nhiều nền tảng, phải lướt tìm kiếm và so sánh thủ công (20 phút).
  * Bước 3: Thiếu thông tin lộ trình tối ưu giữa các tầng, đi thang cuốn lên xuống lòng vòng gây mệt mỏi và dễ trễ giờ (15 phút).
* **Điểm chuyển giao (🔄 Handoffs):** Giữa ý định cá nhân với quyết định tập thể, và giữa thông tin trên mạng với không gian thực tế của TTTM.

### Căn cứ khoa học và nghiên cứu xác lập mốc thời gian Baseline (45 - 60 phút):
Mốc thời gian 45 - 60 phút được tổng hợp từ 3 nghiên cứu hành vi và khảo sát thị trường thực tế:
1. **Khảo sát "Food Indecision" của OnePoll:** Người tiêu dùng mất trung bình 16 - 20 phút mỗi bữa để phân vân chọn món; khi đi theo nhóm (Group Dining), các tranh luận và bất đồng ý kiến kéo dài thêm 15 phút.
2. **Nghiên cứu "The Paradox of Choice" (Barry Schwartz) & Khảo sát F&B của Q&Me:** Khi đối mặt với trên 40 gian hàng tại TTTM, hiện tượng "tê liệt quyết định" (Decision Paralysis) khiến người trẻ mất 15 - 20 phút lướt mạng xã hội xem review và so sánh giá.
3. **Nghiên cứu "Indoor Wayfinding" của Hiệp hội TTTM Quốc tế (ICSC):** Khách hàng tại các Mega Mall nhiều tầng mất trung bình 10 - 15 phút để định hướng sơ đồ và di chuyển thang cuốn tìm đúng gian hàng thực tế.
=> **Tổng thời gian lãng phí cộng dồn:** Khoảng 45 - 60 phút / lượt.

---

## 3.2. Problem Statement (6-Field)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Khách hàng cá nhân hoặc nhóm bạn trẻ (từ 18 đến 28 tuổi: học sinh, sinh viên, nhân viên văn phòng) đến vui chơi tại TTTM Vincom. |
| **2. Current Workflow** | Tự tìm kiếm bằng cách lướt mạng xã hội, hỏi bạn bè, xem sơ đồ tại sảnh TTTM; tự tính toán tiền nong qua trao đổi miệng; tự đi tìm gian hàng giữa các tầng theo biển chỉ dẫn vật lý. Quy trình gồm 4 bước thủ công, tốn từ 45 đến 60 phút. |
| **3. Bottleneck** | Tốn quá nhiều thời gian tranh luận và chọn lựa; dễ bị vượt ngân sách dự kiến do không tính trước được tổng chi phí; đi lại lòng vòng tốn sức do không tối ưu hóa lộ trình di chuyển theo tầng. |
| **4. Business Impact** | Khách hàng giảm mức độ hài lòng, dễ rời TTTM sớm hơn dự kiến; Vincom bị rò rỉ cơ hội gia tăng doanh thu bán chéo (cross-selling) giữa các ngành hàng Ẩm thực (F&B), Giải trí (Rạp chiếu phim, khu vui chơi) và Mua sắm (Retail). Ước tính giảm 15% mức chi tiêu tiềm năng tại chỗ. |
| **5. Success Metric** | - **System Metrics:** Tỷ lệ hoàn thành tác vụ (Task completion rate) > 92%, độ trễ P95 < 2 giây, chi phí vận hành dưới 0.005 USD/session.<br>- **Business KPIs:** Thời gian lên lịch trình và tìm đường giảm từ 50 phút xuống dưới 2 phút; 100% địa điểm đúng ngân sách; điểm hài lòng CSAT > 90%; tỷ lệ hoàn thành theo lộ trình đạt > 75%. |
| **6. Operational Boundary** | **AI được phép:** Đọc yêu cầu ngôn ngữ tự nhiên, phân tích sở thích/tính cách, truy vấn danh sách gian hàng trong TTTM Vincom tương ứng, gợi ý lộ trình theo mốc thời gian và tính tổng chi phí dự kiến.<br>**AI tuyệt đối không được phép:** Tự động trừ tiền hay thanh toán bất kỳ khoản phí nào của khách; không cam kết giữ chỗ/giữ bàn ăn nếu hệ thống đặt bàn chưa trả về xác nhận thành công; không được bịa đặt (hallucinate) các gian hàng không có thật trong TTTM.<br>**Điểm duyệt (HITL):** Khách hàng có toàn quyền xem bản nháp lộ trình, bấm nút đổi món/đổi địa điểm nếu chưa ưng ý trước khi bấm "Bắt đầu trải nghiệm". |

---

## 3.3. Quy trình tương lai (Future-State Flow) và Đánh giá AI Fit

### A. Đánh giá mức độ phù hợp (AI-Fit)
* **Kiểm tra Rule-based:** Quy tắc tĩnh (If/Else) không thể hiểu được các yêu cầu cảm xúc, phong cách tự do và đa dạng của khách hàng (ví dụ: "nhóm con trai thích đồ nướng, muốn ngồi nơi yên tĩnh nói chuyện, ngân sách 200k mỗi người, rảnh từ 18h đến 21h").
* **Kiểm tra Multi-Agent:** Quá phức tạp và tốn chi phí token không cần thiết cho một bài toán tiếp xúc trực tiếp với người dùng B2C cần tốc độ phản hồi nhanh (dưới 3 giây).
* **Kết luận kiến trúc:** Phù hợp nhất với **LLM Feature kết hợp Rule Database Filter**:
  * LLM đóng vai trò thấu hiểu ngôn ngữ tự nhiên, trích xuất tiêu chí (ngân sách, thời gian, sở thích, phong cách) và biên tập thành bản lộ trình hấp dẫn, gần gũi.
  * Hệ thống Rule/Database đóng vai trò kiểm tra tính chính xác của danh mục gian hàng, vị trí tầng, bảng giá thực tế và các voucher giảm giá còn hiệu lực.

### B. Sơ đồ quy trình tương lai tích hợp AI

```text
┌──────────────────────────────────────────────┐
│ [Người dùng] Nhập yêu cầu bằng ngôn ngữ      │
│ tự nhiên (ngân sách, thời gian, gu ăn uống)  │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ [AI Step 1 - LLM NLU]                        │
│ Trích xuất tiêu chí: Ngân sách, Thời gian,   │
│ Số lượng người, Tính cách và Phong cách      │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ [Rule Step - DB Query]                       │
│ Lọc danh mục gian hàng hợp lệ còn hoạt động  │
│ tại TTTM Vincom và so sánh khung giá         │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ [AI Step 2 - LLM Planner]                    │
│ Sắp xếp Timeline tối ưu thứ tự các tầng:     │
│ Tầng 4 (Ăn uống) -> Tầng 3 (Cà phê) -> Phim  │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ [Human Step - HITL]                          │
│ Người dùng xem lộ trình, bấm Đổi quán hoặc   │
│ bấm Đồng ý để bắt đầu trải nghiệm            │
└──────────────────────────────────────────────┘
```

### C. Cơ chế dự phòng (Fallback) khi gặp lỗi
* **Trường hợp 1: Ngân sách quá thấp so với mặt bằng giá của TTTM:**
  * LLM trả về thông báo lịch sự: "Ngân sách 50.000đ/người hiện chưa đủ cho bữa ăn tiêu chuẩn tại TTTM, hệ thống gợi ý các lựa chọn ăn nhẹ tại quầy Kiosk tầng B1 hoặc bạn có thể nâng ngân sách lên 120.000đ/người".
* **Trường hợp 2: Khung thời gian quá ngắn (dưới 45 phút):**
  * Tự động chuyển về chế độ "Grab & Go" (gợi ý đồ uống/đồ ăn nhanh mang đi ngay tại tầng gần cửa ra vào nhất).
* **Trường hợp 3: Gian hàng hết bàn hoặc rạp phim hết vé:**
  * Hệ thống tự động thay thế bằng gian hàng dự phòng cùng phân khúc trên cùng tầng mà không làm phá vỡ cấu trúc thời gian của người dùng.

---

## 3.4. Ma trận phân quyền RACI-Lite & Stakeholder Grid

| Nhóm Stakeholder | Vai trò trong hệ thống | Quyền hạn và Trách nhiệm |
|---|---|---|
| **Biz / Ops (Ban quản lý Vincom Retail)** | Business Owner | Sở hữu KPI kinh doanh, phê duyệt triển khai tại các TTTM |
| **User (Khách hàng Vincom)** | End-User & Reviewer (HITL) | Trực tiếp sử dụng, xem xét và phê duyệt lộ trình cá nhân |
| **AI Engineer (Đội phát triển AI)** | Builder & Evaluator | Thiết kế Prompt, xây dựng Pipeline, tối ưu độ trễ và đánh giá Eval |
| **Platform / IT Team** | System Support | Cung cấp API gian hàng, đồng bộ hệ thống sơ đồ tầng và voucher |
| **Risk / Legal** | Compliance & Safety | Kiểm soát chính sách bảo mật dữ liệu, không lưu trữ thông tin nhạy cảm |

### Bảng phân công trách nhiệm RACI-Lite:

| Hoạt động dự án | Biz/Ops | AI Eng | Platform/Data | Risk/Legal |
|---|:---:|:---:|:---:|:---:|
| Xác định bài toán và Business Metrics | **A/R** | C | I | I |
| Xây dựng tập dữ liệu test và Eval set | C | **A/R** | R | I |
| Lựa chọn kiến trúc (LLM Feature + DB) | C | **A/R** | C | C |
| Thiết lập ranh giới HITL và Fallback | **A** | C | C | R |
| Phê duyệt Go-live và cơ chế Rollback | **A** | C | R | R |

*(Chú thích: **A** = Accountable - Người chịu trách nhiệm chính; **R** = Responsible - Người thực hiện; **C** = Consulted - Người tham vấn; **I** = Informed - Người nhận thông tin).*

---

## 3.5. Đánh giá tính khả thi 3 trụ cột (Feasibility Check)

| Trụ cột đánh giá | Câu hỏi kiểm tra | Kết quả | Bằng chứng thực tế |
|---|---|:---:|---|
| **1. Kỹ thuật (Technical)** | Có baseline để so sánh không? | **ĐẠT** | Đã có baseline thủ công: khách hàng mất 50 phút (dao động 45 - 60 phút) để bàn bạc, tra cứu và tìm đường. |
| | Có dữ liệu mẫu / danh mục để eval không? | **ĐẠT** | Có sẵn danh mục 250 gian hàng, phân tầng và khung giá của Vincom. |
| | Độ trễ và chi phí chấp nhận được? | **ĐẠT** | Độ trễ Gemini Flash < 2s; chi phí < 0.005 USD/request (rất tối ưu). |
| | Kiến trúc có đơn giản không? | **ĐẠT** | Dùng LLM Feature kết hợp DB Query, không dùng Multi-Agent phức tạp. |
| **2. Vận hành (Operational)** | Có hệ thống logging và giám sát? | **ĐẠT** | Log toàn bộ phiên nhập liệu, kết quả JSON và phản hồi người dùng. |
| | Có cơ chế HITL và phê duyệt? | **ĐẠT** | Khách hàng phải bấm xác nhận hoặc tùy chọn đổi món mới chốt lịch. |
| | Có phương án rollback khi lỗi? | **ĐẠT** | Tự động fallback về danh sách top quán ăn theo tầng nếu LLM lỗi. |
| | Có người chịu trách nhiệm khi output sai? | **ĐẠT** | Trưởng nhóm Sản phẩm Vincom Retail chịu trách nhiệm nghiệp vụ. |
| **3. Kinh doanh (Business)** | ROI có dương ở quy mô thử nghiệm? | **ĐẠT** | Chi phí API ~15 USD/tháng cho 3.000 lượt khách; tăng 15% mức chi tiêu chéo. |
| | Ranh giới an toàn và tuân thủ rõ ràng? | **ĐẠT** | AI không chạm vào thẻ thanh toán hay tài khoản ngân hàng của khách. |
| | Đội vận hành sẵn sàng áp dụng? | **ĐẠT** | Vincom Retail đang đẩy mạnh số hóa trải nghiệm để thu hút khách trẻ. |

---

## 3.6. Phase 5: EVALUATE — Đánh giá độ sẵn sàng và Ra quyết định

### AI Readiness Checklist (5 câu hỏi nhanh):
1. **Value:** [x] YES — Bài toán xảy ra hàng ngày tại các TTTM, gây mất 45-60 phút của khách và làm giảm doanh thu bán chéo của Vincom.
2. **Baseline:** [x] YES — Có baseline thủ công rõ ràng (thời gian 50 phút, tỷ lệ khách bỏ về sớm do mệt mỏi khoảng 20%).
3. **Eval:** [x] YES — Có bộ 50 prompt kịch bản mẫu về ngân sách và khẩu vị để đo task success rate và độ chính xác thực tế.
4. **Tolerance:** [x] YES — Sai số về mặt gợi ý không gây nguy hiểm, khách hàng có nút tùy chỉnh (HITL) để đổi quán theo ý thích.
5. **Operations:** [x] YES — Có quy trình rollback về chế độ tra cứu cơ bản, có nhật ký ghi nhận lỗi định kỳ.

### Quyết định cuối cùng:
**[x] GO (Tiến hành xây dựng bản mẫu Prototype)**

### Thử nghiệm tiếp theo nhỏ nhất (Smallest Next Experiment):
* **Nội dung thử nghiệm:** Xây dựng một prompt prototype chạy trên Gemini Flash để kiểm thử bộ 50 tình huống khách hàng điển hình (ví dụ: "3 bạn nam, 250k/người, 3 tiếng, thích ăn đồ nướng") kết hợp danh mục 50 gian hàng thực tế tại **Vincom Mega Mall Smart City**.
* **Tiêu chí đánh giá nghiệm thu (Exit Criteria):**
  1. 100% món ăn và gian hàng được gợi ý có thật trong TTTM.
  2. 100% tổng chi phí ước tính không vượt quá ngân sách yêu cầu.
  3. Thời gian phản hồi trung bình dưới 2.5 giây.
