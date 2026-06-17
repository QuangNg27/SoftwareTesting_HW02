# Báo cáo Kiểm thử HW02 - Domain Testing & BVA on EShop

## 1. Tuyên bố Sử dụng AI (AI Declaration)
Tôi sử dụng AI trợ lý (Gemini) để hỗ trợ phân tích miền, phân tích giá trị biên và thiết kế các kịch bản kiểm thử trong bài tập này. Toàn bộ lịch sử các prompt được ghi nhận chi tiết tại file [promt_log.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/promt_log.md).

---

## 2. Danh sách Tính năng Kiểm thử (Features Table)

| ID | Feature Name | Description |
| :--- | :--- | :--- |
| **FR05** | Xem danh sách & Tìm kiếm sản phẩm | - Trang chủ hiển thị danh sách tất cả sản phẩm dạng lưới (grid).<br>- Mỗi sản phẩm hiển thị: Ảnh (tỷ lệ chuẩn, có alt text mô tả), Tên sản phẩm, Giá (đơn vị: ₫, định dạng phân cách hàng nghìn).<br>- Thanh tìm kiếm tìm theo tên sản phẩm. Từ khóa tìm kiếm phải được hiển thị an toàn (không render HTML).<br>- Khi đang tải dữ liệu phải hiển thị trạng thái loading.<br>- Khi không có kết quả tìm kiếm phải hiển thị thông báo empty state phù hợp.<br>- Trang chủ chỉ có đúng một thẻ `<h1>`.<br>- Mỗi trang chỉ có 1 `<h1>` duy nhất. |
| **[Feature_B]** | *[Chọn 1 tính năng từ Pool B]* | *Sẽ bổ sung trong quá trình thực hiện.* |
| **[Feature_C]** | *[Chọn 1 tính năng từ Pool C]* | *Sẽ bổ sung trong quá trình thực hiện.* |
| **[Feature_D]** | *[Chọn 1 tính năng từ Pool D (Mobile)]* | *Sẽ bổ sung trong quá trình thực hiện.* |

---

## 3. Chi tiết Phân tích Miền & Giá trị Biên (Domain Testing & BVA Analysis)

### 3.1. Tính năng FR-05: Xem danh sách & Tìm kiếm sản phẩm

#### 3.1.1. Phân tích Miền (Domain Testing) cho FR-05

*   **Bước 1: Xác định biến Đầu vào (Input) & Đầu ra (Output)**
    *   **Đầu vào:** `searchQuery` (Kiểu dữ liệu: Chuỗi ký tự - String. Độ dài $L$ từ $0$ đến $255$ ký tự).
    *   **Đầu ra:** Giao diện danh sách dạng lưới (Grid) hiển thị đầy đủ thông tin sản phẩm (ảnh có `alt`, tên, giá bán dạng `₫` phân tách hàng nghìn), trạng thái Loading, màn hình Empty State khi không tìm thấy kết quả, hiển thị từ khóa an toàn không render HTML, SEO chỉ chứa đúng duy nhất một thẻ `<h1>` trên trang chủ.
*   **Bước 2: Xác định các Lớp Tương đương (Equivalence Classes)**
    *   `EC01` (Hợp lệ): Chuỗi trống ($L = 0$). Mong đợi hiển thị toàn bộ sản phẩm.
    *   `EC02` (Hợp lệ): Tên sản phẩm tồn tại hoàn toàn hoặc một phần (Ví dụ: `"iPhone"`). Mong đợi hiển thị danh sách sản phẩm khớp.
    *   `EC03` (Hợp lệ): Tên sản phẩm không tồn tại (Ví dụ: `"Nokia 1280"`). Mong đợi hiển thị Empty State.
    *   `EC04` (Hợp lệ): Chuỗi tìm kiếm chứa khoảng trắng thừa (Ví dụ: `"   iPhone   "`). Mong đợi trim khoảng trắng và tìm bình thường.
    *   `EC05` (Độc hại): Chuỗi chứa mã độc HTML/Script (Ví dụ: `"<img src=x onerror=alert('XSS')>"`). Mong đợi escape HTML hiển thị dạng text an toàn.
    *   `EC06` (Độc hại): Chuỗi chứa mã độc SQL Injection (Ví dụ: `"iPhone' OR 1=1 --"`). Mong đợi xử lý Parameterized Query an toàn.
    *   `EC07` (Không hợp lệ): Chuỗi có độ dài cực lớn ($L > 255$ ký tự). Mong đợi chặn nhập, tự động cắt chuỗi hoặc báo lỗi an toàn.
*   **Bước 3: Tìm giá trị Đại diện Tốt nhất (Best Representatives)**
    *   Chọn từ khóa đại diện tiêu biểu cho các lớp tương đương không có thứ tự và áp dụng BVA cho lớp tương đương có thứ tự (độ dài chuỗi $L$).

#### 3.1.2. Phân tích Giá trị Biên (BVA) cho FR-05
Giới hạn độ dài chuỗi tìm kiếm đầu vào: $0 \le L \le 255$.
*   **Biên dưới ($LB = 0$):**
    *   $L = 0$ (LB): Chuỗi rỗng `""` $\rightarrow$ Hiện tất cả sản phẩm.
    *   $L = 1$ (LB+1): Chuỗi 1 ký tự, ví dụ `"i"` $\rightarrow$ Tìm sản phẩm khớp một phần.
*   **Biên trên ($UB = 255$):**
    *   $L = 254$ (UB-1): Chuỗi dài 254 ký tự $\rightarrow$ Xử lý bình thường (Empty State).
    *   $L = 255$ (UB): Chuỗi dài 255 ký tự $\rightarrow$ Xử lý bình thường (Empty State).
    *   $L = 256$ (UB+1): Chuỗi dài 256 ký tự $\rightarrow$ Cắt chuỗi/chặn nhập hoặc báo lỗi đầu vào.

---

### 3.2. Tính năng [Feature_B]
*(Sẽ bổ sung chi tiết phân tích miền và biên sau khi lựa chọn tính năng)*

---

### 3.3. Tính năng [Feature_C]
*(Sẽ bổ sung chi tiết phân tích miền và biên sau khi lựa chọn tính năng)*

---

### 3.4. Tính năng [Feature_D]
*(Sẽ bổ sung chi tiết phân tích miền và biên sau khi lựa chọn tính năng)*

---

## 4. Kịch bản Kiểm thử Chi tiết (Test Cases Table)

### 4.1. Kịch bản kiểm thử cho FR-05: Xem danh sách & Tìm kiếm sản phẩm

| Function/Feature ID | Case ID | Test case name | Pre-requisites / Conditions | Test step | Expected Result (ER) | Actual Result | Status | Tester | Tested Date |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| FR-05 | TC-FR05-001 | Hiển thị mặc định danh sách sản phẩm khi vào trang chủ (EC01, LB) | Cơ sở dữ liệu đã khởi tạo và có sẵn dữ liệu sản phẩm mẫu. | 1. Truy cập URL trang chủ của khách hàng (`http://localhost:5173`).<br>2. Quan sát giao diện danh sách sản phẩm.<br>3. Kiểm tra mã nguồn HTML của trang chủ. | 1. Trang chủ hiển thị đầy đủ danh sách các sản phẩm mẫu dạng lưới (Grid).<br>2. Mỗi sản phẩm hiển thị đủ: Ảnh sản phẩm, Tên sản phẩm, Giá bán (định dạng VND với ký hiệu `₫`, phân tách hàng nghìn, ví dụ: `30,000,000 ₫`).<br>3. Trang chủ chỉ chứa duy nhất một thẻ `<h1>` mô tả trang chủ. | 1. Đầy đủ sản phẩm dạng lưới.<br>2. Giá hiển thị đơn vị "VND" thay vì "₫".<br>3. Trang chứa 2 thẻ `<h1>`. | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-002 | Kiểm tra thuộc tính Alt của hình ảnh sản phẩm | Đã truy cập trang chủ và danh sách sản phẩm đã hiển thị thành công. | 1. Nhấp chuột phải vào một sản phẩm bất kỳ trên lưới danh sách.<br>2. Chọn "Inspect" (Kiểm tra phần tử) để xem mã nguồn thẻ hình ảnh `<img>` của sản phẩm đó. | Thẻ hình ảnh `<img>` của sản phẩm phải chứa thuộc tính `alt` mô tả nội dung hình ảnh rõ ràng (không được để rỗng hoặc thiếu). | Thuộc tính `alt` bị rỗng. | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-003 | Tìm kiếm khớp hoàn toàn tên sản phẩm (EC02) | Sản phẩm "iPhone 15 Pro Max" có tồn tại trong database. | 1. Nhập từ khóa `"iPhone 15 Pro Max"` vào ô tìm kiếm.<br>2. Nhấn phím Enter hoặc nút Tìm kiếm (nếu có). | 1. Hệ thống chỉ hiển thị duy nhất sản phẩm "iPhone 15 Pro Max" trên lưới.<br>2. Từ khóa tìm kiếm hiển thị trên màn hình là `"iPhone 15 Pro Max"`. | Kết quả giống ER | Pass | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-004 | Tìm kiếm khớp một phần tên sản phẩm (EC02, LB+1) | Có nhiều sản phẩm chứa ký tự "S" hoặc "s" trong database (ví dụ: Samsung Galaxy S24 Ultra). | 1. Nhập từ khóa `"S"` (độ dài L=1) vào ô tìm kiếm.<br>2. Nhấn phím Enter hoặc nút Tìm kiếm. | 1. Hệ thống hiển thị tất cả các sản phẩm có tên chứa chữ "S" hoặc "s".<br>2. Các sản phẩm khác không khớp sẽ bị ẩn đi. | Kết quả giống ER | Pass | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-005 | Tìm kiếm sản phẩm không tồn tại (EC03) | Sản phẩm có tên "Nokia 1280" chắc chắn không tồn tại trong database. | 1. Nhập từ khóa `"Nokia 1280"` vào ô tìm kiếm.<br>2. Nhấn phím Enter hoặc nút Tìm kiếm. | 1. Hệ thống không hiển thị sản phẩm nào.<br>2. Hiển thị màn hình Empty State có icon/hình minh họa và message thân thiện phù hợp. | Hệ thống không hiển thị màn hình empty state | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-006 | Tự động cắt bỏ khoảng trắng thừa ở hai đầu từ khóa (EC04) | Sản phẩm "iPhone 15 Pro Max" có tồn tại trong database. | 1. Nhập từ khóa `" iPhone 15 Pro Max "` (có khoảng trắng thừa ở đầu và cuối) vào ô tìm kiếm.<br>2. Nhấn Enter hoặc nút Tìm kiếm. | 1. Hệ thống tự động bỏ khoảng trắng và tìm ra sản phẩm "iPhone 15 Pro Max".<br>2. Kết quả hiển thị đúng như khi tìm kiếm không có khoảng trắng thừa. | Không tìm thấy kết quả nếu để khoảng trắng ở đầu từ khóa tìm kiếm | Fail | NMQuang | 16-06-2026 |
| FR-05 | TC-FR05-007 | Tìm kiếm với độ dài từ khóa cận biên trên hợp lệ (UB-1) | Đang ở trang chủ tìm kiếm. | 1. Nhập một chuỗi ngẫu nhiên dài đúng 254 ký tự vào ô tìm kiếm.<br>2. Nhấn Enter hoặc nút Tìm kiếm. | 1. Hệ thống tiếp nhận chuỗi tìm kiếm mà không báo lỗi.<br>2. Trả về kết quả tìm kiếm Empty State (do không có sản phẩm nào có tên dài và khớp như vậy). | Hệ thống không hiển thị màn hình empty state | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-008 | Tìm kiếm với độ dài từ khóa chạm biên trên hợp lệ (UB) | Đang ở trang chủ tìm kiếm. | 1. Nhập một chuỗi ngẫu nhiên dài đúng 255 ký tự vào ô tìm kiếm.<br>2. Nhấn Enter hoặc nút Tìm kiếm. | 1. Hệ thống tiếp nhận chuỗi tìm kiếm bình thường.<br>2. Trả về kết quả tìm kiếm Empty State. | Hệ thống không hiển thị màn hình empty state | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-009 | Tìm kiếm với độ dài từ khóa vượt biên trên (UB+1) | Đang ở trang chủ tìm kiếm. | 1. Cố gắng nhập hoặc dán một chuỗi dài 256 ký tự vào ô tìm kiếm.<br>2. Quan sát phản ứng ô nhập liệu.<br>3. Nhấn Enter hoặc nút Tìm kiếm. | Ô nhập liệu giới hạn không cho phép nhập ký tự thứ 256 (maxlength=255) HOẶC nếu hệ thống cho phép nhập thì khi nhấn tìm kiếm, hệ thống sẽ tự động cắt ngắn chuỗi còn 255 ký tự hoặc báo lỗi đầu vào không hợp lệ một cách an toàn mà không làm crash ứng dụng. | Kết quả tìm kiếm hiển thị toàn bộ chuỗi khiến cho trang bị tràn lề bên phải | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-010 | Kiểm tra bảo mật chống tấn công XSS (HTML/Script/Event Injection) (EC05) | Đang ở trang chủ tìm kiếm. | 1. Nhập chuỗi mã độc: `"<img src=x onerror=alert('XSS')>"` vào ô tìm kiếm.<br>2. Kiểm tra xem có hộp thoại thông báo alert nào hiện lên hay không.<br>3. Kiểm tra chuỗi hiển thị từ khóa trên giao diện. | 1. Tuyệt đối không có hộp thoại alert nào xuất hiện trên màn hình.<br>2. Từ khóa tìm kiếm hiển thị an toàn trên giao diện dưới dạng chuỗi thuần túy: `"<img src=x onerror=alert('XSS')>"` (không render HTML). | Trình duyệt thực thi mã độc và hiện thông báo alert 'XSS'. | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-011 | Kiểm tra bảo mật chống tấn công SQL Injection (EC06) | Đang ở trang chủ tìm kiếm. | 1. Nhập chuỗi mã độc SQL: `"iPhone' OR '1'='1' --"` vào ô tìm kiếm.<br>2. Nhấn Enter hoặc nút Tìm kiếm.<br>3. Nhập tiếp chuỗi: `"'; DROP TABLE products; --"`.<br>4. Nhấn Enter và quan sát phản hồi của trang web và database. | 1. Cơ sở dữ liệu không bị lỗi, không bị drop table và hệ thống không bị crash.<br>2. API trả về danh sách trống (Empty State) hoặc chỉ hiển thị sản phẩm nếu có tên chứa đúng chuỗi đó theo nghĩa đen (không trả về toàn bộ sản phẩm do lỗi logic SQL). | Hệ thống hiện ra tất cả sản phẩm | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-012 | Kiểm tra trạng thái đang tải dữ liệu (Loading State) | Mạng kết nối ổn định (có thể giả lập mạng chậm bằng Network Throttling trong DevTools). | 1. Truy cập trang chủ hoặc thực hiện một tìm kiếm bất kỳ.<br>2. Ngay khi dữ liệu đang được gọi từ API, quan sát giao diện. | Trong thời gian chờ phản hồi từ API, màn hình phải hiển thị trạng thái đang tải dữ liệu rõ ràng (Loading spinner/indicator). | Không có trạng thái loading khi đang tải dữ liệu | Fail | NMQuang | 17-06-2026 |
| FR-05 | TC-FR05-013 | Kiểm tra thứ tự tập trung tiêu điểm bằng phím Tab (Tab Order) | Đang ở trang chủ. | 1. Đặt trỏ chuột ở thanh địa chỉ của trình duyệt.<br>2. Nhấn liên tiếp phím Tab trên bàn phím.<br>3. Quan sát tiêu điểm di chuyển trên các phần tử tương tác (thanh điều hướng, thanh tìm kiếm, các sản phẩm). | Thứ tự tiêu điểm di chuyển một cách tuần tự từ trên xuống dưới, từ trái sang phải, không nhảy cóc hoặc bỏ sót các phần tử tương tác chính. | Kết quả giống ER | Pass | NMQuang | 17-06-2026 |

---

### 4.2. Kịch bản kiểm thử cho [Feature_B]
*(Sẽ bổ sung trong quá trình thực hiện)*

---

### 4.3. Kịch bản kiểm thử cho [Feature_C]
*(Sẽ bổ sung trong quá trình thực hiện)*

---

### 4.4. Kịch bản kiểm thử cho [Feature_D]
*(Sẽ bổ sung trong quá trình thực hiện)*
