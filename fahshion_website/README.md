# Tài Liệu Hướng Dẫn Sử Dụng Bruno Làm Việc Nhóm - Fashion Website API

Tài liệu này hướng dẫn các thành viên trong nhóm cách kéo (clone) project, cấu hình **Bruno** và quy ước tạo thư mục (folder) riêng để cùng xây dựng & kiểm thử (test) hệ thống API cho dự án **Fashion Website**.

---

## 1. Cài đặt công cụ
1. **Git**: Đảm bảo máy đã cài đặt Git ([Tải Git tại đây](https://git-scm.com/)).
2. **Bruno**: Tải và cài đặt phần mềm quản lý API **Bruno** tại trang chủ: [https://www.usebruno.com/downloads](https://www.usebruno.com/downloads).

---

## 2. Kéo (Clone) bộ API từ GitHub về máy

Mở **Terminal / Git Bash / PowerShell** tại thư mục bạn muốn lưu trữ và chạy lệnh:

```bash
git clone https://github.com/vantruong6112005/document_api_fashionwebsite.git
```

Sau khi clone về, cấu trúc thư mục hiện tại như sau:

```text
document_api_fashionwebsite/
└── fahshion_website/           <-- Thư mục gốc của Bruno Collection
    ├── opencollection.yml      <-- File cấu hình Collection chung
    ├── auth/                   <-- Thư mục API Xác thực (Ví dụ: Trường đã tạo)
    │   ├── login.yml
    │   ├── Register.yml
    │   ├── forgotpassword.yml
    │   ├── ResetPW.yml
    │   ├── google.yml
    │   └── ...
    └── user/                   <-- Thư mục API Người dùng (Ví dụ: Trường đã tạo)
        ├── getAllUsers.yml
        ├── getCurrentUser.yml
        ├── UpdateUser.yml
        └── ...
```

---

## 3. Mở Collection trong Bruno

1. Mở ứng dụng **Bruno**.
2. Nhấn vào nút **Open Collection** (ở màn hình chính hoặc góc trái trên cùng).
3. Chọn đường dẫn trỏ thẳng vào thư mục **`fahshion_website`** nằm bên trong thư mục vừa clone về (`.../document_api_fashionwebsite/fahshion_website`).
4. Sau khi mở, bạn sẽ thấy Collection **`fahshion_website`** cùng các thư mục có sẵn như `auth` và `user`.

---

## 4. Cấu hình `{{baseurl}}` và Token xác thực (Auth)

### 4.1. Thiết lập biến `{{baseurl}}`
Trong các endpoint mẫu (như `auth/login` hay `user/UpdateUser`), đường dẫn đang được cấu hình động qua biến `{{baseurl}}` (ví dụ: `{{baseurl}}/auth/login`). Để chạy được request:

1. Ở góc trên bên phải của Bruno, nhấn vào mục **No Environment** (hoặc biểu tượng môi trường) -> chọn **Configure**.
2. Chọn **Create Environment** (hoặc cấu hình trong **Collection Variables**):
   - Đặt tên môi trường: `Local`
   - Thêm biến mới:
     - **Name**: `baseurl`
     - **Value**: `http://localhost:8080` *(hoặc địa chỉ/port backend trên máy bạn)*
3. Nhấn **Save** và chọn môi trường **`Local`** vừa tạo.

### 4.2. Sử dụng chung Bearer Token
Collection đã được bật sẵn chế độ kế thừa Token (**Auth: Inherit**):
1. Chạy API **`auth/login`** để lấy chuỗi `token` (Access Token) từ kết quả trả về.
2. Nhấn vào tên Collection **`fahshion_website`** (hoặc biểu tượng `...` cạnh tên Collection -> **Settings**) -> chuyển sang tab **Auth**.
3. Chọn kiểu **Bearer Token** và dán token vừa lấy vào ô **Token**, sau đó nhấn **Save**.
4. Tất cả các endpoint để chế độ `Auth: Inherit` sẽ tự động đính kèm token này khi gửi request.

---

## 5. Quy tắc phân chia thư mục cho từng thành viên

Để tránh xung đột (conflict) khi làm chung và giúp quản lý API rõ ràng, **mỗi thành viên phụ trách phần (module) nào sẽ tạo một thư mục riêng cho phần đó**:

### Ví dụ mẫu đã có sẵn:
Thành viên **Trường** phụ trách module Xác thực và Người dùng đã tạo 2 thư mục:
- **`auth/`**: Chứa toàn bộ endpoint liên quan đến đăng nhập, đăng ký, quên mật khẩu, Google OAuth, làm mới token...
- **`user/`**: Chứa toàn bộ endpoint quản lý thông tin user, đổi mật khẩu, cập nhật avatar...

### Cách thành viên mới tạo thư mục & endpoint của mình:
Giả sử bạn phụ trách phần **Sản phẩm (`product`)**, **Giỏ hàng (`cart`)** hoặc **Đơn hàng (`order`)**:

1. **Tạo thư mục riêng**:
   - Chuột phải vào Collection **`fahshion_website`** -> chọn **New Folder**.
   - Đặt tên thư mục ngắn gọn, chữ thường, không dấu theo tên module (ví dụ: `product`, `category`, `cart`, `order`, `voucher`...).
2. **Tạo Endpoint (Request) bên trong thư mục**:
   - Chuột phải vào thư mục của bạn -> chọn **New Request**.
   - Chọn **HTTP Request**, đặt tên rõ ràng theo chức năng (ví dụ: `getAllProducts`, `createProduct`, `updateProduct`, `deleteProduct`).
   - Chọn phương thức (`GET`, `POST`, `PUT`, `DELETE`...).
   - **Nhập URL chuẩn với `{{baseurl}}`**:
     ```text
     {{baseurl}}/products
     ```
     *(Lưu ý: Luôn sử dụng `{{baseurl}}` thay vì gõ cứng `http://localhost:8080` để các thành viên khác kéo về có thể test ngay).*
3. **Cấu hình Body & Auth cho Request**:
   - Tab **Body**: Chọn `JSON` (hoặc `Multipart Form` nếu upload ảnh như `user/UpdateAvt`) và điền dữ liệu mẫu để cả nhóm dễ test.
   - Tab **Auth**: Chọn **Inherit** để dùng chung Bearer Token của Collection.
4. Nhấn **`Ctrl + S`** (hoặc `Cmd + S` trên Mac) để lưu request.

---

## 6. Quy trình đẩy và cập nhật code với Git

Vì Bruno lưu mỗi API thành một file `.yml` độc lập theo từng thư mục, các thành viên cần tuân thủ quy trình sau để không bị xung đột code:

### Bước 1: Luôn kéo code mới nhất về trước khi làm
Trước khi thêm/sửa API trên Bruno, hãy mở Terminal và chạy:
```bash
git pull origin main
```

### Bước 2: Tạo và test Endpoint trong thư mục của mình
- Chỉ thêm/chỉnh sửa các request thuộc **thư mục module mình phụ trách**.
- Hạn chế chỉnh sửa vào thư mục của thành viên khác nếu chưa trao đổi trước.

### Bước 3: Đẩy (Push) các Endpoint mới lên GitHub cho nhóm cùng test
Sau khi đã lưu (`Ctrl + S`) và test thành công trên Bruno:

## 7. Một số lưu ý quan trọng cho nhóm
- **Không xóa/sửa nhầm thư mục của người khác**: Mỗi người quản lý thư mục module riêng của mình.
- **Luôn dùng `{{baseurl}}`**: Giúp đồng bộ cấu hình URL trên mọi máy.
- **Lưu dữ liệu mẫu hợp lệ trong Body**: Khi tạo API `POST` / `PUT`, hãy để lại JSON mẫu chuẩn trong mục `Body` để các thành viên làm Frontend hoặc thành viên khác kéo về là có thể bấm **Send** chạy thử ngay.

