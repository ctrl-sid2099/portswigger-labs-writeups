# 🧪 Lab Write-Up: SQL Injection UNION Attack – Retrieving Data from Other Tables

## 📌 Lab Overview

This lab contains a **SQL injection vulnerability** in the product category filter. Because the query results are reflected in the response, we can exploit this using a **UNION-based SQL injection** to retrieve data from other tables.


<img width="940" height="496" alt="1 lab" src="https://github.com/user-attachments/assets/fff28462-41c9-4661-a858-9264d4115133" />

### 🎯 Objective

* Determine the **number of columns** in the query.
* Identify which columns support **text data**.
* Use a `UNION SELECT` statement to extract **usernames and passwords** from the `users` table.
* Log in as the **administrator** using retrieved credentials.

---

## 🔍 Step 1: Determining the Number of Columns

We begin by identifying how many columns the original query returns. This is necessary because a `UNION` query must have the **same number of columns** as the original query.

### 🧪 Payloads Tested:

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
```

### 📊 Observations:

* The first payload (`NULL--`) caused an **Internal Server Error**, indicating a mismatch in column count.
* The second payload (`NULL,NULL--`) returned a **valid response**, meaning:

✅ The query has **2 columns**.

<img width="1086" height="618" alt="2 (2-coloumns)" src="https://github.com/user-attachments/assets/efecac6f-8f36-4f31-8ae1-2e21db40db49" />

### 💡 Why This Matters:

If the column count is incorrect, the database throws an error. Using `NULL` is safe because it is compatible with most data types.

---

## 🔎 Step 2: Identifying Columns that Support Text Data

Next, we check which columns can hold **string (text) values**, since we want to retrieve usernames and passwords (which are text).

### 🧪 Payload:

```sql
' UNION SELECT 'String1','String2'--
```

### 📊 Observations:

* Both `'String1'` and `'String2'` were successfully reflected in the response.

✅ This confirms that **both columns accept text data**.

<img width="1101" height="702" alt="3 (both-coloumns)" src="https://github.com/user-attachments/assets/ea41dbae-ac50-4b56-92c5-3cffc73ad4f5" />

### 💡 Why This Matters:

We now know we can directly inject text-based data (like usernames and passwords) into either column without causing type errors.

---

## 🚀 Step 3: Extracting Data from the `users` Table

Now that we know:

* There are **2 columns**, and
* Both accept **text data**,

we can craft a payload to retrieve sensitive data from another table.

### 🧪 Payload:

```sql
' UNION SELECT username, password FROM users--
```

### 📊 Result:

The application returned the following data:

```
carlos
2v30zetzj71ybnp9klnb

administrator
iwglfvw91dwq9ibyz8se

wiener
5j34leq088t8m613qgok
```

<img width="1078" height="801" alt="4 (user-pass)" src="https://github.com/user-attachments/assets/eed76403-6125-4f1b-baf6-777be91dcf61" />

### 💡 Why This Works:

* `UNION SELECT` combines results from the original query with our injected query.
* We directly queried the `users` table to extract `username` and `password`.

---

## 🔐 Step 4: Logging in as Administrator

Using the retrieved credentials:

* **Username:** administrator
* **Password:** iwglfvw91dwq9ibyz8se

We navigated to the **"My Account"** page and logged in.

<img width="755" height="463" alt="5 admin-creds" src="https://github.com/user-attachments/assets/8164929f-2164-4517-b706-e9365ae38e2a" />

### ✅ Result:

* Login was **successful**.
* The lab was **solved**.

<img width="764" height="470" alt="6 successfull" src="https://github.com/user-attachments/assets/38e1c96f-0c0f-45f7-94e4-4f86e2e077f3" />

---

## 🧠 Key Takeaways

* Always match the **number of columns** in UNION attacks.
* Use `NULL` to safely probe column structure.
* Identify **text-compatible columns** before injecting string data.
* Once confirmed, extract sensitive data from other tables.

---

## ✅ Conclusion

By systematically determining column count, verifying data types, and crafting a precise UNION query, we successfully extracted user credentials and escalated access to the administrator account.

