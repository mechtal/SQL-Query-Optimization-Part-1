### The query
```sql
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    inner join DIAGNOSIS_DOCTOR as dd on dd.diagnosis_id = d.diagnosis_id and dd.is_responsible = 1
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DIAG_DOCT.png?raw=true)

### The solution
The common step
```sql
alter table DIAGNOSIS_DOCTOR alter column diagnosis_doctor_id int not null
alter table DIAGNOSIS_DOCTOR add CONSTRAINT PK_dbo_DIAGNOSIS_DOCTOR_diagnosis_doctor_id PRIMARY KEY (diagnosis_doctor_id)
```
#### The first
```
alter table DIAGNOSIS_DOCTOR add CONSTRAINT FK_dbo_DOAGNOSIS_DOCTOR_diagnosis_id FOREIGN KEY (diagnosis_id) REFERENCES DIAGNOSIS(diagnosis_id)
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DIAG_DOCT_res1.png?raw=true)
=> The server is not sure that we have only one responsible doctor.
```sql
alter table DIAGNOSIS_DOCTOR drop CONSTRAINT FK_dbo_DOAGNOSIS_DOCTOR_diagnosis_id
```
#### The second
```sql
create unique NONCLUSTERED index fix1_diagnosis_doctor
on diagnosis_doctor
(diagnosis_id)
where is_responsible = 1

-- inner join -> left join
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    left join DIAGNOSIS_DOCTOR as dd on dd.diagnosis_id = d.diagnosis_id and dd.is_responsible = 1
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DIAG_DOCT_res2.png?raw=true)
=> The server is not sure that we have only one responsible doctor.
```sql
drop INDEX fix1_diagnosis_doctor on DIAGNOSIS_DOCTOR
```

