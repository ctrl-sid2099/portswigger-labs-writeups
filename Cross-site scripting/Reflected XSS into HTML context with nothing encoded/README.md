# 🛡️ Reflected XSS into HTML Context (Nothing Encoded)

## 📌 Lab Details
- **Lab Name:** Reflected XSS into HTML context with nothing encoded  
- **Category:** Cross-Site Scripting (XSS)  
- **Type:** Reflected XSS  

<img width="934" height="373" alt="1 lab" src="https://github.com/user-attachments/assets/be9c75ed-a2eb-44e5-b799-0a189e28d263" />

---

## 🎯 Objective
Perform a cross-site scripting attack that executes JavaScript using the `alert()` function.

---

## 🔎 Vulnerability Summary
The application reflects user input from the search parameter directly into the HTML response without any encoding or sanitization. This allows an attacker to inject malicious JavaScript that executes in the victim’s browser.

---

## 🧠 Why This is Vulnerable
When user input is inserted into a webpage without encoding, the browser interprets it as HTML instead of plain text.

In this lab:
- Input is reflected inside the HTML body
- Special characters like `<` and `>` are not encoded
- This allows injection of executable JavaScript

As a result, an attacker can break out of the intended context and execute arbitrary scripts.

---

## ⚙️ Steps to Reproduce

### 1. Identify Reflection Point
Enter the following into the search function:

    Test123

### ✅ Observation
The response displays:

    0 search results for "Test123"

<img width="859" height="397" alt="2 reflected" src="https://github.com/user-attachments/assets/09fffe28-0ea9-41b5-8c65-4d2760b91ff0" />

This confirms that:
- User input is reflected in the response
- The application is dynamically rendering user-controlled data

---

### 2. Confirm Lack of Encoding
Since the input appears exactly as entered, it indicates that no output encoding is applied.

---

### 3. Inject XSS Payload
Enter the following payload:


    <script>alert('XSS');</script>

<img width="795" height="402" alt="3 payload" src="https://github.com/user-attachments/assets/5e8de2f6-124f-4881-bf32-f51f181afb47" />

---

### 💥 Result
- The payload is reflected directly in the response
- The browser interprets it as JavaScript
- A popup appears showing:

      XSS
  
- The lab is successfully solved

<img width="1031" height="345" alt="4 reflected" src="https://github.com/user-attachments/assets/30086425-02f2-4c68-9bc5-1004f384f661" />

---

<img width="1014" height="412" alt="5 sucess" src="https://github.com/user-attachments/assets/e5a7f268-1581-4c09-97e1-c0ee774378aa" />

---

## ⚠️ Impact
This vulnerability allows attackers to:
- Execute arbitrary JavaScript in a user's browser
- Steal session cookies
- Perform actions on behalf of the user
- Redirect users to malicious websites

---

## 🛡️ Prevention & Mitigation

### ✅ 1. Output Encoding
Encode all user input before rendering:

- `<` → `&lt;`
- `>` → `&gt;`
- `"` → `&quot;`

### ✅ 2. Input Validation
- Restrict input to expected formats
- Filter or reject malicious patterns

### ✅ 3. Use Secure Frameworks
- Use templating engines that automatically escape output

### ✅ 4. Content Security Policy (CSP)
- Implement CSP to reduce impact of XSS attacks

---

## 📚 Conclusion
This lab demonstrates how failing to encode user input leads to reflected XSS.
Proper output encoding and secure handling of user data are essential to prevent such vulnerabilities.

