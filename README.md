# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session18_Ex02
## 1. Phân tích: Cache vs. Cookies

| Tiêu chí | Cookies (~4KB) | Cache / Web Storage (Vài MB - GB) |
| --- | --- | --- |
| **Dung lượng** | Cực nhỏ, giới hạn dưới ~4KB. | Lớn (từ 5MB đến vài GB). |
| **Cơ chế gửi** | **Tự động gửi kèm** vào mọi HTTP request lên server. | Lưu cục bộ ở client, không tự gửi lên server. |
| **Mục đích** | Lưu Token định danh, duy trì phiên đăng nhập. | Lưu dữ liệu tĩnh, giao diện, giỏ hàng phụ tùng tạm. |

---

## 2. Triển khai: Mẩu tin Cookie bảo mật

Chuyển từ Session Cookie sang Persistent Cookie bằng cách thêm `Max-Age` và gắn cờ bảo mật trong HTTP response header từ phía server:

http
Set-Cookie: session_id=xyz789_token; Max-Age=2592000; Secure; HttpOnly; SameSite=Strict; Path=/



* **`session_id`**: Token định danh shipper.
* **`Max-Age=2592000`**: Lưu cookie trong 30 ngày (Persistent Cookie), đóng trình duyệt hay load lại trang không bị mất.
* **`HttpOnly`**: Cấm JavaScript (`document.cookie`) đọc dữ liệu, chặn đánh cắp Session qua lỗi XSS.
* **`Secure`**: Chỉ gửi cookie qua kết nối HTTPS đã mã hóa.
* **`SameSite=Strict`**: Chặn tấn công giả mạo CSRF từ trang khác.

---

## 3. Kiểm thử: Xóa Cache bằng Hard Reload

Khi giao diện đổi màu nhưng trình duyệt vẫn giữ file CSS/JS cũ trong cache:

1. Mở trang web shipper.
2. Thực hiện phím tắt:
* **Windows/Linux:** Nhấn `Ctrl` + `F5` hoặc `Ctrl` + `Shift` + `R`.
* **macOS:** Nhấn `Cmd` + `Shift` + `R`.


3. Hoặc nhấn **F12** --> chuột phải vào nút **Reload (Tải lại)** --> chọn **Hard Reload (Tải lại cứng)** để buộc trình duyệt lấy giao diện mới nhất từ server.
