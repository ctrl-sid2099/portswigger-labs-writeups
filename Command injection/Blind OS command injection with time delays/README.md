# 🔗 Lab Details

* **Lab Name:** Blind OS command injection with time delays
* **Category:** Command Injection (Blind)
* **Objective:** Trigger a 10-second delay using command injection

<img width="961" height="402" alt="1 lab" src="https://github.com/user-attachments/assets/6aaed948-f08d-4528-bd76-29186e544638" />

---

## 📌 Overview

This lab demonstrates a **blind OS command injection** vulnerability in the feedback submission feature.

Unlike visible command injection, the output of executed commands is **not returned in the response**, making it harder to detect. Instead, we rely on **side effects** such as time delays to confirm successful exploitation.

---

## ⚙️ Steps to Reproduce

### 1. Submit Feedback & Capture Request

* Go to the feedback form and enter random details.
* Submit the form while intercepting the request in Burp Suite.

<img width="951" height="740" alt="2 submit-feedback" src="https://github.com/user-attachments/assets/52dfb32c-eaec-400f-9315-2bc3e85de7e6" />

---

Captured request:

---

```id="a1d9f3"
csrf=ntmrvUChhVq0iFvE56p7RchmoWpeuiT6&name=admin&email=admin123%40gmail.com&subject=food&message=Hungry%21%21%21
```

---

### 2. Send to Repeater

* Forward the request to **Repeater** for testing.

---

### 3. Identify Injection Point

* The `email` parameter is a good candidate for injection.
* Since this is a **blind** vulnerability, we test using time-based payloads.

---

### 4. Inject Time Delay Payload

Modify the `email` parameter:

```id="p0ks9d"
admin123@gmail.com || sleep 10s ||
```

URL-encoded version:

```id="z8d2la"
admin123@gmail.com+||+sleep+10s+||+
```

Final request:

```id="u3pl9m"
csrf=ntmrvUChhVq0iFvE56p7RchmoWpeuiT6&name=admin&email=admin123@gmail.com+||+sleep+10s+||+&subject=food&message=Hungry!!!
```

---

### 5. Send the Request

* Send the modified request in Repeater.
* Observe the response time.

<img width="1179" height="760" alt="3 10s+delay" src="https://github.com/user-attachments/assets/3aacecb4-2b0b-4217-9521-6fba1ef167c6" />

---

## ✅ Result

* The response is delayed by approximately **10 seconds**.
* This confirms that the injected command (`sleep 10s`) executed successfully.

<img width="954" height="666" alt="4 success" src="https://github.com/user-attachments/assets/219ff7dc-2afb-429b-82a1-0e3feb7766d8" />

---

## 🏁 Conclusion

The application is vulnerable to **blind OS command injection**. Even though command output is not visible, we can confirm execution using timing-based techniques.

By injecting `sleep 10s`, we successfully triggered a delay and solved the lab.

---


