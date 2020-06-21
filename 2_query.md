### The query
```sql
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    inner join DISEASE as ds ON ds.disease_class_id = d.disease_class_id and ds.disease_number = d.disease_number
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DISEAS.png?raw=true)

### The solution
