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

### The solution
```sql
alter table PATIENT alter column patient_id int not null

alter table PATIENT add CONSTRAINT PK_dbo_PATIENT_patient_id primary key (patient_id)

alter table DIAGNOSIS alter column patient_id int not NULL

alter table DIAGNOSIS add CONSTRAINT FK_dbo_DIAGNOSIS_patient_id FOREIGN key (patient_id)
REFERENCES PATIENT(patient_id)
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_PATIENT_result.png?raw=true)
