# 🧪 Lab: Username Enumeration via Subtly Different Responses

## 📌 Objective
This lab is vulnerable to:
- Username enumeration
- Password brute-force attacks

<img width="944" height="514" alt="1 lab" src="https://github.com/user-attachments/assets/5448ed62-9d2c-4518-b0d3-77314b25493c" />

---

## 🚀 Step-by-Step Solution

### 🔹 Step 1: Capture Login Request
1. Go to the **"My Account"** login page.
2. Enter any random credentials.
3. Enable **Burp Intercept** and capture the request:
   ```
   POST /login HTTP/1.1
   ```
4. Send this request to **Intruder**.

<img width="723" height="434" alt="2 login-panel" src="https://github.com/user-attachments/assets/b7f81b06-21da-451f-a080-b711d0869c01" />

---

### 🔹 Step 2: Enumerate Valid Username
1. In **Intruder → Positions tab**:
   - Highlight the **username value**
   - Click **"Add §"** to mark it as payload position

2. In **Payloads tab**:
   - Load the **username wordlist** provided by the lab

3. Start the attack.

<img width="1599" height="867" alt="3 user-position" src="https://github.com/user-attachments/assets/33082316-6060-46eb-91fd-10673c02645e" />

---

### 🔍 Step 3: Identify Response Differences
1. Observe responses for invalid usernames:
   ```
   Invalid username or password.
   ```

2. Go to **Intruder → Filter tab**:
   - Paste:
     ```
     Invalid username or password.
     ```
   - Enable **Negative Search**
   - Click **Apply**

<img width="1599" height="865" alt="4 incorect-response" src="https://github.com/user-attachments/assets/328036b2-6c24-4052-9edf-abfe0f8df342" />

3. Analyze results:
   - Look for responses that slightly differ
   - In this case, one username (`ao`) returns:
     ```
     Invalid username or password
     ```
     (⚠️ Notice the missing period `.`)

✅ This confirms `ao` is a **valid username**

<img width="1599" height="863" alt="5 username-found" src="https://github.com/user-attachments/assets/72283b04-08f3-4f9a-95c5-6120482dc9c5" />

---

### 🔹 Step 4: Brute-force Password
1. Go back to **Intruder**
2. Replace username with:
   ```
   ao
   ```
3. Set payload position on the **password field**
4. Load the **password wordlist** provided

5. Start the attack

<img width="1599" height="867" alt="6 pass-position" src="https://github.com/user-attachments/assets/936b951d-88a6-42af-b16f-334a1873a758" />

---

### 🔍 Step 5: Find Correct Password
1. After attack completes:
   - Sort results by **Status Code**

2. Look for anomalies:
   - Most responses → `200 OK`
   - One response → `302 Found`

3. The password corresponding to `302` is:
   ```
   123456789
   ```

<img width="1599" height="864" alt="7 password-found" src="https://github.com/user-attachments/assets/dfc8c54a-06f1-472f-9d31-2abd7a96dd32" />

---

### 🔹 Step 6: Login Successfully
1. Go back to login page
2. Enter credentials:
   ```
   Username: ao
   Password: 123456789
   ```
3. Click **Login**

<img width="784" height="437" alt="8 creds" src="https://github.com/user-attachments/assets/05d5ac67-7956-4b84-a5c7-7a492cbede7a" />

✅ Login successful  
🎉 Lab solved!

<img width="772" height="466" alt="9 success" src="https://github.com/user-attachments/assets/926a2998-622e-45be-9992-7cf5cec1ae5a" />

---

## 🧠 Key Takeaways
- Small differences in server responses can reveal valid usernames
- Filtering responses is critical for efficient enumeration
- Status code changes (e.g., 200 → 302) often indicate successful authentication
- Always analyze both **content** and **metadata** (length, status, timing)


