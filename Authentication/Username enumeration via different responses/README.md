# 🛡️ Username Enumeration via Different Responses (Lab Write-up with Explanation)

## 📌 Overview

This lab is vulnerable to:

* **Username enumeration**
* **Password brute-force attacks**

The application leaks information through **different error messages**, which allows us to:

* First identify a **valid username**
* Then brute-force the **password efficiently**

<img width="961" height="477" alt="1 lab" src="https://github.com/user-attachments/assets/0760dc1c-f9e9-4a51-87d7-42e210c54a9e" />

---

## 🎯 Objective

1. Enumerate a **valid username**
2. Brute-force the **password**
3. Log in to the account

---


## 🧠 Attack Strategy (Why this works)

The login system behaves differently based on input:

* ❌ Invalid username → `"Invalid username"`
* ⚠️ Valid username but wrong password → `"Incorrect password"`

👉 This difference creates a **side-channel leak**, letting us confirm whether a username exists *without knowing the password*.

---

## 🚶 Step-by-Step Exploitation

### 🔹 Step 1: Capture Login Request

* Enter any random credentials and intercept the request.
* Send it to **Intruder**.

```id="req001"
POST /login HTTP/1.1
```

### 💡 Why?

We need a **baseline request** so we can automate multiple login attempts using payloads.

---

### 🔹 Step 2: Username Enumeration

1. Go to **Intruder → Positions**

2. Select only the `username` parameter as payload position

<img width="1062" height="787" alt="3 intruder" src="https://github.com/user-attachments/assets/5b8204d5-db70-4733-be04-ef70d4d87e57" />

3. Load the **username wordlist**

<img width="533" height="741" alt="4 payload-list" src="https://github.com/user-attachments/assets/40fae436-90e3-4773-bd40-8d1983572640" />

4. Run attack using **Sniper mode**

### 💡 Why?

* We test **one username at a time**
* Password stays constant → isolates the variable
* Helps us identify which usernames exist

---

### 🔍 Step 3: Identify Invalid Pattern

* Look at responses for most entries:

  ```
  Invalid username
  ```

### 💡 Why?

We first identify the **common failure pattern**, so we can eliminate noise.

---

### 🔹 Step 4: Filter Responses

* Go to **Filter tab**
* Add:

  ```
  Invalid username
  ```
* Enable:
  ✅ Negative search

### 💡 Why?

This removes all responses containing `"Invalid username"` and shows only **interesting deviations** — likely valid usernames.

<img width="1599" height="866" alt="5 filter" src="https://github.com/user-attachments/assets/c8954473-8bcf-4b56-85c8-db7d4d71cff3" />

---

### ✅ Step 5: Find Valid Username

* Look for responses like:

  ```
  Incorrect password
  ```

### 💡 Why?

This means:

* Username exists ✔️
* Password is wrong ❌

👉 That’s exactly what we want — confirmation of a valid account.

✔️ Discovered:

```id="cred001"
username = agenda
```

<img width="1599" height="865" alt="6 username" src="https://github.com/user-attachments/assets/d42a0626-6996-4f39-877e-6330d57b3aa8" />

---

### 🔹 Step 6: Password Brute-force

1. Replace request parameter:

   ```
   username=agenda
   ```

2. Set payload position on `password`

<img width="1067" height="786" alt="7 password-position" src="https://github.com/user-attachments/assets/c0a3cb68-b718-4733-9199-7f47a40f18cc" />

3. Load **password wordlist**

<img width="535" height="700" alt="8 password-payload" src="https://github.com/user-attachments/assets/7dd0fa39-f567-40d0-bf5a-d573fae144d3" />

4. Run **Sniper attack**

### 💡 Why?

Now that we know the username is valid:

* We brute-force only the password
* This reduces attack scope and increases efficiency

---

### 🔍 Step 7: Detect Successful Login

* Filter/sort results by:

  * **Status Code**

* Look for:

  ```
  302 Found
  ```
  
<img width="1599" height="867" alt="9 password" src="https://github.com/user-attachments/assets/db500bfc-c5ee-4313-9ad8-6df0738a1c6a" />

### 💡 Why?

* `200 OK` → login failed (page reload)
* `302 Found` → redirect after successful login

👉 Redirect is a strong indicator of authentication success

---

### ✅ Step 8: Identify Correct Credentials

✔️ Found:

```id="cred002"
Username: agenda
Password: superman
```

---

### 🔹 Step 9: Login to Account

* Use credentials on login page:

  ```
  agenda : superman
  ```

<img width="785" height="504" alt="10 creds" src="https://github.com/user-attachments/assets/19427381-f5d2-4756-ad8e-b8f46967428c" />

---

## 🏁 Result

✅ Valid username enumerated
✅ Password successfully brute-forced
✅ Account accessed
✅ Lab solved

<img width="733" height="464" alt="11 successfull" src="https://github.com/user-attachments/assets/4f1fde3b-8d90-4bc8-abe0-8f41a39e6d9b" />

---

## 🔑 Key Takeaways

* Error message differences = **information leakage**
* Always isolate variables during brute-force
* Filtering responses saves massive time
* Status codes are often more reliable than page content



