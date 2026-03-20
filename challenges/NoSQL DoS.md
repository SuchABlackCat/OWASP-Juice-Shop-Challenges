# NoSQL DoS

- **Vulnerability**: Injection
- **Difficult Level**: 4/6
- **Description**: Let the server sleep for some time. (It has done more than enough hard work for you)

---
Endpoint được sử dụng để chèn là request `PUT /rest/products/:id/reviews`

<img width="955" height="511" alt="image" src="https://github.com/user-attachments/assets/7acac444-55cc-4602-a70a-5246303e3e89" />

Với endpoint này, ta có thể chèn theo 2 cách:
- Cách 1: chèn qua request body, phần JSON
- Cách 2: chèn qua URL, cụ thể là :id

Với cách 1:

Ta đổi phần body từ:

```
{
  "message":"a",
  "author":"blackcat@gmail.com"
}
```
sang:
```
{
  "id":"sleep(2000)",
  "message":"test test"
}
```
<img width="957" height="469" alt="image" src="https://github.com/user-attachments/assets/b7a6c01d-6777-476f-af71-9469bde539a4" />

Có vẻ là không được nên ta đổi sang chèn thẳng vào URL:

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/44007f37-41af-489f-9994-10d61141af2c" />

Chỉ cần F5 là giải được challenge.
