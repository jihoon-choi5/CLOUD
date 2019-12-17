## [Entity]


### 1.교수 
professor(**ssn**, name, age, rank, speciality)

### 2. 학과
dept(**dno**, dname, office, ***run_professor_ssn***)

### 3. 대학원생
graduate(**ssn**, age, deg_prog, ***dept_dno***, ***graduate_ssn***)

### 4. 과제
project(**pid**, sponser, start_date, end_date, budget, ***manager_ssn***)

## [Relationship]

### 1. 교수의 학과 참여

### 2. 교수의 과제 수행

### 3. 대학원생의 과제 수행
