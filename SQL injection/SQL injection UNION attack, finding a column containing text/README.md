# 🧪 Lab Write-Up: SQL Injection UNION Attack – Finding a Column Containing Text

## 📌 Lab Overview

This lab demonstrates a **SQL injection vulnerability** in the product category filter. Since the query results are reflected in the application's response, it is possible to use a **UNION-based SQL injection** to extract data from other tables.

<img width="934" height="554" alt="1 lab" src="https://github.com/user-attachments/assets/97b4ef37-12c7-4659-87c0-19d7a33e2870" />

### 🎯 Objective

* Determine the **number of columns** in the original query.
* Identify which column(s) support **string data**.
* Inject a **UNION query** that returns a row containing a **specific provided value**.

---

## 🔍 Step 1: Determining the Number of Columns

To identify the number of columns, we used the `UNION SELECT` technique with `NULL` placeholders.

### 🧪 Payloads Tested:

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

### 📊 Observations:

* The first two payloads (`NULL--`, `NULL,NULL--`) resulted in an **Internal Server Error**.
* The third payload (`NULL,NULL,NULL--`) returned a **valid response**, meaning:

✅ The query has **3 columns**.

<img width="1172" height="636" alt="2 3-coloumns" src="https://github.com/user-attachments/assets/e20c9fff-6508-4c36-a781-340c293d7da5" />

---

## 🔎 Step 2: Identifying a Column that Accepts String Data

Next, we tested which column supports **string input** by replacing each `NULL` with a string value `'string'`.

### 🧪 Payloads Tested:

```sql
' UNION SELECT 'string',NULL,NULL--
' UNION SELECT NULL,'string',NULL--
' UNION SELECT NULL,NULL,'string'--
```

### 📊 Observations:

* Only the second payload worked and reflected `'string'` in the response.

✅ This confirms that the **second column accepts string data**.

<img width="1206" height="723" alt="3 string-2" src="https://github.com/user-attachments/assets/b5e7ba93-c668-45b9-8ad7-dff1fa61d1cf" />

---

## 🚀 Step 3: Injecting the Required Value

Finally, we replaced `'string'` with the lab-provided value:

### 🧪 Final Payload:

```sql
' UNION SELECT NULL,'W4T8jz',NULL--
```

### ✅ Result:

* The value **`W4T8jz`** appeared in the application response.
* The lab was successfully solved.

<img width="1014" height="549" alt="4 sucess" src="https://github.com/user-attachments/assets/eb295b12-8578-4ae3-b308-14f847965262" />

---

## 🧠 Key Takeaways

* Use `NULL` placeholders to safely determine column count.
* Replace `NULL` values one by one with strings to find **text-compatible columns**.
* Once identified, inject meaningful data into the correct column.

---

## ✅ Conclusion

By combining **column enumeration** and **data type testing**, we successfully executed a UNION-based SQL injection and retrieved the required value, completing the lab.



