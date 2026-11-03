# Bài 1: Sự cố không truy cập được hệ thống quản trị

## 2. Yêu cầu

### Chức năng 1 (Phân tích mạng)

Giải thích chi tiết tiến trình DNS Lookup từ khi Nam gõ tên miền đến khi trình duyệt tìm thấy địa chỉ IP máy chủ.

- **DNS Lookup:** trình duyệt yêu cầu DNS tra cứu IP của trang web.
- **TCP / TLS Handshake:** trình duyệt kết nối thành công tới IP của trang web.
- **HTTP Request:** trình duyệt sẽ gửi yêu cầu lấy nội dung từ trang web.
- **Browser Render:** trình duyệt nhận dữ liệu tải về, phân tích HTML và CSS để làm cây DOM và CSSOM, sau đó tính toán bố cục Layout và vẽ các ảnh lên màn hình để người dùng nhìn thấy giao diện hoàn chỉnh.

### Chức năng 2 (Chẩn đoán lỗi)

Phân biệt bản chất giữa lỗi DNS và lỗi 500 Internal Server Error để xác định lỗi này nằm ở phía Client hay Server.

- **Lỗi DNS:** là do cá nhân nhập sai địa chỉ IP hay đường dẫn không tồn tại dẫn đến lỗi.
- **Lỗi 500 Internal Server Error:** là do chương trình bên trong máy chủ bị sập hoặc quá tải nên mặc dù vẫn đúng địa chỉ IP nhưng vẫn không vào được trang web.

## 3. Ràng buộc

- Nam đang gõ nhầm địa chỉ thành `phong-dieu-phoi.grenlog.vn` (thiếu một chữ 'e'). Hãy chỉ ra hậu quả của việc sai lệch tên miền trong "cuốn danh bạ Internet".
- Việc gõ nhầm địa chỉ dẫn đến việc DNS không dịch ra đúng với địa chỉ IP của trang web dẫn đến lỗi 404 Not Found.

## Phần 1. Phân tích

Xác định Input (Tên miền) / Output (IP address) và mô tả sơ đồ luồng dữ liệu của DNS.

- **Input:** nhập tên miền.
- DNS nhận được tên miền và dịch ra địa chỉ IP của tên miền so với trang web.
- **Output:**
  - **Thành công:** trả về địa chỉ IP tương ứng. Sau đó trình duyệt sẽ dùng IP này để kết nối với web.
  - **Thất bại:** trả về thông báo không tìm thấy tên miền, không phân giải được ra IP.

## Phần 2. Triển khai

Đề xuất quy trình kiểm tra mã trạng thái HTTP Status Code để xác nhận xem yêu cầu đã chạm tới máy chủ hay chưa.

1. Nhấn **F12**.
2. Chọn tab **Network**.
3. Chọn tên miền và sẽ ra khung **Headers**.
4. Kéo xuống xem phần **Status Code**: `200 OK`, `404 Not Found`, `500 Internal Server Error`.

## Phần 3. Kiểm thử

Giả định Nam sửa đúng tên miền nhưng vẫn nhận mã 404 Not Found, hệ thống đang gặp vấn đề gì?

- Có thể người dùng đã xóa đi trang Web nên mặc dù đúng IP nhưng vẫn hiện lỗi.
- Nam gõ tên miền thì đúng nhưng có thể gõ sai phần phía sau hoặc thiếu dấu.
