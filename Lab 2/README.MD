LAB 2 - BASIC SELECT, ALIASES AND FUNCTIONS
===========================================

ABOUT THIS LAB
--------------
Basic SELECT statements: aliases, DISTINCT, ORDER BY, arithmetic, aggregate functions (SUM, MAX, MIN, AVG, COUNT), string functions 
(UPPER, SUBSTR, CONCAT, TRIM, LENGTH), REGEXP, LIMIT and ROUND.

Number of questions: 19
Database used: hr

FILES
-----
queries2.txt        The questions and their queries (one query per line).
hr_database.sql      Creates and fills the hr database.
Lab_2_Outputs.pdf   Screenshots of each question, its query and its output.

REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client

HOW TO USE
----------
1. Open the MySQL Command Line Client and log in.
2. Load the database:  source C:/sqlshots/hr_database.sql
3. Select it:  use hr;
4. Run each query from queries2.txt and check the output.

NOTES
-----
- Q17 returns an empty set because no first name contains a digit. This is the correct answer.
- Q13 does not use a table. The answer is 37219.
- Row counts and some values in Lab_2_Outputs.pdf come from the database version used when the screenshots were taken. With the current hr_database.sql a few counts may differ slightly.

QUESTIONS AND QUERIES
---------------------

Q1. Write a query to display the names (first_name, last_name) using alias name "First Name", "Last Name"
Query: select first_name as `First Name`, last_name as `Last Name` from employees;

Q2. Write a query to get unique department ID from employee table.
Query: select distinct department_id from employees;

Q3. Write a query to get all employee details from the employee table order by first name, descending.
Query: select * from employees order by first_name desc;

Q4. Write a query to get the names (first_name, last_name), salary, PF of all the employees (PF is calculated as 15% of salary).
Query: select first_name, last_name, salary, salary * 0.15 as PF from employees;

Q5. Write a query to get the employee ID, names (first_name, last_name), salary in ascending order of salary.
Query: select employee_id, first_name, last_name, salary from employees order by salary asc;

Q6. Write a query to get the total salaries payable to employees.
Query: select sum(salary) as total_salary from employees;

Q7. Write a query to get the maximum and minimum salary from employees table.
Query: select max(salary) as maximum_salary, min(salary) as minimum_salary from employees;

Q8. Write a query to get the average salary and number of employees in the employees table.
Query: select avg(salary) as average_salary, count(*) as no_of_employees from employees;

Q9. Write a query to get the number of employees working with the company.
Query: select count(*) as no_of_employees from employees;

Q10. Write a query to get the number of jobs available in the employees table.
Query: select count(distinct job_id) as no_of_jobs from employees;

Q11. Write a query get all first name from employees table in upper case.
Query: select upper(first_name) as first_name_upper from employees;

Q12. Write a query to get the first 3 characters of first name from employees table.
Query: select substr(first_name, 1, 3) as first_3_chars from employees;

Q13. Write a query to calculate 171*214+625.
Query: select 171 * 214 + 625 as result;

Q14. Write a query to get the names (for example Ellen Abel, Sundar Ande etc.) of all the employees from employees table.
Query: select concat(first_name, ' ', last_name) as employee_name from employees;

Q15. Write a query to get first name from employees table after removing white spaces from both side.
Query: select trim(first_name) as first_name_trimmed from employees;

Q16. Write a query to get the length of the employee names (first_name, last_name) from employees table.
Query: select first_name, last_name, length(first_name) + length(last_name) as `Length of Names` from employees;

Q17. Write a query to check if the first_name fields of the employees table contains numbers.
Query: select * from employees where first_name regexp '[0-9]';

Q18. Write a query to select first 10 records from a table.
Query: select * from employees limit 10;

Q19. Write a query to get monthly salary (round 2 decimal places) of each and every employee. Note : Assume the salary field provides the 'annual salary' information.
Query: select first_name, last_name, round(salary / 12, 2) as monthly_salary from employees;
