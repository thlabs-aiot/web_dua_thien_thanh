# Dừa Xanh Lục Thiên Thanh — Tài liệu phát triển (Developer Guide)

Tài liệu này tóm tắt cấu trúc dự án và các nâng cấp giao diện (Premium Upgrades v2.0) đã thực hiện. Bất kỳ AI hoặc lập trình viên nào tiếp quản dự án này trong tương lai hãy đọc tài liệu này để hiểu nhanh các thay đổi mà không cần đọc lại toàn bộ mã nguồn.

---

## 📂 Cấu trúc thư mục (Project Structure)
* `denhatdua.html`: File mã nguồn chính chứa toàn bộ HTML, CSS và JavaScript của ứng dụng (Single-page Premium Website).
* `anh/`: Thư mục chứa tài nguyên hình ảnh.
  * `anh/logo_dua_thien_thanh.png` hoặc `logo.png`: Logo chính thức của thương hiệu.

---

## 🛠️ Các cải tiến & Sửa lỗi quan trọng (Premium Upgrades & Fixes)

### 1. Hệ thống dịch thuật đa ngôn ngữ động (Dynamic Multilingual Support - VI/EN)
* **Vấn đề trước đây:** Nút chuyển đổi ngôn ngữ ở header chỉ là giao diện tĩnh, không dịch được nội dung.
* **Giải pháp đã triển khai:**
  * Xây dựng hàm chuyển ngữ động `setLanguage(lang)` trong file HTML chính.
  * Lập trình viên/AI tiếp theo chỉ cần thêm thuộc tính `data-en="..."` vào bất kỳ thẻ HTML nào để dịch nội dung của thẻ đó sang Tiếng Anh.
  * Đối với các ô nhập liệu (input), sử dụng `data-en-placeholder="..."` để dịch gợi ý nhập.
  * **Cơ chế hoạt động:** Khi người dùng chuyển sang tiếng Anh (`EN`), JavaScript sẽ ghi nhớ nội dung gốc Tiếng Việt vào thuộc tính đệm `data-vi` (và `data-vi-placeholder`) để khi quay lại tiếng Việt (`VI`), nội dung được khôi phục nguyên bản mà không cần reload trang hoặc viết code JS dịch thuật thủ công phức tạp.
  * Toàn bộ trang web (Header, Hero Section, Bento Grid Products, Brand Story, Reviews, Map, CTA, Footer) đã được cấu hình và dịch thuật sẵn sang tiếng Anh cao cấp (Premium English).

### 2. Định dạng lại nút "ĐẶT HÀNG" ở Header (`.btn-order`)
* **Vấn đề trước đây:** Chữ "ĐẶT HÀNG" bị ngắt dòng thành 2 dòng (ĐẶT \n HÀNG) trên một số kích thước màn hình nhìn rất mất thẩm mỹ.
* **Giải pháp đã triển khai:**
  * Thêm `white-space: nowrap;` để bắt buộc văn bản luôn nằm trên 1 dòng duy nhất.
  * Thêm `display: inline-flex; align-items: center; justify-content: center;` để chữ luôn được căn giữa hoàn hảo.
  * Thêm `flex-shrink: 0;` để ngăn chặn việc nút bị bóp méo khi menu điều hướng thu nhỏ.

### 3. Sửa lỗi 2 nút mũi tên điều hướng ảnh bị che cắt đôi (`.car-arrow`)
* **Vấn đề trước đây:** Nút Trước/Sau của Carousel trong Hero section được căn lề âm (`left: -22px`, `right: -22px`) để nhô ra ngoài khung. Tuy nhiên do khung `.carousel` có `overflow: hidden`, hai nút này bị cắt đi một nửa nhìn rất mất thẩm mỹ.
* **Giải pháp đã triển khai:**
  * Chuyển vị trí nút điều hướng vào bên trong khung ảnh một chút: `left: 16px;` và `right: 16px;`.
  * Giờ đây hai nút hình tròn `<` và `>` hiển thị trọn vẹn, sắc nét ngay trên hình nền sản phẩm rất cao cấp.

### 4. Sửa lỗi hiển thị & Nâng cấp kích cỡ 3 số chạy thống kê (`.stat`)
* **Vấn đề trước đây:** Số đếm chạy (`5`, `100`, `50`) bị ghi đè kích thước rất nhỏ (~12px) so với các ký tự đi kèm (`+`, `%`, `K+`) vốn rất lớn.
* **Nguyên nhân:** Do bộ chọn CSS bị chồng chéo (selector collision): `.stat span` vô tình áp dụng cho cả thẻ `<span class="count">` thay vì chỉ áp dụng cho nhãn mô tả ở dưới.
* **Giải pháp đã triển khai:**
  * Đổi bộ chọn mô tả từ `.stat span` thành `.stat > span` để chỉ nhắm mục tiêu vào nhãn chữ trực tiếp của khối.
  * Giúp thẻ số chạy `.count` kế thừa kích thước lớn, đậm của thẻ cha `<b>` một cách tự nhiên.
  * Đồng thời nâng cỡ chữ thống kê lên `clamp(2.2rem, 4vw, 3rem)` giúp 3 con số hiển thị to, rõ ràng, hiện đại và đẳng cấp hơn rất nhiều.

### 5. Sửa lỗi dính chữ/chồng chéo ở thanh Menu (Header Nav) trên màn hình trung bình
* **Vấn đề trước đây:** Khi chuyển sang tiếng Anh, vì độ dài các ký tự tiếng Anh lớn hơn tiếng Việt rất nhiều (ví dụ "Thien Thanh Green Coconut" dài hơn "Dừa Xanh Lục Thiên Thanh"), dẫn đến tình trạng logo, menu và cụm nút ngôn ngữ bị đè lên nhau (overlap) trên các màn hình có chiều rộng trung bình (từ 1080px đến 1280px). Đồng thời do chịu lực nén flexbox, chữ trên menu bị tự động ngắt dòng xuống hàng (Ví dụ: "Sản \n phẩm") nhìn rất mất thẩm mỹ.
* **Giải pháp đã triển khai:**
  * Thêm thuộc tính `white-space: nowrap;` vào các liên kết menu `.menu>li>a` để **bắt buộc chữ trên menu luôn luôn nằm trên một dòng**, không bao giờ bị ngắt dòng khi chuyển đổi VI / EN.
  * Thêm khoảng cách tối thiểu `gap: 20px;` vào container `.nav` để đảm bảo Logo, Menu và Cụm bên phải không bao giờ có thể chạm hay đè lên nhau.
  * Tối ưu hóa Media Query `@media(max-width:1280px)` để thu nhỏ thông minh khi màn hình hẹp lại:
    * Giảm khoảng cách (`gap`) của menu xuống `14px`.
    * Giảm khoảng cách của cụm chức năng bên phải (`.nav-right`) xuống `10px`.
    * Thu nhỏ cỡ chữ logo (`font-size: 1.1rem`) và các liên kết menu (`font-size: 0.85rem`).
    * Giảm padding của nút đặt hàng giúp tiết kiệm diện tích.
  * Nhờ giải pháp này, thanh Menu luôn hiển thị rộng rãi, cân đối và hoàn toàn không bị dính chữ trước khi chuyển sang giao diện Mobile (dưới 1080px).

### 6. Hướng dẫn thay thế Logo thương hiệu
* **Vị trí thay thế logo:** 
  * Logo được hiển thị ở 3 vị trí chính trong file `denhatdua.html`:
    1. **Màn hình tải trang (Page Loader):** Thẻ `<img>` bên trong `.loader-logo` (dòng ~2615).
    2. **Thanh menu (Header Nav):** Thẻ `<img>` bên trong `.logo-mark` (dòng ~2630).
    3. **Chân trang (Footer Logo):** Thẻ `<img>` bên trong `.foot-about .logo-mark` (dòng ~3021).
  * Tất cả các thẻ trên đã được tối ưu hóa CSS để tự động co giãn kích thước (responsive).
  * **Cách thay đổi:** Người dùng chỉ cần thiết kế file logo định dạng `.png` hoặc `.svg` rồi lưu đè vào đường dẫn `anh/logo_dua_thien_thanh.png` (hoặc cấu hình lại thuộc tính `src` của thẻ `<img>` dẫn tới file logo mới).

---

## 💡 Lưu ý dành cho AI tiếp quản (Notes for Future AI)
1. **Duy trì tính năng đa ngôn ngữ:** Khi thêm mới các khối HTML chứa nội dung chữ tĩnh, hãy luôn nhớ đặt thuộc tính `data-en="..."` cho thẻ đó. Nếu là thẻ có thuộc tính `placeholder` (như `<input>` hay `<textarea>`), hãy đặt `data-en-placeholder="..."`.
2. **Tránh thay đổi `.carousel { overflow: hidden; }`:** Việc giữ nguyên thuộc tính này là bắt buộc để các slide trượt không bị tràn ra ngoài màn hình. Vị trí nút `.car-arrow` nên được duy trì bên trong biên của ảnh (`left: 16px` / `right: 16px`).
3. **Responsive Web Design:** Khi chỉnh sửa CSS, hãy chú ý cấu trúc `clamp(...)` và các Media Query từ dòng ~2240 trở đi để giữ cho trang luôn hiển thị hoàn mỹ trên cả Mobile, Tablet và Desktop.
