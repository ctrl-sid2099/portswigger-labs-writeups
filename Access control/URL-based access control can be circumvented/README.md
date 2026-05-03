# 🧪 Lab Write-Up: URL-Based Access Control Can Be Circumvented

## 📌 Lab Overview

This lab demonstrates a flaw in **URL-based access control**. Although the `/admin` panel is supposedly restricted, the back-end framework trusts a special header: `X-Original-URL`.

This creates a mismatch between:

* **Front-end restrictions** (blocking `/admin`)
* **Back-end behavior** (still honoring rewritten paths)

<img width="949" height="395" alt="1 lab" src="https://github.com/user-attachments/assets/03228455-4c81-4567-ad76-790b618aba9f" />

### 🎯 Objective

* Bypass access controls to reach the `/admin` panel.
* Use admin functionality to **delete the user `carlos`**.

---

## 🚧 Step 1: Attempting Direct Access

We first tried to access the admin panel directly:

```http
GET /admin HTTP/1.1
```

### 📊 Result:

* Response: **Access Denied**

<img width="840" height="229" alt="2 access-denied" src="https://github.com/user-attachments/assets/4d0bab41-2e1e-419b-9c2a-2eb562c3dac6" />

### 💡 Why This Happens:

The **front-end filter** blocks requests to `/admin`, preventing direct access.

---

## 🛠️ Step 2: Capturing the Request in Burp Suite

We refreshed the page and intercepted the request in **Burp Suite**:

```http
GET /admin HTTP/1.1
```

<img width="1178" height="738" alt="3 intercept" src="https://github.com/user-attachments/assets/0a2977ef-618c-4397-bea7-12a26d0cbce7" />

### 💡 Why We Did This:

We need to **manipulate the request manually**, bypassing front-end restrictions.

---

## 🔍 Step 3: Testing Header Behavior

We modified the request to:

```http
GET / HTTP/1.1
X-Original-URL: /invalid
```

<img width="1182" height="540" alt="4 (invalid)" src="https://github.com/user-attachments/assets/cbc9c380-e4d4-46d1-a4b4-8ff199a031c4" />

### 📊 Result:

* Response: **Not Found**

<img width="823" height="246" alt="6 Not-Found" src="https://github.com/user-attachments/assets/61786e2f-34ae-4404-a5ad-d05d87db3651" />

### 💡 Why This Step Matters:

This confirms that:

* The back-end is actually using the `X-Original-URL` header to determine routing.
* Our injected path (`/invalid`) is being processed.

---

## 🚀 Step 4: Bypassing Access Control

Next, we crafted the key payload:

```http
GET / HTTP/1.1
X-Original-URL: /admin
```

<img width="1182" height="540" alt="7 admin" src="https://github.com/user-attachments/assets/bed13b0a-b936-4a77-8780-f7f723660281" />

### 📊 Result:

* Successfully accessed the **Admin panel**
* Saw the list of users and available actions

<img width="835" height="471" alt="8 admin-panel" src="https://github.com/user-attachments/assets/6a81ba85-7343-4049-a6c3-f0ebb82dd29b" />

### 💡 Why This Works:

* The front-end only checks the visible URL (`/`)
* The back-end trusts `X-Original-URL` and internally routes to `/admin`

👉 This mismatch allows us to **bypass access control entirely**

---

## 🧨 Step 5: Attempting to Delete User `carlos`

We clicked the delete option for `carlos` and intercepted the request:

```http
GET /admin/delete?username=carlos HTTP/2
```

<img width="1176" height="539" alt="9 delete" src="https://github.com/user-attachments/assets/63f6d7b8-674a-48fd-9724-c840c64fb3a1" />

We modified it to:

```http
GET ?username=carlos
X-Original-URL: /admin/delete
```

<img width="1174" height="541" alt="10 modify" src="https://github.com/user-attachments/assets/108aa834-2713-463d-a61f-b8def345f0e2" />

### 📊 Result:

* Response: **Access Denied**

<img width="824" height="286" alt="11 access-denied" src="https://github.com/user-attachments/assets/e932b259-817e-4c63-a86e-f7ffc769847f" />

### 💡 Why It Failed Initially:

Even though we used the header trick, the request format was slightly off and still triggered restrictions.

---

## 🔄 Step 6: Final Outcome

After forwarding the request and refreshing the page:

### ✅ Result:

* The lab was marked as **solved**
* The user `carlos` was successfully deleted

<img width="1175" height="447" alt="12 lab-solved" src="https://github.com/user-attachments/assets/3bd689b7-f4af-4f29-9d14-4b9adfa36ff7" />

### 💡 Why This Worked:

* The back-end processed the deletion request via `X-Original-URL`
* The action completed even though the response showed "Access Denied"
* The UI state updated upon refresh, confirming success

---

## 🧠 Key Takeaways

* Front-end access controls are **not reliable security mechanisms**
* Back-end systems may trust headers like `X-Original-URL`
* Mismatched validation between layers can lead to **critical bypasses**
* Always test alternate headers when dealing with restricted endpoints

---

## ✅ Conclusion

By exploiting the `X-Original-URL` header, we bypassed front-end access controls, accessed the admin panel, and performed privileged actions—successfully completing the lab.


