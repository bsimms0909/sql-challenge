# sql-challenge

Entity Relationship Diagram of the tables
# The Departments:
Departments
Department_Emp
Department_Mgr
Employees
Salaries
Titles

# The relationships between the departments
Employees (emp_no) -> Salaries (emp_no)
Employees (emp_title_id) -> Titles (title_id)
Department_Emp (emp_no) -> Employees (emp_no)
Department_Emp (dept_no) -> Departments (dept_no)
Department_Mgr (emp_no) -> Employees (emp_no)
Department_Mgr (dept_no) -> Departments (dept_no)

Data Analysis

--List the employee number, last name, first name, sex, and salary of each employee.
-- aliases Employees as E, Salaries as S
SELECT E.emp_no, E.last_name, E.first_name, E.sex, S.salary
FROM Employees AS E
INNER JOIN Salaries AS S ON E.emp_no = S.emp_no;

--List the first name, last name, and hire date for the employees who were hired in 1986.
SELECT first_name, last_name, hire_date
FROM Employees
WHERE EXTRACT(YEAR FROM hire_date) = 1986;

--List the manager of each department along with their department number, department name, employee number, last name, and first name.
--aliases :Department_Mgr as DM, Departments as D, Employees as E.
SELECT
    DM.emp_no AS manager_emp_no,
    D.dept_no,
    D.dept_name,
    E.emp_no AS employee_emp_no,
    E.last_name,
    E.first_name
FROM Department_Mgr DM
JOIN Departments D ON DM.dept_no = D.dept_no
JOIN Employees E ON DM.emp_no = E.emp_no;

--List the department number for each employee along with that employee’s employee number, last name, first name, and department name.
-- aliases Department_Emp as DE, Employees as E, Departments as D.
SELECT
    DE.emp_no AS employee_emp_no,
    E.last_name,
    E.first_name,
    DE.dept_no,
    D.dept_name
FROM Department_Emp DE
JOIN Employees E ON DE.emp_no = E.emp_no
JOIN Departments D ON DE.dept_no = D.dept_no;

--List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.
--% is a wildcard that represents any number of characters.
SELECT first_name, last_name, sex
FROM Employees
WHERE first_name = 'Hercules' AND last_name LIKE 'B%';

--List each employee in the Sales department, including their employee number, last name, and first name.
-- Aliases: Employees as E, Department_Emp as DE
SELECT E.emp_no, E.last_name, E.first_name
FROM Employees E
JOIN Department_Emp DE ON E.emp_no = DE.emp_no
WHERE DE.dept_no IN ('d007'); 


--List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.
--aliases :Employees as E, Department_Emp as DE, Departments as D.
SELECT E.emp_no, E.last_name, E.first_name, D.dept_name
FROM Employees E
INNER JOIN Department_Emp DE ON E.emp_no = DE.emp_no
INNER JOIN Departments D ON DE.dept_no = D.dept_no
WHERE D.dept_name IN ('Sales', 'Development');

--List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).
SELECT last_name, COUNT(*) as name_count
FROM Employees
GROUP BY last_name
ORDER BY name_count DESC, last_name;
