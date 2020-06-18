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
![image](https://github.com/mechtal/plans/blob/master/Query1.png?raw=true)
