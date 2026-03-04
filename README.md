# OWASP-Juice-Shop-Challenges

## How to Run
1. Chạy docker (có thể dùng GUI hoặc chạy bằng CLI) trong PowerShell:
```
docker run --rm -p 3000:3000 docker_name
```
3. Chạy lệnh `docker ps` để xem hiện tại các docker đang chạy có docker mình muốn không
4. Mở web trong `http://localhost:3000`

## Challenge Classification based on OWASP
Top 10:2025 List
- A01:2025 - Broken Access Control
- A02:2025 - Security Misconfiguration
- A03:2025 - Software Supply Chain Failures
- A04:2025 - Cryptographic Failures
- A05:2025 - Injection
- A06:2025 - Insecure Design
- A07:2025 - Authentication Failures
- A08:2025 - Software or Data Integrity Failures
- A09:2025 - Security Logging and Alerting Failures
- A10:2025 - Mishandling of Exceptional Conditions
