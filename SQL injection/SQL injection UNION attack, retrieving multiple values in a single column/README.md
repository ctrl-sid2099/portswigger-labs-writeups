# 🧪 Lab Write-Up: SQL Injection UNION Attack – Retrieving Multiple Values in a Single Column

## 📌 Lab Overview

This lab involves a **SQL injection vulnerability** in the product category filter. Since query results are reflected in the application's response, we can exploit this using a **UNION-based SQL injection**.

Unlike the previous lab, the challenge here is that we must retrieve **multiple values (username + password)** but display them through **a single column**.

<img width="941" height="534" alt="1 lab" src="https://github.com/user-attachments/assets/a55f0e9c-aef4-472c-8162-eb6a75fa99b4" />

### 🎯 Objective

* Determine the **number of columns** in the query.
* Identify which column supports **text data**.
* Combine multiple fields (`username` and `password`) into **one column output**.
* Log in as the **administrator** using retrieved credentials.

---

## 🔍 Step 1: Determining the Number of Columns

We first identify how many columns the original query has. This is required because `UNION SELECT` must match the same column count.

### 🧪 Payloads Tested:

```sql id="m4q2wx"
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
```

### 📊 Observations:

* The first payload caused an **Internal Server Error**, indicating incorrect column count.
* The second payload returned a **valid reflected response**.

✅ The query has **2 columns**.

<img width="1062" height="633" alt="2 (2-COLOUMNS)" src="https://github.com/user-attachments/assets/086a3759-7e8f-4f2b-abd2-dea0fe3efbdc" />

### 💡 Why We Did This:

A mismatch in column numbers breaks the query. Using `NULL` lets us test safely without worrying about data types.

---

## 🔎 Step 2: Identifying a Text-Compatible Column

Next, we determine which column can handle **string data**, since we need to display text (usernames and passwords).

### 🧪 Payloads Tested:

```sql id="v7o2rf"
' UNION SELECT 'String1',NULL--
' UNION SELECT NULL,'String2'--
```

### 📊 Observations:

* The first payload caused an **Internal Server Error** → first column does NOT accept text.
* The second payload worked and reflected `'String2'`.

✅ The **second column supports text data**.

<img width="1078" height="677" alt="3 (2-column)" src="https://github.com/user-attachments/assets/0af0bd9e-ca84-4df6-ba2d-f7b0b45e12fb" />

### 💡 Why We Did This:

We must inject text into a compatible column; otherwise, the database throws a type error.

---

## 🚀 Step 3: Retrieving Multiple Values in One Column

We now need to extract both `username` and `password`, but we only have **one usable text column**.

### 🔧 Solution:

Use string concatenation to merge both values into one column.

### 🧪 Payload:

```sql id="e4t1yn"
' UNION SELECT NULL, username || '~' || password FROM users--
```

### 📊 Result:

```id="y8t3kd"
carlos~sicw0k28d76y0k3uzczy
administrator~p1g0n5h86iuq8foofux1
wiener~3t48rd0yhh31xqax7k41
```

<img width="1079" height="781" alt="4 (creds)" src="https://github.com/user-attachments/assets/9967bfb0-c3a0-4694-a3df-305184367916" />

### 💡 Why This Works:

* `||` is the SQL concatenation operator.
* `'~'` acts as a **delimiter** to clearly separate username and password.
* Since only one column supports text, we combine both values into a **single output string**.

---

## 🔐 Step 4: Logging in as Administrator

From the extracted data:

* **Username:** administrator
* **Password:** p1g0n5h86iuq8foofux1

We navigated to the **"My Account"** page and logged in.

<img width="714" height="435" alt="5 admin-creds" src="https://github.com/user-attachments/assets/51b8134c-8e5f-48e5-9a2b-9da48c2bb295" />

### ✅ Result:

* Login was **successful**.
* The lab was **completed**.

<img width="715" height="502" alt="6 sucessfull" src="https://github.com/user-attachments/assets/a3c3de51-a2ba-4fe4-b943-24bacfc1308b" />

---

## 🧠 Key Takeaways

* Always match the **exact number of columns** in UNION queries.
* Identify which column supports **text data** before injecting strings.
* If limited to one text column, use **concatenation (`||`)** to combine multiple values.
* Use a delimiter (like `~`) to keep extracted data readable.

---

## ✅ Conclusion

By combining column enumeration, data type testing, and string concatenation, we successfully extracted multiple fields into a single column and used them to gain administrator access.

