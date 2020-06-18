### The query
```sql
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    inner join PATIENT as p on p.patient_id = d.patient_id
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_PATIENT.png?raw=true)
