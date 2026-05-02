# 🧪 Lab: User Role Can Be Modified in User Profile

## 🎯 Objective

Access the admin panel and delete the user **carlos** by exploiting improper role control in the user profile functionality.

<img width="954" height="384" alt="1 lab" src="https://github.com/user-attachments/assets/821b783d-0235-4698-b65d-d4ef1b90453a" />

---

## 🚶 Step-by-Step Solution

### 🔐 1. Login as a Normal User

* Log in using:

  ```
  wiener:peter
  ```

<img width="735" height="448" alt="2 normal-user" src="https://github.com/user-attachments/assets/268fa7a7-c728-41ec-b774-2abbad4150a1" />

---

### 🚫 2. Attempt to Access Admin Panel

* Navigate to:

  ```
  /admin
  ```
* You will see:

  ```
  "Admin interface only available if logged in as an administrator"
  ```

<img width="751" height="370" alt="3 admin=false" src="https://github.com/user-attachments/assets/0008fa30-0fec-4fd0-bee5-3e18816ff25a" />

👉 **Why?**
The application restricts access based on a `roleid`, where:

* `roleid=1` → normal user
* `roleid=2` → administrator

---

### 🕵️ 3. Initial Interception (No Useful Data)

* Intercept the `/admin` request using Burp Suite.
* You won’t find anything useful here.

👉 **Why?**
Because the role is **not controlled via cookies or URL parameters** in this lab.

---

### 🧾 4. Explore "My Account" Functionality

* Go to:

  ```
  My Account
  ```

* Update the email to:

  ```
  hello123@gmail.com
  ```

<img width="831" height="425" alt="4 1 email-update" src="https://github.com/user-attachments/assets/b1bfa4db-a519-465f-b5b8-793175897682" />

* Intercept the request:

  ```
  POST /my-account/change-email HTTP/2
  ```

---

### 🔁 5. Send Request to Repeater

* Right-click → **Send to Repeater**
* Turn **Intercept OFF**
* Send the request in Repeater

<img width="589" height="725" alt="4 repeater" src="https://github.com/user-attachments/assets/417b4bd6-195e-408b-adb1-4363c32f9a20" />

---

### 🔍 6. Analyze the Response

* Response:

  ```
  HTTP/2 302 Found
  ```

* You will notice JSON-like data:

  ```json
  {
    "username": "wiener",
    "email": "hello123@gmail.com",
    "apikey": "xyz",
    "roleid": 1
  }
  ```

<img width="1174" height="753" alt="5 response" src="https://github.com/user-attachments/assets/d715096f-7614-4236-9100-8d4afbbe62f6" />

👉 **Why is this important?**

* The server exposes `roleid` in the response.
* This suggests the role is being handled in a **modifiable request body**, not securely enforced server-side.

---

### 🧠 7. Exploit the Vulnerability

* Go back to the request in Repeater:

  ```
  POST /my-account/change-email HTTP/2
  ```

* Modify the JSON body by adding:

  ```json
  "roleid": 2
  ```

* Final request body example:

  ```json
  {
    "email": "hello123@gmail.com",
    "roleid": 2
  }
  ```

* Send the request.

<img width="587" height="722" alt="6 role-id=2" src="https://github.com/user-attachments/assets/3d01db3c-75e5-48b7-bc40-dcc6a0b815c9" />

---

### 🚀 8. Privilege Escalation Successful

* Response:

  ```
  HTTP/2 302 Found
  ```

<img width="593" height="625" alt="7 response-302" src="https://github.com/user-attachments/assets/bec6be96-394a-42db-b284-c8423fc79491" />

👉 **Why this works?**

* The server **blindly trusts client-supplied input**.
* It updates your role to admin without validation.

---

### 🧩 9. Access Admin Panel

* Go back to browser → **My Account**

* Refresh the page

* You will now see access to:

  ```
  Admin panel
  ```

<img width="1248" height="513" alt="8 admin-panel" src="https://github.com/user-attachments/assets/ea109efa-4266-47f9-9a05-5e5c760c5230" />

* Open it to view:

  * User list (carlos, wiener)
  * Delete functionality

<img width="701" height="324" alt="9 user-panel" src="https://github.com/user-attachments/assets/ad135a54-e21e-4de0-b08b-8bfe6c229300" />

---

### 🗑️ 10. Delete User "carlos"

* Click:

  ```
  Delete carlos
  ```

👉 No interception needed this time since you already have admin privileges.

---

### ✅ 11. Lab Solved

* The user **carlos** is deleted.
* The lab is complete.

<img width="614" height="338" alt="10 success" src="https://github.com/user-attachments/assets/d3938b11-3335-489b-90c2-20419827b79d" />

---

## 💡 Key Takeaways

* 🚨 **Never trust client-side input for authorization**
* 🚨 Sensitive fields like `roleid` must not be user-controllable
* ✅ Always enforce roles on the **server side**
* 🔒 Hidden fields ≠ secure fields

---

## 🧠 Why This Attack Works

* The application:

  * Exposes internal user data (`roleid`)
  * Accepts additional parameters without validation
  * This creates an **Insecure Direct Object Modification / Privilege Escalation** vulnerability

---
