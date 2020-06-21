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

*We want to explain to the server that the count of rows DIAGNOSIS + DISEASE equals the count of rows DIAGNOSIS.*

### The solution
```sql
alter table DISEASE alter column disease_class_id int not NULL
alter table DISEASE alter column disease_number int not NULL
alter table DISEASE add CONSTRAINT PK_dbo_DISEASE_disease_class_id_disease_number primary key (disease_class_id, disease_number)

alter table DIAGNOSIS alter column disease_class_id int not NULL
alter table DIAGNOSIS alter column disease_number int not NULL
alter table DISEASE add CONSTRAINT FK_dbo_DIAGNOSIS_disease_class_id_disease_number FOREIGN key (disease_class_id, disease_number)
REFERENCES DISEASE(disease_class_id, disease_number)
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DISEAS_res1.png?raw=true)
SQL Server doesn't use a multicolumn foreign key for an optimization. :(
But the Foreign Key protects us.
```sql
-- inner join -> left join
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    left join DISEASE as ds ON ds.disease_class_id = d.disease_class_id and ds.disease_number = d.disease_number
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/DIAG_DISEAS_res2.png?raw=true)
