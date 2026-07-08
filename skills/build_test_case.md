# Hướng dẫn Kỹ năng: Thiết kế Test Case trên Markdown
Tài liệu quản lý test case phải được tổ chức trực tiếp trong các file Markdown (ví dụ: `test_cases.md`), sử dụng các bảng (Markdown Tables) để biểu diễn hai phần chính: **Features (Danh sách tính năng)** và **Test cases (Chi tiết kịch bản kiểm thử)**.

## 1. Bảng "Features" (Danh sách tính năng)
Dùng để liệt kê tất cả các Use Case hoặc các Tính năng chính cần kiểm thử.
- **Cấu trúc cột:**
  1. **ID:** Mã định danh tính năng (Ví dụ: `UC01`, `UC02`).
  2. **Feature Name:** Tên tính năng (Ví dụ: `Đăng nhập hệ thống`).
  3. **Description:** Mô tả ngắn gọn về nghiệp vụ hoặc hành vi của tính năng.
*(Lưu ý: Không bao gồm cột Remark).*

### Ví dụ Bảng Features:
| ID | Feature Name | Description |
| :--- | :--- | :--- |
| UC01 | Đăng nhập hệ thống | Kiểm thử chức năng đăng nhập của người dùng bằng tài khoản và mật khẩu. |
| UC02 | Đặt lại mật khẩu | Kiểm thử chức năng khôi phục mật khẩu qua Email. |

## 2. Bảng "Test cases" (Chi tiết kịch bản kiểm thử)
Liệt kê chi tiết các kịch bản kiểm thử tương ứng cho từng tính năng.
- **Cấu trúc cột:**
  1. **Function/Feature ID:** Liên kết với ID trong bảng Features (Ví dụ: `UC01`).
  2. **Case ID:** Mã định danh kịch bản (Ví dụ: `UI01`, `TC-001`).
  3. **Test case name:** Mục tiêu/Tên kịch bản (Ngắn gọn, rõ nghĩa hành vi muốn test).
  4. **Pre-requisites / Conditions (Điều kiện tiên quyết):** Trạng thái hệ thống/thiết bị hoặc dữ liệu cần có sẵn trước khi test (Ví dụ: `AC đang tắt; nguồn điện 220V sẵn sàng`, hoặc `Tài khoản kiểm thử đã được tạo và kích hoạt`).
  5. **Test step (Các bước thực hiện):** Chuỗi các hành động thực thi, được đánh số thứ tự rõ ràng. Trong bảng Markdown, sử dụng thẻ `<br>` để xuống dòng giữa các bước (Ví dụ: `1. Truy cập trang đăng nhập <br> 2. Nhập thông tin`).
  6. **Expected Result (ER - Kết quả mong đợi):** Trạng thái đầu ra mong muốn của hệ thống/thiết bị tương ứng với từng bước hành động. Sử dụng thẻ `<br>` tương ứng nếu có nhiều bước.
  7. **Actual Result (Kết quả thực tế):** Ghi lại trạng thái hệ thống khi test trực tiếp. Nếu khớp với ER, ghi `The same ER` hoặc mô tả cụ thể.
  8. **Status (Trạng thái):** Kết quả kiểm thử (`Pass`, `Fail`, `Block`).
  9. **Tester:** Tên người thực hiện test (Ví dụ: `HTThanh`).
  10. **Tested Date:** Ngày thực hiện kiểm thử (Định dạng ngày cụ thể).
*(Lưu ý: Không bao gồm cột Remark).*

### Ví dụ Bảng Test cases:
| Function/Feature ID | Case ID | Test case name | Pre-requisites / Conditions | Test step | Expected Result (ER) | Actual Result | Status | Tester | Tested Date |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| UC01 | TC001 | Đăng nhập thành công với thông tin hợp lệ | Tài khoản `testuser` đã được kích hoạt trong hệ thống. | 1. Truy cập trang đăng nhập.<br>2. Nhập username `testuser`.<br>3. Nhập password đúng.<br>4. Nhấn nút "Đăng nhập". | 1. Hiển thị trang đăng nhập.<br>2. Username hiển thị đúng.<br>3. Password hiển thị dạng ẩn (*).<br>4. Điều hướng vào trang Dashboard. | The same ER | Pass | HTThanh | 09-06-2026 |
| UC01 | TC002 | Đăng nhập thất bại khi để trống mật khẩu | Không có | 1. Truy cập trang đăng nhập.<br>2. Nhập username `testuser`.<br>3. Để trống mật khẩu.<br>4. Nhấn nút "Đăng nhập". | 1. Hiển thị trang đăng nhập.<br>2. Username hiển thị đúng.<br>3. Trống mật khẩu.<br>4. Hiển thị thông báo lỗi: "Mật khẩu không được để trống". | The same ER | Pass | HTThanh | 09-06-2026 |

## 3. Nguyên tắc khi viết Test Case:
- **Tính nguyên tử (Atomicity):** Mỗi test case chỉ kiểm tra một mục tiêu duy nhất. Tránh gộp nhiều mục đích kiểm thử khác nhau vào cùng một Case ID.
- **Các bước rõ ràng:** Các bước thực hiện phải đủ chi tiết để bất kỳ tester nào đọc cũng có thể tái lập chính xác.
- **Kết quả mong đợi rõ ràng:** Kết quả mong đợi phải mang tính xác định (deterministic), có thể đo lường hoặc quan sát được trực tiếp, không viết chung chung.