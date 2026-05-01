# 🔗 Lab Details

* **Lab Name:** OS command injection, simple case
* **Category:** Command Injection
* **Objective:** Execute the `whoami` command to identify the current user

<img width="979" height="405" alt="1 lab" src="https://github.com/user-attachments/assets/538d43d9-bb87-44ee-b6fb-de63deac365f" />

---

## 📌 Overview

This lab contains an OS command injection vulnerability in the product stock checker functionality. The application takes user input (`productId` and `storeId`) and includes it directly in a system command without proper sanitization.

Because of this, it is possible to inject additional commands and execute them on the server.

---

## ⚙️ Steps to Reproduce

### 1. Capture the Request

* Open the lab and click on **"Check stock"** for any product.
* Intercept the request using Burp Suite.

<img width="1215" height="413" alt="2 check-stock" src="https://github.com/user-attachments/assets/cbe235f0-4fbf-4c5e-be8e-3c3c817bbf1d" />

Example request:

```
productId=1&storeId=2
```

---

### 2. Send to Repeater

* Forward the intercepted request to **Repeater** for manual testing.

---

### 3. Identify Input Parameters

The application uses the following parameters:

* `productId`
* `storeId`

These are potential injection points.

---

### 4. Test for Command Injection

Modify the `storeId` parameter by appending a command:

```
storeId=2 & whoami
```

Here:

* `&` is used to chain commands
* `whoami` returns the current system user

---

### 5. URL Encode the Payload

Encode the payload using Burp (Ctrl + U):

```
2 & whoami  →  2+%26+whoami
```

Final request:

```
productId=1&storeId=2+%26+whoami
```

---

### 6. Send the Request

* Send the modified request from Repeater.
* Observe the response.

<img width="1178" height="751" alt="3 user-name" src="https://github.com/user-attachments/assets/ebd2b184-14ca-4b44-add8-dcbf906afedd" />

---

## ✅ Result

The response includes the output of the injected command:

```
peter-T6G7cP
```

<img width="1198" height="782" alt="4 success" src="https://github.com/user-attachments/assets/70a576ba-a762-4b6c-a2eb-f90147b9ba0e" />

This confirms that the command executed successfully.

---

## 🏁 Conclusion

The application is vulnerable to OS command injection because it directly passes user input into a system command.

By injecting `& whoami`, it is possible to execute arbitrary commands on the server and retrieve sensitive information.

---
















