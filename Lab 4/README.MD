LAB 4 - AGGREGATE FUNCTIONS AND GROUP BY
========================================

ABOUT THIS LAB
--------------
Aggregate functions with GROUP BY and HAVING: COUNT, SUM, MIN, MAX, AVG.

Number of questions: 14
Database used: hr

FILES
-----
queries4.txt        The questions and their queries (one query per line).
hr_database.sql      Creates and fills the hr database.
Lab_4_Outputs.pdf   Screenshots of each question, its query and its output.

REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client

HOW TO USE
----------
1. Open the MySQL Command Line Client and log in.
2. Load the database:  source C:/sqlshots/hr_database.sql
3. Select it:  use hr;
4. Run each query from queries4.txt and check the output.

NOTES
-----
- Q10 shows a NULL group for the employee who has no department.
- Q14 returns an empty set because no department has more than 10 employees. The query is correct.
- Q9 leaves out the top employee, who has no manager (NULL manager_id).
- Row counts and some values in Lab_4_Outputs.pdf come from the database version used when the screenshots were taken. With the current hr_database.sql a few counts may differ slightly.

QUESTIONS AND QUERIES
---------------------

Q1. Write a query to list the number of jobs available in the employees table.
Query: select count(distinct job_id) as no_of_jobs from employees;

Q2. Write a query to get the total salaries payable to employees.
Query: select sum(salary) as total_salary from employees;

Q3. Write a query to get the minimum salary from employees table.
Query: select min(salary) as minimum_salary from employees;

Q4. Write a query to get the maximum salary of an employee working as a Programmer.
Query: select max(salary) as maximum_salary from employees where job_id = 'IT_PROG';

Q5. Write a query to get the average salary and number of employees working the department 90.
Query: select avg(salary) as average_salary, count(*) as no_of_employees from employees where department_id = 90;

Q6. Write a query to get the highest, lowest, sum, and average salary of all employees.
Query: select max(salary) as highest_salary, min(salary) as lowest_salary, sum(salary) as total_salary, avg(salary) as average_salary from employees;

Q7. Write a query to get the number of employees with the same job.
Query: select job_id, count(*) as no_of_employees from employees group by job_id;

Q8. Write a query to get the difference between the highest and lowest salaries.
Query: select max(salary) - min(salary) as salary_difference from employees;

Q9. Write a query to find the manager ID and the salary of the lowest-paid employee for that manager.
Query: select manager_id, min(salary) as lowest_salary from employees where manager_id is not null group by manager_id;

Q10. Write a query to get the department ID and the total salary payable in each department.
Query: select department_id, sum(salary) as total_salary from employees group by department_id;

Q11. Write a query to get the average salary for each job ID excluding programmer.
Query: select job_id, avg(salary) as average_salary from employees where job_id <> 'IT_PROG' group by job_id;

Q12. Write a query to get the total salary, maximum, minimum, average salary of employees (job ID wise), for department ID 90 only.
Query: select job_id, sum(salary) as total_salary, max(salary) as maximum_salary, min(salary) as minimum_salary, avg(salary) as average_salary from employees where department_id = 90 group by job_id;

Q13. Write a query to get the job ID and maximum salary of the employees where maximum salary is greater than or equal to $4000.
Query: select job_id, max(salary) as maximum_salary from employees group by job_id having max(salary) >= 4000;

Q14. Write a query to get the average salary for all departments employing more than 10 employees.
Query: select department_id, avg(salary) as average_salary from employees group by department_id having count(*) > 10;
