# plans
### Create the database *HOUSE*
```sql
CREATE DATABASE HOUSE
go
USE HOUSE
go
CREATE TABLE [dbo].[DISEASE](
    [disease_class_id] [int] NULL,
    [disease_number] [int] NULL,
    [disease_name] [varchar](100) NULL)
go
CREATE TABLE [dbo].[DIAGNOSIS](
    [diagnosis_id] [int] NULL,
    [patient_id] [int] NULL,
    [hospital_id] [int] NULL,
    [disease_class_id] [int] NULL,
    [disease_number] [int] NULL)
go
CREATE TABLE [dbo].[DIAGNOSIS_DOCTOR](
    [diagnosis_doctor_id] [int] NULL,
    [diagnosis_id] [int] NULL,
    [doctor_id] [int] NULL,
    [is_responsible] [int] NULL)
go
CREATE TABLE [dbo].[DOCTOR](
    [doctor_id] [int] NULL,
    [doctor_name] [varchar](100) NULL)
go    
CREATE TABLE [dbo].[PATIENT](
    [patient_id] [int] NULL,
    [patient_name] [varchar](100) NULL)
```
### Optimize the query
```sql
------------------------------------------
SET STATISTICS XML ON
------------------------------------------
declare @hospital_id int = 5

select count(*)
from DIAGNOSIS as d
    inner join PATIENT as p on p.patient_id = d.patient_id
    inner join DISEASE as ds ON ds.disease_class_id = d.disease_class_id and ds.disease_number = d.disease_number
    inner join DIAGNOSIS_DOCTOR as dd on dd.diagnosis_id = d.diagnosis_id and dd.is_responsible = 1
    inner join DOCTOR as dct on dd.doctor_id = dct.doctor_id 
where hospital_id = @hospital_id
------------------------------------------
SET STATISTICS XML OFF
------------------------------------------
```
![image](https://github.com/mechtal/plans/blob/master/Query_main.png?raw=true)

the_first_query.md
