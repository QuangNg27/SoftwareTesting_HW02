# Hướng dẫn Kỹ năng: Phân tích Miền (Domain Testing) & Phân tích Giá trị Biên (BVA)

## 1. Khái niệm Cốt lõi
- **Domain Testing (Phân tích miền):** Là chiến lược lấy mẫu phân tầng (stratified sampling) nhằm chia nhỏ miền giá trị của biến đầu vào/đầu ra thành các miền con (lớp tương đương - equivalence classes). Hai giá trị kiểm thử thuộc cùng một lớp tương đương nếu hệ thống xử lý chúng theo cùng một cách thức và trả về kết quả cùng loại.
- **Boundary Value Analysis (BVA - Phân tích giá trị biên):** Là kỹ thuật chọn lựa các giá trị kiểm thử nằm ngay trên đường biên hoặc lân cận đường biên của các miền tương đương, vì đây là những nơi có xác suất xảy ra lỗi lập trình (lỗi off-by-one, sai dấu so sánh `<=`, `<`) cao nhất.

---

## 2. Quy trình Thiết kế Lớp Tương Đương (Equivalence Classes)
Để thiết kế kịch bản test bằng phân tích miền, thực hiện theo 4 bước chuẩn hóa sau:

### Bước 1: Xác định biến Đầu vào (Input) & Đầu ra (Output)
- Liệt kê toàn bộ các biến đầu vào của hệ thống (ví dụ: các trường nhập liệu, tham số API, các nút bấm vật lý, trạng thái đăng nhập, v.v.).
- Liệt kê toàn bộ các biến đầu ra hoặc phản hồi của hệ thống tương ứng (ví dụ: thông báo lỗi, thay đổi trạng thái, giao diện hiển thị, v.v.).

### Bước 2: Xác định các Lớp Tương đương (Equivalence Classes)
Chia miền giá trị của từng biến thành các lớp:
- **Lớp tương đương Hợp lệ (Valid Equivalence Classes):** Các giá trị đúng quy cách, được hệ thống chấp nhận.
- **Lớp tương đương Không hợp lệ (Invalid Equivalence Classes):** Các giá trị sai quy cách, lỗi, bị hệ thống từ chối hoặc trả về thông báo lỗi.

### Bước 3: Tìm giá trị Đại diện Tốt nhất (Best Representatives)
- Chọn giá trị đại diện tiêu biểu cho từng lớp tương đương (đặc biệt đối với các miền không có thứ tự hoặc các giá trị đặc biệt như XSS, SQL Injection).

### Bước 4: Tạo Dữ liệu Kiểm thử (Test Data)
- Thiết lập bảng Dữ liệu Kiểm thử (Test Data) dựa trên các lớp tương đương đã phân tích.
- **Cấu trúc bảng:** Từng biến đầu vào được xác định ở Bước 1 phải có riêng 1 cột trong bảng test data để mô tả cụ thể giá trị truyền vào.
- Các cột còn lại bao gồm: **ID**, **Expected Output (Đầu ra mong đợi)**, và **Phủ EP**.
- Các dòng test data phải khớp và đồng bộ 1-to-1 với các kịch bản kiểm thử (Test Cases) chi tiết.

---

## 3. Quy trình Phân tích Giá trị Biên (BVA)
BVA được thực hiện độc lập và biểu diễn thành một phần phân tích riêng biệt:

- Xác định các giới hạn biên có thứ tự của biến (ví dụ: độ dài chuỗi $L$, giá trị số đơn hàng, số lượt sử dụng).
- Áp dụng quy tắc kiểm thử biên:
  - **Biên dưới (LB):** Kiểm thử `LB - 1` (Không hợp lệ), `LB` (Hợp lệ), `LB + 1` (Hợp lệ).
  - **Biên trên (UB):** Kiểm thử `UB - 1` (Hợp lệ), `UB` (Hợp lệ), `UB + 1` (Không hợp lệ).
- **Tạo Dữ liệu Kiểm thử (Test Data) cho BVA:**
  - Thiết lập bảng test data cho BVA tương tự như phần EP.
  - **Cấu trúc bảng:** Mỗi biến đầu vào phải có riêng 1 cột, các cột còn lại là **ID**, **Expected Output (Đầu ra mong đợi)**, và **Phủ BVA**.

---

## 4. Quy tắc Lựa chọn & Phối hợp Test Case để tối ưu hóa
Khi thiết kế test case chi tiết từ bảng Test Data:

1. **Nguyên tắc bao phủ lớp Hợp lệ (Valid Classes):**
   - Thiết kế các kịch bản kiểm thử sao cho **một test case có thể bao phủ đồng thời nhiều lớp tương đương hợp lệ** của các biến khác nhau cùng một lúc để giảm thiểu số lượng test case cần chạy.
2. **Nguyên tắc bao phủ lớp Không hợp lệ (Invalid Classes):**
   - **Mỗi test case chỉ được phép chứa duy nhất một giá trị không hợp lệ** của một biến, các biến khác phải giữ ở giá trị hợp lệ. Việc này nhằm tránh hiện tượng che khuất lỗi (error masking).

---

## 5. Ví dụ Thực tế Thiết kế Bảng phân hoạch & Dữ liệu Kiểm thử
**Yêu cầu:** Nhập một số nguyên dương nhỏ hơn 100.
**Biến đầu vào:** `X` (Kiểu số nguyên).

### Bảng phân hoạch lớp tương đương:
| ID | Điều kiện | Lớp tương đương Hợp lệ | Lớp tương đương Không hợp lệ |
|---|---|---|---|
| **C1** | Kiểu dữ liệu | EC1: Là số nguyên | EC2: Không phải số nguyên (ký tự, số thực) |
| **C2** | Giá trị của X | EC3: `0 < X < 100` | EC4: `X <= 0` <br> EC5: `X >= 100` |

### Bước 4: Tạo Dữ liệu Kiểm thử (Test Data) cho EP
| ID | X | Expected Output (Đầu ra mong đợi) | Phủ EP |
| :---: | :--- | :--- | :--- |
| 1 | `50` | Hệ thống chấp nhận giá trị và tiếp tục xử lý. | `EC1`, `EC3` (Hợp lệ) |
| 2 | `0` | Hệ thống từ chối và hiển thị thông báo lỗi giá trị không hợp lệ. | `EC4` (Không hợp lệ) |
| 3 | `100` | Hệ thống từ chối và hiển thị thông báo lỗi giá trị không hợp lệ. | `EC5` (Không hợp lệ) |
| 4 | `5.5` hoặc `'abc'` | Hệ thống từ chối và hiển thị thông báo lỗi sai định dạng kiểu dữ liệu. | `EC2` (Không hợp lệ) |

### Phân tích Giá trị Biên (BVA):
- Biên dưới: `LB = 1` $\rightarrow$ Điểm test: `0` (LB-1), `1` (LB), `2` (LB+1).
- Biên trên: `UB = 99` $\rightarrow$ Điểm test: `98` (UB-1), `99` (UB), `100` (UB+1).

### Tạo Dữ liệu Kiểm thử (Test Data) cho BVA:
| ID | X | Expected Output (Đầu ra mong đợi) | Phủ BVA |
| :---: | :--- | :--- | :--- |
| 1 | `1` | Hệ thống chấp nhận giá trị. | Biên dưới hợp lệ `LB` (`X = 1`) |
| 2 | `2` | Hệ thống chấp nhận giá trị. | Cận biên dưới hợp lệ `LB+1` (`X = 2`) |
| 3 | `99` | Hệ thống chấp nhận giá trị. | Biên trên hợp lệ `UB` (`X = 99`) |
| 4 | `98` | Hệ thống chấp nhận giá trị. | Cận biên trên hợp lệ `UB-1` (`X = 98`) |
| 5 | `0` | Hệ thống từ chối và hiển thị lỗi. | Dưới biên dưới `LB-1` (`X = 0`) |
| 6 | `100` | Hệ thống từ chối và hiển thị lỗi. | Vượt biên trên `UB+1` (`X = 100`) |
