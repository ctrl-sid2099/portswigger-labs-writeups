# 🧪 Lab: User Role Controlled by Request Parameter

## 🎯 Objective

Access the admin panel and delete the user **carlos** by exploiting a role-based access control flaw using a forgeable cookie.

<img width="973" height="373" alt="1 lab" src="https://github.com/user-attachments/assets/01a15327-359e-43f0-ab8b-bdc8709b5630" />

---

## 🚶 Step-by-Step Solution

### 🔐 1. Login as a Normal User

* Log in using the provided credentials:

  ```
  wiener:peter
  ```

<img width="824" height="440" alt="2 1 wiener" src="https://github.com/user-attachments/assets/7c958e12-0629-4de2-a3b3-871e6acb0256" />

---

### 🚫 2. Attempt to Access Admin Panel

* Manually navigate to:

  ```
  /admin
  ```
* You will receive the message:

  ```
  "Admin interface only available if logged in as an administrator"
  ```

<img width="1101" height="420" alt="2 admin-restrict" src="https://github.com/user-attachments/assets/5994257d-69fb-479b-bf83-e97f8d152333" />

---

### 🕵️ 3. Intercept the Request in Burp Suite

* Turn **Intercept ON** in Burp Suite.

* Reload or revisit:

  ```
  /admin
  ```

* Capture the request:

  ```
  GET /admin HTTP/2
  ```

* Observe the **Cookie header**, which contains:

  ```
  Admin=false
  ```

---

### 🧠 4. Understand the Vulnerability

* The application determines admin privileges using a client-side cookie:

  ```
  Admin=false
  ```
* This is insecure because:

  * It is **fully controllable by the user**
  * There is **no server-side validation**
* This allows privilege escalation by modifying the cookie.

---

### 🚀 5. Escalate Privileges

* Modify the cookie in the intercepted request:

  ```
  Admin=true
  ```
* Forward the request.

<img width="1170" height="751" alt="3 admin=true-panel" src="https://github.com/user-attachments/assets/765dbd5e-be56-4929-be1d-8de87d48bde4" />

---

### 🧩 6. Access Admin Panel

* Return to the browser.
* You will now see the **Admin panel**, which includes:

  * User listing (e.g., *carlos*, *wiener*)
  * Delete functionality

<img width="773" height="349" alt="4 user-panel" src="https://github.com/user-attachments/assets/4ebfcff5-fc2e-408d-b188-3c9cb7135d3b" />

---

### 🗑️ 7. Delete User "carlos"

* Click on:

  ```
  Delete carlos
  ```

* Intercept the request in Burp:

  ```
  GET /admin/delete?username=carlos HTTP/2
  ```

* Again, observe:

  ```
  Admin=false
  ```

---

### ⚙️ 8. Modify and Forward the Delete Request

* Change:

  ```
  Admin=false → Admin=true
  ```
* Forward the request.

<img width="1171" height="751" alt="5 delete-carlos=true" src="https://github.com/user-attachments/assets/e21fc30a-7c2e-4c85-9708-60b5620567d6" />

---

### ✅ 9. Lab Solved

* The user **carlos** is successfully deleted.
* The lab is now complete.

<img width="707" height="385" alt="6 refresh-success" src="https://github.com/user-attachments/assets/da4b9cd0-4f6a-4d06-b910-9d5683189fca" />

---

## 💡 Key Takeaway

* **Never trust client-side data for authorization decisions.**
* Role checks must always be enforced on the **server side**, not via modifiable cookies.

---
