LAB 5 - SUBQUERIES
==================

ABOUT THIS LAB
--------------
Single-row, multi-row and correlated subqueries: IN, NOT IN, ALL, comparison with AVG, nth highest and lowest salary using LIMIT and OFFSET, ROW_NUMBER().

Number of questions: 22
Database used: hr

FILES
-----
queries5.txt        The questions and their queries (one query per line).
hr_database.sql      Creates and fills the hr database.
Lab_5_Outputs.pdf   Screenshots of each question, its query and its output.

REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client

HOW TO USE
----------
1. Open the MySQL Command Line Client and log in.
2. Load the database:  source C:/sqlshots/hr_database.sql
3. Select it:  use hr;
4. Run each query from queries5.txt and check the output.

NOTES
-----
- Q15 and Q18 need MySQL 8.0 or newer (Q15 uses ROW_NUMBER).
- Q6 compares the salary with the min_salary of the employee's job in the jobs table.
- Q22 has the position fixed at 3 (third highest salary). Change the 3 in the query to get another position.
- Q19 lists departments that have no employees (Treasury and Corporate Tax).
- Row counts and some values in Lab_5_Outputs.pdf come from the database version used when the screenshots were taken. With the current hr_database.sql a few counts may differ slightly.

QUESTIONS AND QUERIES
---------------------

Q1. Write a query to find the name (first_name, last_name) and the salary of the employees who have a higher salary than the employee whose last_name='Bull'.
Query: select first_name, last_name, salary from employees where salary > (select salary from employees where last_name = 'Bull');

Q2. Write a query to find the name (first_name, last_name) of all employees who works in the IT department.
Query: select first_name, last_name from employees where department_id = (select department_id from departments where department_name = 'IT');

Q3. Write a query to find the name (first_name, last_name) of the employees who have a manager and worked in a USA based department. Hint : Write single-row and multiple-row subqueries
Query: select first_name, last_name from employees where manager_id is not null and department_id in (select department_id from departments where location_id in (select location_id from locations where country_id = 'US'));

Q4. Write a query to find the name (first_name, last_name) of the employees who are managers.
Query: select first_name, last_name from employees where employee_id in (select manager_id from employees where manager_id is not null);

Q5. Write a query to find the name (first_name, last_name), and salary of the employees whose salary is greater than the average salary.
Query: select first_name, last_name, salary from employees where salary > (select avg(salary) from employees);

Q6. Write a query to find the name (first_name, last_name), and salary of the employees whose salary is equal to the minimum salary for their job grade.
Query: select first_name, last_name, salary from employees where salary = (select min_salary from jobs where jobs.job_id = employees.job_id);

Q7. Write a query to find the name (first_name, last_name), and salary of the employees who earns more than the average salary and works in any of the IT departments.
Query: select first_name, last_name, salary from employees where salary > (select avg(salary) from employees) and department_id in (select department_id from departments where department_name like 'IT%');

Q8. Write a query to find the name (first_name, last_name), and salary of the employees who earns more than the earning of Mr. Bell.
Query: select first_name, last_name, salary from employees where salary > (select salary from employees where last_name = 'Bell');

Q9. Write a query to find the name (first_name, last_name), and salary of the employees who earn the same salary as the minimum salary for all departments.
Query: select first_name, last_name, salary from employees where salary in (select min(salary) from employees group by department_id);

Q10. Write a query to find the name (first_name, last_name), and salary of the employees whose salary is greater than the average salary of all departments.
Query: select first_name, last_name, salary from employees where salary > all (select avg(salary) from employees group by department_id);

Q11. Write a query to find the name (first_name, last_name) and salary of the employees who earn a salary that is higher than the salary of all the Shipping Clerk (JOB_ID = 'SH_CLERK'). Sort the results of the salary of the lowest to highest.
Query: select first_name, last_name, salary from employees where salary > all (select salary from employees where job_id = 'SH_CLERK') order by salary asc;

Q12. Write a query to find the name (first_name, last_name) of the employees who are not supervisors.
Query: select first_name, last_name from employees where employee_id not in (select manager_id from employees where manager_id is not null);

Q13. Write a query to display the employee ID, first name, last name, and department names of all employees.
Query: select e.employee_id, e.first_name, e.last_name, d.department_name from employees e left join departments d on e.department_id = d.department_id;

Q14. Write a query to display the employee ID, first name, last name, salary of all employees whose salary is above average for their departments.
Query: select employee_id, first_name, last_name, salary from employees e where salary > (select avg(salary) from employees where department_id = e.department_id);

Q15. Write a query to fetch even numbered records from employees table.
Query: select * from (select e.*, row_number() over (order by employee_id) as row_num from employees e) t where mod(row_num, 2) = 0;

Q16. Write a query to find the 5th maximum salary in the employees table.
Query: select distinct salary as fifth_max_salary from employees order by salary desc limit 1 offset 4;

Q17. Write a query to find the 4th minimum salary in the employees table.
Query: select distinct salary as fourth_min_salary from employees order by salary asc limit 1 offset 3;

Q18. Write a query to select last 10 records from a table.
Query: select * from (select * from employees order by employee_id desc limit 10) t order by employee_id asc;

Q19. Write a query to list the department ID and name of all the departments where no employee is working.
Query: select department_id, department_name from departments where department_id not in (select department_id from employees where department_id is not null);

Q20. Write a query to get 3 maximum salaries.
Query: select distinct salary from employees order by salary desc limit 3;

Q21. Write a query to get 3 minimum salaries.
Query: select distinct salary from employees order by salary asc limit 3;

Q22. Write a query to get nth max salaries of employees.
Query: select distinct salary from employees e1 where 3 = (select count(distinct salary) from employees e2 where e2.salary >= e1.salary);
