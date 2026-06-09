# Hướng dẫn Kỹ năng: Thiết kế Test Case & Báo cáo Lỗi (Bug Report) trên Markdown

## Part 1: Kỹ năng Thiết kế Test Case
Tài liệu quản lý test case phải được tổ chức trực tiếp trong các file Markdown (ví dụ: `test_cases.md`), sử dụng các bảng (Markdown Tables) để biểu diễn hai phần chính: **Features (Danh sách tính năng)** và **Test cases (Chi tiết kịch bản kiểm thử)**.

### 1. Bảng "Features" (Danh sách tính năng)
Dùng để liệt kê tất cả các Use Case hoặc các Tính năng chính cần kiểm thử.
- **Cấu trúc cột:**
  1. **ID:** Mã định danh tính năng (Ví dụ: `UC01`, `UC02`).
  2. **Feature Name:** Tên tính năng (Ví dụ: `Đăng nhập hệ thống`).
  3. **Description:** Mô tả ngắn gọn về nghiệp vụ hoặc hành vi của tính năng.
*(Lưu ý: Không bao gồm cột Remark).*

#### Ví dụ Bảng Features:
| ID | Feature Name | Description |
| :--- | :--- | :--- |
| UC01 | Đăng nhập hệ thống | Kiểm thử chức năng đăng nhập của người dùng bằng tài khoản và mật khẩu. |
| UC02 | Đặt lại mật khẩu | Kiểm thử chức năng khôi phục mật khẩu qua Email. |

### 2. Bảng "Test cases" (Chi tiết kịch bản kiểm thử)
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

#### Ví dụ Bảng Test cases:
| Function/Feature ID | Case ID | Test case name | Pre-requisites / Conditions | Test step | Expected Result (ER) | Actual Result | Status | Tester | Tested Date |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| UC01 | TC001 | Đăng nhập thành công với thông tin hợp lệ | Tài khoản `testuser` đã được kích hoạt trong hệ thống. | 1. Truy cập trang đăng nhập.<br>2. Nhập username `testuser`.<br>3. Nhập password đúng.<br>4. Nhấn nút "Đăng nhập". | 1. Hiển thị trang đăng nhập.<br>2. Username hiển thị đúng.<br>3. Password hiển thị dạng ẩn (*).<br>4. Điều hướng vào trang Dashboard. | The same ER | Pass | HTThanh | 09-06-2026 |
| UC01 | TC002 | Đăng nhập thất bại khi để trống mật khẩu | Không có | 1. Truy cập trang đăng nhập.<br>2. Nhập username `testuser`.<br>3. Để trống mật khẩu.<br>4. Nhấn nút "Đăng nhập". | 1. Hiển thị trang đăng nhập.<br>2. Username hiển thị đúng.<br>3. Trống mật khẩu.<br>4. Hiển thị thông báo lỗi: "Mật khẩu không được để trống". | The same ER | Pass | HTThanh | 09-06-2026 |

### 3. Nguyên tắc khi viết Test Case:
- **Tính nguyên tử (Atomicity):** Mỗi test case chỉ kiểm tra một mục tiêu duy nhất. Tránh gộp nhiều mục đích kiểm thử khác nhau vào cùng một Case ID.
- **Các bước rõ ràng:** Các bước thực hiện phải đủ chi tiết để bất kỳ tester nào đọc cũng có thể tái lập chính xác.
- **Kết quả mong đợi rõ ràng:** Kết quả mong đợi phải mang tính xác định (deterministic), có thể đo lường hoặc quan sát được trực tiếp, không viết chung chung.

---

## Part 2: Kỹ năng Viết Báo cáo Lỗi (Bug Report)
Khi phát hiện lỗi hệ thống, báo cáo lỗi phải được ghi nhận rõ ràng vào tài liệu Markdown (ví dụ: `bug_reports.md`) dưới dạng bảng quản lý lỗi (Bug Report Table) để lập trình viên có thể hiểu và tái hiện được lỗi ngay lập tức.

### 1. Cấu trúc cột trong bảng Bug Report:
1. **Defect ID:** Mã định danh lỗi duy nhất (Ví dụ: `B001`, `B002`).
2. **Defect Title (Tiêu đề lỗi):** Tóm tắt ngắn gọn lỗi xảy ra theo công thức:
   > **`[Tên_Mô_đun/Tính_năng] Hành vi lỗi - Trong điều kiện nào`**
   - *Ví dụ tốt:* `[Đăng nhập] Không báo lỗi khi để trống mật khẩu`
   - *Ví dụ tệ:* `Lỗi đăng nhập`
3. **Defect Description (Mô tả chi tiết lỗi):** Chứa đầy đủ thông tin để lập trình viên tái hiện lỗi. Trong bảng Markdown, sử dụng thẻ `<br>` để phân tách các mục:
   - **Pre-conditions (Điều kiện tiên quyết).**
   - **Steps to Reproduce (Các bước tái hiện lỗi):** Đánh số thứ tự từng bước cụ thể.
   - **Actual Result (Kết quả thực tế):** Hệ thống đã bị lỗi như thế nào.
   - **Expected Result (Kết quả mong đợi):** Đúng ra hệ thống phải hoạt động thế nào.
   - **Environment (Môi trường test):** OS, Browser, Phiên bản app hoặc thiết bị kiểm thử.
4. **Function ID:** ID của Use Case/Tính năng phát sinh lỗi (Ví dụ: `UC01`).
5. **Severity (Độ nghiêm trọng):**
   - **Critical (Khẩn cấp):** Hệ thống bị crash, hỏng dữ liệu, lỗi bảo mật nghiêm trọng hoặc chặn hoàn toàn luồng nghiệp vụ chính mà không có cách giải quyết thay thế.
   - **High (Cao):** Tính năng quan trọng hoạt động sai hoặc không hoạt động, ảnh hưởng lớn đến người dùng nhưng hệ thống vẫn chạy được.
   - **Medium (Trung bình):** Các tính năng phụ/biên hoạt động sai, hoặc luồng chính lỗi nhưng có phương án giải quyết tạm thời (workaround).
   - **Low (Thấp):** Lỗi giao diện (UI), lỗi chính tả, hoặc các lỗi thẩm mỹ không ảnh hưởng đến chức năng.
6. **Reported By:** Tên người phát hiện và báo cáo lỗi.
7. **Date Reported:** Ngày báo cáo lỗi.
8. **Status:** Trạng thái lỗi (`Open`, `In Progress`, `Resolved`, `Closed`).
9. **Comment:** Ghi chú hoặc thông tin bổ sung (nếu có).

#### Ví dụ Bảng Bug Report:
| Defect ID | Defect Title | Defect Description | Function ID | Severity | Reported By | Date Reported | Status | Comment |
| :---: | :--- | :--- | :---: | :---: | :--- | :---: | :---: | :--- |
| B001 | [Đăng nhập] Không báo lỗi khi để trống mật khẩu | **Pre-conditions:** Tài khoản `testuser` đã được kích hoạt.<br>**Steps to Reproduce:**<br>1. Truy cập trang đăng nhập.<br>2. Nhập username `testuser`.<br>3. Để trống mật khẩu.<br>4. Nhấn "Đăng nhập".<br>**Actual Result:** Trang web load vô tận và không hiển thị thông báo lỗi.<br>**Expected Result:** Hiển thị lỗi đỏ: "Mật khẩu không được để trống".<br>**Environment:** Chrome v120, Windows 11. | UC01 | High | HTThanh | 09-06-2026 | Open | Cần kiểm tra xem API có trả về mã 400 không. |

### 2. Nguyên tắc viết Bug Report hiệu quả:
- **Tái hiện được (Reproducibility):** Trước khi báo cáo, hãy chắc chắn lỗi đó có thể tái hiện được (ví dụ thử lại 3 lần trên môi trường sạch).
- **Không suy diễn cá nhân:** Chỉ mô tả thực tế hệ thống chạy sai thế nào so với đặc tả, tránh dùng các từ ngữ cảm xúc hay phán xét lập trình viên.
- **Đầy đủ bằng chứng:** Luôn đính kèm ảnh chụp màn hình (screenshot) hoặc video minh họa nếu lỗi liên quan đến giao diện hoặc luồng nghiệp vụ phức tạp.
