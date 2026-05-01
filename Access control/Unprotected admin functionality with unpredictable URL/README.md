# 🔐 PortSwigger Lab: Unprotected Admin Functionality with Unpredictable URL

## 🎯 Objective
Access the admin panel and delete the user **carlos**.

<img width="960" height="412" alt="1 lab" src="https://github.com/user-attachments/assets/8223c65c-0450-49c2-9767-3ee4db58bc3b" />

---

## 🧠 Approach

Even if an admin panel is not directly visible, it may still be exposed somewhere in the application's frontend code.

- Developers sometimes leave sensitive endpoints inside JavaScript files or page source.
- These endpoints may be intended for admin users but are still accessible to everyone.
- Therefore, analyzing the **page source or HTTP responses** is a key step.

---

## 🪜 Steps to Solve

### 1. Intercept the Home Page Request

- Open the lab and intercept the request using Burp Suite.
- Capture the request:
```
GET / HTTP/1.1
```

- Send this request to **Repeater**.

<img width="709" height="672" alt="2 intercept-home-page" src="https://github.com/user-attachments/assets/e234259c-2d3f-43bf-87a3-643777466c24" />

---

### 2. Analyze the Response

- In Repeater, send the request and inspect the response.
- Look carefully through the HTML/JavaScript.

You will find a hidden path like:
```
/admin-krtr68
```

<img width="1599" height="751" alt="3 secret-panel" src="https://github.com/user-attachments/assets/c6ab3904-2999-4ba3-acbf-9c3d1cf371cf" />

---

### 3. Understand the Finding

- This path appears to be an admin panel endpoint.
- It is likely intended to be accessed only by authenticated admin users.
- However, due to poor security, it is exposed in the response and accessible to anyone.

---

### 4. Access the Admin Panel

Navigate to:
```
https://<lab-id>.web-security-academy.net/admin-krtr68
```

- The admin panel loads without authentication.

<img width="892" height="390" alt="4 admin-panel" src="https://github.com/user-attachments/assets/349b8541-52f2-4fc7-8bed-c03c20e8b61e" />

---

### 5. Delete the User

- Locate the user management section.
- Find the user **carlos**.
- Click the delete option next to the user.

---

## ✅ Result

- The user **carlos** is deleted  
- The lab is successfully solved  

<img width="728" height="350" alt="5 success" src="https://github.com/user-attachments/assets/0c2db9a2-e46f-4bcb-84dc-2f969195175a" />

---

## 📝 Key Takeaways

- Sensitive endpoints should never be exposed in frontend code  
- Obscure or "unpredictable" URLs are not secure by themselves  
- Always enforce proper authentication and authorization checks  
- Inspecting HTTP responses can reveal hidden functionality  
