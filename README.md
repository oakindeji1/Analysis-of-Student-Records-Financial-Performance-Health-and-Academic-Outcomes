# Analysis-of-Student-Records-Financial-Performance-Health-and-Academic-Outcomes
###  Enhancing University Operations: Using SQL for a Comprehensive Analysis of Student Records, Financial Performance, Health, and Academic Outcomes
## Objectives: This project reflects is a multi-faceted analysis. From the dataset, I will cover critical aspects like:

- Enrollment Trends: Insights into student population growth.
- Fee Payment Performance: Evaluation of financial health and compliance.
- Student Health Status: Understanding the well-being of the student body.
- Academic Performance: Assessing educational outcomes and department effectiveness.
  
### Possible outline:

### Enrollment Dynamics:
- Focusing on patterns in student admissions over time and by department.
- create database
  
```
   create database university_records;
   use university_records
```
creating tables

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

### Financial Health of Students:
- Analyze fee payment trends, outstanding balances, and financial aid implications.

### Student Well-being:
- Investigate health trends and their impact on academic success.
### Academic Success Metrics:
- GPA trends, graduation rates, and departmental performance.
### Data-Driven Recommendations:
- Strategies to improve enrollment, fee compliance, student support, and academic outcomes.
