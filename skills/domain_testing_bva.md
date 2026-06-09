# Hướng dẫn Kỹ năng: Phân tích Miền (Domain Testing) & Phân tích Giá trị Biên (BVA)

## 1. Khái niệm Cốt lõi
- **Domain Testing (Phân tích miền):** Là chiến lược lấy mẫu phân tầng (stratified sampling) nhằm chia nhỏ miền giá trị của biến đầu vào/đầu ra thành các miền con (lớp tương đương - equivalence classes). Hai giá trị kiểm thử thuộc cùng một lớp tương đương nếu hệ thống xử lý chúng theo cùng một cách thức và trả về kết quả cùng loại.
- **Boundary Value Analysis (BVA - Phân tích giá trị biên):** Là kỹ thuật chọn lựa các giá trị kiểm thử nằm ngay trên đường biên hoặc lân cận đường biên của các miền tương đương, vì đây là những nơi có xác suất xảy ra lỗi lập trình (lỗi off-by-one, sai dấu so sánh `<=`, `<`) cao nhất.

---

## 2. Quy trình 4 Bước Thiết kế Tổng quát
Để thiết kế test case cho một tính năng dựa trên phân tích miền, thực hiện theo 4 bước sau:

### Bước 1: Xác định biến Đầu vào (Input) & Đầu ra (Output)
- Liệt kê toàn bộ các biến đầu vào của hệ thống (ví dụ: các ô nhập liệu, tham số API, các nút bấm vật lý).
- Liệt kê toàn bộ các biến đầu ra hoặc phản hồi của hệ thống tương ứng (ví dụ: thông báo lỗi, trạng thái thiết bị, giá trị tính toán trả về).

### Bước 2: Xác định các Lớp Tương đương (Equivalence Classes)
Chia miền giá trị của từng biến thành các lớp:
- **Lớp tương đương Hợp lệ (Valid Equivalence Classes):** Đại diện cho các giá trị đầu vào đúng quy cách, được chấp nhận.
- **Lớp tương đương Không hợp lệ (Invalid Equivalence Classes):** Đại diện cho các giá trị đầu vào sai quy cách, lỗi, bị hệ thống từ chối hoặc trả về thông báo lỗi.

#### Hướng dẫn phân hoạch (Heuristics):
1. **Nếu đầu vào là một khoảng giá trị (Range, ví dụ: `1 <= count <= 999`):**
   - Xác định 1 lớp hợp lệ: `1 <= count <= 999`.
   - Xác định 2 lớp không hợp lệ: `count < 1` và `count > 999`.
2. **Nếu đầu vào là một tập hợp các giá trị (Set of values, ví dụ: Loại xe phải là `BUS`, `TRUCK`, `TAXI-CAB`, `PASSENGER`, hoặc `MOTORCYCLE`):**
   - Xác định 1 lớp hợp lệ cho mỗi phần tử trong tập hợp.
   - Xác định 1 lớp không hợp lệ cho bất kỳ giá trị nào ngoài tập hợp (ví dụ: `TRAILER`).
3. **Nếu đầu vào bắt buộc tuân theo điều kiện ("Must be", ví dụ: Ký tự đầu tiên của mã định danh phải là chữ cái):**
   - Xác định 1 lớp hợp lệ: Ký tự đầu tiên là chữ cái.
   - Xác định 1 lớp không hợp lệ: Ký tự đầu tiên không phải là chữ cái.
4. **Quy tắc phân rã (Split Heuristic):** Nếu có lý do tin rằng các phần tử trong cùng một lớp tương đương được hệ thống xử lý khác nhau, hãy chia nhỏ lớp đó thành các lớp tương đương nhỏ hơn.

### Bước 3: Tìm giá trị Đại diện Tốt nhất (Best Representatives)
- Với các miền không có thứ tự (như tập hợp chuỗi, loại xe), chọn bất kỳ giá trị cụ thể nào trong lớp làm đại diện.
- Với các miền có thứ tự (như số, ngày tháng, độ dài chuỗi), giá trị đại diện tốt nhất chính là các **Giá trị biên** (Boundary values).

### Bước 4: Thực hiện Phân tích Giá trị Biên (BVA)
Đối với mỗi biên của một phân hoạch có thứ tự, xác định các điểm kiểm thử sau:
Giả sử miền hợp lệ được giới hạn bởi **LB (Lower Boundary - Biên dưới)** và **UB (Upper Boundary - Biên trên)**:

- **Tại Biên dưới (LB):**
  - `LB - 1` (Giá trị Không hợp lệ - Invalid)
  - `LB` (Giá trị Hợp lệ - Valid)
  - `LB + 1` (Giá trị Hợp lệ - Valid)
- **Tại Biên trên (UB):**
  - `UB - 1` (Giá trị Hợp lệ - Valid)
  - `UB` (Giá trị Hợp lệ - Valid)
  - `UB + 1` (Giá trị Không hợp lệ - Invalid)
- **Các giá trị cực hạn (Extreme bounds):** Các giá trị nhỏ nhất/lớn nhất mà giao diện người dùng (UI) cho phép nhập hoặc giới hạn vật lý của kiểu dữ liệu (ký hiệu là `-alpha` và `alpha`).

> [!TIP]
> Đối với mỗi biên trong phân hoạch có thứ tự, chúng ta sẽ chọn tối đa 9 điểm kiểm thử để bao phủ toàn bộ biên hợp lệ và không hợp lệ (như hình vẽ ở slide 26 trong tài liệu học: `LB-1`, `LB`, `LB+1`, `UB-1`, `UB`, `UB+1`, các điểm hợp lệ nằm giữa và hai điểm biên cực hạn của hệ thống `-alpha`, `alpha`).

---

## 3. Quy tắc Lựa chọn & Phối hợp Test Case để tối ưu hóa
Khi đã có danh sách các lớp tương đương và giá trị biên, áp dụng nguyên tắc sau để viết kịch bản test:

1. **Nguyên tắc bao phủ lớp Hợp lệ (Valid Classes):**
   - Thiết kế các kịch bản kiểm thử sao cho **một test case có thể bao phủ đồng thời nhiều lớp tương đương hợp lệ** của các biến khác nhau cùng một lúc. Điều này giúp giảm thiểu số lượng test case hợp lệ cần chạy một cách tối đa.
2. **Nguyên tắc bao phủ lớp Không hợp lệ (Invalid Classes):**
   - **Mỗi test case chỉ được phép chứa duy nhất một giá trị không hợp lệ** của một biến, các biến còn lại phải giữ ở giá trị hợp lệ.
   - *Lý do:* Nếu gộp nhiều giá trị không hợp lệ vào cùng một test case, hệ thống có thể từ chối đầu vào ngay khi gặp lỗi đầu tiên, dẫn đến việc các lỗi không hợp lệ phía sau bị "che khuất" (masking) và không thực sự được kiểm thử.

---

## 4. Ví dụ Thực tế Thiết kế Bảng phân hoạch & Kịch bản Test
**Yêu cầu:** Nhập một số nguyên dương nhỏ hơn 100.
**Biến:** `X` (Kiểu số nguyên).

### Bảng phân hoạch lớp tương đương:
| ID | Điều kiện | Lớp tương đương Hợp lệ | Lớp tương đương Không hợp lệ |
|---|---|---|---|
| **C1** | Kiểu dữ liệu | EC1: Là số nguyên | EC2: Không phải số nguyên (ký tự, số thực) |
| **C2** | Giá trị của X | EC3: `0 < X < 100` | EC4: `X <= 0` <br> EC5: `X >= 100` |

### Các điểm biên cần test (BVA):
- Biên dưới: `LB = 1` $\rightarrow$ Điểm test: `0` (LB-1), `1` (LB), `2` (LB+1).
- Biên trên: `UB = 99` $\rightarrow$ Điểm test: `98` (UB-1), `99` (UB), `100` (UB+1).

### Danh sách Test Cases tối ưu:
| Mã TC | Phân hoạch được test | Giá trị đầu vào X | Kết quả mong đợi |
| :---: | :--- | :---: | :--- |
| **TC1** | EC1, EC3 (Giá trị hợp lệ ở giữa miền) | `50` | Hệ thống chấp nhận giá trị |
| **TC2** | Kiểm thử biên dưới hợp lệ (LB) | `1` | Hệ thống chấp nhận giá trị |
| **TC3** | Kiểm thử cận biên dưới hợp lệ (LB+1) | `2` | Hệ thống chấp nhận giá trị |
| **TC4** | Kiểm thử biên trên hợp lệ (UB) | `99` | Hệ thống chấp nhận giá trị |
| **TC5** | Kiểm thử cận biên trên hợp lệ (UB-1) | `98` | Hệ thống chấp nhận giá trị |
| **TC6** | EC4 (Viên dưới không hợp lệ - LB-1) | `0` | Hệ thống từ chối & hiển thị lỗi |
| **TC7** | EC5 (Biên trên không hợp lệ - UB+1) | `100` | Hệ thống từ chối & hiển thị lỗi |
| **TC8** | EC2 (Không phải số nguyên - Số thực) | `5.5` | Hệ thống từ chối & hiển thị lỗi |
| **TC9** | EC2 (Không phải số nguyên - Ký tự) | `'abc'` | Hệ thống từ chối & hiển thị lỗi |
