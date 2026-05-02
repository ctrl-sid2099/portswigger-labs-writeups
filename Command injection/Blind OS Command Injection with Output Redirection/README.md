# 🧪 Lab: Blind OS Command Injection with Output Redirection

## 🎯 Objective

Exploit a blind OS command injection vulnerability to execute the `whoami` command and retrieve its output by redirecting it to a readable file.

<img width="1002" height="623" alt="1 lab" src="https://github.com/user-attachments/assets/8965025f-6022-430c-9178-7fb1ce354fde" />

---

## 🔍 Lab Overview

* The application executes system commands using user input.
* Output is **not directly visible** (blind injection).
* Writable directory available:

  ```
  /var/www/images/
  ```
* This directory is publicly accessible via the web.

👉 **Idea:** Redirect command output to a file in this directory, then retrieve it through the browser.

---

## 🚶 Step-by-Step Solution

### 📝 1. Locate the Injection Point

* Navigate to:

  ```
  Submit Feedback
  ```

* Fill in random values and submit while intercepting the request.

<img width="844" height="709" alt="2 submit-feedback" src="https://github.com/user-attachments/assets/db81e64c-32e7-4328-9a20-9e78cad8f5d8" />

* Captured request:

  ```
  POST /feedback/submit HTTP/1.1
  ```

---

### 🕵️ 2. Send Request to Repeater

* Send the request to **Repeater** in Burp Suite.
* This allows controlled testing of payloads.

<img width="882" height="724" alt="3 intercept-repeater" src="https://github.com/user-attachments/assets/5f97526f-da89-44fa-8c27-244529148576" />

---

### ⏱️ 3. Confirm Blind Command Injection (Time-Based)

* Modify the `email` parameter:

  ```
  email=hello123%40gmail.com+||+sleep+5s+||+
  ```

* Send the request.

* Observe:

  ```
  ~5000 ms response delay
  ```
  
<img width="1599" height="748" alt="4 checking-blind-os" src="https://github.com/user-attachments/assets/1e662331-2902-4e2e-8c71-f03f461dff48" />


👉 **Why this works?**

* `||` allows command chaining.
* `sleep 5` pauses execution for 5 seconds.
* Delay confirms **command execution on server**.

---

### 💡 4. Prepare Output Redirection Payload

* Since output is not visible, redirect it to a file:

  ```
  /var/www/images/output.txt
  ```

---

### ⚙️ 5. Execute `whoami` with Redirection

* Modify payload:

  ```
  email=hello123%40gmail.com+||+whoami>/var/www/images/output.txt+||+
  ```

* Send the request.

<img width="904" height="717" alt="5 modify" src="https://github.com/user-attachments/assets/69d6da09-ef93-4ebb-90f8-7e59908e270d" />

* Response:

  ```
  HTTP/2 200 OK
  ```

<img width="716" height="753" alt="6 200-ok-response" src="https://github.com/user-attachments/assets/7251e678-1a71-4129-a8db-0964a04256a2" />


👉 **Why this works?**

* `whoami` executes on server
* `>` redirects output into a file
* File is saved in a **web-accessible directory**

---

### 🖼️ 6. Find Image Retrieval Endpoint

* Go to any product page.
* Intercept an image request:

  ```
  GET /image?filename=x.jpg
  ```

👉 This endpoint loads files from `/var/www/images/`.

<img width="777" height="717" alt="7 jpg-request" src="https://github.com/user-attachments/assets/3c249210-ad79-4668-ab3c-4aa4b62a9faf" />

* Send request to Repeater and modify:

  ```
  GET /image?filename=output.txt
  ```
  
---

### 🔁 7. Retrieve Command Output

* Send the request.

<img width="779" height="749" alt="8 modify-request" src="https://github.com/user-attachments/assets/efebf4e9-0d11-489b-ba93-f6389d5618ed" />

---

### 📄 8. Analyze Response

* Response:

  ```
  HTTP/2 200 OK
  ```

* Output:

  ```
  peter-Ked7fv
  ```

<img width="822" height="698" alt="9 user-name" src="https://github.com/user-attachments/assets/8a2d0a4d-a627-42bd-9b4b-8c33d7222126" />

👉 This is the result of the `whoami` command.

---

### ✅ 9. Lab Solved

* Successfully executed a command.
* Retrieved output via file redirection.
* Lab is complete.

<img width="908" height="712" alt="10 success" src="https://github.com/user-attachments/assets/88b6fbbc-34fe-4142-b570-1f8a9f66fec4" />

---

## 💡 Key Takeaways

* 🚨 Blind OS command injection can still be exploited without direct output
* ⏱️ Time-based payloads help confirm vulnerabilities
* 📁 Writable directories can be abused for data exfiltration
* 🔓 Never pass user input directly into system commands

---

## 🧠 Why This Attack Works

* The application:

  * Executes system commands using unsanitized input
  * Does not return output (blind)
  * Allows writing to a publicly accessible directory

* Attacker strategy:

  1. Confirm execution via delay
  2. Redirect output to file
  3. Retrieve file via HTTP

---
