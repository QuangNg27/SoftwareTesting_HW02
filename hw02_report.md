# Báo cáo Kiểm thử HW02 - Domain Testing & BVA on EShop

## 1. Tuyên bố Sử dụng AI (AI Declaration)
Tôi sử dụng AI trợ lý (Gemini) để hỗ trợ phân tích miền, phân tích giá trị biên và thiết kế các kịch bản kiểm thử trong bài tập này. Toàn bộ lịch sử các prompt được ghi nhận chi tiết tại file [promt_log.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/promt_log.md).

---

## 2. Danh sách Tính năng Kiểm thử (Features Table)

| ID | Feature Name | Description |
| :--- | :--- | :--- |
| **FR05** | Xem danh sách & Tìm kiếm sản phẩm | - Trang chủ hiển thị danh sách tất cả sản phẩm dạng lưới (grid).<br>- Mỗi sản phẩm hiển thị: Ảnh (tỷ lệ chuẩn, có alt text mô tả), Tên sản phẩm, Giá (đơn vị: ₫, định dạng phân cách hàng nghìn).<br>- Thanh tìm kiếm tìm theo tên sản phẩm. Từ khóa tìm kiếm phải được hiển thị an toàn (không render HTML).<br>- Khi đang tải dữ liệu phải hiển thị trạng thái loading.<br>- Khi không có kết quả tìm kiếm phải hiển thị thông báo empty state phù hợp.<br>- Trang chủ chỉ có đúng một thẻ `<h1>`.<br>- Mỗi trang chỉ có 1 `<h1>` duy nhất. |
| **FR-09** | Mã Giảm Giá (Coupon) | Tại bước Checkout, người dùng có thể nhập mã giảm giá. Hệ thống áp dụng giảm giá dựa trên 5 điều kiện (Mã tồn tại, Còn hạn sử dụng, Đủ ngưỡng đơn hàng, Đã đăng nhập, Chưa dùng hết lượt) theo loại phần trăm hoặc cố định. |
| **FR-14** | Quản lý Danh mục (Category CRUD) | Admin có thể Thêm / Xem / Xóa danh mục. Tên danh mục là bắt buộc, không được để trống. |
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

### 3.2. Tính năng FR-09: Mã Giảm Giá (Coupon)

#### 3.2.1. Phân tích Miền (Domain Testing) cho FR-09

*   **Bước 1: Xác định biến Đầu vào (Input) & Đầu ra (Output)**
    *   **Đầu vào:**
        *   `couponCode` (Kiểu dữ liệu: Chuỗi ký tự - String. Tên mã giảm giá nhập vào).
        *   `orderTotal` (Kiểu dữ liệu: Số thực dương - Float. Tổng giá trị đơn hàng trước khi áp mã).
        *   `isLoggedIn` (Kiểu dữ liệu: Boolean. Trạng thái xác thực của người dùng).
        *   `userUsageCount` (Kiểu dữ liệu: Số nguyên không âm - Integer. Số lần người dùng đã sử dụng mã này).
    *   **Đầu ra:** Giao diện áp dụng mã thành công với số tiền giảm `discount_amount` và tổng tiền thanh toán mới `final_amount = orderTotal - discount_amount` HOẶC thông báo lỗi phù hợp khi không thỏa mãn một trong các điều kiện C1-C5.
*   **Bước 2: Xác định các Lớp Tương đương (Equivalence Classes)**
    *   **Điều kiện C1 (Mã tồn tại & hoạt động):**
        *   `EC01` (Hợp lệ): Mã tồn tại trong CSDL và đang hoạt động (`is_active = 1`).
        *   `EC02` (Không hợp lệ): Mã tồn tại nhưng đã ngưng hoạt động (`is_active = 0`).
        *   `EC03` (Không hợp lệ): Mã không tồn tại trong hệ thống.
    *   **Điều kiện C2 (Hạn sử dụng):**
        *   `EC04` (Hợp lệ): Ngày hiện tại trước ngày hết hạn (`current_date < expired_at`).
        *   `EC05` (Không hợp lệ): Ngày hiện tại bằng hoặc sau ngày hết hạn (`current_date >= expired_at`).
    *   **Điều kiện C3 (Ngưỡng đơn hàng):**
        *   `EC06` (Hợp lệ): Tổng đơn hàng lớn hơn hoặc bằng ngưỡng tối thiểu (`orderTotal >= min_order_amount`).
        *   `EC07` (Không hợp lệ): Tổng đơn hàng nhỏ hơn ngưỡng tối thiểu (`orderTotal < min_order_amount`).
    *   **Điều kiện C4 (Trạng thái đăng nhập):**
        *   `EC08` (Hợp lệ): Người dùng đã đăng nhập (có Token JWT hợp lệ).
        *   `EC09` (Không hợp lệ): Người dùng chưa đăng nhập.
    *   **Điều kiện C5 (Lượt sử dụng cá nhân):**
        *   `EC10` (Hợp lệ): Số lần đã dùng của user nhỏ hơn giới hạn tối đa (`userUsageCount < max_uses_per_user`).
        *   `EC11` (Không hợp lệ): Số lần đã dùng của user bằng hoặc lớn hơn giới hạn tối đa (`userUsageCount >= max_uses_per_user`).
    *   **Phương thức giảm giá:**
        *   `EC12` (Hợp lệ): Loại giảm giá theo tỷ lệ phần trăm (`type = 'percent'`).
        *   `EC13` (Hợp lệ): Loại giảm giá theo số tiền cố định (`type = 'fixed'`).
        *   `EC14` (Hợp lệ - Biên): Loại cố định có giá trị giảm lớn hơn hoặc bằng tổng tiền đơn hàng (`discount_value >= orderTotal`).
*   **Bước 3: Tìm giá trị Đại diện Tốt nhất (Best Representatives)**
    *   Sử dụng các mã giảm giá thực tế có trong hệ thống làm giá trị đại diện: `SAVE10` (10%, tối thiểu 300,000 ₫), `BIGBUY` (cố định 50,000 ₫, tối thiểu 500,000 ₫), `VIP100` (cố định 100,000 ₫, tối thiểu 300,000 ₫, tối đa 2 lần/người) và `EXPIRED` (hết hạn).

#### 3.2.2. Phân tích Giá trị Biên (BVA) cho FR-09

Áp dụng BVA cho hai biến định lượng có ngưỡng giới hạn:
1.  **Tổng đơn hàng (`orderTotal`) đối với điều kiện C3 (`min_order_amount`):**
    *   Xét mã `SAVE10` có `min_order_amount = 300,000` ₫.
        *   **Biên dưới hợp lệ:** `orderTotal = 300,000` ₫ (Đạt ngưỡng tối thiểu) $\rightarrow$ Áp dụng thành công.
        *   **Biên dưới không hợp lệ:** `orderTotal = 299,000` ₫ (Dưới ngưỡng tối thiểu) $\rightarrow$ Báo lỗi.
        *   **Trên biên hợp lệ:** `orderTotal = 301,000` ₫ $\rightarrow$ Áp dụng thành công.
2.  **Lượt sử dụng của User (`userUsageCount`) đối với điều kiện C5 (`max_uses_per_user`):**
    *   Xét mã `VIP100` có `max_uses_per_user = 2`.
        *   **Biên dưới hợp lệ:** `userUsageCount = 1` (Đã dùng 1 lần, vẫn còn 1 lượt) $\rightarrow$ Áp dụng thành công.
        *   **Biên chạm giới hạn:** `userUsageCount = 2` (Đã dùng 2 lần, hết lượt) $\rightarrow$ Báo lỗi.
        *   **Chưa dùng:** `userUsageCount = 0` (Còn nguyên 2 lượt) $\rightarrow$ Áp dụng thành công.

---

### 3.3. Tính năng FR-14: Quản lý Danh mục (Category CRUD)

#### 3.3.1. Phân tích Miền (Domain Testing) cho FR-14

*   **Bước 1: Xác định biến Đầu vào (Input) & Đầu ra (Output)**
    *   **Đầu vào:**
        *   `categoryName` (Kiểu dữ liệu: Chuỗi ký tự - String. Tên của danh mục cần thêm).
        *   `categoryId` (Kiểu dữ liệu: Số nguyên dương - Integer. ID của danh mục khi thực hiện thao tác Xóa).
        *   `isCategoryLinkedToProducts` (Kiểu dữ liệu: Boolean. Trạng thái danh mục có đang được liên kết với bất kỳ sản phẩm nào trong cơ sở dữ liệu hay không).
    *   **Đầu ra:** 
        *   Thông báo thêm/xóa danh mục thành công, giao diện hiển thị danh mục mới được cập nhật, hoặc thông báo lỗi dữ liệu đầu vào hoặc lỗi ràng buộc liên kết sản phẩm (ví dụ: "Tên danh mục là bắt buộc", hoặc báo lỗi ràng buộc liên kết sản phẩm).
*   **Bước 2: Xác định các Lớp Tương đương (Equivalence Classes)**
    *   **Tên danh mục (`categoryName`):**
        *   `EC01` (Hợp lệ): Tên danh mục hợp lệ có độ dài $L \ge 1$ (ví dụ: `"Phụ kiện"`).
        *   `EC02` (Không hợp lệ): Tên danh mục bị rỗng ($L = 0$, chuỗi `""`).
        *   `EC03` (Không hợp lệ): Tên danh mục chỉ gồm khoảng trắng (ví dụ: `"   "`). Mong muốn trim khoảng trắng và chặn.
        *   `EC04` (Không hợp lệ/Nghi ngờ): Tên danh mục trùng tên với danh mục hiện có (ví dụ: `"Điện thoại"` đã có sẵn).
        *   `EC05` (Độc hại): Tên chứa mã độc HTML/Script để kiểm tra XSS (ví dụ: `"<img src=x onerror=alert('XSS')>"`).
        *   `EC06` (Độc hại): Tên chứa SQL Injection (ví dụ: `"Category' OR '1'='1' --"`).
    *   **Điều kiện Xóa danh mục (`isCategoryLinkedToProducts`):**
        *   `EC07` (Hợp lệ): Danh mục không chứa sản phẩm nào (xóa thành công).
        *   `EC08` (Biên/Ràng buộc dữ liệu): Danh mục đang chứa sản phẩm liên kết (hệ thống xử lý an toàn, chặn xóa hoặc xóa cascade/mất liên kết).
*   **Bước 3: Tìm giá trị Đại diện Tốt nhất (Best Representatives)**
    *   Sử dụng các chuỗi đại diện hợp lệ, rỗng, khoảng trắng và các mã độc để nhập vào form quản lý danh mục. Thao tác trên các danh mục không có sản phẩm liên kết và danh mục đang có sản phẩm để kiểm tra chức năng xóa.

#### 3.3.2. Phân tích Giá trị Biên (BVA) cho FR-14
Áp dụng BVA cho độ dài tên danh mục (`L`):
*   **Biên dưới ($L = 1$):**
    *   `L = 0` (Dưới biên): Chuỗi rỗng `""` $\rightarrow$ Hệ thống báo lỗi "Tên danh mục là bắt buộc" hoặc tương đương.
    *   `L = 1` (Chạm biên hợp lệ): Tên danh mục có đúng 1 ký tự, ví dụ `"A"` $\rightarrow$ Thêm thành công.
*   **Biên trên (Giới hạn tối đa giả định $L = 255$ ký tự):**
    *   `L = 255` (Chạm biên trên hợp lệ): Chuỗi dài 255 ký tự $\rightarrow$ Thêm thành công.
    *   `L = 256` (Vượt biên trên): Chuỗi dài 256 ký tự $\rightarrow$ Chặn nhập hoặc tự động cắt còn 255 ký tự hoặc báo lỗi.

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

### 4.2. Kịch bản kiểm thử cho FR-09: Mã Giảm Giá (Coupon)

| Function/Feature ID | Case ID | Test case name | Pre-requisites / Conditions | Test step | Expected Result (ER) | Actual Result | Status | Tester | Tested Date |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| FR-09 | TC-FR09-001 | Áp dụng mã giảm giá theo tỷ lệ phần trăm (percent) hợp lệ (EC01, EC04, EC06, EC08, EC10, EC12) | 1. Người dùng đăng nhập tài khoản test.<br>2. Sản phẩm trong giỏ hàng có tổng giá trị tối thiểu là 300,000 ₫.<br>3. Mã giảm giá `SAVE10` hoạt động và user chưa dùng lần nào. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SAVE10"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Mã giảm giá được áp dụng thành công.<br>2. Hiển thị số tiền tiết kiệm là 10% của tổng tiền.<br>3. Số tiền thanh toán cuối cùng bằng tổng tiền trừ đi tiền tiết kiệm. | Nếu tổng tiền thanh toán bằng mức tối thiểu (300,000 ₫) thì hệ thống hiển thị Đơn hàng chưa đủ giá trị tối thiểu 300,000 ₫ để áp dụng mã này. Còn áp dụng thành công thì tiền tiết kiệm bị âm gấp 9 lần tiền thanh toán và số tiền cuối cùng lại bằng tổng tiền thanh toán - tiền tiết kiệm | Fail | NMQuang | 19-06-2026 |
| FR-09 | TC-FR09-002 | Áp dụng mã giảm giá cố định (fixed) hợp lệ (EC01, EC04, EC06, EC08, EC10, EC13) | 1. Người dùng đăng nhập tài khoản test.<br>2. Sản phẩm trong giỏ hàng có tổng giá trị tối thiểu là 500,000 ₫.<br>3. Mã giảm giá `BIGBUY` hoạt động và user chưa dùng lần nào. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"BIGBUY"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Mã giảm giá được áp dụng thành công.<br>2. Hiển thị số tiền giảm giá là 50,000 ₫.<br>3. Số tiền thanh toán cuối cùng = tổng tiền - 50,000 ₫. | Nếu giá trị đơn hàng bằng đúng mức tối thiểu thì hệ thống hiển thị Đơn hàng chưa đủ giá trị tối thiểu 500,000 ₫ để áp dụng mã này. Còn áp dụng thành công thì kết quả giống ER | Fail | NMQuang | 19-06-2026 |
| FR-09 | TC-FR09-003 | Kiểm tra mã giảm giá không tồn tại trong hệ thống (EC03) | 1. Người dùng đăng nhập tài khoản test.<br>2. Giỏ hàng có sản phẩm bất kỳ. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"INVALID999"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống hiển thị thông báo lỗi phù hợp (ví dụ: "Mã giảm giá không tồn tại").<br>2. Không áp dụng giảm giá, tổng số tiền thanh toán giữ nguyên. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-004 | Kiểm tra mã giảm giá tồn tại nhưng bị vô hiệu hóa (EC02) | 1. Người dùng đăng nhập tài khoản test.<br>2. Giỏ hàng có tổng giá trị >300,000 ₫.<br>3. Mã giảm giá `SAVE10` tồn tại nhưng thuộc tính `is_active = 0` trong database. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SAVE10"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống hiển thị thông báo lỗi phù hợp (ví dụ: "Mã giảm giá đã bị vô hiệu hóa").<br>2. Không áp dụng giảm giá, tổng tiền thanh toán giữ nguyên. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-005 | Kiểm tra mã giảm giá đã hết hạn sử dụng (EC05) | 1. Người dùng đăng nhập tài khoản test.<br>2. Giỏ hàng có tổng giá trị hơn 100,000 ₫.<br>3. Mã giảm giá `EXPIRED` có ngày hết hạn là `2020-01-01` (đã qua). | 1. Truy cập trang Checkout.<br>2. Nhập mã `"EXPIRED"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống hiển thị thông báo lỗi phù hợp (ví dụ: "Mã giảm giá đã hết hạn sử dụng").<br>2. Không áp dụng giảm giá, tổng tiền thanh toán giữ nguyên. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-006 | Kiểm tra đơn hàng không đạt giá trị tối thiểu (BVA - Dưới biên C3) | 1. Người dùng đăng nhập tài khoản test.<br>2. Giỏ hàng có tổng tiền dưới 300,000 ₫ (ngưỡng tối thiểu của `SAVE10` là 300,000 ₫).<br>3. Mã giảm giá `SAVE10` hoạt động và user chưa dùng lần nào. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SAVE10"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống hiển thị thông báo lỗi phù hợp (ví dụ: "Đơn hàng chưa đạt giá trị tối thiểu").<br>2. Không áp dụng giảm giá, tổng tiền thanh toán giữ nguyên. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-007 | Kiểm tra đơn hàng vừa bằng giá trị tối thiểu (BVA - Biên C3) | 1. Người dùng đăng nhập tài khoản test.<br>2. Sản phẩm trong giỏ hàng có tổng giá trị là 300,000 ₫ (vừa bằng ngưỡng tối thiểu của `SAVE10`).<br>3. Mã giảm giá `SAVE10` hoạt động và user chưa dùng lần nào. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SAVE10"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Mã giảm giá được áp dụng thành công.<br>2. Hiển thị số tiền giảm giá là 30,000 ₫ (10% của 300,000 ₫).<br>3. Số tiền thanh toán cuối cùng hiển thị là 270,000 ₫. | Hệ thống hiển thị Đơn hàng chưa đủ giá trị tối thiểu 300,000 ₫ để áp dụng mã này | Fail | NMQuang | 19-06-2026 |
| FR-09 | TC-FR09-008 | Kiểm tra người dùng chưa đăng nhập (EC09) | 1. Người dùng là khách vãng lai (chưa đăng nhập).<br>2. Giỏ hàng có sản phẩm. | 1. Truy cập trang Giỏ hàng và nhấn Checkout.<br>2. Nhập mã `"SAVE10"` và nhấn Áp dụng (nếu hệ thống cho phép truy cập checkout mà không chặn đăng nhập trước). | Hệ thống yêu cầu đăng nhập trước khi checkout. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-009 | Kiểm tra số lượt sử dụng cá nhân chạm ngưỡng giới hạn (BVA - Biên C5) | 1. Người dùng đăng nhập tài khoản test.<br>2. Mã giảm giá `SAVE10` có giới hạn `max_uses_per_user = 1`. User test đã sử dụng mã này thành công 1 lần trong đơn hàng trước đó.<br>3. Giỏ hàng hiện tại có tổng giá trị >300,000 ₫. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SAVE10"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống hiển thị thông báo lỗi phù hợp (ví dụ: "Bạn đã hết lượt sử dụng mã giảm giá này").<br>2. Không áp dụng giảm giá, tổng tiền thanh toán giữ nguyên. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-010 | Kiểm tra số lượt sử dụng cá nhân còn lại hợp lệ (BVA - Dưới biên C5) | 1. Người dùng đăng nhập tài khoản test.<br>2. Mã giảm giá `VIP100` có giới hạn `max_uses_per_user = 2`. User test đã sử dụng mã này thành công 1 lần trong đơn hàng trước đó.<br>3. Giỏ hàng hiện tại có tổng giá trị >500,000 ₫. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"VIP100"` vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Mã giảm giá được áp dụng thành công.<br>2. Hiển thị số tiền giảm giá là 100,000 ₫.<br>3. Số tiền thanh toán cuối cùng hiển thị là 400,000 ₫. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-011 | Kiểm tra giá trị giảm cố định lớn hơn tổng giá trị đơn hàng (EC14) | 1. Người dùng đăng nhập tài khoản test.<br>2. Một mã giảm giá cố định `SUPERFIX` được thiết lập giảm 400,000 ₫ cho đơn hàng tối thiểu 200,000 ₫.<br>3. Giỏ hàng có tổng giá trị là 300,000 ₫. | 1. Truy cập trang Checkout.<br>2. Nhập mã `"SUPERFIX"` và nhấn Áp dụng. | 1. Mã được áp dụng thành công.<br>2. Số tiền thanh toán cuối cùng hiển thị là 0 ₫ (hoặc hệ thống chặn không cho tổng tiền nhỏ hơn 0, giữ ở mức 0 ₫ chứ không hiển thị số tiền âm). | Số tiền thanh toán hiển thị số âm | Fail | NMQuang | 18-06-2026 |
| FR-09 | TC-FR09-012 | Kiểm tra tự động cắt khoảng trắng đầu/cuối của mã nhập vào (Trải nghiệm người dùng) | 1. Người dùng đăng nhập tài khoản test.<br>2. Giỏ hàng có tổng giá trị >300,000 ₫.<br>3. Mã giảm giá `VIP100` hoạt động và user chưa dùng lần nào. | 1. Truy cập trang Checkout.<br>2. Nhập chuỗi `"  VIP100  "` (có khoảng trắng ở đầu và cuối) vào ô nhập mã giảm giá.<br>3. Nhấn nút Áp dụng. | 1. Hệ thống tự động cắt bỏ các khoảng trắng thừa ở đầu và cuối.<br>2. Mã giảm giá được áp dụng thành công. | Kết quả giống ER | Pass | NMQuang | 18-06-2026 |

---

### 4.3. Kịch bản kiểm thử cho FR-14: Quản lý Danh mục (Category CRUD)

| Function/Feature ID | Case ID | Test case name | Pre-requisites / Conditions | Test step | Expected Result (ER) | Actual Result | Status | Tester | Tested Date |
| :---: | :---: | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :---: |
| FR-14 | TC-FR14-001 | Thêm danh mục mới hợp lệ (EC01, LB+1) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập tên danh mục `"Sách"` vào ô nhập tên danh mục.<br>2. Nhấn nút "Thêm mới". | 1. Danh mục `"Sách"` được thêm thành công.<br>2. Danh sách danh mục cập nhật thêm `"Sách"`. | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-002 | Thêm danh mục với tên trống (EC02, LB) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Để trống ô nhập tên danh mục.<br>2. Nhấn nút "Thêm mới". | Hệ thống hiển thị thông báo lỗi yêu cầu nhập tên danh mục (ví dụ: "Tên danh mục là bắt buộc") và không cho phép lưu. | Hệ thống thêm danh mục với tên rỗng | Fail | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-003 | Thêm danh mục với tên chỉ chứa khoảng trắng (EC03) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập chuỗi `"   "` (khoảng trắng) vào ô nhập tên danh mục.<br>2. Nhấn nút "Thêm mới". | Hệ thống tự động cắt bỏ khoảng trắng thừa, nhận diện tên danh mục rỗng và báo lỗi tương tự như tên danh mục rỗng. | Hệ thống thêm danh mục mới | Fail | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-004 | Thêm danh mục với tên dài cận biên trên hợp lệ (BVA) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập một chuỗi ký tự dài đúng 255 ký tự vào ô tên danh mục.<br>2. Nhấn nút "Thêm mới". | Danh mục được thêm thành công và tên được hiển thị đầy đủ, không gây lỗi giao diện. | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-005 | Thêm danh mục với tên dài vượt biên trên (BVA) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập một chuỗi ký tự dài 256 ký tự vào ô tên danh mục.<br>2. Nhấn nút "Thêm mới". | Hệ thống giới hạn không cho phép nhập ký tự thứ 256 HOẶC tự động cắt ngắn chuỗi còn 255 ký tự HOẶC thông báo lỗi dữ liệu đầu vào quá dài. | Hệ thống thêm danh mục mới với tên bằng đúng số ký tự nhập |  Fail | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-006 | Kiểm tra chống tấn công XSS qua tên danh mục (EC05) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập chuỗi mã độc: `"<img src=x onerror=alert('XSS')>"` vào ô tên danh mục.<br>2. Nhấn nút "Thêm mới".<br>3. Xem danh sách danh mục hiển thị trên giao diện. | 1. Danh mục được thêm thành công.<br>2. Không có hộp thoại alert nào xuất hiện trên màn hình.<br>3. Tên danh mục hiển thị an toàn dưới dạng text thuần túy: `"<img src=x onerror=alert('XSS')>"`. | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-007 | Kiểm tra chống SQL Injection qua tên danh mục (EC06) | Tài khoản admin đã đăng nhập, đang ở trang quản lý danh mục. | 1. Nhập chuỗi mã độc: `"Category' OR '1'='1' --"` vào ô tên danh mục.<br>2. Nhấn nút "Thêm mới". | 1. Tên danh mục lưu nguyên văn chuỗi trong cơ sở dữ liệu.<br>2. Cơ sở dữ liệu hoạt động bình thường, không xảy ra lỗi cú pháp. | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-008 | Xem danh sách các danh mục hiện có (Thao tác Xem) | Tài khoản admin đã đăng nhập. | 1. Truy cập vào trang quản lý danh mục.<br>2. Quan sát danh sách danh mục. | Hệ thống hiển thị đầy đủ danh sách các danh mục hiện có trong cơ sở dữ liệu (ví dụ: Điện thoại, Laptop, Phụ kiện, v.v.). | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-009 | Xóa danh mục hợp lệ không chứa sản phẩm (Thao tác Xóa - EC07) | Tài khoản admin đã đăng nhập. Đã có một danh mục trống không liên kết với sản phẩm nào. | 1. Tìm danh mục trống vừa tạo.<br>2. Nhấn nút "Xóa" bên cạnh danh mục đó. | 1. Danh mục được xóa thành công khỏi hệ thống.<br>2. Danh mục không còn xuất hiện trong danh sách hiển thị. | Kết quả giống ER | Pass | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-010 | Xóa danh mục đang chứa sản phẩm liên kết (Ràng buộc dữ liệu - EC08) | Tài khoản admin đã đăng nhập. Có danh mục (ví dụ: "Điện thoại") đang chứa sản phẩm liên kết trong cơ sở dữ liệu. | 1. Nhấn nút "Xóa" bên cạnh danh mục "Điện thoại".<br>2. Xác nhận xóa. | Hệ thống ngăn chặn hành động xóa và hiển thị thông báo lỗi ràng buộc (ví dụ: "Không thể xóa danh mục đang có sản phẩm") để bảo vệ tính toàn vẹn dữ liệu. | Hệ thống cho phép xóa. | Fail | NMQuang | 20-06-2026 |
| FR-14 | TC-FR14-011 | Thêm danh mục trùng tên với danh mục hiện có (EC04) | Tài khoản admin đã đăng nhập. Danh mục "Laptop" đã tồn tại sẵn trong hệ thống. | 1. Nhập tên danh mục `"Laptop"` vào ô nhập tên danh mục.<br>2. Nhấn nút "Thêm". | Hệ thống kiểm tra và thông báo lỗi trùng lặp tên danh mục, không cho phép lưu danh mục trùng. | Hệ thống thêm danh mục với tên trùng. | Fail | NMQuang | 20-06-2026 |

---

### 4.4. Kịch bản kiểm thử cho [Feature_D]
*(Sẽ bổ sung trong quá trình thực hiện)*
