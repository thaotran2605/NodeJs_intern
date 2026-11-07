# Chuyên đề 3: API Authentication


---

## 1. Giới thiệu (Introduction)
- **Hệ thống xác thực dạng Token (Token-based authentication)**: Cho phép người dùng nhập username và password một lần để lấy về một **Access Token**.
- Token này cho phép người dùng truy xuất tài nguyên từ server ở những lần tiếp theo mà **không cần nhập lại thông tin đăng nhập (credentials)**.

---

## 2. So sánh các phương pháp xác thực (Compare authenticate method)

### 2.1 HTTP Basic (Truyền thống: Session và Cookies)
- **Cơ chế**:
  1. **Session**: Được lưu trữ trên server, tham chiếu tới hồ sơ người dùng (user profile).
  2. **Cookies**: Được lưu trữ ở client, chứa token/session ID để đối chiếu với session ID trên server.
- **Quy trình hoạt động**:
  1. User gửi username/password qua HTTP Request lên server.
  2. Server kiểm tra username/password với database (nếu sai thì từ chối truy cập).
  3. Nếu đúng, server tạo một session ID duy nhất đại diện cho phiên người dùng và đính kèm vào response (qua `Set-Cookie`).
  4. Ở các request tiếp theo, trình duyệt tự đính kèm cookie. Server kiểm tra session data và cấp quyền truy cập.
- **Ưu điểm (Advantage)**:
  - Dễ triển khai (Easy to implement).
- **Nhược điểm (Disadvantages)**:
  1. Server phải chịu tải quản lý session của user.
  2. Hiệu năng chậm (Slow performance): Server phải tra cứu và deserialize session ở mỗi request.
  3. Khả năng mở rộng kém (Scalability): Session và cookies không thể dùng trực tiếp trên nhiều server backend cùng lúc.
  4. Khó sử dụng với các ứng dụng không chạy trên trình duyệt (Ví dụ: Mobile app).

---

### 2.2 JSON Web Token (JWT)
- Là chuẩn mở (**RFC 7519**) định nghĩa phương thức nhỏ gọn và tự chứa (compact and self-contained) để truyền thông tin an toàn giữa các bên dưới dạng đối tượng JSON.
- Được dùng làm payload cho **JWS (Json Web Signature - RFC 7515)** hoặc dạng văn bản mã hóa cho **JWE (Json Web Encryption - RFC 7516)** nhằm chống giả mạo từ phía client.
- **Luồng hoạt động**:
  1. Client gửi `POST /authenticate` (username, password) tới **Authenticate Server**.
  2. Authenticate Server xác thực và sinh Access Token, trả về: `HTTP 200 OK Token: [...]`.
  3. Client gửi request tới **API Server** với header: `Authorization: Bearer <Token>`.
  4. API Server tự xác thực token và trả về dữ liệu: `HTTP 200 OK`.

#### Cấu trúc của JWT:
Bao gồm 3 phần phân tách nhau bằng dấu chấm (`.`): `HEADER.PAYLOAD.SIGNATURE` (Ví dụ: `ey... . ey... . wr...`).

1. **HEADER**: Gồm 2 thông tin:
   - Thuật toán mã hóa (`alg`: ví dụ `"HS256"`).
   - Loại token (`typ`: `"JWT"`).
2. **PAYLOAD**: Chứa các "yêu sách" (claims) về người dùng (theo RFC 7519), gồm 3 loại:
   - *Registered Claims* (các claims chuẩn hóa).
   - *Public Claims*.
   - *Private Claims*.
   - Ví dụ: `{"sub": "1234567890", "name": "John Doe", "admin": true}`
3. **SIGNATURE (Chữ ký)**:
   - Dùng để xác minh JWT không bị chỉnh sửa/giả mạo trên đường truyền.
   - Được tạo từ: `header`, `payload`, `secret key` thông qua thuật toán mã hóa.
   - Ví dụ với HMAC SHA256:
     ```text
     HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
     ```

#### Ưu điểm của JWT:
1. **Khả năng mở rộng (Scalability)**: Server không cần lưu session của user; user có thể truy cập nhiều server backend khác nhau.
2. **Tính khả chuyển (Portability)**: Dễ dàng sử dụng cho các ứng dụng không dùng trình duyệt (Mobile apps).
3. **Bảo mật (Security)**: Chữ ký số mã hóa giúp ngăn chặn can thiệp, giả mạo dữ liệu từ phía client.
4. **Hiệu năng nhanh hơn (Faster performance)**: Token nhỏ gọn và tự chứa đầy đủ thông tin (credentials, thời hạn hết hạn, các claims do người dùng định nghĩa).

#### Nhược điểm của JWT:
1. Kích thước lớn hơn nhiều lần so với Session ID đơn giản (đánh đổi dung lượng mạng lấy việc giảm tải độ trễ cho server).
2. Có thể bị tấn công qua **CSRF** hoặc **XSS**. **JWT không được dùng để lưu trữ dữ liệu nhạy cảm**.

---

## 3. OAuth (Open Authorization)
- **OAuth** là giao thức ủy quyền chuẩn mở (**RFC 5849, RFC 6749**) cho phép người dùng xác thực an toàn vào các dịch vụ bên thứ ba mà **không cần chia sẻ thông tin đăng nhập (username/password)**.

### 3.1 OAuth 1.0 (RFC 5849)
- **Các thuật ngữ**:
  - `User`: Người muốn truy cập tài nguyên được bảo vệ.
  - `Consumer`: Ứng dụng sử dụng OAuth để thay mặt User truy cập vào Service Provider.
  - `Service Provider`: Ứng dụng web cho phép truy cập qua OAuth.
  - `Protected Resources`: Dữ liệu được bảo vệ mà người dùng muốn truy cập.
- **Các loại Token**:
  - `Request Token`: Dùng để hỏi xin quyền ủy quyền từ service.
  - `Access Token`: Dùng để truy cập tài nguyên được bảo vệ.
- **Luồng hoạt động**:
  - Consumer lấy Request Token từ Service Provider.
  - Redirect User sang Service Provider để cấp quyền.
  - User đồng ý -> Redirect lại về Consumer.
  - Consumer gửi yêu cầu đổi Request Token lấy Access Token.
  - Service Provider cấp Access Token -> Consumer dùng Access Token để truy cập tài nguyên.

---

### 3.2 OAuth 2.0 (RFC 6749)
- Được xây dựng dựa trên kinh nghiệm triển khai OAuth 1.0.
- **Lưu ý**: Giao thức **OAuth 2.0 KHÔNG tương thích ngược với OAuth 1.0** (có rất ít điểm tương đồng về cách triển khai).
- **4 Thuật ngữ / Vai trò chính trong OAuth 2.0**:
  1. **Resource Owner**: Thực thể có quyền cấp quyền truy cập tài nguyên (chính là User).
  2. **Resource Server**: Máy chủ lưu trữ tài nguyên được bảo vệ.
  3. **Client**: Ứng dụng yêu cầu truy cập tài nguyên thay mặt cho Resource Owner.
  4. **Authorization Server**: Máy chủ cấp Access Token cho Client sau khi xác thực thành công.
- **Luồng hoạt động cơ bản**:
  1. Client gửi *Authorization Request* tới Resource Owner.
  2. Resource Owner trả về *Authorization Grant*.
  3. Client gửi *Authorization Grant* tới Authorization Server.
  4. Authorization Server cấp *Access Token* (và tùy chọn cấp thêm *Refresh Token*).
  5. Client dùng *Access Token* gửi tới Resource Server để lấy *Protected Resource*.
- **Cơ chế Refresh Token (Optional refresh token)**:
  - Khi Access Token hết hạn hoặc không hợp lệ:
  - Client gửi `Refresh Token` tới **Authorization Server**.
  - Authorization Server xác minh và cấp lại `Access Token` mới (kèm Refresh Token mới nếu có).
