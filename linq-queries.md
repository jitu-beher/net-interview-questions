•	Write a simple LINQ query using left join
```sql
  // LINQ Left Join using Query Syntax
        var query = from emp in employees
                    join dept in departments on emp.DepartmentId equals dept.Id into deptGroup
                    from dept in deptGroup.DefaultIfEmpty() // Left Join
                    select new
                    {
                        EmployeeName = emp.Name,
                        DepartmentName = dept?.Name ?? "No Department" // Handle NULL
                    };
```