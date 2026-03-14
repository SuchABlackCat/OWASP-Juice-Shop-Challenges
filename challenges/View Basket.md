# View Basket

- **Vulnerability**: Broken Access Control
- **Difficult Level**: 2/6
- **Description**: 
---
## Cách 1: Không dùng Burp Suite
Tạo một account bình thường.

Sau khi tạo account, nhấn chuột phải để chọn `Inspect` -> Chọn tab `Network`. Nhớ reload lại page để nó hiện lên những request.

<img width="1366" height="734" alt="image" src="https://github.com/user-attachments/assets/6bfd788b-5420-4dd7-8d9e-b5be041677ca" />

Nhấn thử vào 1 request có tên khá lạ là "9". Ta thấy có đoạn code JSON miểu tả đúng những sản phẩm đang có trong basket của mình nên đây là request để xem basket.

<img width="1366" height="734" alt="image" src="https://github.com/user-attachments/assets/f5302c77-5f1b-43ec-bdda-83006c3d6aea" />

Vẫn request đó, chuyển sang tab `Header`.

<img width="693" height="291" alt="image" src="https://github.com/user-attachments/assets/05a51b55-ee67-45fe-9665-f4e70ceb949c" />

Ta thấy request URL là `.../request/basket/6`. Rất có thể ID của mình là 6. Mình sẽ thử thay đổi thành 1,2,3 xem.

Nhấn chuột phải vào request ở tab `Name`. Chọn `Edit and Resend`.

Sửa số 6 thành 1 để xem thử và xem lại response xem có view được basket không.

## Cách 2: Dùng Burp Suite
### Bước 1 — Mở Burp và bật Intercept
- Khởi động Burp Suite. Chuyển sang tab `Proxy`.
- Bật chế độ bắt request bằng cách click <strong>Intercept on</strong>. Khi bật, Burp sẽ giữ các HTTP request từ browser lại để mình xem trước khi gửi lên server.
- Mở browser của Burp, truy cập `http://localhost:3000`. Khi browser gửi request, Burp sẽ giữ lại chúng.

### Bước 2 — Tạo tài khoản và sinh request "View Basket"
- Tạo một account mới trong Juice Shop.
- Đăng nhập bằng account đó, và click vào mục **Your basket** đồng nghĩa đang tạo 1 request xem basket.
- Khi đó, request này sẽ bị bắt lại bởi Burp.

### Bước 3 — Gửi request tới Repeater để sửa
- Chọn request có dạng là `GET /rest/basket/...`.
- Chuột phải vào request → chọn `Send to Repeater`. Tab Repeater cho phép chỉnh sửa request và gửi lại nhiều lần để so sánh kết quả.

<img width="1358" height="732" alt="image" src="https://github.com/user-attachments/assets/cb719127-58ce-4a7a-a7ff-d8ba94af04a8" />

### Bước 4 — Thử thay đổi ID
Quan sát đường dẫn `GET /rest/basket/7`, ta thấy id của mình = 7, vì vậy rất có thể những người khác cũng có id là 1,2,3...

Trong Repeater, sửa phần path từ `/rest/basket/7` thành `/rest/basket/1 (hoặc các id khác).

Giữ nguyên phần còn lại vì mình đang giả định attacker giữ được token của mình nhưng muốn truy cập giỏ hàng của user khác.

Nhấn **Send** và quan sát HTTP response.

Đây là giỏ hàng của mình (before):
<img width="1017" height="503" alt="image" src="https://github.com/user-attachments/assets/1356baa6-0e2a-4a7a-b8c0-97b5dabfee24" />

Thử với `/rest/basket/1`
<img width="1024" height="508" alt="image" src="https://github.com/user-attachments/assets/955574da-e4bb-45f5-9aa6-588a5d9f5f9f" />
