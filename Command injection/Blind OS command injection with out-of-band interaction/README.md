# 🧪 Lab Write-up: Blind OS Command Injection with Out-of-Band Interaction

## 📌 Lab Overview

This lab demonstrates a **blind OS command injection vulnerability** in a web application's feedback functionality.

* The application executes a shell command using user-supplied input.
* The command runs **asynchronously**, meaning no output is returned in the HTTP response.
* Direct output retrieval is not possible.
* However, **out-of-band (OOB) techniques** like DNS lookups can be used to detect successful exploitation.

<img width="946" height="427" alt="1 lab" src="https://github.com/user-attachments/assets/49d9eced-c81f-4c12-84b3-655aab362838" />

---

## 🎯 Objective

Exploit the vulnerability to trigger a **DNS lookup** to an external domain (e.g., Burp Collaborator), confirming command execution.

---

## 🛠️ Tools Required

* Burp Suite Professional ( For Collaborator features )

<img width="860" height="298" alt="image" src="https://github.com/user-attachments/assets/c5d7e7df-f555-4577-858e-d3b2baba637c" />

---

## 🚶 Step-by-Step Exploitation

### 1. Navigate to Feedback Form

* Open the lab web application.
* Go to the **"Submit Feedback"** section.

---

### 2. Fill in the Form

Enter any random values:

```
Name: test
Email: test@test.com
Subject: test
Message: test
```

<img width="873" height="714" alt="2 submit-feedback" src="https://github.com/user-attachments/assets/9050cc3b-8501-493f-b6ac-fcf7b9e47d7a" />

---

### 3. Intercept the Request

* Enable **intercept** in Burp Suite.
* Submit the form.
* Capture the request:

```
POST /feedback/submit HTTP/1.1
```

---

### 4. Send to Repeater

* Right-click the intercepted request.
* Select **"Send to Repeater"**.

<img width="781" height="722" alt="3 repeater" src="https://github.com/user-attachments/assets/3837fb34-ccd9-4465-abad-669ca7433abd" />

---

### 5. Inject Payload into Email Parameter

Modify the `email` parameter to inject a command:

```
email=test@test.com'||nslookup+<your-collaborator-payload>||'
```

👉 Replace `<your-collaborator-payload>` with the domain generated from:

* Right-click → **Insert Collaborator payload**

---

### 6. Send the Request

* Click **Send** in Repeater.

<img width="1173" height="754" alt="4 send-payload" src="https://github.com/user-attachments/assets/e25472b5-99c0-434a-bcb7-6fb897658c5c" />

---

### 7. Check Burp Collaborator

* Go to the **Collaborator tab**.
* Click **Poll now**.

✅ If successful, you will see a **DNS interaction** from the target server.

<img width="1246" height="657" alt="5 response" src="https://github.com/user-attachments/assets/453ca733-319b-4ca5-8674-2e09ded7db6a" />

---

## 🧠 Explanation

* The payload uses shell operators:

  * `||` → Executes the next command if the previous fails
* `nslookup` triggers a DNS request to your Collaborator domain.
* Since the server executes commands in the background, this DNS request confirms code execution.

---

## ✅ Result

* A DNS lookup is observed in Burp Collaborator.
* This confirms successful **blind OS command injection**.
* 🎉 Lab solved!

<img width="767" height="568" alt="6 success" src="https://github.com/user-attachments/assets/2089e421-89e6-41df-8975-4a15c0d3f996" />

---

## 🔐 Key Takeaways

* Blind command injection requires indirect verification.
* Out-of-band channels (DNS/HTTP) are powerful for detection.
* Always sanitize and validate user input on the server side.


