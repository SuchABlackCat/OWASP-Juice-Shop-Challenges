<h1>A01 — Broken Access Control</h1>
<h2>1. Khái niệm</h2>
<p>Broken Access Control: lỗi kiểm soát truy cập cho phép user thực hiện hành vi vượt quyền (xem/sửa/xóa dữ liệu, thực hiện chức năng admin…) ngoài phạm vi được phép.</p>
<p>Hậu quả: lộ thông tin nhạy cảm, priviledge escalation, ...</p>
<h2>2. Examples</h2>
<ul>
  <li><b>Parameter tampering / URL modification</b> (thay ID trong URL → truy cập tài khoản người khác — IDOR).</li>
  <li><b>Force browsing</b>: truy cập trực tiếp các đường dẫn admin/ẩn mà không kiểm tra.</li>
  <li><b>Missing access controls on API methods</b>: POST/PUT/DELETE không được bảo vệ.</li>
  <li><b>Metadata / token tampering</b>: sửa JWT, cookie, hidden field để nâng quyền.</li>
  <li><b>CORS misconfiguration</b>: cho phép origin không tin cậy truy cập API.</li>
  <li><b>Directory listing / file exposure</b>: .git, backup file, file sensitive nằm trong webroot.</li>
  <li><b>Replay / session reuse</b>: session không bị invalidate sau logout hoặc JWT quá dài hạn.</li>
</ul>
<h2>3. Testing</h3>
<ul>
  <li>Thử truy cập endpoint cần auth khi chưa login (force browse).</li>
  <li>Thử thay đổi ID/param để xem dữ liệu của user khác (IDOR).</li>
  <li>Kiểm tra HTTP methods (DELETE/PUT) có bị bảo vệ không.</li>
  <li>Fuzz / enumerate đường dẫn ẩn bằng wordlist (ffuf/gobuster).</li>
  <li>Kiểm tra header CORS bằng request có Origin giả mạo.</li>
  <li>Decode &amp; thử tamper JWT; kiểm tra token expiry / revocation.</li>
  <li>Tìm directory listing, file .git, backup files.</li>
  <li>Tìm SUID / permission sai trên VM cho privilege escalation.</li>
</ul>
<h2>4. Best practices</h2>
<ul>
  <li>Kiểm soát phía server: mọi authorization phải enforced server-side (không dựa vào UI).</li>
  <li>Deny-by-default: mặc định từ chối, chỉ cấp quyền rõ ràng.</li>
  <li>Enforce record ownership: ví dụ WHERE owner_id = current_user_id.</li>
  <li>Áp ACL cho tất cả method/endpoint, bao gồm API.</li>
  <li>Token/session management: JWT ngắn hạn, refresh + revocation; invalidate session sau logout.</li>
  <li>Giảm thiểu CORS, whitelist origins; không sử dụng *.</li>
  <li>Xóa file nhạy cảm khỏi webroot; tắt directory listing.</li>
  <li>Logging &amp; alerting cho failed access attempts; rate-limit endpoints.</li>
  <li>Unit/integration tests cho access control logic, đưa vào CI.</li>
</ul>
