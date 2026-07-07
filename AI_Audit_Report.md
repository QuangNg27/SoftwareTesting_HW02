# AI Audit Report - HW02

Báo cáo này thực hiện audit các kết quả do AI sinh ra trong suốt quá trình thiết kế kịch bản kiểm thử cho dự án EShop, đối chiếu sự khác biệt giữa kết quả đề xuất của AI và bản hiệu chỉnh thực tế của sinh viên.

> [NOTE]
> Do AI được sử dụng ở chế độ Agent, các câu trả lời trực tiếp trong giao diện chat có thể rất ngắn gọn vì AI đã tự động sử dụng các công cụ để thao tác và cập nhật nội dung trực tiếp trên các file báo cáo/mã nguồn.

---

## 1. Chi tiết Kiểm định Tác vụ (Task Audits)

### Tác vụ 1: Thiết kế kịch bản kiểm thử & Dữ liệu kiểm thử cho FR-05
- **Prompt + Tool**: `[16-06-2026 14:54] | Gemini`
  > *Prompt*: "tạo cho tôi test case cho chức năng FR 05, sử dụng đủ các skill khi viết test case, phần bug report thì hiện tại chỉ tạo khung thông tin thôi"
- **AI Output**: Đưa ra danh sách 13 test case cho chức năng tìm kiếm sản phẩm. Phần phân tích lớp tương đương (EP) và BVA bị gộp chung các ví dụ vào phần định nghĩa lý thuyết bước 2 mà không tách thành bảng dữ liệu kiểm thử riêng biệt.

  **Câu trả lời của AI trong chat:**
  ```text
  Mình đã hoàn thành việc thiết kế các test case cho chức năng **FR-05: Xem danh sách & Tìm kiếm sản phẩm** cùng với khung báo cáo lỗi theo đúng kế hoạch đã được phê duyệt.

  Các file mới được tạo trong workspace của bạn bao gồm:
  *   [hw02.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/hw02.md): Chứa toàn bộ nội dung phân tích miền (Domain Testing), phân tích giá trị biên (BVA), bảng mô tả tính năng (Features Table) và **13 kịch bản kiểm thử chi tiết** bao quát mọi trường hợp (tìm kiếm đúng, sai, rỗng, biên độ dài ký tự, các loại mã độc XSS/SQL Injection, giao diện, loading, SEO H1 và khả năng tiếp cận bằng phím Tab).
  *   [bug_reports.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/bug_reports.md): Chứa khung thông tin bảng báo cáo lỗi chuẩn chỉnh cùng hướng dẫn và mẫu điền lỗi để bạn sử dụng trong quá trình chạy test thực tế trên SUT.
  *   [promt_log.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/promt_log.md): Nhật ký prompt tương tác với AI đã được cập nhật đầy đủ.

  Bạn có thể xem chi tiết quá trình hoàn thành trong tài liệu bàn giao tại file [walkthrough.md](file:///C:/Users/admin/.gemini/antigravity-ide/brain/d8af59bf-ba49-40d1-afe4-1ab95fbd5c67/walkthrough.md).

  Vui lòng kiểm tra lại các file trong workspace. Nếu bạn muốn tiếp tục thực hiện kiểm thử các chức năng tiếp theo hoặc có điều chỉnh gì, hãy cho mình biết nhé!
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI sinh cấu trúc báo cáo chưa tối ưu, không tuân thủ hoàn toàn hướng dẫn của môn học về việc tách riêng bảng Dữ liệu Kiểm thử (Test Data) ở Bước 4. Các test case tìm kiếm cũng bỏ sót kiểm tra thuộc tính SEO hình ảnh (`alt`) và hiển thị thẻ tiêu đề `<h1>` độc nhất trên trang chủ.
- **Student Fix**: Yêu cầu AI tách các ví dụ cụ thể ra thành bảng Test Data ở bước 4 cho cả EP và BVA. Đồng thời bổ sung test case `TC-FR05-002` kiểm tra thuộc tính `alt` của thẻ `<img>`, kiểm tra số lượng thẻ `<h1>` trên trang chủ, và gộp các giá trị kiểm thử trùng lớp tương đương (`EC01`, `EC02`) để tối ưu hóa dữ liệu kiểm thử.

---

### Tác vụ 2: Kiểm thử thủ công & Ghi nhận bug reports cho FR-05
- **Prompt + Tool**: `[18-06-2026 11:32] | Gemini`
  > *Prompt*: "coi kết quả các test case rồi ghi ra bug report"
- **AI Output**: Kiểm tra kết quả các test case thất bại của FR-05 và tự động soạn thảo 6 bug report tương ứng (từ B004 đến B009).

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã kiểm tra bảng kịch bản kiểm thử trong file [hw02_report.md] và ghi nhận thêm các bug report mới (từ B004 đến B009) tương ứng với các test case có trạng thái Fail vào file [bug_reports.md]:
  - B004 (Thấp): Thuộc tính alt bị rỗng.
  - B005 (Trung bình): Không hiển thị màn hình empty state.
  - B006 (Trung bình): Không tự động cắt khoảng trắng thừa.
  - B007 (Thấp): Bị tràn lề bên phải khi tìm kiếm chuỗi dài.
  - B008 (Nghiêm trọng): Lỗ hổng SQL Injection.
  - B009 (Thấp): Thiếu hiệu ứng Loading khi tải dữ liệu.
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI sinh tài liệu mô tả lỗi nhanh nhưng các bước tái hiện lỗi (Steps to Reproduce) và kết quả thực tế (Actual Result) còn sơ sài, thiếu chi tiết môi trường test và chưa phân loại đúng mức độ nghiêm trọng (Severity) theo quy chuẩn dự án di động thực tế.
- **Student Fix**: Sinh viên đã rà soát lại mô tả của từng bug (đặc biệt là B004, B005, B006) để khớp hoàn toàn với thực tế và sửa lại các trường dữ liệu bị thiếu sót hoặc sai định dạng.

---

### Tác vụ 3: Thiết kế kịch bản kiểm thử cho FR-09 (Coupon)
- **Prompt + Tool**: `[18-06-2026 15:01] | Gemini`
  > *Prompt*: "xem đặc tả của FR-09 và tạo test case cho chức năng"
- **AI Output**: Sinh ra danh sách test case kiểm thử tính năng áp dụng mã giảm giá (loại phần trăm, loại cố định, hết hạn, dưới giá trị đơn hàng tối thiểu, chạm giới hạn lượt dùng).

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã xem xét đặc tả yêu cầu của tính năng **FR-09: Mã Giảm Giá (Coupon)** trong [README.md] và thực hiện các cập nhật sau vào báo cáo kiểm thử [hw02_report.md]:

  ### 1. Phân tích Miền (Domain Testing) & Giá trị Biên (BVA) cho FR-09
  *   **Các biến đầu vào:** couponCode (String), orderTotal (Float), isLoggedIn (Boolean), userUsageCount (Integer).
  *   **14 Lớp tương đương:** gồm các mã giảm giá hợp lệ, ngưng hoạt động, không tồn tại, hết hạn, vượt quá lượt dùng, hoặc giảm cố định lớn hơn giá trị đơn hàng.

  ### 2. Thiết kế 12 Kịch bản Kiểm thử chi tiết cho FR-09 (từ TC-FR09-001 đến TC-FR09-012)
  Các kịch bản này đã được điền đầy đủ thông tin chuẩn xác (Tester: NMQuang, trạng thái: Chưa chạy, ngày thử nghiệm: 18-06-2026) dưới bảng 4.2 trong file [hw02_report.md].
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI đã bỏ sót kịch bản kiểm thử biên quan trọng của hệ thống: Người dùng đã đăng nhập, thêm sản phẩm vào giỏ hàng, đi tới trang thanh toán nhưng bấm đăng xuất ngay trước khi áp dụng mã giảm giá. Điều này dẫn đến bỏ sót lỗi bảo mật nghiêm trọng (mã coupon vẫn được áp dụng thành công cho khách vãng lai).
- **Student Fix**: Bổ sung test case `TC-FR09-013` để kiểm thử hành vi áp dụng mã sau khi đăng xuất tại màn hình Checkout. Thực hiện test và phát hiện bug bảo mật nghiêm trọng `B016`, sau đó log vào file `bug_reports.md` và đẩy lên GitHub Issues.

---

### Tác vụ 4: Hiệu chỉnh kịch bản kiểm thử cho FR-09 & Ghi nhận lỗi
- **Prompt + Tool**: `[19-06-2026 14:24] & [19-06-2026 15:44] | Gemini`
  > *Prompt*: "xóa TC-FR09-010 do tương tự TC-FR09-008 đồng thời điều chỉnh lại 2 test case TC-FR09-001 và TC-FR09-002 là giá trị giỏ hàng tối thiểu tại vì nó có bug là không nhận giá trị = tối thiểu" và "check kết quả các test case của FR-09 và tổng hợp bug ra bug_reports"
- **AI Output**: Thực hiện xóa test case trùng lặp, điều chỉnh lại giá trị biên giỏ hàng tối thiểu và tạo các báo cáo lỗi B010, B011, B012.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã thực hiện các điều chỉnh theo yêu cầu của bạn vào báo cáo kiểm thử [hw02_report.md] và danh sách lỗi [bug_reports.md]:
  - Xóa test case trùng lặp: Đã xóa test case TC-FR09-010 (kiểm tra chạm giới hạn lượt dùng 2/2 của mã VIP100) do hành vi trùng lặp với TC-FR09-008.
  - Đã cập nhật đầy đủ các lỗi phát hiện được vào file [bug_reports.md]: B010, B011, B012 và các chi tiết về giá trị biên.
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI không tự động phát hiện ra việc trùng lặp hành vi giữa các test case lượt dùng (`VIP100` vs `SAVE10`) và không phát hiện lỗi backend không nhận giá trị đơn hàng bằng đúng giá trị tối thiểu cho đến khi được sinh viên chỉ ra.
- **Student Fix**: Chỉ ra logic sai sót của AI, yêu cầu xóa test case thừa và cập nhật lại điều kiện biên tối thiểu của đơn hàng, đồng thời ghi nhận bug `B010` và `B012` (tiền giảm giá bị âm khiến tổng tiền thanh toán bị âm).

---

### Tác vụ 5: Thiết kế kịch bản kiểm thử cho FR-14 (CRUD Danh mục)
- **Prompt + Tool**: `[19-06-2026 21:47] | Gemini`
  > *Prompt*: "tiếp tục thiết kế test case cho FR-14 dựa trên đặc tả (không được xem source code)"
- **AI Output**: Sinh ra các kịch bản kiểm thử CRUD (Thêm, Sửa, Xóa, Xem) danh mục sản phẩm.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã hoàn tất việc thiết kế kịch bản kiểm thử cho tính năng **FR-14: Quản lý Danh mục (Category CRUD)** và tích hợp trực tiếp vào báo cáo.

  ### Nội dung đã thực hiện:
  1. **Cập nhật danh sách tính năng**: Thay thế [Feature_C] thành FR-14 trong bảng tính năng chung.
  2. **Phân tích Miền (Domain Testing) & Giá trị Biên (BVA)**: Thêm mục 3.3 phân tích các biến đầu vào (name, token, id), xác định các lớp tương đương hợp lệ, không hợp lệ, phân quyền người dùng (User vs Admin), kiểm tra mã độc (XSS) và độ dài tên danh mục cận biên.
  3. **Kịch bản Kiểm thử Chi tiết (Test Cases)**: Bổ sung 9 kịch bản kiểm thử chi tiết từ TC-FR14-001 đến TC-FR14-009 vào mục 4.3 trong [hw02_report.md].
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI ban đầu bỏ qua điều kiện tiên quyết (Pre-requisites) bắt buộc là phải đăng nhập bằng tài khoản có quyền Quản trị viên (Admin) để thực hiện các thao tác CRUD danh mục sản phẩm, dẫn đến kịch bản kiểm thử thiếu thực tế và không bao phủ được khía cạnh phân quyền.
- **Student Fix**: Bổ sung điều kiện tiên quyết đăng nhập bằng tài khoản admin mẫu vào toàn bộ các kịch bản kiểm thử từ `TC-FR14-001` đến `TC-FR14-011`.

---

### Tác vụ 6: Cập nhật báo cáo lỗi & Tạo GitHub Issues cho FR-14
- **Prompt + Tool**: `[21-06-2026 22:58] & [22-06-2026 13:21] | Gemini`
  > *Prompt*: "từ kết quả các test cases của FR-14 hãy thêm vào bug report" và "cập nhật bug của FR-14 lên github issues"
- **AI Output**: Ghi nhận các bug B013, B014, B015 vào bug_reports.md và chạy script push lên GitHub issues.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã kiểm tra các test case thất bại của FR-14 và ghi nhận các bug B013, B014, B015 vào bug_reports.md và chạy script push lên GitHub issues thành công.
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI ban đầu mô tả lỗi `B014` (Không giới hạn ký tự tên danh mục) chưa rõ ràng về hành vi gây tràn giao diện và không chỉ rõ ảnh hưởng của nó đến tính toàn vẹn dữ liệu khi xóa danh mục có chứa sản phẩm (`B015`).
- **Student Fix**: Sinh viên đã hiệu chỉnh lại description và actual result của các lỗi `B013`, `B014`, `B015` trong `bug_reports.md`.

---

### Tác vụ 7: Báo cáo lỗi B016 cho kịch bản TC-FR09-013
- **Prompt + Tool**: `[02-07-2026 17:36] & [02-07-2026 17:39] | Gemini`
  > *Prompt*: "report bug cho test case TC-FR09-013" và "điều chỉnh lại bug report là đăng nhập trước đó rồi nhưng lúc trước khi áp dụng mã giảm giá thì bấm đăng xuất"
- **AI Output**: Ghi nhận lỗi B016 vào bug_reports.md và chạy script tạo GitHub Issue #60.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã thành công báo cáo bug B016 cho TC-FR09-013.
  - Bug ID: B016
  - Title: [Checkout - Coupon] Áp dụng mã giảm giá thành công khi người dùng đã đăng xuất (chưa đăng nhập)
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI ban đầu chỉ mô tả lỗi là "khách vãng lai áp dụng mã coupon thành công" mà không nắm rõ kịch bản thực tế là người dùng phải đăng nhập trước để đi tới trang Checkout, sau đó bấm nút Đăng xuất tại màn hình Checkout rồi mới áp dụng mã giảm giá.
- **Student Fix**: Hiệu chỉnh lại pre-conditions và steps to reproduce của bug `B016` để thể hiện đúng quy trình tái hiện lỗi (Đăng nhập -> Vào Checkout -> Bấm Đăng xuất -> Áp dụng mã), và cập nhật Github Issue.

---

### Tác vụ 8: Thiết kế kịch bản kiểm thử cho FR-20 (Thanh toán)
- **Prompt + Tool**: `[02-07-2026 20:05] | Gemini`
  > *Prompt*: "đọc spec của FR-08 và đưa ra test case"
- **AI Output**: Sinh ra danh sách kịch bản test cho tính năng thanh toán với ID tính năng sai lệch (`FR-08` thay vì `FR-20`) và giao diện kịch bản dành cho Web thay vì Mobile App.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã đọc các đặc tả của **FR-08: Thanh toán (Checkout)** và thiết kế một bộ kịch bản kiểm thử toàn diện áp dụng Domain Testing và BVA.
  Tóm tắt các thay đổi đã thực hiện:
  - Replaced [Feature_D] placeholder in Section 2 with FR-08: Thanh toán (Checkout) details.
  - Added Section 3.4 for full EP and BVA analysis of isLoggedIn, cartItems, and clientTotalAmount. Defined boundaries for product quantities and order amounts.
  - Added 6 detailed test cases (TC-FR08-001 to TC-FR08-006): thanh toán thành công, chặn khách vãng lai, kiểm tra thuộc tính chỉ đọc của tổng tiền, chống thao túng giá từ client, kiểm thử biên số lượng sản phẩm, và chặn thanh toán giỏ hàng rỗng.
  ```
- **Verdict**: **INVALID**
- **Reasoning**: AI bị ảo tưởng (hallucination) sai ID tính năng thanh toán từ `FR-20` thành `FR-08`. Ngoài ra, kịch bản test do AI đề xuất ban đầu sử dụng các thao tác click chuột trên nền tảng Web thay vì chạm/vuốt màn hình, kiểm tra hiển thị bàn phím ảo, và các công cụ proxy thu thập API trên nền tảng di động (Mobile App) theo đúng yêu cầu đề bài.
- **Student Fix**: Yêu cầu AI đính chính lại ID tính năng là `FR-20`, thiết kế lại toàn bộ 6 kịch bản kiểm thử cho phù hợp hoàn toàn với môi trường ứng dụng di động (sử dụng Expo Go, các cử chỉ chạm trên mobile, kiểm tra trạng thái chỉ đọc của tổng tiền để chặn hiển thị bàn phím ảo).

---

### Tác vụ 9: Tách biệt và tối ưu hóa Dữ liệu Kiểm thử cho FR-05
- **Prompt + Tool**: `[04-07-2026 21:55], [04-07-2026 22:04] & [05-07-2026 14:01] | Gemini`
  > *Prompt*:
  > - "ở phần domain testing và bva analysis của FR-05: các ví dụ trong bước 2 tách thành bước 4 tạo test data (tạo bảng có cột ID làm số thứ tự và các input, output, phủ EP), lấy sao cho khớp theo test case đã có"
  > - "phần test data của BVA tách riêng ở phần của nó"
  > - "EP nào giống thì gộp lại trong test data của FR-05"
- **AI Output**: Tách biệt các ví dụ từ bước 2 của FR-05 thành bảng dữ liệu kiểm thử độc lập ở bước 4, tách riêng Test Data cho BVA và tối ưu hóa bảng dữ liệu bằng cách gộp các lớp tương đương trùng giá trị đầu vào.

  **Câu trả lời của AI trong chat:**
  - *Cho prompt tách biệt dữ liệu kiểm thử (04-07-2026 21:55):*
  ```text
  Tôi đã hoàn tất việc cập nhật file báo cáo [hw02_report.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/hw02_report.md) theo yêu cầu của bạn. 

  Dưới đây là tóm tắt các thay đổi đã thực hiện:
  1. **Tại phần Domain Testing của FR-05 (Mục 3.1.1):** 
     - Tách toàn bộ các ví dụ cụ thể khỏi **Bước 2 (Xác định các Lớp Tương đương)** để giữ phần định nghĩa lớp tương đương ở dạng tổng quát và lý thuyết.
     - Thêm mới **Bước 4: Tạo Dữ liệu Kiểm thử (Test Data)** chứa bảng dữ liệu với các cột:
       - `ID` (Làm số thứ tự từ 1 đến 13)
       - `Input` (Các giá trị tìm kiếm `searchQuery` hoặc hành động cụ thể)
       - `Expected Output` (Đầu ra mong đợi chi tiết)
       - `Phủ EP / BVA` (Các lớp tương đương hoặc giá trị biên được bao phủ)
  2. **Tại phần BVA Analysis của FR-05 (Mục 3.1.2):**
     - Loại bỏ các giá trị ví dụ cụ thể trong phần phân tích biên dưới và biên trên, chuyển sang dạng định nghĩa độ dài tổng quát ($L=0, L=1, L=254, L=255, L=256$).
     - Dẫn chiếu các ví dụ biên này trực tiếp vào bảng dữ liệu kiểm thử ở Bước 4.
  3. **Khớp với các Test Case đã có (Mục 4.1):**
     - Đảm bảo dữ liệu từ ID 1 đến 13 khớp hoàn toàn với các kịch bản kiểm thử từ `TC-FR05-001` đến `TC-FR05-013` (kể cả các case đặc biệt như thuộc tính `alt`, `Loading State`, `Tab Order` và các mã độc bảo mật `XSS`, `SQL Injection`).
  ```
  - *Cho prompt tách riêng Test Data của BVA (04-07-2026 22:04):*
  ```text
  Tôi đã hoàn thành việc tách riêng phần Test Data cho **Equivalence Classes (Phân vùng tương đương)** và **BVA (Phân tích giá trị biên)** của tính năng FR-05:

  1. **Phần Equivalence Classes (ở mục 3.1.1):** 
     * Có bảng **Bước 4: Tạo Dữ liệu Kiểm thử (Test Data)** riêng. Bảng này gồm 11 dòng dữ liệu (tương ứng với các lớp tương đương hợp lệ/không hợp lệ/độc hại, XSS, SQL Injection, cùng các tương tác giao diện và phím Tab), khớp theo thứ tự các test case tương ứng.
  2. **Phần BVA (ở mục 3.1.2):** 
     * Có bảng **Bước 4: Tạo Dữ liệu Kiểm thử (Test Data) cho BVA** riêng. Bảng này chỉ tập trung vào việc thiết lập dữ liệu đầu vào theo độ dài chuỗi ký tự biên của input (`searchQuery`), gồm 5 dòng:
       - Dòng 1: Biên dưới $L = 0$ (`LB`)
       - Dòng 2: Cận biên dưới $L = 1$ (`LB+1`)
       - Dòng 3: Cận biên trên $L = 254$ (`UB-1`)
       - Dòng 4: Biên trên $L = 255$ (`UB`)
       - Dòng 5: Vượt biên trên $L = 256$ (`UB+1`)

  Cả hai bảng đều được căn lề chuẩn Markdown và khớp hoàn hảo với các mô tả trong kịch bản kiểm thử (Test Cases) chi tiết ở Section 4.1.
  ```
  - *Cho prompt gộp các EP trùng nhau (05-07-2026 14:01):*
  ```text
  I have successfully adjusted and optimized the test data table for **FR-05: Xem danh sách & Tìm kiếm sản phẩm** by merging rows containing similar equivalence partitions (EPs):

  ### 1. Merging Details
  - **Merged `EC01` (Hợp lệ: Chuỗi trống)**:
    - Combined the general search display check (previously ID 1) and the alt text validation on image tags (previously ID 2) into a single row. Both test inputs were identical (`searchQuery = ""`).
  - **Merged `EC02` (Hợp lệ: Tên sản phẩm tồn tại hoàn toàn hoặc một phần)**:
    - Combined the exact match check (`"iPhone 15 Pro Max"`, previously ID 3) and the partial match check (`"S"`, previously ID 4) into a single representative test data row containing both variations.

  ### 2. Log & Documentation Updates
  - **File**: [hw02_report.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/hw02_report.md#L45) has been updated with the merged test data table.
  - **Audit Logs**: Appended the request to [promt_log.md](file:///d:/NAM_3/HK3/KTPM/HW02/SoftwareTesting_HW02/promt_log.md#L205).
  - **Walkthrough**: Documented in [walkthrough.md](file:///C:/Users/admin/.gemini/antigravity-ide/brain/3f9833da-5c25-46df-903e-4f9b4863ab70/walkthrough.md#L55).
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI không tự động tách biệt phần Phân tích lớp tương đương (Mục 3) và Bảng dữ liệu kiểm thử (Mục 4) mà cần tester cung cấp hướng dẫn định dạng chi tiết, đồng thời bỏ sót bước tối ưu hóa dữ liệu kiểm thử (gộp các lớp tương đương giống nhau) cho đến khi được chỉ định rõ ràng.
- **Student Fix**: Hướng dẫn AI chuẩn hóa cấu trúc phân tích miền của FR-05 bằng cách tách riêng bảng Test Data (EP và BVA) ở Bước 4 với các cột biến đầu vào rõ ràng, và gộp các giá trị kiểm thử trùng lớp tương đương.

---

### Tác vụ 10: Tái cấu trúc cấu liệu kiểm thử & Tinh chỉnh TC-FR20-004
- **Prompt + Tool**: `[05-07-2026 14:11] & [05-07-2026 17:52] | Gemini`
  > *Prompt*: "sửa lại phần 3 (phân tích) của FR-09, FR-14, FR-20 theo FR-05 (đảm bảo test data nó phải giống theo test case đã có, cấu trúc bảng test data thì từng biến đầu vào sẽ có 1 cột còn lại thì cứ làm giống như mẫu của FR-05)" và "điều chỉnh test step của test case TC-FR20-004 thêm việc xem phần network trong trình debug đã bật api checkout copy các thông tin cần thiết ra tool API để sửa là giá trị tổng tiền"
- **AI Output**: Tái cấu trúc các bảng dữ liệu kiểm thử với cột riêng biệt cho mỗi biến đầu vào. Thiết kế bước test cho `TC-FR20-004` (thao túng giá tiền API) bằng cách sử dụng các proxy cài đặt phức tạp.

  **Câu trả lời của AI trong chat:**
  ```text
  Tôi đã thành công tái cấu trúc và căn chỉnh Mục 3 (Phân tích) của **FR-09**, **FR-14**, và **FR-20** để khớp hoàn toàn với định dạng của **FR-05**:
  1. **Bảng dữ liệu kiểm thử**: Tạo bảng Step 4: Test Data cho phân hoạch lớp tương đương (EP) và bảng Test Data cho BVA.
  2. **Cấu trúc phân cột**: Từng biến đầu vào (ví dụ: couponCode, orderTotal, isLoggedIn, v.v.) được định vị một cột riêng.
  3. **Độ khớp 1-to-1**: Đảm bảo toàn bộ các dòng test data tương ứng 1-to-1 với các kịch bản kiểm thử ở Mục 4.
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: Quy trình giả lập thao túng tham số API trên mobile bằng proxy cài đặt độc lập ban đầu của AI quá phức tạp và khó thực hiện. AI chưa đưa ra giải pháp tận dụng tính năng JS Debugger có sẵn của Expo Go kết hợp với Postman để đơn giản hóa quá trình test API đối với ứng dụng React Native.
- **Student Fix**: Hiệu chỉnh lại các bước kiểm thử của `TC-FR20-004` và bảng Test Data: Hướng dẫn chi tiết cách mở JS Debugger của Expo Go trên thiết bị, theo dõi tab Network để lấy thông tin request `POST /api/checkout`, sau đó copy dữ liệu sang Postman để chỉnh sửa tham số `total_amount` thành `1,000` ₫ nhằm thực hiện thao túng giá đơn hàng. Kết quả ghi nhận lỗi bảo mật nghiêm trọng `B017` và đẩy thành công lên GitHub Issue #63.

---

### Tác vụ 11: Cập nhật hướng dẫn kỹ năng domain_testing_bva.md
- **Prompt + Tool**: `[06-07-2026 10:36] | Gemini`
  > *Prompt*: "cập nhật lại skill domain_testing_bva theo report"
- **AI Output**: Cập nhật tệp tài liệu kỹ năng domain_testing_bva.md để chuẩn hóa định dạng bảng dữ liệu kiểm thử phân cột.

  **Câu trả lời của AI trong chat:**
  ```text
  I have updated the **Domain Testing & BVA** skill instructions in the project:
  1. **Updated Skill Document**: Refactored domain_testing_bva.md to follow the parameterized table format for Step 4.
  2. **Formatting Standards**: Specified the 1-to-1 mapping rule and separate columns for each identified input variable.
  3. **Practical Example**: Refactored the numerical division example (0 < X < 100) to strictly follow this parameterized table format.
  ```
- **Verdict**: **INCOMPLETE**
- **Reasoning**: AI không tự động cập nhật tài liệu kỹ năng của dự án dựa trên cấu trúc báo cáo thực tế đã thống nhất cho đến khi có yêu cầu cụ thể từ sinh viên thông qua prompt.
- **Student Fix**: Chỉ định AI cập nhật tài liệu mẫu trong file kỹ năng theo đúng cấu trúc bảng phân tích 4 bước thực tế đã áp dụng trong báo cáo để đồng bộ hóa quy trình làm việc cho các thành viên khác.

---

## 2. Tổng hợp Tỷ lệ Chính xác (Overall Accuracy Ratio)

| Trạng thái | Số lượng | Tỷ lệ phần trăm |
|---|---|---|
| VALID | 0 | 0% |
| INVALID | 1 | 9.1% |
| INCOMPLETE | 10 | 90.9% |
| **Tổng số tác vụ kiểm định** | **11** | **100%** |

## 3. AI Critique

Qua quá trình test, AI thể hiện rõ cả điểm mạnh lẫn hạn chế: AI mắc sai sót như tự nhận diện sai ID tính năng (FR-20 thành FR-08) và mặc định môi trường Web (sử dụng thao tác click chuột...) thay vì môi trường di động trên Expo Go theo yêu cầu đề bài. Bên cạnh đó, các kết quả sinh ra thường incomplete vì AI bỏ qua các kịch bản kiểm thử bảo mật nâng cao và các luồng nghiệp vụ phức tạp đòi hỏi sự phối hợp giữa nhiều trạng thái hệ thống (như việc đăng xuất ngay trước khi áp dụng mã coupon ở màn hình Checkout). Nguyên nhân chủ yếu là do AI thiếu ngữ cảnh thực tế của hệ thống đang chạy (SUT) và chỉ hoạt động dựa trên việc suy diễn văn bản tĩnh từ đặc tả, ngoài ra là do promt chưa chính xác dẫn đến AI hiểu sai. 

Bài học và nguyên tắc cốt lõi rút ra là AI chỉ đóng vai trò là một trợ lý hỗ trợ soạn thảo khung tài liệu và gợi ý ban đầu. Người tester bắt buộc phải đóng vai trò chủ đạo, trực tiếp tư duy và phân tích, từ đó đưa cho AI các thông tin chính xác để thiết kế các test case tự động bám sát với specification và chịu trách nhiệm cuối cùng cho chất lượng sản phẩm.