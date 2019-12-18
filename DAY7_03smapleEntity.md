## 1.Entity

> **굵은글자**는 기본키, ***기울어진 글자*** 참조키

### 1.1교수 
professor(**ssn**, name, age, rank, speciality)

### 1.2 학과
dept(**dno**, dname, office, *run_professor_ssn*)

### 1.3 대학원생
graduate(**ssn**, age, deg_prog, *dept_dno*, *graduate_ssn*)

### 1.4 과제
project(**pid**, sponser, start_date, end_date, budget, *manager_ssn*)


## 2.[Relationship]

### 2.1 교수의 학과 참여 > 6번
Work_dept(***professor_ssn***, ***dept_dno***, pct_time참여율)

### 2.2 교수의 과제 수행 > 10번
Work_in(***professor_ssn***, ***pid***)

### 2.3 대학원생의 과제 수행 >11번
Work_prog(***graduate_ssn***, ***pid***)
