# 🔐 PortSwigger Lab: Unprotected Admin Functionality

## 🎯 Objective
Delete the user **carlos** from the admin panel.

<img width="960" height="338" alt="1 lab" src="https://github.com/user-attachments/assets/90429dda-ea79-4bd4-8c66-da3fbc1734f8" />

---

## 🧠 Approach

During reconnaissance, we checked the `/robots.txt` file because it often contains paths that developers don’t want search engines to index. Sometimes, sensitive endpoints like admin panels are accidentally exposed here.

---

## 🪜 Steps to Solve

### 1. Check `/robots.txt`

Navigate to:
```
https://<lab-id>.web-security-academy.net/robots.txt
```

You will see:
```
User-agent: *
Disallow: /administrator-panel
```

<img width="893" height="214" alt="2 robots" src="https://github.com/user-attachments/assets/25ebb6fc-fe21-424e-90b5-debb2443278b" />

---

### 2. Discover Admin Panel

The `Disallow` entry reveals the hidden admin panel path:
```
/administrator-panel
```

---

### 3. Access the Admin Panel

Go to:
```
https://<lab-id>.web-security-academy.net/administrator-panel
```

<img width="1092" height="441" alt="3 admin-panel" src="https://github.com/user-attachments/assets/84cbbeb7-520f-41cb-b59a-433aa653e4db" />

The panel loads without authentication since it is unprotected.

---

### 4. Locate User Management

Inside the admin panel, find the section where users are managed.

---

### 5. Delete User

- Find the user **carlos**
- Click the delete option next to the user

---

## ✅ Result

- The user **carlos** is deleted
- The lab is successfully solved

<img width="752" height="326" alt="4 success" src="https://github.com/user-attachments/assets/e22340a1-d9fd-4ae9-95b2-9d4f6c891af0" />

---

## 📝 Key Takeaways

- `robots.txt` is publicly accessible and should not contain sensitive information  
- Hidden paths are not secure without proper authentication  
- Always protect admin functionality with authorization controls  
