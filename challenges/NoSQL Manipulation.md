# NoSQL Manipulation

- **Vulnerability**: Injection
- **Difficult Level**: 4/6
- **Description**: Update multiple product reviews at the same time.

---

Mở lab trong browser Burp Suite.

Login bằng tài khoản cá nhân.

Ấn vào 1 sản phẩm, bình luận và bắt request `PUT /products/x/reviews`:

<img width="1010" height="488" alt="image" src="https://github.com/user-attachments/assets/44d932a2-0e8f-473f-bf05-c238c83b673f" />

Gửi vào Repeater.

<img width="1317" height="511" alt="image" src="https://github.com/user-attachments/assets/87ff5403-9251-48ee-a757-23011180ffa9" />

Trong phần `Inspector` -> `Request attribute`, thay đổi HTTP method từ PUT sang PATCH.

<img width="361" height="513" alt="image" src="https://github.com/user-attachments/assets/23a65c87-7454-4377-b461-9ca79b07054e" />

Trong nội dung request, đổi toàn bộ từ:
```
{
  "message":"a",
  "author":"blackcat@gmail.com"
}
```
sang
```
{
  "id": {
    "$ne": -1
  },
  "message": "Have a nice day hehehe!"
}
```

Trong MongoDB, `{ id: { $ne: -1 } }` = `id NOT EQUAL -1`, tức ta đang dùng id của mọi người trong DB để review, nên mới có thể review nhiều lần.

<img width="955" height="470" alt="image" src="https://github.com/user-attachments/assets/0b556197-5ffe-4a84-a1af-ff098a8805dc" />

Nếu nó có lỗi đường dẫn, hãy đổi từ `PATCH /rest/products/6/reviews` sang `PATCH /rest/products/reviews`.

<img width="960" height="470" alt="image" src="https://github.com/user-attachments/assets/e2379fbc-502d-4f83-a29b-43f292bdbfdf" />

