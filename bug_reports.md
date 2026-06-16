# Danh sách Báo cáo Lỗi (Bug Reports) - HW02

Dưới đây là bảng quản lý các lỗi phát hiện được trong quá trình kiểm thử hệ thống EShop. Bảng được thiết kế theo đúng quy chuẩn định nghĩa trong tài liệu hướng dẫn kiểm thử.

| Defect ID | Defect Title | Defect Description | Function ID | Severity | Reported By | Date Reported | Status | Comment |
| :---: | :--- | :--- | :---: | :---: | :--- | :---: | :---: | :--- |
| **B001** | `[Mô_đun/Tính_năng] Tóm_tắt_lỗi_ở_đây - Khi_làm_hành_động_gì` | **Pre-conditions:**<br>- Điền các điều kiện cần thiết ở đây.<br>**Steps to Reproduce:**<br>1. Bước 1.<br>2. Bước 2.<br>3. Bước 3.<br>**Actual Result:** Mô tả lỗi thực tế hệ thống chạy sai.<br>**Expected Result:** Mô tả hệ thống đúng ra phải chạy như thế nào.<br>**Environment:** OS (ví dụ: Windows 11), Browser (ví dụ: Chrome v120), App version/commit. | FR05 | Medium | [Tên_Tester] | 16-06-2026 | Open | Ghi chú thêm (nếu có) |
| **B002** | *[Khung thông tin trống]* | **Pre-conditions:**<br><br>**Steps to Reproduce:**<br>1. <br>2. <br>**Actual Result:** <br>**Expected Result:** <br>**Environment:**  | FR05 | Medium | [Tên_Tester] | 16-06-2026 | Open | |

---

## Hướng dẫn sử dụng bảng Báo cáo Lỗi (Bug Report Guidelines)

1. **Defect ID:** Đánh mã tăng dần duy nhất (`B001`, `B002`, `B003`,...).
2. **Defect Title:** Đặt tên ngắn gọn, súc tích theo cấu trúc: `[Tên_Mô_đun] Hành vi lỗi - Trong điều kiện nào`.
3. **Defect Description:** Cung cấp đầy đủ các thông tin:
   * **Pre-conditions:** Trạng thái hệ thống trước khi thực hiện test.
   * **Steps to Reproduce:** Đánh số thứ tự các bước để bất kỳ ai cũng có thể thực hiện lại và thấy lỗi.
   * **Actual Result & Expected Result:** Chỉ rõ sự khác biệt giữa thực tế lỗi và tài liệu đặc tả yêu cầu.
   * **Environment:** Ghi lại hệ điều hành, trình duyệt và phiên bản kiểm thử.
4. **Severity:** Chọn một trong các mức độ nghiêm trọng:
   * `Critical` (Khẩn cấp - Crash hệ thống, bảo mật nghiêm trọng).
   * `High` (Cao - Sai logic nghiệp vụ chính, không có giải pháp thay thế).
   * `Medium` (Trung bình - Lỗi tính năng phụ, có cách tránh tạm thời).
   * `Low` (Thấp - Lỗi giao diện UI, sai chính tả).
5. **Đính kèm minh chứng:** Đối với mỗi lỗi tìm thấy, cần chụp ảnh màn hình lỗi (screenshot) và tải lên làm minh chứng khi tạo GitHub Issues trên trang của nhóm.
