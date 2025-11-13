<h1>View Basket</h1>
<p>Difficulty: 2/6</p>
<h2>Cách 1: Không dùng Burp Suite</h2>
<p>Tạo một account bình thường.</p>
<p>Sau khi tạo account, nhấn chuột phải để chọn <b>Inspect</b> -> Chọn tab <b>Network</b>. Nhớ reload lại page để nó hiện lên những request.</p>
<img width="1366" height="734" alt="image" src="https://github.com/user-attachments/assets/6bfd788b-5420-4dd7-8d9e-b5be041677ca" />
<p>Nhấn thử vào 1 request có tên khá lạ là "9". Ta thấy có đoạn code JSON miểu tả đúng những sản phẩm đang có trong basket của mình nên đây là request để xem basket.</p>
<img width="1366" height="734" alt="image" src="https://github.com/user-attachments/assets/f5302c77-5f1b-43ec-bdda-83006c3d6aea" />
<p>Vẫn request đó, chuyển sang tab <b>Header</b></p>
<img width="693" height="291" alt="image" src="https://github.com/user-attachments/assets/05a51b55-ee67-45fe-9665-f4e70ceb949c" />
<p>Ta thấy request URL là ".../request/basket/6". Rất có thể ID của mình là 6. Mình sẽ thử thay đổi thành 1,2,3 xem.</p>
<p>Nhấn chuột phải vào request ở tab <b>Name</b>. Chọn <b>Edit and Resend</b></p>
<p>Sửa số 6 thành 1 để xem thử và xem lại response xem có view được basket không.</p>

<h2>Cách 2: Dùng Burp Suite</h2>
<h3>Bước 1 — Mở Burp và bật Intercept</h3>
  <ol>
    <li>Khởi động Burp Suite. Chuyển sang tab <strong>Proxy</strong>.</li>
    <li>Bật chế độ bắt request bằng cách click <strong>Intercept on</strong>. Khi bật, Burp sẽ giữ các HTTP request từ browser lại để mình xem trước khi gửi lên server.</li>
    <li>Mở browser của Burp, truy cập <code>http://localhost:3000</code>. Khi browser gửi request, Burp sẽ giữ lại chúng.</li>
  </ol>
<h3>Bước 2 — Tạo tài khoản và sinh request “View Basket”</h3>
  <ol>
    <li>Tạo một account mới trong Juice Shop.</li>
    <li>Đăng nhập bằng account đó, và click vào mục <strong>Your basket</strong> đồng nghĩa đang tạo 1 request xem basket.</li>
    <li>Khi đó, request này sẽ bị bắt lại bởi Burp.</li>
  </ol>
<h3>Bước 3 — Gửi request tới Repeater để sửa</h3>
  <ol>
    <li>Chọn request có dạng là <code>GET /rest/basket/...</code>.</li>
    <li>Chuột phải vào request → chọn <strong>Send to Repeater</strong>. Tab Repeater cho phép chỉnh sửa request và gửi lại nhiều lần để so sánh kết quả.</li>
  </ol>
  <figure>
    <img width="1358" height="732" alt="image" src="https://github.com/user-attachments/assets/cb719127-58ce-4a7a-a7ff-d8ba94af04a8" />
    <figcaption>Hình minh họa: request được gửi tới Repeater</figcaption>
  </figure>

<h3>Bước 4 — Thử thay đổi ID</h3>
  <p> Quan sát đường dẫn <code>GET /rest/basket/7</code>, ta thấy id của mình = 7, vì vậy rất có thể những người khác cũng có id là 1,2,3...</p>
  <ol>
    <li>Trong Repeater, sửa phần path từ <code>/rest/basket/7</code> thành <code>/rest/basket/1</code> (hoặc các id khác).</li>
    <li>Giữ nguyên phần còn lại vì mình đang giả định attacker giữ được token của mình nhưng muốn truy cập giỏ hàng của user khác.</li>
    <li>Nhấn <strong>Send</strong> và quan sát HTTP response.</li>
  </ol>
  <h4>Đây là giỏ hàng của mình (before)</h4>
  <figure>
    <img width="1017" height="503" alt="image" src="https://github.com/user-attachments/assets/1356baa6-0e2a-4a7a-b8c0-97b5dabfee24" />
    <figcaption>Basket của user hiện tại (id = 7)</figcaption>
  </figure>
  <h4>Thử với <code>/rest/basket/1</code></h4>
  <figure>
    <img width="1024" height="508" alt="image" src="https://github.com/user-attachments/assets/955574da-e4bb-45f5-9aa6-588a5d9f5f9f" />
    <figcaption>Kết quả response sau khi thay id sang 1. Ta có thấy những order khác với chúng ta ban đầu -> Đã thành công xem order người khác.</figcaption>
  </figure>
  
