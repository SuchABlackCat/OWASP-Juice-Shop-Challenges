# Christmas Special

- **Vulnerability**: Injection
- **Difficult Level**: 4/6
- **Description**: Order the Christmas special offer of 2014.

---
Thử inject payload `1'` thì ta thấy server trả về cả câu SQL:
```
SELECT * FROM Products WHERE ((name LIKE '%1'%' OR description LIKE '%1'%') AND deletedAt IS NULL) ORDER BY name
```
<img width="1026" height="527" alt="image" src="https://github.com/user-attachments/assets/16605dc5-3386-4512-b054-ac67dec52b80" />


Vậy muốn trả về mọi sản phẩm ta có thể thử payload `1')) or 1=1--`. Sau khi request trả về ta thấy name của Christmas Special là `Christmas Super-Surprise-Box (2014 Edition)`

<img width="1026" height="526" alt="image" src="https://github.com/user-attachments/assets/e98010e2-a4c5-496d-b4f6-09319459da01" />

Vậy ta sẽ lấy 1 request order để inject (note: về trang chủ, chọn product thứ 2 thì mới có request POST order). Sửa `id=10` (id của Christmas Special Box):

<img width="1028" height="548" alt="image" src="https://github.com/user-attachments/assets/ff2faf8d-3c8c-4a8c-bab1-16df4377f395" />

Gửi request. Sau đó vào Your basket, thực hiện các bước mua hàng là xong challenge.
