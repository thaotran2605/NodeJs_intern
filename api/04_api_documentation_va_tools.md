# Chuyên đề 4: API Documentation & Tools


---

## 1. API Documentation là gì?
- Là một sản phẩm nội dung kỹ thuật bàn giao (technical content deliverable), chứa hướng dẫn về cách sử dụng và tích hợp với API một cách hiệu quả.
- Là cuốn cẩm nang tham khảo súc tích chứa mọi thông tin cần thiết: hàm, lớp, kiểu trả về, đối số/tham số... đi kèm các bài hướng dẫn (tutorials) và ví dụ mẫu.
- Truyền thống thường được tạo bằng các công cụ soạn thảo nội dung văn bản thông thường.

### Các thông tin bắt buộc phải có trong tài liệu API:
1. **URL**: Đường dẫn endpoint cụ thể.
2. **Method**: Phương thức HTTP (`GET`, `POST`, `PUT`, `DELETE`...).
3. **Data of the request**: Dữ liệu gửi lên trong request (tên tham số, kiểu dữ liệu, mô tả).
4. **Authentication requests**: Quy định cách thức xác thực (Token, Key...).
5. **Configuring request headers**: Các header cần cấu hình (Ví dụ: `Content-Type: application/json`).
6. **Response example**: Ví dụ mẫu về dữ liệu trả về và mã status code.

---

## 2. Why Document APIs? (Tại sao cần viết tài liệu API?)
- Giúp client/người dùng hiểu cách thức hoạt động và dễ dàng tích hợp tương tác với code.
- Bao quát mọi thông tin người dùng cần và hỗ trợ khi họ thao tác sai sót trong quá trình dùng API.
- Tài liệu tốt sẽ khuyến khích khách hàng tiềm năng sử dụng hoặc mua dịch vụ.

### 4 Lợi ích chính:
1. **Improved User Adoption (Tăng tỷ lệ tiếp nhận)**: Tài liệu rõ ràng giúp người dùng nhanh chóng nhận thấy giá trị của dịch vụ, dẫn tới tăng trưởng tốt hơn.
2. **Increased Awareness (Tăng độ nhận diện)**: Người dùng học và dùng nhanh sẽ trở thành người ủng hộ và chia sẻ, tạo hiệu ứng mạng lưới (network effect).
3. **Saves Support Time and Costs (Tiết kiệm thời gian & chi phí hỗ trợ)**: Giúp giảm tải vô số giờ đội ngũ phải trả lời email và cuộc gọi hỗ trợ.
4. **Easier Maintenance (Bảo trì dễ dàng hơn)**: Giúp các nhóm nội bộ nắm rõ chi tiết về tài nguyên, phương thức, request và response liên quan, giúp cập nhật và bảo trì nhanh hơn.

---

## 3. API Design Template (Mẫu thiết kế tài liệu API)
- Phương pháp truyền thống là sử dụng các công cụ văn phòng thông thường.
- Nhiều dự án tại Sun Asterisk sử dụng mẫu **"API Design" Template** của công ty.
- **Quy ước đặt tên file**: Phải bao gồm Project ID & Project Name (Ví dụ: `[123] ABC System - API Design`).

### Tài liệu API Design bao gồm các phần chính:
1. **History of changes**: Nhật ký các thay đổi đã thực hiện trên file thiết kế API.
2. **Common Conventions**: Danh sách các quy ước chung cần tuân theo khi thiết kế API cho dự án.
3. **Status code**: Danh sách tất cả các mã trạng thái (status code) được dùng trong dự án.
4. **API List**: Danh sách tổng hợp toàn bộ các API trong hệ thống.
5. **API Detail**: Mỗi API trong danh sách sẽ có một sheet mô tả kỹ thuật chi tiết (Request Params, Response format...).
6. **Appendix (Phụ lục)**: Các thông tin khác như danh sách actions, quyền hạn (members), các kiểu dữ liệu và kiểu tham số.

---

## 4. Tools (Các công cụ làm việc với API)
Bên cạnh làm tài liệu thủ công, phương pháp khác là sử dụng trực tiếp các công cụ chuyên dụng. Hai công cụ chính:

### 4.1 Postman
- Là nền tảng API (API platform) dùng để xây dựng và sử dụng các API.
- **Lợi ích**:
  - Xuất bản tài liệu nhanh chóng và dễ dàng (*Publish documentation quickly and easily*).
  - Tự động cập nhật tài liệu khi có thay đổi (*Automatically update your documentation*).
  - Chia sẻ dễ dàng thông qua nút *Run in Postman Button*.
  - Tăng tỷ lệ sử dụng API (*Gain adoption of your API*).
  - Hỗ trợ làm việc nhóm trên tài liệu (*Collaborate with your Team on Docs*).
- **Cách gửi Request trên Postman**:
  1. Bấm nút dấu cộng `[+]`.
  2. Chọn phương thức HTTP (HTTP method).
  3. Nhập đường dẫn API URL.
  4. Điền các biến/tham số của request (Request variables).
  5. Bấm nút **"Send"**.
- **Cách xem Response**:
  1. Kiểm tra mã trạng thái (**Status code**).
  2. Xem dữ liệu trả về (**Response body**).
- **Các tính năng sau khi cài đặt Postman**:
  - Gửi request (`Making requests`).
  - Kiểm thử API (`Testing APIs`).
  - Xây dựng và quản lý API (`Building and managing APIs`).
  - Xuất bản tài liệu (`Publishing APIs`).
  - Cộng tác với đội ngũ (`Collaborating with your team`).
  - Phát triển API (`Developing with Postman`).

---

### 4.2 Swagger & OpenAPI Specification

#### Chuẩn OpenAPI Specification (trước đây là Swagger Specification):
- Là định dạng mô tả chuẩn dành cho các RESTful API.
- File OpenAPI cho phép mô tả toàn bộ hệ thống API bao gồm:
  - Các endpoints khả dụng (Ví dụ: `/users`) và các thao tác trên từng endpoint (`GET /users`, `POST /users`).
  - Tham số thao tác: Dữ liệu đầu vào (Input) và đầu ra (Output) cho mỗi thao tác.
  - Các phương thức xác thực (Authentication methods).
  - Thông tin liên hệ, giấy phép (license), điều khoản sử dụng và các thông tin khác.

#### Bộ 3 công cụ chính của Swagger:
1. **Swagger Editor**: Trình chỉnh sửa trên trình duyệt giúp lập trình viên viết file đặc tả OpenAPI (YAML/JSON).
2. **Swagger UI**: Tạo giao diện hiển thị trực quan tài liệu API có tính tương tác cao (cho phép thử nghiệm API ngay trên trình duyệt).
3. **Swagger Codegen**: Tự động sinh mã nguồn máy chủ (server stubs) và thư viện máy khách (client libraries) từ file đặc tả OpenAPI.
