### Write a SQL query using Group by and Having Clause using employee table : Get DeptId and count of deptId which employees are more than 5

```sql
SELECT DeptId, COUNT(*) AS EmployeeCount
FROM Employee
GROUP BY DeptId
HAVING COUNT(*) > 5;
```
