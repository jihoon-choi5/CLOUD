## MySQL 쿼리 문제
### [1] 사원정보(EMPLOYEE) 테이블에서 사원번호, 이름, 급여, 업무, 입사일, 상사의 사원번호를 출력하시오. 이때 이름은 성과 이름을 연결하여 Name이라는 별칭으로 출력하시오.
```sql
select employee_id, concat(first_name, ' ', last_name) as Name, salary, job_id, hire_date, manager_id
from employees;
```


### [2] HR 부서에서 예산 편성 문제로 급여 정보 보고서를 작성하려고 한다. 사원정보 (EMPLOYEES) 테이블에서 급여가 $7,000~$10,000 범위 이외인 사람의 성과 이름(Name으로 별칭) 및 급여를 급여가 작은 순서로 출력하시오.
``` SQL
select concat(first_name, ' ', last_name) as Name, salary
from employees
where salary NOT between 7000 and 10000
order by salary;
```


### [3] 사원의 이름(last_name) 중에 ‘e’ 및 ‘o’ 글자가 포함된 사원을 출력하시오. 이때 머리글은 ‘e and o Name’라고 출력하시오.
```sql
select last_name as 'e AND o Name'
from employees
where last_name LIKE '%e%' and last_name LIKE '%o%';
```

### [4] HR 부서에서는 급여(salary)와 수당율(commission_pct)에 대한 지출 보고서를 작성하려고 한다. 이에 수당을 받는 모든 사원의 성과 이름(Name으로 별칭), 급여, 업무, 수당율을 출력하시오. 이때 급여가 큰 순서대로 정렬하되, 급여가 같으면 수당율이 큰 순서대로 정렬하시오.
```sql
select concat(first_name, ' ', last_name) as Name, salary, job_id, commission_pct, (salary * commission_pct) as bonus
from employees
where commission_pct is not null
order by salary DESC, commission_pct DESC;
```

### [5] 사원들의 업무별 전체 급여 평균이 $10,000보다 큰 경우를 조회하여 업무, 급여 평균을 출력하시오. 단 업무에 사원(CLERK)이 포함된 경우는 제외하고 전체 급여 평균이 높은 순서대로 출력하시오.
```sql
select job_id, avg(salary) as 'Avg Salary' from employees
where job_id not like '%CLERK%'
group by job_id
having avg(salary) > 10000
order by avg(salary) DESC;
```

### [6] 각 이름이 ‘s’로 끝나는 사원들의 이름과 업무를 아래의 예와 같이 출력하고자 한다. 출력 시 성과 이름은 첫 글자가 대문자, 업무는 모두 대문자로 출력하고 머리글은 Employee JOBs로 표시하시오.
- 예 Sigal Tobias is a PU_CLERK
```sql
select concat(first_name , ' ', last_name,' is a ',  upper(job_id)) as 'Employee JOBs.'
from employees
where first_name LIKE '%s';
```


### [7] 모든 사원의 연봉을 표시하는 보고서를 작성하려고 한다. 보고서에 사원의 성과 이름(Name으로 별칭), 급여, 수당여부에 따른 연봉을 포함하여 출력하시오. 수당여부는 수당이 있으면 “Salary + Commission”, 수당이 없으면 “Salary only”라고 표시하고, 별칭은 적절히 붙인다. 또한 출력 시 연봉이 높은 순으로 정렬한다.
```sql
select concat(first_name,' ',last_name) as 'Name'
		,salary
        ,(salary*12) + (salary * 12 * ifnull(commission_pct,0)) as 'Anuual Salary'
        ,if(commission_pct is not null,'Salary + Commission', 'Salary only' ) as 'Salary Type'
from employees
order by salary DESC;
```


### [8] 모든 사원들 성과 이름(Name으로 별칭), 입사일 그리고 입사일이 어떤 요일이였는지 출력하시오. 이때 주(week)의 시작인 일요일부터 출력되도록 정렬하시오.
```sql
select concat(first_name, ' ', last_name) as 'Name'
		, hire_date
        , date_format(hire_date, '%W') as 'Week Day'
       , count(*) as count
from employees
group by date_format(hire_date, '%W')
order by date_format(hire_date, '%w');

```

### [9] 모든 사원은 직속 상사 및 직속 직원을 갖는다. 단, 최상위 또는 최하위 직원은 직속상사 및 직원이 없다. 소속된 사원들 중 어떤 사원의 상사로 근무 중인 사원의 총 수를 출력하시오.
```sql
select count( distinct manager_id)
from employees
```

### [10] 각 사원이 소속된 부서별로 급여 합계, 급여 평균, 급여 최대값, 급여 최소값을 집계하고자 한다. 계산된 출력값은 6자리와 세 자리 구분기호, $ 표시와 함께 출력하고 부서번호의 오름차순 정렬하시오. 단, 부서에 소속되지 않은 사원에 대한 정보는 제외하고 출력시 머리글은 칭(alias) 처리하시오.
```sql
select department_id, concat("$",format(sum(salary),0)) as'sum'
	, concat("$",format(avg(salary),2)) as 'avg'
    , concat("$",format(max(salary),0)) as 'max'
    , concat("$",format(min(salary),0)) as 'min'
from employees
where department_id is not null
group by department_id;
```
## [11] 가장 많은 부하직원을 가지고 있는 사원
```sql
select employee_id, max(cnt)
from 
	(select employee_id , count(manager_id) as cnt
	from hr.employees 
	group by manager_id) as result;
```
