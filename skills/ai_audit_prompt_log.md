# Hướng dẫn Kỹ năng: AI Audit Report & Prompt Logging

Tài liệu này định nghĩa kỹ năng và quy trình chuẩn để ghi lại lịch sử tương tác với AI và thực hiện kiểm định chất lượng đầu ra của AI trong suốt quá trình làm việc.

---

## 1. Mục tiêu & Ý nghĩa
- **Tính minh bạch (Transparency):** Lưu vết toàn bộ quá trình sử dụng các mô hình ngôn ngữ lớn (LLM/AI) trong bài làm.
- **Kiểm định chất lượng (AI Auditing):** Phát hiện các lỗi ảo tưởng (hallucination), thiên vị (bias) hoặc thiếu sót của AI nhằm đảm bảo tính chính xác theo chuẩn ISTQB và tài liệu môn học.
- **Tuân thủ quy chế (Ethics & Compliance):** Đảm bảo sự trung thực học thuật, phân định rõ ràng giữa nội dung do AI đề xuất và phần sinh viên tự hiệu chỉnh.

---

## 2. Quy trình Ghi nhận Prompt (`promt_log.md`)
Bất kỳ khi nào thực hiện giao tiếp với AI (Gemini, ChatGPT, Claude, Copilot, Cursor...), người thực hiện hoặc AI trợ lý phải ghi nhận lại chính xác và tuần tự vào file promt_log.md theo định dạng sau:

### Định dạng ghi nhận prompt:
```markdown
### [DD-MM-YYYY HH:MM] | Tên_Công_Cụ
```text
<Nội dung câu lệnh (Prompt) được gửi đi - Giữ nguyên gốc không dịch/tóm tắt>
```
```

### Ví dụ thực tế:
```markdown
### [27-05-2026 10:54] | Gemini
```text
my device is Daikin Inverter 1 HP FTKC25QVMV, search for more information then Declare brand, model, year, serial number (mask the middle 4 chars). Design 15 test cases (Objective / Input / Steps / Expected / Actual / Verdict) simple create a excel file for this: a sheet for test summary, a sheet for test cases
```

---

## 3. Quy trình Lập báo cáo AI Audit (`AI_Audit_Report.md`)
Khi hoàn thành một tác vụ quan trọng bằng AI, phải đối chiếu kết quả của AI với tài liệu kỹ thuật hoặc kiến thức chuẩn để đánh giá chất lượng. 

### 3.1. Cấu trúc bảng Audit cho mỗi tác vụ:
| Mục | Nội dung | Hướng dẫn chi tiết |
| :--- | :--- | :--- |
| **Prompt + tool** | Câu lệnh + Công cụ sử dụng | Ghi rõ prompt chính kèm theo tên công cụ và thời gian thực hiện. |
| **AI output** | Kết quả đầu ra của AI | Trích dẫn kết quả đầu ra hoặc đường dẫn ảnh chụp màn hình (ví dụ: `![Alt Text](image/abc.png)`) khoanh đỏ phần lỗi. |
| **Verdict** | Đánh giá | Chỉ chọn một trong ba trạng thái: **VALID** / **INVALID** / **INCOMPLETE**. |
| **Reasoning** | Lý do đánh giá | Viết từ 2-5 câu giải thích rõ dựa trên tài liệu thiết bị, slide bài giảng hoặc tiêu chuẩn ISTQB. |
| **Student fix** | Bản sửa đổi của sinh viên | Nội dung/Ảnh chụp màn hình sau khi sinh viên đã tự tay chỉnh sửa lại cho đúng. |

### 3.2. Tiêu chí đánh giá đầu ra (Verdict Guidelines):
1. **VALID (Hợp lệ):** AI đáp ứng đầy đủ và chính xác tất cả các yêu cầu trong prompt, không bị sai lệch thông tin thực tế, không có lỗi ảo tưởng (hallucination) hay thiên vị (bias).
2. **INVALID (Không hợp lệ):** AI sinh ra kết quả sai lệch nghiêm trọng, vi phạm các nguyên tắc kiểm thử cơ bản (như ISTQB) hoặc chứa các thông tin ảo tưởng (ví dụ: bịa đặt link tài liệu, bịa tính năng của thiết bị không có thật).
3. **INCOMPLETE (Chưa hoàn thiện):** AI đi đúng hướng nhưng bỏ sót các điều kiện biên, thiếu ngữ cảnh chi tiết, thiếu một số testcase đặc thù, hoặc cần sự bổ sung của con người mới đưa vào sử dụng thực tế được.

---

## 4. Tổng hợp Tỷ lệ Chính xác (Overall Accuracy Ratio)
Ở cuối báo cáo AI Audit (hoặc phần Appendix), luôn phải có bảng thống kê tổng hợp:

```markdown
### Overall AI Accuracy Ratio

| Trạng thái | Số lượng | Tỷ lệ phần trăm |
|---|---|---|
| VALID | X | X% |
| INVALID | Y | Y% |
| INCOMPLETE | Z | Z% |
| **Tổng số tác vụ kiểm định** | **N** | **100%** |
```

Kèm theo phần nhận xét đúc kết ngắn gọn:
- **AI Strengths (Điểm mạnh):** Những phần AI làm tốt (ví dụ: sinh template nhanh, gợi ý ý tưởng ban đầu, định dạng markdown).
- **AI Limitations (Hạn chế):** Những điểm AI thường xuyên làm sai (ví dụ: không kiểm thử được thiết bị vật lý, tạo link hỏng, lỗi logic nghiệp vụ sâu).
- **When to Use (Nên dùng khi nào):** Soạn thảo bản nháp, tạo khung tài liệu, chuyển đổi định dạng, gợi ý danh mục kiểm tra sơ bộ.
- **When NOT to Use (Không nên dùng khi nào):** Xác minh tính đúng đắn của sự thật lịch sử/kỹ thuật, đưa ra quyết định an toàn/bảo mật, kiểm thử các tính năng phần cứng/vật lý thực tế.
- **Key Principle (Nguyên tắc cốt lõi):** AI chỉ là trợ lý soạn thảo có thể sai sót, không phải là động cơ nghiên cứu có thẩm quyền. Sinh viên/Tester chịu trách nhiệm cuối cùng về chất lượng và độ chính xác của sản phẩm bàn giao.
