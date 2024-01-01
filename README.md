# SQL_XAMPP
### This is a note to use XAMPP to run MYSQL Query

1. Install XAMPP 
2. Start XAMPP and run 
*   MySQL Database, and 
*   Apache Web Server
3. Open browser and go to https://localhost/phpmyadmin/
4. You will find the database schema on the left panel
5. start terminal and run the following commend to start a new database
```bash
cd /Applications/XAMPP

mysql -u root -p
# press enter if the terminal ask for password
```

6. run the following comment to build a database schema
```bash
# a new database called G will be created
create database G;
```
-----------------------
### SQL Data type
```sql
INT             
DECIMAL(100, 2) 
VARCHAR(100)
BLOB
DATE
TIMESTAMP
```

### Simple database example
1. create table
```sql
CREATE TABLE student(
    student_id INT AUTO_INCREMENT,
    name VARCHAR(20) NOT NULL, 
    major VARCHAR(20) default 'undecided',
    PRIMARY KEY (student_id));
```

2. check the column data type
```sql
DESCRIBE student
```

3. add another column
```sql
-- ADD column gpa
ALTER TABLE student ADD gpa DECIMAL(3,2);

-- REMOVE the gpa column
ALTER TABLE student DROP COLUMN gpa;
```

4. delete the table
```sql
DROP TABLE student;
```

5. insert data
```sql
INSERT INTO student VALUES( 1, 'jack', 'IT');
INSERT INTO student (name) VALUES( 'Mary');
INSERT INTO student (name, major) VALUES( 'Sam', 'IT');

```

 6. update data
 ```sql
 UPDATE student
 SET major= 'biology'
 WHERE major= 'undecided'
 ```

7. Query
```sql
SELECT *
FROM student
ORDER BY major, name DESC 
LIMIT 2
-- comparision >, <, >=, <=, <>, !=, AND, OR, IS NULL
-- IN 
```
```sql
-- IN (criterias)
SELECT *
FROM student
WHERE name IN ('jack', 'Mary')
```
 ---------------------------------------------------

### More complex Company SQL database

```sql

CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);

CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;

ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);

CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);


-- -----------------------------------------------------------------------------

-- Corporate
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);

INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);

-- Scranton
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

-- Stamford
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);

INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');

UPDATE employee
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);


-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

-- CLIENT
INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
INSERT INTO client VALUES(401, 'Lackawana Country', 2);
INSERT INTO client VALUES(402, 'FedEx', 3);
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
INSERT INTO client VALUES(405, 'Times Newspaper', 3);
INSERT INTO client VALUES(406, 'FedEx', 2);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 55000);
INSERT INTO works_with VALUES(102, 401, 267000);
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);

```

### Company data queries
```sql
-- find all employees ordered by salary
SELECT * 
FROM employee
ORDER BY salary DESC
```

```sql
-- find all employees ordered by sex then name
SELECT * 
FROM employee
ORDER BY sex, first_name, last_name
```

```sql
-- find first 5 employees in the table
SELECT * 
FROM employee
ORDER BY sex, first_name, last_name
```

```sql
-- find first and last name of employees
SELECT first_name, last_name
FROM employee
```


```sql
-- find the forename (firstname) and surnames of all employees
SELECT first_name AS forename,last_name AS surname
FROM employee
```

```sql
-- find all the different genders
SELECT DISTINCT sex 
FROM employee; 
```

### queries using functions
```sql
-- find the number of employees
SELECT COUNT(emp_id)
FROM employee
-- ans = 9
```

```sql
-- find the number of female employees born after 1970
SELECT COUNT(emp_id)
FROM employee
WHERE sex = 'F' AND
birth_day > '1971-01-01'
```

```sql
-- find the average of all employee's salary
SELECT AVG(salary)
FROM employee
```

```sql
-- find the sum of all employee's salary
SELECT SUM(salary)
FROM employee
```

```sql
-- find out how many males and females there are
SELECT COUNT(sex), sex
FROM employee
GROUP BY sex
```

```sql
-- find the total sales of each salesman
SELECT SUM(total_sales), emp_id
FROM works_with
GROUP BY emp_id
```

```sql
-- find the total sales of each client spent
SELECT SUM(total_sales), client_id
FROM works_with
GROUP BY client_id
```

### wildcards
grap data in a certain pattern
```sql
-- % = any characters, _ = one character
-- find any client who are an LLC
SELECT * 
FROM client
WHERE client_name LIKE '%LLC'
```

```sql
-- find any branch supplier who are in the label business
-- supplier's name contains 'Label'
SELECT * 
FROM `branch_supplier` 
WHERE supplier_name LIKE '%LABEL%'

```

```sql
-- find any employee born in October
SELECT * 
FROM employee
WHERE birth_day LIKE '____-10%'
```

```sql
-- find any client who are school
SELECT * 
FROM client 
WHERE client_name LIKE '%school%'
```


### Union
put multiple SELECT statements into one
```sql
-- find a list of client name and branch supplier's name
SELECT client_name, branch_id
FROM client
UNION
SELECT supplier_name, branch_id
FROM branch_supplier
```

### Join
first add a record into the branch table
4 | Buffalo | Null | Null

```sql
INSERT INTO branch (branch_id, branch_name) VALUES (4, 'Buffalo')
```

```sql
-- find all branches and the names of their managers
SELECT employee.emp_id , employee.first_name, branch.branch_name, branch.mgr_id
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id
-- inner join will only show the non null matches
-- while left join and right join they will show the left or right columns with and without matches (will show null if no matches)
```

### Nested queries
```sql
-- find names of all employees who have sold
-- over 30,000 to a single client

-- step 1, get all the employee who have sales >30000
SELECT works_with.emp_id
FROM works_with
WHERE works_with.total_sales > 300000

```

```sql
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN(
    SELECT works_with.emp_id
    FROM works_with
    WHERE works_with.total_sales > 30000
)
```

```sql
-- find all clients who are handled by the branch 
-- that Michael Scott manages
-- Assume you kn
-- step 1:
SELECT branch.branch_id
FROM branch
WHERE branch.mgr_id = 102

-- step 2:
SELECT client.client_name
FROM client
WHERE client.branch_id = (
    SELECT branch.branch_id
    FROM branch
    WHERE branch.mgr_id = 102
    LIMIT 1
)

```

### on delete
https://www.youtube.com/watch?v=HXV3zeQKqGY&t=8735s