# 🔐 Broken brute-force protection, IP block

<img width="977" height="515" alt="1 lab" src="https://github.com/user-attachments/assets/f57c8782-0c81-498e-956c-9d48979cb895" />

## 📌 Overview

This lab demonstrates a logic flaw in brute-force protection where the login attempt counter resets after a successful login.

The goal was to exploit this behavior to brute-force a victim account (carlos) while bypassing the lockout mechanism by periodically logging into a valid account (wiener).

## 🧠 Understanding the Vulnerability

The application implements a protection mechanism:

  - After 2 failed login attempts, further attempts are blocked.
  - However, a successful login resets the attempt counter.

👉 This creates a flaw:
If we insert a valid login after every 2 failed attempts, we can bypass the lockout entirely.

## ⚙️ Step-by-Step Exploitation

### 1️⃣ Analyze the Login Mechanism
  
  - Start by sending login attempts with a valid username and random passwords.
  - Observe behavior:

      - After 2 failed attempts → next attempts get blocked.
      - After a successful login, the counter resets.

<img width="807" height="423" alt="2 2-attemempts" src="https://github.com/user-attachments/assets/f6ca3d6d-2967-459b-a317-12cabb6a0010" />

✅ Conclusion:

We must ensure every third request is a valid login to keep the attack running.

## 2️⃣ Automating Wordlist Injection

To automate this pattern, I built a custom Python tool:

📄 Checkout the [tool](https://github.com/ctrl-sid2099/auth-bypass-bruteforce-logic-flaw/blob/main/wordlist-injector.py).

### 🔧 What it does:
  
  - Takes a wordlist file
  - Injects a specific password after every N lines
  - Includes:

      - File validation
      - Input validation
      - Custom output handling

### 💡 Usage Logic:

  - After every 2 password attempts → insert a known valid password
  - Example:

        pass1
        pass2
        VALID_PASS
        pass3
        pass4
        VALID_PASS

This ensures the login attempt counter is constantly reset.

## 3️⃣ Structuring the Attack Pattern

We need to alternate between:

  - Target user → carlos
  - Valid user → wiener

Pattern:

    carlos → attempt 1
    carlos → attempt 2
    wiener → valid login (reset)
    carlos → attempt 3
    carlos → attempt 4
    wiener → valid login (reset)

To achieve this:
  
  - Use this script to prepare the password injection

<img width="980" height="754" alt="5 user-txt" src="https://github.com/user-attachments/assets/20d6f235-d83e-49f9-93aa-61495106e0dc" />

## 4️⃣ Configuring Burp Suite Intruder

### 🎯 Key Settings:
  
  - Positions tab:
    - Set payload positions for:
      - Username
      - Password
  - Payloads:
    - Use generated wordlist with injected valid password
   
<img width="1065" height="744" alt="6 set-position" src="https://github.com/user-attachments/assets/fb3ce4aa-d969-436d-91a3-d0b757f12dc5" />

## ⚠️ Resource Pool Configuration (Important)

Set Resource Pool = 1 request at a time

<img width="489" height="685" alt="7 resourcePool-1" src="https://github.com/user-attachments/assets/40b071dd-7dc2-4fb5-99db-a7a7f9015478" />

### ❓ Why?

If multiple requests are sent in parallel:

  - The server may process them out of order
  - The “reset” request (valid login) might not occur exactly after 2 failures
  - This breaks the bypass logic

👉 By forcing sequential requests, we ensure:

    fail → fail → success → fail → fail → success

✅ This guarantees the counter resets correctly.

## 5️⃣ Identifying Valid Credentials

In Burp Intruder:

  - Filter results by:
    - Username = wiener (to locate reset points)
  - Then:
    - Sort responses by HTTP status code
  
🔍 What to look for:
 
  - 302 Found → indicates successful login

<img width="1599" height="761" alt="8 success" src="https://github.com/user-attachments/assets/c6334edf-ca3e-4fdf-818e-201740de51a6" />

This helps identify:

  - Valid login responses
  - Eventually, the correct password for carlos

## 6️⃣ Account Takeover

  - Use the discovered credentials
  - Log in via My Account panel

✅ Successful authentication confirms the exploit

<img width="928" height="516" alt="9 end" src="https://github.com/user-attachments/assets/e8b88ecd-0ef2-4caa-942b-bf3d23a12261" />

## 🧩 Key Takeaways

  - Brute-force protections can fail due to logic flaws, not just weak thresholds
  - Reset mechanisms can be abused if not tied properly to session/user context
  - Sequential request control is critical in exploitation
