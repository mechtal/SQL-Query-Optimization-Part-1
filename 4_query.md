### The query
```sql
DIAGNOSIS_DOCTOR and DOCTOR
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DOCT_DOCT.png?raw=true)

### The solution
```sql
alter table DOCTOR alter COLUMN doctor_id int not null
alter table DOCTOR add CONSTRAINT PK_dbo_DOCTOR_doctor_id PRIMARY key (doctor_id)

alter table DIAGNOSIS_DOCTOR alter column doctor_id int not null
alter table DIAGNOSIS_DOCTOR add CONSTRAINT FK_dbo_DIAGNOSIS_DOCTOR foreign key (doctor_id)
REFERENCES DOCTOR(doctor_id)

inner join -> left join
```
```sql
select count(*)
from DIAGNOSIS as d
    inner join PATIENT as p on p.patient_id = d.patient_id
    left join DISEASE as ds ON ds.disease_class_id = d.disease_class_id and ds.disease_number = d.disease_number
    outer apply (select top 1 dd.diagnosis_id, dd.doctor_id
                 from DIAGNOSIS_DOCTOR as dd
                 where is_responsible = 1
                 and dd.diagnosis_id = d.diagnosis_id) as dd
    left join DOCTOR as dct on dd.doctor_id = dct.doctor_id 
where hospital_id = @hospital_id
```
![image](https://github.com/mechtal/plans/blob/master/Query_main_res.png?raw=true)
