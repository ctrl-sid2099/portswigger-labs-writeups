# 💉 SQL injection
In this section, we explain:

  - What SQL injection (SQLi) is.
  - How to find and exploit different types of SQLi vulnerabilities.
  - How to prevent SQLi.

---

<img width="781" height="440" alt="sql-injection" src="https://github.com/user-attachments/assets/59117a9e-a15e-45aa-a9d9-11740072c5c5" />

## ❓ What is SQL injection (SQLi)?
SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This 
can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable 
them to perform denial-of-service attacks.

---

## ⚙️ What is the impact of a successful SQL injection attack?
A successful SQL injection attack can result in unauthorized access to sensitive data, such as:

  - Passwords.
  - Credit card details.
  - Personal user information.

SQL injection attacks have been used in many high-profile data breaches over the years. These have caused reputational damage and regulatory fines. In some 
cases, an attacker can obtain a persistent backdoor into an organization's systems, leading to a long-term compromise that can go unnoticed for an extended 
period.




