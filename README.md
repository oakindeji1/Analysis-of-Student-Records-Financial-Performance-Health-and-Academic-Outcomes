# Analysis-of-Student-Records-Financial-Performance-Health-and-Academic-Outcomes
###  Enhancing University Operations: Using SQL for a Comprehensive Analysis of Student Records, Financial Performance, Health, and Academic Outcomes
## Objectives: This project reflects is a multi-faceted analysis. From the dataset, I will cover critical aspects like:

- Enrollment Trends: Insights into student population growth.
- Fee Payment Performance: Evaluation of financial health and compliance.
- Student Health Status: Understanding the well-being of the student body.
- Academic Performance: Assessing educational outcomes and department effectiveness.
  
### Possible outline:


- create database
  
```
   create database university_records;
   use university_records
```
- creating tables

```
create table FactStudent
(
FactID int not null primary key,
StudentID int not null,
FeeID int not null,
MedicalID int not null,
ResultID int not null,
EnrollmentDate datetime
)

create table StudentTable
(
StudentID int not null,
Full_Name varchar(60) not null,
DOB Datetime not null,
Gender varchar(10) not null,
Department varchar(30)
)

create table feestable
(
FeeID int not null,
Totalfees decimal not null,
PaidAmount decimal not null,
paymentstatus varchar(20)
)

create table Medicaltable
(
MedicalID int not null,
HealthStatus varchar not null,
LastCheckupDate datetime
)

create table Resultstable
(
ResultID int not null,
GPA int,
Graduatestatus varchar(20),
Completion_year datetime
)

```

- Reading the tables

```
select * from [dbo].[FactStudent]
select * from [dbo].[Feestable]
select * from [dbo].[Medicaltable]
select * from [dbo].[Resultstable]
select * from [dbo].[Studenttable]
```
![image](https://github.com/user-attachments/assets/56ffd92e-dd32-46ed-9677-9d89957df997)

### Enrollment Dynamics:
- Focusing on patterns in student admissions over time and by department.

- Getting the total number of student Enrolled
```  
select count(*) from FactStudent
```
![image](https://github.com/user-attachments/assets/d542ddfe-6488-4743-ba9e-7b921dc23e60)

- Getting to know the year that has the highest enrollment
```
SELECT 
    YEAR(EnrollmentDate) AS EnrollmentYear, 
    COUNT(*) AS TotalEnrollments
FROM FactStudent
GROUP BY YEAR(EnrollmentDate)
ORDER BY EnrollmentYear;
```
![image](https://github.com/user-attachments/assets/cdf27fc5-83b5-4360-8383-42bb9f0bde01)

- Geting the student that pays highest fees
```
SELECT fs.StudentID,ft.Totalfee, ft.PaidAmount, ft.Balance
FROM FactStudent fs
JOIN FeesTable ft
ON ft.FeeID = fs.FeeID
WHERE ft.TotalFee = (
    SELECT MAX(TotalFee)
    FROM FeesTable
);
```
![image](https://github.com/user-attachments/assets/8ecc2555-eefa-4133-8e53-4126220423f3)

- Getting the student with the highest GPA
```
SELECT st.fullName,rt.GPA
FROM FactStudent fs
JOIN ResultsTable rt
ON fs.FactID = rt.ResultID
JOIN StudentTable st
ON fs.FactID = st.StudentID
WHERE rt.GPA = (
    SELECT MAX(GPA)
    FROM Resultstable
);
```
![image](https://github.com/user-attachments/assets/41290c2e-2228-4bd5-873e-af2702bc0f81)

- Getting the graduation rate
```  
SELECT 
    d.Department,
    COUNT(fs.FactID) AS TotalStudents,
    AVG(dr.GPA) AS AvgGPA,
    SUM(CASE WHEN dr.Graduated = 'Yes' THEN 1 ELSE 0 END) AS Graduates,
    ROUND(SUM(CASE WHEN dr.Graduated = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(fs.FactID), 2) AS GraduationRate
FROM FactStudent fs
JOIN Studenttable d ON fs.StudentID = d.StudentID
JOIN Resultstable dr ON fs.ResultID = dr.ResultID
GROUP BY d.Department
ORDER BY AvgGPA DESC;
```
![image](https://github.com/user-attachments/assets/983aab7f-654b-4fe4-baf3-773c4f888a05)

### Financial Health of Students:
- Analyze fee payment trends and outstanding balances.
- Getting the number of Students that owes
```
select count(*) from [dbo].[Feestable]
where PaymentStatus = 'Partial'
```
![image](https://github.com/user-attachments/assets/86a7eaf9-5905-4bce-ad94-a2966070a6ce)

- Working on the fees Table using some fomulars
```
--Adding Balance row
Alter table [dbo].[Feestable]
add balance decimal(18,2) 

select balance * from [dbo].[Feestable]
select (TotalFee - PaidAmount) as balance from [dbo].[Feestable]
select FeeID, TotalFee,PaidAmount,(TotalFee - PaidAmount) as balance from [dbo].[Feestable]

update [dbo].[Feestable]
set balance = (TotalFee - PaidAmount)
where PaymentStatus = 'Partial'
select * from [dbo].[Feestable]

--Getting the max, Min and the Average Balance
select max(Balance) from [dbo].[Feestable]
select min(Balance) from [dbo].[Feestable]
select Avg(Balance) from [dbo].[Feestable]

--Getting the total Fees and Balance
select Sum(Balance) from [dbo].[Feestable]
select Sum(Totalfee) from [dbo].[Feestable]
select Avg(Totalfee) from [dbo].[Feestable]
```
```
SELECT 
    d.Department, 
    COUNT(fs.FactID) AS TotalStudents, 
    AVG(df.TotalFee) AS AvgTotalFee, 
    AVG(df.Balance) AS AvgOutstandingBalance,
    SUM(CASE WHEN df.PaymentStatus = 'Paid' THEN 1 ELSE 0 END) AS FullyPaidStudents
FROM FactStudent fs
JOIN Studenttable d ON fs.StudentID = d.StudentID
JOIN Feestable df ON fs.FeeID = df.FeeID
GROUP BY d.Department
ORDER BY AvgOutstandingBalance DESC;
```
![image](https://github.com/user-attachments/assets/1b980825-903a-48b9-adc4-1aa529240b6f)


### Student Well-being:
- Investigate health trends and their impact on academic success.
```
SELECT 
    dm.HealthStatus, 
    COUNT(*) AS TotalStudents,
    ROUND((COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Medicaltable)), 2) AS Percentage
FROM Medicaltable dm
GROUP BY dm.HealthStatus
ORDER BY TotalStudents DESC;
```
![image](https://github.com/user-attachments/assets/0557a8f3-54a0-4d5d-bd06-e10e3850ab8b)

### Academic Success Metrics:
- GPA trends, graduation rates, and departmental performance.
### Data-Driven Recommendations:
- Strategies to improve enrollment, fee compliance, student support, and academic outcomes.
