# 🏗️ Phase 3 — DEEP-DIVE (Cá nhân)
## 3.1. Current-State Workflow

Quy trình tiếp nhận, phân loại và điều phối ticket sửa chữa của cư dân tại Ban quản lý tòa nhà Vinhomes:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2  🔴  │     │ Bước 3 🔴    │
│ Cư dân gửi   │     │ CSKH đọc và  │     │ Phân loại    │
│ ticket qua   │ ──→ │ xác minh nội │ ──→ │ loại sự cố   │
│ ứng dụng     │     │ dung, ảnh    │     │ và mức độ    │
│              │     │              │     │ ưu tiên      │
│ Ai: Cư dân   │     │ Ai: CSKH     │     │ Ai: CSKH     │
│ In: Text/Ảnh │     │ In: Ticket   │     │ In: Nội dung │
│ Out: Ticket  │     │ Out: Xác minh│     │ Out: Category│
│ ⏱ 1 phút*   │      │ ⏱ 2 phút*   │     │ ⏱ 3 phút*   │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                 │
                                                 ▼
                    ┌──────────────┐     ┌──────────────┐
                    │ Bước 5       │     │ Bước 4 🔴    │
                    │ Cập nhật     │     │ Điều phối    │
                    │ trạng thái   │ ←── │ ticket đến   │
                    │ và thông báo │     │ tổ kỹ thuật  │
                    │ cư dân       │     │ phù hợp      │
                    │              │     │              │
                    │ Ai: CSKH     │     │ Ai: CSKH     │
                    │ In: Kết quả  │     │ In: Category │
                    │ Out: Thông báo│    │ Out: Team    │
                    │ ⏱ 1 phút*   │     │ ⏱ 2 phút*    │
                    └──────────────┘     └──────────────┘

🔴 = Bottlenecks
⏱ Tổng thời gian thao tác thủ công ước tính: 5–10 phút/ticket.
```

Phân tích Bottleneck:
- Bước 2: Nhân viên CSKH phải đọc từng ticket, kiểm tra mô tả tự do và xem ảnh đính kèm. Cư dân có thể mô tả thiếu thông tin, sai chính tả hoặc sử dụng từ ngữ không thống nhất.
- Bước 3: Nhân viên phải xác định loại sự cố và mức độ ưu tiên. Một ticket có thể chứa nhiều vấn đề hoặc mô tả không rõ nguyên nhân.
- Bước 4: Nhân viên phải xác định tổ kỹ thuật phụ trách dựa trên category và quy định vận hành. Nếu chuyển nhầm tổ, ticket phải qua thêm một vòng điều phối.
Handoff quan trọng: CSKH → Tổ kỹ thuật. Việc phân loại hoặc điều phối sai có thể làm tăng thời gian xử lý và ảnh hưởng SLA.

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH/Ban quản lý tòa nhà Vinhomes là người tiếp nhận, xác minh, phân loại và điều phối ticket. Cư dân là người gửi yêu cầu qua ứng dụng. |
| **2. Current Workflow** | Cư dân gửi ticket kèm mô tả văn bản và ảnh (nếu có). Nhân viên CSKH đọc nội dung, xác minh thông tin, gắn tag phân loại sự cố, xác định mức độ ưu tiên và chuyển ticket đến tổ kỹ thuật phù hợp. Quy trình gồm 5 bước, mất khoảng 5–10 phút thao tác thủ công/ticket (ước tính ban đầu). |
| **3. Bottleneck** | Bước 2, 3 và 4: Nhân viên phải đọc mô tả, phân tích ảnh, xác định category, priority và tìm tổ kỹ thuật phù hợp. Ticket mơ hồ hoặc chứa nhiều sự cố dễ bị phân loại sai, dẫn đến chuyển nhầm bộ phận. |
| **4. Business Impact** | 	Tốn thời gian lao động của nhân viên CSKH; tăng chi phí vận hành khi số lượng ticket lớn; phát sinh thời gian xử lý lại do chuyển nhầm tổ; có nguy cơ vi phạm SLA và làm giảm trải nghiệm cư dân. |
| **5. Success Metric** | 	1. Giảm thời gian phân loại và điều phối ticket từ 5–10 phút xuống dưới 1 phút (Efficiency).<br>2. Đảm bảo ticket được phân loại đúng nhóm sự cố và đề xuất đúng tổ kỹ thuật phụ trách, hướng tới độ chính xác ≥ 90% (Quality). |
| **6. Operational Boundary** | **AI được phép:** Phân tích nội dung ticket, trích xuất thông tin, đề xuất category, priority, tổ kỹ thuật và tạo bản tóm tắt. **CẤM:** AI không được tự kết luận nguyên nhân kỹ thuật, xác nhận đã sửa chữa hoặc tự đóng ticket. Ticket có dấu hiệu nguy hiểm, confidence thấp hoặc thông tin không đầy đủ bắt buộc chuyển cho nhân viên xác minh (HITL). |

---

## 3.3. Future-State Flow & AI Fit
* **AI Fit:** họn **Hybrid Architecture** (Rule-based + LLM Feature). Không cần Agentic Loop vì quy trình phân loại và điều phối ticket có các bước xác định, quy tắc nghiệp vụ rõ ràng. Backend workflow chịu trách nhiệm thực thi các hành động có kiểm soát.

* **Quy trình tương lai (Future-State):**
```text
┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │
│ Cư dân gửi   │     │ Backend kiểm │
│ ticket qua   │ ──→ │ tra dữ liệu  │
│ ứng dụng     │     │ đầu vào      │
│              │     │              │
│ In: Text/Ảnh │     │ Rule: Validate│
└──────────────┘     └──────┬───────┘
                            │
                            ▼
                  ┌──────────────────┐
                  │ Ticket rõ ràng?  │
                  └───────┬──────────┘
                          │
              ┌───────────┴───────────┐
              ▼                       ▼
      ┌──────────────┐       ┌──────────────┐
      │ RULE-BASED   │       │ 🔵 LLM       │
      │ Phân loại    │       │ Hiểu mô tả   │
      │ và mapping   │       │ tự do, trích │
      │ tổ kỹ thuật  │       │ xuất thông tin│
      └──────┬───────┘       └──────┬───────┘
             └──────────┬───────────┘
                        ▼
              ┌──────────────────┐
              │ Bước 4           │
              │ Validation Layer │
              │ Kiểm tra schema, │
              │ category, priority│
              │ và confidence    │
              └────────┬─────────┘
                       ▼
              ┌──────────────────┐
              │ Đủ điều kiện     │
              │ auto-routing?    │
              └────────┬─────────┘
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
      ┌──────────────┐   ┌────────────────┐
      │ Backend tự   │   │ 🟢 HUMAN       │
      │ động chuyển  │   │ CSKH xác nhận  │
      │ đúng tổ      │   │ và chỉnh sửa   │
      └──────┬───────┘   └───────┬────────┘
             └─────────┬─────────┘
                       ▼
              ┌──────────────────┐
              │ Bước 5           │
              │ Cập nhật ticket  │
              │ và thông báo     │
              │ cho cư dân       │
              └──────────────────┘

↩️ FALLBACK:
- LLM lỗi API hoặc timeout → chuyển ticket về CSKH.
- Confidence thấp → yêu cầu nhân viên xác nhận.
- Không tìm thấy tổ kỹ thuật phù hợp → chuyển queue thủ công.
```
---

# 🏁 Phase 5 — EVALUATE 
### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[x] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Hiện tại mới dừng ở mức phân tích ý tưởng, chưa có dữ liệu ticket thực tế được gán nhãn để kiểm thử, chưa xác định được baseline chính xác về thời gian xử lý và tỷ lệ phân loại sai. Vì vậy, chưa đủ bằng chứng để khẳng định mô hình AI hiệu quả và tiết kiệm chi phí hơn quy trình hiện tại. Tôi đề xuất lựa chọn NOT YET, ưu tiên thu thập dữ liệu, xác lập baseline và so sánh ba phương án Rule-based, LLM và Hybrid (Rule-based + LLM).

---