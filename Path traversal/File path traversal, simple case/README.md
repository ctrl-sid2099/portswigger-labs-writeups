# 🔐 PortSwigger Lab: File Path Traversal (Simple Case)

## 🎯 Objective
Retrieve the contents of the `/etc/passwd` file.

<img width="963" height="340" alt="1 lab" src="https://github.com/user-attachments/assets/54ab3d6e-e5ce-4a75-aa7c-e88f7e657992" />

---

## 🧠 Approach

This lab demonstrates a **path traversal vulnerability**.

- The application loads product images using a filename parameter.
- If user input is not properly validated, attackers can manipulate file paths.
- By using traversal sequences like `../`, we can move outside the intended directory and access sensitive system files.

---

## 🪜 Steps to Solve

### 1. Capture Image Request

- Open the lab and click on any product.
- Intercept the request for the product image using Burp Suite.

You will see a request like:
```
GET /image?filename=x.jpg HTTP/1.1
```

- Send this request to **Repeater**.

<img width="778" height="730" alt="2 repeater" src="https://github.com/user-attachments/assets/313e3a0c-f269-41c6-a696-5f95e0a01cff" />

---

### 2. Modify the Request

- In Repeater, change the `filename` parameter to:
```
../../../etc/passwd
```

So the request becomes:
```
GET /image?filename=../../../etc/passwd HTTP/1.1
```

<img width="778" height="718" alt="4 1 modify" src="https://github.com/user-attachments/assets/2c6ad454-e566-4323-946f-b7befeb73c95" />

---

### 3. Send the Request

- Click **Send** in Repeater.
- Observe the response.

---

### 4. Analyze the Response

- The server returns a `200 OK` response.
- The response body contains the contents of:
```
/etc/passwd
```

<img width="1599" height="751" alt="3 passwd" src="https://github.com/user-attachments/assets/724d8e04-312b-4b7b-9056-4ce0f225e380" />

---

## ✅ Result

- Successfully retrieved sensitive system file `/etc/passwd`  
- The lab is solved  

<img width="785" height="327" alt="4 success" src="https://github.com/user-attachments/assets/29e67ace-c83b-4b92-a597-ac5f932c0256" />

---

## 📝 Key Takeaways

- Path traversal occurs when user input is not properly sanitized  
- `../` sequences allow navigation outside intended directories  
- Applications should validate and restrict file paths securely  
- Never trust user-controlled input for file system access  
