LAB 6 - JOINS
=============

ABOUT THIS LAB
--------------
Joins: NATURAL JOIN, INNER JOIN, LEFT JOIN, self join, joins across three tables, GROUP BY with joins, DATEDIFF and TIMESTAMPDIFF.

Number of questions: 13
Database used: hr

FILES
-----
queries6.txt        The questions and their queries (one query per line).
hr_database.sql      Creates and fills the hr database.
Lab_6_Outputs.pdf   Screenshots of each question, its query and its output.

REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client

HOW TO USE
----------
1. Open the MySQL Command Line Client and log in.
2. Load the database:  source C:/sqlshots/hr_database.sql
3. Select it:  use hr;
4. Run each query from queries6.txt and check the output.

NOTES
-----
- Q3 shows the 8 Sales employees, who work in London, with the current hr_database.sql. An older version returned an empty set.
- Q5 shows the employees hired after Vance Jones (2007-03-17) with the current hr_database.sql. An older version returned an empty set.
- Q13 uses today's date, so the result depends on when you run it.
- Row counts and some values in Lab_6_Outputs.pdf come from the database version used when the screenshots were taken. With the current hr_database.sql a few counts may differ slightly.

QUESTIONS AND QUERIES
---------------------

Q1. Write a query to find the addresses (location_id, street_address, city, state_province, country_name) of all the departments. Hint : Use NATURAL JOIN.
Query: select location_id, street_address, city, state_province, country_name from departments natural join locations natural join countries;

Q2. Write a query to find the name (first_name, last name), department ID and name of all the employees.
Query: select e.first_name, e.last_name, e.department_id, d.department_name from employees e left join departments d on e.department_id = d.department_id;

Q3. Write a query to find the name (first_name, last_name), job, department ID and name of the employees who works in London.
Query: select e.first_name, e.last_name, e.job_id, e.department_id, d.department_name from employees e join departments d on e.department_id = d.department_id join locations l on d.location_id = l.location_id where l.city = 'London';

Q4. Write a query to find the employee id, name (last_name) along with their manager_id and name (last_name).
Query: select e.employee_id, e.last_name, e.manager_id, m.last_name as manager_last_name from employees e left join employees m on e.manager_id = m.employee_id;

Q5. Write a query to find the name (first_name, last_name) and hire date of the employees who was hired after 'Jones'.
Query: select first_name, last_name, hire_date from employees where hire_date > (select hire_date from employees where last_name = 'Jones');

Q6. Write a query to get the department name and number of employees in the department.
Query: select d.department_name, count(e.employee_id) as no_of_employees from departments d left join employees e on d.department_id = e.department_id group by d.department_name;

Q7. Write a query to find the employee ID, job title, number of days between ending date and starting date for all jobs in department 90.
Query: select h.employee_id, j.job_title, datediff(h.end_date, h.start_date) as no_of_days from job_history h join jobs j on h.job_id = j.job_id where h.department_id = 90;

Q8. Write a query to display the department ID and name and first name of manager.
Query: select d.department_id, d.department_name, e.first_name as manager_first_name from departments d join employees e on d.manager_id = e.employee_id;

Q9. Write a query to display the department name, manager name, and city.
Query: select d.department_name, concat(e.first_name, ' ', e.last_name) as manager_name, l.city from departments d join employees e on d.manager_id = e.employee_id join locations l on d.location_id = l.location_id;

Q10. Write a query to display the job title and average salary of employees.
Query: select j.job_title, avg(e.salary) as average_salary from employees e join jobs j on e.job_id = j.job_id group by j.job_title;

Q11. Write a query to display job title, employee name, and the difference between salary of the employee and minimum salary for the job.
Query: select j.job_title, concat(e.first_name, ' ', e.last_name) as employee_name, e.salary - j.min_salary as salary_difference from employees e join jobs j on e.job_id = j.job_id;

Q12. Write a query to display the job history that were done by any employee who is currently drawing more than 10000 of salary.
Query: select * from job_history where employee_id in (select employee_id from employees where salary > 10000);

Q13. Write a query to display department name, name (first_name, last_name), hire date, salary of the manager for all managers whose experience is more than 15 years.
Query: select d.department_name, e.first_name, e.last_name, e.hire_date, e.salary from departments d join employees e on d.manager_id = e.employee_id where timestampdiff(year, e.hire_date, curdate()) > 15;
