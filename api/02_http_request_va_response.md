# Chuyên đề 2: Request and Response


---

## 1. Request / Response Structure (Cấu trúc Request và Response)

### 1.1 HTTP Method (Phương thức HTTP)
- Là thành phần quan trọng của một request tới dịch vụ REST API, dùng để xác định **loại hành động** được yêu cầu:

| HTTP Method | Mô tả hành động (Description) |
| :--- | :--- |
| **GET** | Lấy thông tin về đối tượng được tham chiếu (Retrieves information). |
| **POST** | Tạo mới một đối tượng (Creates a new object). |
| **PUT / PATCH** | Cập nhật các thuộc tính của đối tượng đã tồn tại (Updates properties). |
| **DELETE** | Xóa đối tượng được chỉ định (Deletes the referenced object). |

> **Lưu ý**: REST API service có thể trả về phản hồi **`405 Method Not Allowed`** nếu request gửi phương thức HTTP không được hỗ trợ.

---

### 1.2 Resource names and request URL (Tên tài nguyên & URL)
- Các API thao tác trên cùng một kiểu đối tượng nên sử dụng chung một **Resource Name** (tên tài nguyên, thường là danh từ số nhiều, ví dụ: `"objects"` -> `/api/objects`).

| HTTP Method | Request URL | Mô tả chức năng |
| :--- | :--- | :--- |
| **GET** | `/api/objects` hoặc `/api/objects/{id}` | Lấy danh sách đối tượng hoặc lấy 1 đối tượng theo ID. |
| **POST** | `/api/objects` | Thêm mới một đối tượng. |
| **PUT / PATCH** | `/api/objects/{id}` | Cập nhật một đối tượng theo ID. |
| **DELETE** | `/api/objects/{id}` | Xóa một đối tượng theo ID. |

---

### 1.3 Request structure (Cấu trúc Request)

#### Khuôn mẫu:
```text
// Request-line
<method> <request-URL> <http-version>
// (optional)
<header>
// one empty line, mark as end of header
// (optional)
<body>
```

#### Chi tiết các thành phần:
- **Request-line**:
  - `method`: Phương thức HTTP (GET, POST...).
  - `request-URL`: Nếu request không yêu cầu tài nguyên cụ thể thì URL là `*`.
  - `http-version`: Phiên bản HTTP.
- **Request-header**:
  - Cung cấp thông tin về request để server xác thực và dịch request body.
  - **Giá trị của Request header không phân biệt hoa thường (case-insensitive)**.
  - Các trường thông dụng:
    - `Accept`: Định dạng dữ liệu mong muốn nhận về (Ví dụ: `application/xml`, `application/json`, `text/plain`...).
    - `Accept-encoding`: Các kiểu mã hóa nén được chấp nhận (Ví dụ: `gzip`, `xz`, `br`...).
    - `Authorization`: Token hoặc kiểu xác thực của request.
    - `Content-Type`: Định dạng của request body (Ví dụ: `application/json`, `application/x-www-form-urlencoded`).
    - `Cookie`: Thông tin cookie HTTP gửi lên server.
    - `Content-Length`: Số lượng byte có trong request body.
    - `Host`: Tên host tương ứng với endpoint được yêu cầu.
- **Request body**:
  - Thường dùng cho `POST` và `PUT` để chứa dữ liệu gửi lên server.
  - **Các tham số trong request body có phân biệt hoa thường (case-sensitive)**.
  - Kiểu dữ liệu: Thông điệp **JSON** hoặc phần tử **XML**.

#### Ví dụ Request:
```http
POST <webservice>/Login HTTP/1.1
Host: <host name>
Accept: (application/xml | application/json)
Content-Type: application/json

{
  "domain": "",
  "username": "",
  "password": "",
  "commserver": ""
}
```

---

### 1.4 Response structure (Cấu trúc Response)

#### Khuôn mẫu:
```text
// Status-line
<http-version> <status> <reason-phrase>
// (optional)
<header>
// one empty line, mark as end of header
// (optional)
<body>
```

#### Chi tiết các thành phần:
- **Status-line**:
  - `http-version`: Phiên bản HTTP.
  - `status-code`: Mã trạng thái phản hồi HTTP.
  - `reason-phrase`: Mô tả ngắn gọn về status code.
- **Response Headers**: Cung cấp thông tin về kết quả phản hồi:
  - `Cache-Control` (hoặc `Cache-Controller`): Chính sách kiểm soát bộ nhớ cache cho body.
  - `Content-Length`: Số byte trong phần response body.
  - `Content-Type`: Định dạng của response body (`application/json; charset=utf-8` hoặc `application/xml`).
  - `Set-Cookie`: Cookie gửi từ server để trình duyệt lưu lại và gửi ngược lại ở các request sau.
  - `Date`: Ngày giờ (theo chuẩn UTC) mà request được xử lý.
- **Response body**: Chứa dữ liệu được server trả về.

#### Ví dụ Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "In progress",
  "link": {
    "rel": "cancel",
    "method": "delete",
    "href": "/api/status/12345"
  }
}
```

---

## 2. Content types (Các kiểu nội dung & Form Encoding)
- Header `Content-Type` dùng để chỉ định **media type (MIME type)** của tài nguyên:
  - Trong **Request**: Xác định định dạng dữ liệu trong request body để bên nhận parse phù hợp.
  - Trong **Response**: Xác định định dạng dữ liệu trong response body.
- Thuộc tính `Accept` trong request cũng sử dụng MIME type và có thể chứa nhiều giá trị.
- Trong request `POST` gửi từ HTML form, `Content-Type` được chỉ định qua thuộc tính `enctype` của thẻ `<form>`.

### Các kiểu mã hóa form HTML (HTML form encoding):
1. **`application/x-www-form-urlencoded`**:
   - Là giá trị mặc định nếu không khai báo `enctype`.
   - Tạo ra một chuỗi dài các cặp `(name, value)`. Mỗi cặp phân tách bằng dấu `&`, tên và giá trị phân tách bằng dấu `=`.
2. **`multipart/form-data`**:
   - Dùng khi người dùng cần **upload file** lên server.
   - Giá trị `boundary` cho phép server xác định vị trí bắt đầu và kết thúc của từng tham số dữ liệu.
3. **`text/plain`**:
   - Gửi dữ liệu thô, không qua bất kỳ mã hóa nào.
   - Là giá trị mặc định cho file text thuần túy đọc được (human-readable), **không được chứa dữ liệu nhị phân (binary data)**.
