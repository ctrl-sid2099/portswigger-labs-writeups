# 📂 File Path Traversal – Absolute Path Bypass

## 🧠 Lab Description

This lab contains a path traversal vulnerability in the display of product images.

The application attempts to block traversal sequences (`../`) but treats user input as relative to a default working directory.

<img width="947" height="447" alt="1 lab" src="https://github.com/user-attachments/assets/9248671c-fb1b-4185-8c6d-b5fc57564e81" />

---

## 🎯 Objective

Exploit the vulnerability to retrieve the contents of the `/etc/passwd` file.

---

## 🛠️ Steps to Reproduce

1. Navigate to the homepage where product images are loaded.

2. Intercept the request using **Burp Suite**:

   ```
   GET /image?filename=x.jpg
   ```

3. Send the request to **Repeater**.

<img width="778" height="730" alt="2 1 repeater" src="https://github.com/user-attachments/assets/7fa9d5d0-c230-49ea-961f-3fe1d4f6d3f7" />

4. Attempt a standard path traversal payload:

   ```
   GET /image?filename=../../../etc/passwd
   ```

<img width="779" height="753" alt="2 repeater" src="https://github.com/user-attachments/assets/863bfd20-59a5-4e7a-a4b4-3254780e9597" />

---

   * Result: ❌ *No such file* (blocked)

---

<img width="785" height="623" alt="3 normal-error" src="https://github.com/user-attachments/assets/67fd0029-ed2b-4d80-aca3-d9912d08e462" />

5. Try using an absolute path instead:

   ```
   GET /image?filename=/etc/passwd
   ```
   
6. Send the request.

<img width="776" height="753" alt="4 absolute-path" src="https://github.com/user-attachments/assets/16b577ce-3035-46aa-81fb-a7b89f5e2ee9" />

---

## ✅ Result

Received a **200 OK** response containing the contents of the `/etc/passwd` file.

<img width="781" height="624" alt="5 200-Ok" src="https://github.com/user-attachments/assets/d19867fd-fd9f-4e8c-a9ce-f678c1eec534" />

---

<img width="1217" height="641" alt="6 success" src="https://github.com/user-attachments/assets/59eb8fad-d41c-48d7-95e5-db3837eb8647" />

## 💡 Explanation

### 🔎 Why traversal failed

The application blocks common traversal patterns like:

```id="t1a2b3"
../../../etc/passwd
```

This prevents accessing files using relative path traversal.

---

### ⚠️ Why absolute path works

The application still accepts absolute file paths. When we supply:

```id="x9k2lm"
/etc/passwd
```

The server directly accesses the file from the root directory, bypassing the traversal filter.

Since `/etc/passwd` is a sensitive system file on Linux systems, its contents are returned in the response.

---


## 🚨 Key Takeaway

Blocking traversal sequences alone is not sufficient.

To properly prevent path traversal:

* Validate and restrict file paths strictly
* Use allowlists for permitted files
* Avoid directly using user input in file system operations

---
