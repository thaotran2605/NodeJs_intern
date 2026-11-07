# Chuyên đề 1: API & RESTful API


---

## 1. What is API? (API là gì?)
- **API**: Viết tắt của **Application Programming Interface** (Giao diện lập trình ứng dụng).
- Là một tập hợp các đoạn mã lập trình cho phép truyền tải và trao đổi dữ liệu giữa một sản phẩm phần mềm này với một sản phẩm phần mềm khác.
- Chứa các điều khoản/quy định về việc trao đổi dữ liệu đó.
- **Mô hình hoạt động**:
  - `Web app (Browser)` gửi **Request** qua **Internet** đến **API**.
  - **API** chuyển tới **Web Server**, Web Server truy vấn **Database**.
  - Kết quả được gửi ngược lại qua **Response** về trình duyệt.

---

## 2. What is REST, RESTful API? (REST và RESTful API là gì?)
- **REST**: Viết tắt của **REpresentational State Transfer**. Là một phong cách kiến trúc phần mềm định nghĩa một tập hợp các ràng buộc (constraints) dùng để tạo lập các Web services.
- **RESTful API**: Là các API tuân theo kiến trúc và quy tắc của REST.
- **Ứng dụng**: Cho phép nhiều nền tảng ứng dụng khác nhau (Dynamic Web App, Android App, Desktop App, iPhone App, JavaScript Web App...) kết nối và sử dụng chung một REST API kết nối với Database.

---

## 3. How does RESTful API work? (RESTful API hoạt động như thế nào?)
- Client gửi **HTTP Requests** và đợi phản hồi (**Response**).
- **Các HTTP Verbs điển hình**:
  - `GET`: Đọc dữ liệu từ Database (Ví dụ: `GET /users`).
  - `POST`: Tạo mới một bản ghi trong Database (Ví dụ: `POST /newUser`).
  - `PATCH`: Cập nhật / sửa đổi bản ghi trong Database (Ví dụ: `PATCH /updateUser`).
  - `PUT`: Cập nhật / thay thế bản ghi trong Database.
  - `DELETE`: Xóa bản ghi khỏi Database.
- **Quy trình**:
  1. REST API nhận HTTP request từ Client và thực hiện các yêu cầu cần thiết (ví dụ: tạo user).
  2. REST API truy vấn cơ sở dữ liệu theo yêu cầu.
  3. Khi có dữ liệu, REST API gửi Response về cho Client. Phản hồi này thường ở định dạng **JSON** hoặc **XML**.

---

## 4. REST constraints (Các ràng buộc của REST)
Kiến trúc REST bao gồm 5 ràng buộc chính:
1. **Client-server architecture**: Tách rời phía Client và Server.
2. **Statelessness (Phi trạng thái)**: Trạng thái được lưu ở Client, Server không lưu trữ trạng thái phiên (session).
3. **Cacheability**: Dữ liệu phản hồi phải chỉ định rõ có được lưu cache hay không (Ví dụ qua response header: `cache-control: public, max-age=31536000`).
4. **Layered system (Hệ thống phân tầng)**: Hệ thống có thể chia nhiều lớp trung gian để mở rộng (scaling) giữa Client và Server.
5. **Uniform interface (Giao diện đồng nhất)**, gồm 4 nguyên tắc:
   - Resource identification in requests (Định danh tài nguyên trong request).
   - Resource manipulation through representations (Thao tác tài nguyên thông qua các biểu diễn).
   - Self-descriptive messages (Thông điệp tự mô tả).
   - Hypermedia as the engine of application state - HATEOAS (Siêu phương tiện điều hướng trạng thái ứng dụng).

---

## 5. Advantages & Disadvantages of REST, RESTful API (Ưu và Nhược điểm)
- **Ưu điểm (Advantages)**:
  - Đạt được các yêu cầu phi chức năng mong muốn như:
    - Hiệu năng (Performance).
    - Khả năng mở rộng (Scalability).
    - Đơn giản (Simplicity).
    - Dễ sửa đổi (Modifiability).
    - Tính trực quan (Visibility).
    - Khả chuyển (Portability).
    - Độ tin cậy (Reliability).
- **Nhược điểm (Disadvantages)**:
  - Vấn đề bảo mật và xác thực không được đảm bảo sẵn như SOAP (do đặc tính phi trạng thái - stateless).
