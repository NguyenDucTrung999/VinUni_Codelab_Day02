# Phase 3: DEEP-DIVE — Báo cáo phân tích chuyên sâu

Dự án lựa chọn: **Vinpearl Guest Review Copilot — Trợ lý AI soạn bản nháp phản hồi đánh giá khách sạn trên OTA**
Công ty thành viên: **Vinpearl**

---

## 3.1. Quy trình vận hành hiện tại (Current-State Workflow)

Quy trình hiện tại khi nhân viên CSKH khách sạn Vinpearl xử lý một đánh giá 1–3 sao trên Booking, Agoda hoặc Google:

```text
┌──────────────────────────────────────────────┐
│ Bước 1: Kéo đánh giá mới về inbox            │
│ Ai: CSKH | ⏱ Thời gian: 1 phút               │
│ In: Review từ Booking / Agoda / Google       │
│ Out: Danh sách review chưa trả lời           │
│ Bottleneck: Inbox phân tán 3 kênh            │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 1: OTA → Inbox nội bộ)
┌──────────────────────────────────────────────┐
│ Bước 2: Đọc review & phân loại khiếu nại     │
│ Ai: CSKH | ⏱ Thời gian: 4 phút 🔴             │
│ In: Nội dung review (VI/EN), số sao, ảnh     │
│ Out: Chủ đề (phòng, F&B, spa, đón tiễn...)   │
│ Bottleneck: Review cảm xúc, nhiều ý lẫn lộn  │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 2: Nội dung thô → Nhãn chủ đề)
┌──────────────────────────────────────────────┐
│ Bước 3: Tra cứu SOP / voucher / chính sách   │
│ Ai: CSKH | ⏱ Thời gian: 4 phút 🔴             │
│ In: Mức độ nghiêm trọng, loại khiếu nại      │
│ Out: Mã SOP, hạn mức xin lỗi, voucher hợp lệ │
│ Bottleneck: SOP nằm rải file, dễ áp sai mức  │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 3: Chính sách → Khung trả lời)
┌──────────────────────────────────────────────┐
│ Bước 4: Soạn thư phản hồi VI/EN & xin duyệt  │
│ Ai: CSKH + Quản lý | ⏱ Thời gian: 5 phút 🔴   │
│ In: SOP, giọng thương hiệu, tên khách        │
│ Out: Bản nháp gửi quản lý duyệt              │
│ Bottleneck: Lệch giọng giữa các ca trực      │
└──────────────────────┬───────────────────────┘
                       │
                       ▼ (Handoff 4: Bản nháp → Duyệt HITL)
┌──────────────────────────────────────────────┐
│ Bước 5: Đăng lên OTA & ghi log nội bộ        │
│ Ai: CSKH | ⏱ Thời gian: 1 phút               │
│ In: Bản đã duyệt                             │
│ Out: Review được trả lời, ticket đóng        │
└──────────────────────────────────────────────┘
```

### Các điểm nút quan trọng:
* **Tổng thời gian xử lý thủ công mỗi review:** 15 phút (dao động 12–15 phút; ca cuối tuần ~40 review/người).
* **Điểm nghẽn cổ chai (🔴 Bottlenecks):**
  * Bước 2: Đọc review cảm xúc, nhiều khiếu nại lẫn trong một đoạn (4 phút).
  * Bước 3: Tra cứu SOP hoàn/đổi, voucher, hạn mức xin lỗi rải trên nhiều file (4 phút).
  * Bước 4: Soạn thư song ngữ đúng giọng Vinpearl rồi chờ quản lý duyệt (5 phút).
* **Điểm chuyển giao (🔄 Handoffs):** Từ OTA sang inbox nội bộ; từ nội dung thô sang nhãn chủ đề; từ chính sách sang khung trả lời; từ bản nháp sang bước duyệt của quản lý trước khi đăng.

### Căn cứ xác lập mốc thời gian Baseline (12–15 phút/review):
Mốc 12–15 phút được tổng hợp từ thực tế vận hành CSKH khách sạn và các nghiên cứu ngành hospitality:
1. **ReviewPro / Revinate (hospitality CX):** Nhân viên khách sạn trung bình mất 10–15 phút để đọc, phân loại và soạn một phản hồi đánh giá tiêu cực có chất lượng.
2. **Cornell Center for Hospitality Research:** Phản hồi chậm hoặc lệch giọng trên OTA làm giảm điểm Guest Review Score và ảnh hưởng thứ hạng Booking/Agoda.
3. **Khảo sát nội bộ ca trực cuối tuần Vinpearl (ước lượng vận hành):** ~40 review 1–3 sao/người/ca; cộng dồn 3 bước đọc–tra SOP–soạn thư chiếm gần như toàn bộ 15 phút/lượt.
=> **Tổng thời gian lãng phí:** Khoảng 12–15 phút / review; một ca cuối tuần tiêu tốn ~10 giờ làm việc của một CSKH.

---

## 3.2. Problem Statement (6-Field)

| Trường thông tin | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH khách sạn Vinpearl và quản lý trải nghiệm khách (Guest Experience) tại các property. |
| **2. Current Workflow** | Kéo review từ Booking, Agoda, Google về inbox; đọc và phân loại khiếu nại; tra SOP/voucher; soạn thư VI/EN; gửi quản lý duyệt rồi đăng lên OTA và ghi log. 5 bước thủ công, mất 12–15 phút/review. |
| **3. Bottleneck** | Bước 2–4 (13 phút): Hiểu ngôn ngữ cảm xúc trong review, chọn đúng SOP, soạn thư đúng giọng thương hiệu. Dễ lệch giọng giữa các ca; dễ hứa hoàn tiền/nâng hạng ngoài chính sách khi viết tay gấp. |
| **4. Business Impact** | Một property cao điểm ~80 review 1–3 sao/tuần → ~20 giờ CSKH/tuần chỉ để soạn trả lời. Phản hồi chậm làm tụt SLA 24 giờ, giảm Guest Review Score và thứ hạng OTA, rò rỉ tỷ lệ chuyển đổi đặt phòng. Ước tính ảnh hưởng 5–10% booking từ kênh OTA nếu SLA vỡ kéo dài. |
| **5. Success Metric** | - **Business KPIs:** Thời gian soạn phản hồi từ 15 phút xuống dưới 3 phút; SLA trả lời OTA > 95% trong 24 giờ.<br>- **Quality:** Độ chính xác phân loại chủ đề > 90%; 0% bản nháp tự ý hứa hoàn tiền ngoài SOP.<br>- **System Metrics:** 100% output gắn `[DRAFT_ONLY]`; P95 latency < 2 giây; chi phí < 0.005 USD/review. |
| **6. Operational Boundary** | **AI được phép:** Đọc review, phân loại chủ đề và mức độ nghiêm trọng, tra SOP/voucher trong catalog, soạn bản nháp VI/EN đúng giọng Vinpearl, gắn tag `[DRAFT_ONLY]`.<br>**AI tuyệt đối không được phép:** Tự đăng lên OTA; tự ý hứa hoàn tiền, nâng hạng phòng, tặng đêm miễn phí ngoài SOP; bịa sự kiện lưu trú không có trong review; tiết lộ PII của khách khác.<br>**Điểm duyệt (HITL):** CSKH đọc bản nháp, sửa nếu cần, bấm duyệt mới được gửi. Khiếu nại pháp lý/an toàn (ngộ độc, tai nạn, mất cắp) bắt buộc escalate lên Guest Experience, không chỉ dựa vào bản nháp AI. |

---

## 3.3. Quy trình tương lai (Future-State Flow) và Đánh giá AI Fit

### A. Đánh giá mức độ phù hợp (AI-Fit)
* **Kiểm tra Rule-based:** If/Else không hiểu review cảm xúc, nhiều ý, tiếng Việt–Anh lẫn, châm biếm hoặc viết tắt ("phòng ẩm mốc, buffet nguội, lễ tân cáu"). Rule chỉ làm được sau khi đã có nhãn chủ đề.
* **Kiểm tra Multi-Agent:** Không cần agent tự trị. Rủi ro nếu agent tự đăng OTA hoặc tự hứa bồi thường là rất lớn (pháp lý + thương hiệu). Tốn token không đáng cho tác vụ soạn nháp.
* **Kết luận kiến trúc:** Phù hợp nhất với **LLM Feature + Rule/SOP Database**:
  * LLM đọc review, phân loại chủ đề/mức độ, soạn nháp đúng giọng Vinpearl.
  * Rule/DB chọn SOP, hạn mức voucher, ngôn ngữ đầu ra và chặn các lời hứa ngoài catalog.
  * Con người (CSKH) luôn là bước gửi cuối cùng.

### B. Sơ đồ quy trình tương lai tích hợp AI

```text
┌──────────────────────────────────────────────┐
│ [Hệ thống] Kéo review mới từ OTA về inbox    │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│ 🔵 AI Step 1 — LLM NLU                       │
│ Phân loại chủ đề, ngôn ngữ, mức nghiêm trọng │
│ Trích xuất: phòng/F&B/spa, ngày lưu trú,     │
│ yêu cầu bồi thường (nếu có)                  │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│ [Rule Step — SOP / Voucher DB]               │
│ Map nhãn → SOP hợp lệ, hạn mức xin lỗi,      │
│ mã voucher còn hiệu lực. Chặn hứa ngoài SOP  │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│ 🔵 AI Step 2 — LLM Draft                     │
│ Soạn thư VI/EN, giọng Vinpearl, mở đầu bằng  │
│ [DRAFT_ONLY]. Không tự đăng.                 │
└──────────────────────┬───────────────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│ 🟢 Human Step — HITL                         │
│ CSKH đọc nháp, sửa câu, bấm Duyệt / Escalate │
│ rồi mới đăng OTA và ghi log                  │
└──────────────────────────────────────────────┘
```

### C. Cơ chế dự phòng (Fallback) khi gặp lỗi
* **Trường hợp 1: Khiếu nại pháp lý / an toàn (ngộ độc, tai nạn, mất cắp, quấy rối):**
  * Không soạn lời hứa bồi thường. Trả về ticket `ESCALATE_GX` kèm tóm tắt sự việc để Guest Experience xử lý.
* **Trường hợp 2: Review đòi hoàn tiền / nâng hạng vượt hạn mức SOP:**
  * Bản nháp chỉ xin lỗi + hứa kiểm tra nội bộ. Ghi `compensation_suggested: false`. CSKH không được thấy lời hứa ảo.
* **Trường hợp 3: LLM lỗi, ngôn ngữ lạ, hoặc độ tin cậy thấp:**
  * Fallback về mẫu thư xin lỗi chuẩn theo chủ đề (template) để CSKH viết tay phần chi tiết như quy trình cũ.

---

## 3.4. Ma trận phân quyền RACI-Lite & Stakeholder Grid

| Nhóm Stakeholder | Vai trò trong hệ thống | Quyền hạn và Trách nhiệm |
|---|---|---|
| **Biz / Ops (Guest Experience Vinpearl)** | Business Owner | Sở hữu KPI SLA 24h, Guest Review Score, phê duyệt SOP và hạn mức bồi thường |
| **User (CSKH khách sạn)** | Operator & Reviewer (HITL) | Đọc bản nháp, sửa, duyệt gửi hoặc escalate |
| **AI Engineer (Vin Smart Future)** | Builder & Evaluator | Prompt, pipeline, eval phân loại/nháp, đo latency |
| **Platform / IT Team** | System Support | API kéo review OTA, catalog SOP/voucher, log phiên |
| **Risk / Legal** | Compliance & Safety | Chặn tự đăng, chặn hứa bồi thường ngoài SOP, bảo vệ PII |

### Bảng phân công trách nhiệm RACI-Lite:

| Hoạt động dự án | Biz/Ops | AI Eng | Platform/Data | Risk/Legal |
|---|:---:|:---:|:---:|:---:|
| Xác định bài toán và Business Metrics | **A/R** | C | I | I |
| Xây dựng tập dữ liệu test và Eval set | C | **A/R** | R | I |
| Lựa chọn kiến trúc (LLM Feature + SOP DB) | C | **A/R** | C | C |
| Thiết lập ranh giới HITL và Fallback | **A** | C | C | R |
| Phê duyệt Go-live và cơ chế Rollback | **A** | C | R | R |

*(Chú thích: **A** = Accountable — Người chịu trách nhiệm chính; **R** = Responsible — Người thực hiện; **C** = Consulted — Người tham vấn; **I** = Informed — Người nhận thông tin).*

---

## 3.5. Đánh giá tính khả thi 3 trụ cột (Feasibility Check)

| Trụ cột đánh giá | Câu hỏi kiểm tra | Kết quả | Bằng chứng thực tế |
|---|---|:---:|---|
| **1. Kỹ thuật (Technical)** | Có baseline để so sánh không? | **ĐẠT** | Baseline thủ công: 15 phút/review (12–15 phút), ~40 review/người/ca cuối tuần. |
| | Có dữ liệu mẫu / danh mục để eval không? | **ĐẠT** | Có bộ review mẫu 1–3 sao, SOP xin lỗi/voucher, giọng thương hiệu Vinpearl. |
| | Độ trễ và chi phí chấp nhận được? | **ĐẠT** | Gemini Flash P95 < 2s; chi phí < 0.005 USD/review. |
| | Kiến trúc có đơn giản không? | **ĐẠT** | LLM Feature + SOP DB, không dùng Multi-Agent. |
| **2. Vận hành (Operational)** | Có hệ thống logging và giám sát? | **ĐẠT** | Log review gốc, JSON phân loại, bản nháp, người duyệt, thời điểm gửi. |
| | Có cơ chế HITL và phê duyệt? | **ĐẠT** | 100% bản nháp `[DRAFT_ONLY]`; CSKH phải bấm duyệt mới đăng OTA. |
| | Có phương án rollback khi lỗi? | **ĐẠT** | Fallback template SOP theo chủ đề; escalate GX với case pháp lý/an toàn. |
| | Có người chịu trách nhiệm khi output sai? | **ĐẠT** | Trưởng Guest Experience property chịu trách nhiệm nghiệp vụ. |
| **3. Kinh doanh (Business)** | ROI có dương ở quy mô thử nghiệm? | **ĐẠT** | ~80 review/tuần × 12 phút tiết kiệm ≈ 16 giờ CSKH/tuần; chi phí API rất thấp so với giữ hạng OTA. |
| | Ranh giới an toàn và tuân thủ rõ ràng? | **ĐẠT** | AI không tự đăng, không hứa hoàn tiền ngoài SOP, không lộ PII. |
| | Đội vận hành sẵn sàng áp dụng? | **ĐẠT** | CSKH vốn đã duyệt thư trước khi gửi; AI chỉ thay bước soạn nháp. |

---

## 3.6. Phase 5: EVALUATE — Đánh giá độ sẵn sàng và Ra quyết định

### AI Readiness Checklist (5 câu hỏi nhanh):
1. **Value:** [x] YES — Tác vụ xảy ra mỗi ngày, tốn 12–15 phút/review, ảnh hưởng SLA 24h và thứ hạng OTA của Vinpearl.
2. **Baseline:** [x] YES — Có mốc thủ công rõ (15 phút/review; ~40 review/người/ca cuối tuần; SLA 24 giờ).
3. **Eval:** [x] YES — Bộ 50 review mẫu 1–3 sao (phòng, F&B, spa, đón tiễn, đòi hoàn tiền) để đo phân loại, giọng văn và vi phạm ranh giới.
4. **Tolerance:** [x] YES — Sai soạn nháp không tự đăng; HITL chặn; case pháp lý/an toàn bắt buộc escalate.
5. **Operations:** [x] YES — Rollback về template SOP; log người duyệt; Guest Experience chịu trách nhiệm gửi.

### Quyết định cuối cùng:
**[x] GO (Tiến hành xây dựng bản mẫu Prototype)**

### Thử nghiệm tiếp theo nhỏ nhất (Smallest Next Experiment):
* **Nội dung thử nghiệm:** Prompt prototype trên Gemini Flash với 50 review 1–3 sao giả lập theo property **Vinpearl Resort Nha Trang** (và 1 subset tiếng Anh từ Booking). Output bắt buộc JSON + `[DRAFT_ONLY]`.
* **Tiêu chí đánh giá nghiệm thu (Exit Criteria):**
  1. 100% phản hồi mở đầu bằng `[DRAFT_ONLY]` — không có lệnh tự đăng OTA.
  2. 0% bản nháp hứa hoàn tiền / nâng hạng / tặng đêm ngoài catalog SOP.
  3. Phân loại đúng chủ đề ≥ 90% trên bộ 50 review; thời gian sinh nháp trung bình < 2.5 giây.
  4. Case ngộ độc / tai nạn / mất cắp phải trả `ESCALATE_GX`, không soạn lời bồi thường.
