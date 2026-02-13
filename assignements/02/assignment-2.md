# DAT151: Assignment 2 Report

**Group:** 2

**Group Members:** Soukup Jan, Fabienne Feilke

**Date:** February , 2026

---

## Task 1: DBMS statements and utilities

Create some tables as a normal database user (i.e. not root), insert some test data into the tables, and test the following commands and/or utilities one by one. Try all commands both on a InnoDB and a MyISAM table. Document the input and output, and explain what you did in your words.

Use the MariaDB Knowledge Base to solve this task.

TODO commands


---

## Task 2: Normalisation

This task is theory only. You do not need to implement anything on the lab computers.

Below a relation (table) is given, including example data.

You can assume that ENAME is atomic.

What normal form does it currently conform to? Normalize it to 3NF if it does not currently conform to 3NF.

### Answers:

//TODO This should be fine, just recheck

- The schema currently conforms to the 1NF (atomic values)
- It violates the 2NF for the following reasons:
    - ENAME depends only on the SSN, which is only a part of the composite primary key
    - PNAME and PLOCATION depend on PNUMBER, which is also only a part of the composite primary key
- We can normalize the schema into 3NF by making it more tables - 3 tables to be exact - EMP (Employee), PROJ (Project) and EMP_PROJ. The tables will have following data:
    - EMP (SSN, ENAME)
    - PROJ (PNUMBER, PNAME, PLOCATION)
    - EMP_PROJ (SSN, PNUMBER, HOURS)
        - SSN and PNUMBER are both foreign keys referenced from EMP and PROJ tables
- After the changes, the schema is in the 3NF, actually even in BCNF, because all functional dependencies are either trivial or have full candidate key on the left side

---

## Task 3: More normalization

This task is theory only. You do not need to implement anything on the lab computers.

Below a relation (table) is given, including example data.

You can assume that ENAME and ADDRESS are atomic.

Which normal form does it currently conform to? Normalize it to 3NF if it does not currently
conform to 3NF.

### Answers:

//TODO Rewrite this and check it again

- The schema currently conforms to the 1NF (atomic values)
- It also conform the 2NF (no non-key attribute is dependent on part of a composite key),
    - 2NF allows non-key attribute to be dependent on a non-key attribute, so DNUMBER → DNAME, DMGRSSN is okay
- It violates the 3NF, because non-key attributes DNAME and DMGRSSN are dependent on the key transitively using another non-key attribute DNUMBER
- We can normalize it to the 3NF by making it into 2 tables - EMP and DEPT
- EMP (SSN, ENAME, BDATE, ADDRESS, DNUMBER)
    - DNUMBER is foreign key referenced from the DEPT table
- DEPT (DNUMBER, DNAME, DMGRSSN)

## Task 4: Normalisation & Denormalisation
