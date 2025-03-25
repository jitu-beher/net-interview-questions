### Write a SQL query using Group by and Having Clause using employee table : Get DeptId and count of deptId which employees are more than 5

```sql
SELECT DeptId, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY DeptId
HAVING COUNT(*) > 5;
```

### **🔹 `ROW_NUMBER()` vs `DENSE_RANK()` in SQL**

Both `ROW_NUMBER()` and `DENSE_RANK()` are **window functions** used for ranking rows within a result set. However, they work differently when handling duplicate values.

---

## **1️⃣ `ROW_NUMBER()`**

- Assigns a **unique** sequential number to each row.
- Even if two rows have the same value, they get **different row numbers**.
- **No gaps** in ranking, but the order is strictly sequential.

### **✅ Example: `ROW_NUMBER()`**

```sql
SELECT EmployeeID, Name, Salary,
       ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNum
FROM Employee;
```

### **📝 Output**

| EmployeeID | Name  | Salary | RowNum |
| ---------- | ----- | ------ | ------ |
| 105        | John  | 5000   | 1      |
| 102        | Alice | 4500   | 2      |
| 107        | Bob   | 4500   | 3      |
| 103        | Mark  | 4000   | 4      |

📌 **Key Observation:** Even though Alice and Bob have the same salary, they get different row numbers.

---

## **2️⃣ `DENSE_RANK()`**

- Assigns the **same rank** to rows with **duplicate values**.
- **No gaps** in ranking (i.e., if two people have the same salary, the next rank is not skipped).

### **✅ Example: `DENSE_RANK()`**

```sql
SELECT EmployeeID, Name, Salary,
       DENSE_RANK() OVER (ORDER BY Salary DESC) AS RankNum
FROM Employee;
```

### **📝 Output**

| EmployeeID | Name  | Salary | RankNum |
| ---------- | ----- | ------ | ------- |
| 105        | John  | 5000   | 1       |
| 102        | Alice | 4500   | 2       |
| 107        | Bob   | 4500   | 2       |
| 103        | Mark  | 4000   | 3       |

📌 **Key Observation:**

- Alice and Bob both have salary `4500`, so they get **the same rank (2)**.
- Mark gets **rank 3** (no gap in ranking).

---

## **🔹 Difference Between `ROW_NUMBER()` and `DENSE_RANK()`**

| Feature                  | `ROW_NUMBER()`             | `DENSE_RANK()`               |
| ------------------------ | -------------------------- | ---------------------------- |
| **Assigns unique rank?** | ✅ Yes                     | ❌ No                        |
| **Gaps in ranking?**     | ✅ Yes                     | ❌ No                        |
| **Handles duplicates?**  | ❌ Different row numbers   | ✅ Same rank for duplicates  |
| **Use case?**            | Finding exact row position | Grouping duplicates together |

---

## **🔹 When to Use Which?**

1. **Use `ROW_NUMBER()`** when you need a unique row number for every record.
   - Example: Paginating search results (`LIMIT OFFSET` queries).
2. **Use `DENSE_RANK()`** when ranking data without skipping values.
   - Example: Finding the **second-highest salary** or **top-N performers**.

Would you like a real-world example using both functions? 😊
