# 🧪 Lab Write-Up: Stored XSS into HTML Context with Nothing Encoded

## 📌 Lab Overview

This lab demonstrates a **Stored Cross-Site Scripting (XSS)** vulnerability in a blog’s comment functionality. User input is stored on the server and later rendered in the page **without any encoding or sanitization**.

<img width="940" height="372" alt="1 lab" src="https://github.com/user-attachments/assets/776b7f52-29fa-4791-a9db-b24d6bc351eb" />

### 🎯 Objective

* Inject a payload into the comment field.
* Ensure the payload executes JavaScript (`alert()`) when the blog post is viewed.

---

## 🔍 Step 1: Testing Input Reflection

We first navigated to a blog post and located the **comment section**.

### 🧪 Test Payload:

```html
<"/Hello">
```

<img width="738" height="686" alt="2 comment" src="https://github.com/user-attachments/assets/3353209a-c926-49f6-b2f6-923e797ef3a9" />

### 📊 Observation:

* The comment appeared **exactly as entered** in the comment section.

### 💡 Why We Did This:

This step helps confirm:

* Whether user input is **stored and displayed back (stored behavior)**.
* Whether any **filtering or encoding** is applied.

Since the input was rendered as-is, it strongly suggested that the application is vulnerable to XSS.

---

## 🚀 Step 2: Injecting the XSS Payload

We then submitted a malicious script:

### 🧪 Payload:

```html
<script>alert("XSS");</script>
```

<img width="740" height="773" alt="3 payload" src="https://github.com/user-attachments/assets/21064b63-0369-44ae-b54c-41a0cc312363" />

### 📊 Result:

* The lab was marked as **solved**.

<img width="989" height="330" alt="4 solved" src="https://github.com/user-attachments/assets/002576f3-3c7c-4ecd-b4d7-129fa3b8830b" />

---

## ❗ If you revisit the Blog Post again

<img width="1026" height="349" alt="5 popUp" src="https://github.com/user-attachments/assets/92fbd50d-5e91-4da8-bba0-c02430415ffa" />

---

## 🔁 Why the Alert Appears on Page Reload (Stored vs Reflected XSS)

### 🧠 Key Difference:

| Type              | Behavior                                                                 |
| ----------------- | ------------------------------------------------------------------------ |
| **Reflected XSS** | Payload is sent in the request and reflected immediately in the response |
| **Stored XSS**    | Payload is saved on the server and executed whenever the page is loaded  |

### 💡 In This Lab:

* The payload is **stored in the database**.
* Every time the blog post is loaded, the application **retrieves and renders the stored comment**.
* Since there is **no encoding**, the browser interprets `<script>` as executable JavaScript.

👉 That’s why the alert appears **after revisiting or refreshing the page**, not just immediately after submission.

---

## ⚠️ Why This Vulnerability Exists

* No **input sanitization**
* No **output encoding**
* User input is directly inserted into HTML

This allows attackers to inject arbitrary JavaScript that executes in other users' browsers.

---

## 🧠 Key Takeaways

* Stored XSS is more dangerous than reflected XSS because it affects **every user who views the page**.
* Always test whether input is:

  * Stored
  * Reflected
  * Properly encoded
* Simple payloads like `<script>alert(1)</script>` are often enough to confirm the issue.

---

## ✅ Conclusion

By injecting a script into the comment field, we exploited a stored XSS vulnerability. The payload persisted on the server and executed whenever the blog post was viewed, successfully completing the lab.

