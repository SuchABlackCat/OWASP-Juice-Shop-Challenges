# Login Admin

- **Vulnerability**: Injection
- **Difficult Level**: 2/6
- **Description**: Log in with the administrator's user account.

---

Đang lướt review thì mình đột nhiên thấy username của admin =)))))

<img width="374" height="426" alt="image" src="https://github.com/user-attachments/assets/e57b56bf-1acb-436f-84a9-a8afc8e620eb" />

Ta vào login form và bắt request bằng Burp Suite:

<img width="1029" height="501" alt="image" src="https://github.com/user-attachments/assets/dbb30f76-1e93-4fce-8d52-bf708504cc05" />

Chèn payload vào phần email là
```
admin@juice-sh.op'--
```

<img width="1029" height="528" alt="image" src="https://github.com/user-attachments/assets/b44b3ba4-09c1-4f97-9892-defd94868bea" />

Gửi request đi là xong challenge!
