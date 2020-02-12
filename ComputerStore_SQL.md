# ComputerStore_SQL

## 1.Retrieve the names of all employees who work on at least one of the projects. (In other words, look at the list of projects given in the PROJECT table, and retrieve the names of all employees who work on at least one of them.) 
```
SELECT distinct FNAME, MINIT,LNAME
FROM EMPLOYEE
LEFT JOIN WORKS_ON ON SSN=ESSN
LEFT JOIN PROJECT ON PNUMBER=PNO
WHERE PNO is NOT NULL;
```

## 2.For each department, retrieve the department number,department name, and the average salary of all employees working in that department. Order the output by department number in ascending order. 
```
SELECT DNUMBER,DNAME, AVG(SALARY) FROM EMPLOYEE e INNER JOIN DEPARTMENT d 
ON e.DNO =d.DNUMBER
GROUP BY DNAME,  DNUMBER
ORDER BY DNUMBER;
```

## 3.List the last names of all department managers who have no dependents. 
```
SELECT LNAME
FROM DEPARTMENT
INNER JOIN EMPLOYEE ON MGR_SSN=SSN
WHERE SSN NOT IN( SELECT ESSN FROM DEPENDENT);
```

## 4.Determine the department that has the employee with the lowest salary among all employees.For this department retrieve the names of all employees. Write one query for this question. 
```
SELECT e2.FNAME,e2.MINIT,e2.LNAME FROM EMPLOYEE e2
WHERE e2.DNO IN
(
SELECT e1.DNO FROM EMPLOYEE e1
WHERE e1.SALARY IN (SELECT MIN(e.SALARY) s FROM EMPLOYEE e)
);
```

## 5.Find the total number of employees and the total number of dependents for every department (the number of dependents for a department is the sum of the number of dependents for each employeeworking for that department).Return the result as department name, total number of employees,and total number of dependents.
```
SELECT DNAME, count(distinct e1.SSN) as total_employees,count(d1.RELATIONSHIP) as dependents
FROM EMPLOYEE e1
LEFT JOIN DEPENDENT d1 ON e1.SSN=d1.ESSN
INNER JOIN DEPARTMENT d2 ON e1.DNO=d2.DNUMBER
GROUP BY DNAME;
```

## 6.Retrieve the names of employees whose salary is within $20,000 of the salary of the employee 
--who is paid the most in the company (e.g., if the highest salary in the company is $80,000, 
--retrieve the names of all employees that make at least $60,000).
```
SELECT e1.FNAME,e1.MINIT, e1.LNAME FROM employee e1
WHERE e1.SALARY >(SELECT MAX(e2.SALARY)-20000 FROM employee e2);
```




