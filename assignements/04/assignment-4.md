# DAT151: Assignment 4 Report

**Group:** 5

**Group Members:** Soukup Jan, Fabienne Feilke

**Date:** March ..., 2026

This report was written after Monday's 16.3. review during lab.

---

## Task 1: Triggers

*Objective: As a normal database user (i.e. not root), create triggers on table TheTable so that all changes of TheTable will be logged in LogTable. Make sure all values before and after a change are logged.*

**Requirements:**
* For DELETE, you must log the action DELETE, and the row before values.
* For INSERT, you must log the action INSERT, and the row after values.
* For UPDATE, you must log the action UPDATE, and both the row before and after values.
    * The LogTable must clearly identify the old and the new values.

**Delivery:**
* The DDL of the triggers
* The DDL of the tables
* Output that demonstrates the working of the triggers

---

## Task 2: Temporal database

*Objective: As a normal database user (i.e. not root), create a table AnotherTable as a System-Versioned table. Insert some data, then do some DELETEs, UPDATEs and INSERTs.*

**Requirements:**
* Do not manually create a ROW_START or ROW_END column.
* After all modifications are done, use queries with “FOR SYSTEM_TIME AS OF” to list the before and after values after each modification of the table.
* Every result set should show the state of the table at a specific point in time. Each result sets should include a row only once, showing the state of the row at that specific point in time.

---

## Task 3: Integrity Constraints

*Objective: As a normal database user (i.e. not root), create a teacher table, with numeric attributes for salary, bonus and total.*

**1. Implement the following integrity constraints:**
i. The attribute salary must be between 1 000 and 100 000.
ii. The attribute total must be the sum of salary and bonus, and calculated automatically.

*Insert some test data to verify whether your solution works. Document the output.*

**2. Is the table 3NF?**

---

## Task 4: Order of triggers

**1. Test the following, and give necessary explanations:**
a) Create three tables T1, T2, and T3.
b) Create a trigger tr12 before insert on T1, insert into T2 a row.
c) Create a trigger tr23 after insert on T2, insert into T3 a row.
d) Create a trigger tr13 after insert on T1, insert into T3 a row.
e) Insert a row into T1, in which order are the triggers fired? Why?

![Screenshot 1](images/Task4/Screenshot%20From%202026-03-15%2011-39-16.png)

First just creating simple tables to show the workings of triggers

![Screenshot 2](images/Task4/Screenshot%20From%202026-03-15%2011-39-30.png)

Creation of all triggers according to the task

![Screenshot 3](images/Task4/Screenshot%20From%202026-03-15%2011-44-48.png)

After simple insert into T1 we show the content of all tables

The triggers fire in the following order:

`TR12` -> `TR23` -> (Data is INSERTED into `T1`) -> `TR13`

This is because of the cascading triggers.

When we `INSERT` into `T1` the engine first checks for `BEFORE` triggers -> `TR12` fires first.

`TR12` executes `INSERT` into `T2` - trigger execution is synchronous, so MariaDB pauses the original operation `T1` to handle `T2`.

`INSERT` on `T2` fires its own `AFTER` trigger - `TR23`, which inserts a row into `T3`.

After the operations on `T2` (and its triggers) are complete, engine returns to `T1` and the value is insterted into `T1`.

Then the `AFTER INSERT` trigger `TR13` is fired on `T1`, which inserts the row into `T3`.

**2. Can there be any unexpected result or deadlock regarding the order of triggers?**

Yes, there can be unexpected results and deadlocks. If the triggers form a loop (for example T3 trigger would insert into T1) it would cause infinite recursion. Database engines usually catch this and throw a "maximum recursion depth exceeded" error, which crashes the original transaction.

Also a simple `INSERT` into `T1` can lock `T2` and `T3` with the triggers - could cause performance drops.

---

## Task 5: Pendant DELETE

*Objective: Implement the following as a normal database user (i.e. not root):*

1. Create two tables with a one-to-many relation, and implement referential integrity. (Create foreign keys on the “many”-table).
2. Add some rows to each table. The child table must have several rows referencing each row of the parent rows.
3. Use triggers to implement the rule known as pendant DELETE (Mullins page 435): A row in the parent table should be deleted if no rows in the child table refer to it any more (when you delete the last row in the child table referencing it).

**Explain the point of doing this and why it is difficult compared with other rules on p.435.**

![Screenshot 1](images/Task5/Screenshot%20From%202026-03-15%2011-52-43.png)

Screenshot From 2026-03-15 11-52-43

![Screenshot 2](images/Task5/Screenshot%20From%202026-03-15%2011-52-58.png)

Screenshot From 2026-03-15 11-52-58

![Screenshot 3](images/Task5/Screenshot%20From%202026-03-15%2012-14-57.png)

Screenshot From 2026-03-15 12-14-57

![Screenshot 4](images/Task5/Screenshot%20From%202026-03-15%2012-15-52.png)

Screenshot From 2026-03-15 12-15-52

![Screenshot 5](images/Task5/Screenshot%20From%202026-03-15%2012-17-00.png)

Screenshot From 2026-03-15 12-17-00

![Screenshot 6](images/Task5/Screenshot%20From%202026-03-15%2012-17-14.png)

Screenshot From 2026-03-15 12-17-14

![Screenshot 7](images/Task5/Screenshot%20From%202026-03-15%2012-17-29.png)

Screenshot From 2026-03-15 12-17-29


---

## Task 6: Concurrency

*Objective: An application uses a DBMS to store data about events and participants. Events can have many participants, but the number of spaces should still be limited.*

**6.1 Normalization and Data Loading**

1. Make a normalized, 3NF logical for the data provided in `data.txt` and implement the model in a MariaDB database using InnoDB tables. The model should have an attribute showing the total number of spaces at each event.
*(The fastest, and probably also easiest solution is to create a table that temporarily holds all the data. From this table, load data into the normalized tables and afterwards delete the auxiliary table.)*

![Step 1](images/Task6/part1-setup_&_load/1.png)

Step 1

![Step 2](images/Task6/part1-setup_&_load/2.png)

Step 2

![Step 3](images/Task6/part1-setup_&_load/3.png)

Step 3

![Step 4](images/Task6/part1-setup_&_load/4.png)

Step 4

![Step 5](images/Task6/part1-setup_&_load/5.png)

Step 5

![Screenshot 1](images/Task6/part1-setup_&_load/Screenshot%20From%202026-03-15%2012-43-14.png)

Screenshot From 2026-03-15 12-43-14

![Screenshot 2](images/Task6/part1-setup_&_load/Screenshot%20From%202026-03-15%2012-43-43.png)

Screenshot From 2026-03-15 12-43-43

![Screenshot 3](images/Task6/part1-setup_&_load/Screenshot%20From%202026-03-15%2012-43-58.png)

Screenshot From 2026-03-15 12-43-58

![Screenshot 4](images/Task6/part1-setup_&_load/Screenshot%20From%202026-03-15%2012-46-02.png)

Screenshot From 2026-03-15 12-46-02

![Screenshot 5](images/Task6/part1-setup_&_load/Screenshot%20From%202026-03-15%2012-46-13.png)

Screenshot From 2026-03-15 12-46-13


**6.2 Validation Queries**

2. Before continuing, you must confirm that your model is working, and that the data got correctly loaded into the database. Formulate the queries below and test your database.

i. Find the maximum number of events that a single participant will attend. (Avoid ORDER BY)

![Query i](images/Task6/part2-confirmation/i.png)

Query i

ii. List names of the participants that attend the largest number of events. (Do not use LIMIT, Avoid ORDER BY)

![Query ii](images/Task6/part2-confirmation/ii.png)

Query ii

iii. What events are attended by Ludvig Rustad?

![Query iii](images/Task6/part2-confirmation/iii.png)

Query iii

iv. List participants that attend exactly 3 events.

![Query iv-1](images/Task6/part2-confirmation/iv-1.png)

Query iv-1

![Query iv-2](images/Task6/part2-confirmation/iv-2.png)

Query iv-2

![Query iv-p1](images/Task6/part2-confirmation/iv-p1.png)

Query iv-p1

![Query iv-p2](images/Task6/part2-confirmation/iv-p2.png)

Query iv-p2

v. What events do they attend, the participants that attend 3 events? (Your query should not refer explicitly to any events, nor eventId equal to 5)

![Query v](images/Task6/part2-confirmation/v.png)

Query v

vi. Are there any events that is not attended by anybody that attends three events? (Your query should not refer explicitly any of the events)

![Query vi](images/Task6/part2-confirmation/vi.png)

Query vi

vii. Are there any events that is attended only by the participants that attend only one event? (Your query should not refer explicitly any of the events)

![Query vii fixed](images/Task6/part2-confirmation/vii-fixed.png)

Query vii fixed

![Query vii](images/Task6/part2-confirmation/vii.png)

Query vii


**6.3 Stored Procedure takeSpace**

3. Make a stored procedure `takeSpace` that will book a space for an existing participant at an event.
* If there are available spaces at the event, book a space for the participant.
* If the event is sold out, do nothing.
* The procedure should be accessible by concurrent users, i.e. locking is required with a 3NF model.
* The stored procedure should not modify column totSpaces of the table Event.

![1-setup](images/Task6/part3/1-setup.png)

Setup

![1-setup-old](images/Task6/part3/1-setup-old.png)

Setup Old

![2](images/Task6/part3/2.png)

Step 2

![3](images/Task6/part3/3.png)

Step 3

![Screenshot](images/Task6/part3/Screenshot%20From%202026-03-15%2013-16-02.png)

Screenshot From 2026-03-15 13-16-02

**6.4 Profiling**

4. Start profiling and check the time consumption used by the stored procedure when booking a space at an event. What takes most time?
* Discuss how multiple concurrent bookings for the same event would influence the time consumption.

![setup+prof-q1](images/Task6/part4/setup+prof-q1.png)

Setup + Profiling Question 1

![prof-q2](images/Task6/part4/prof-q2.png)

Profiling Question 2

![prof-q3](images/Task6/part4/prof-q3.png)

Profiling Question 3

![prof-q4](images/Task6/part4/prof-q4.png)

Profiling Question 4

![prof-q5](images/Task6/part4/prof-q5.png)

Profiling Question 5

![answer-1](images/Task6/part4/answer-1.png)

Answer 1

![answer-2](images/Task6/part4/answer-2.png)

Answer 2

**6.5 Denormalisation**

5. Can you advice a denormalisation scheme that would reduce the time consumption?
* Implement the new model, and update `takeSpace` to take the new model into account. Try to avoid the need for explicit locks in the updated version of `takeSpace`.
* Use profiling and measure the time consumption when booking a space at an event with the new version `takeSpace`.

![1-update-tables](images/Task6/part5/1-update-tables.png)

Update Tables

![2-update-proc](images/Task6/part5/2-update-proc.png)

Update Procedure

![3-profiling-new-data](images/Task6/part5/3-profiling-new-data.png)

Profiling New Data

![4-prof-q1](images/Task6/part5/4-prof-q1.png)

Profiling Question 1

![5-prof-q2](images/Task6/part5/5-prof-q2.png)

Profiling Question 2

![answer-1](images/Task6/part5/answer-1.png)

Answer 1

![answer-2-after-edit](images/Task6/part5/answer-2-after-edit.png)

Answer 2 After Edit

