---
title: "Keys in Database"
date: 2022-03-17T14:26:35+08:00
tags: ["database", "mysql"]
categories: ["database"]
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

There are 8 types of keys in DBMS(Database Management system). Keys are used to help developers to identify a row in a relation(table). Understanding each keys will help you understand and develop a database. 

In modern world, data is growing at a rapid pace. So does the demand for professional data analyst. In this article, I will help you understand the keys in DBMS. I suggest you have memorize these terms as it will help you build a logical database and if later on you want to expand the program, you don't have to start over.



In this articles we will identify:
- Super Key
- Candidate Key
- Primary Key
- Alternate Key
- Foreign Key
- Composite Key

### What are DBMS keys?

A DAMS key is an attribute or a set of attributes which help you uniquely a record in a relation. It'll help you to identify and ensure integrity of data in relations. Keys are used to create a relationship among different database relations.




**Student Records**

| Student ID | Name | College   | Std Email            |
|------------|------|-----------|----------------------|
| 1          | John | I.T.Tala  | john11@a.com         |
| 2          | Jack | Harvard   | sacjack@gmail.com    |
| 3          | Jack | I.T.Tala  | a3321342@yahoo.com   |
| 4          | Mary | Cambridge | zilinmary0312@it.com |

### Keys
In the above tables, we cannot uniquely identify a by attribute of name or college. It's because the names and colleges are existing in different tuples(rows). However, we can use **Student ID or Std Email** to uniquely identify each row. In this case, these two attributes are keys in DBMS. These two keys are identifiable and can ensure integrity in a relation.

### Super Key
By def: A super key is a group of single or multiple keys which identifies rows in a table.
Super key is what we elaborate above. It's used to identify a record in a table. In the above example, we can have more than 2 super keys.
1. Student ID
2. Std Email
3. Student ID + Name
4. Student ID + College
5. Student ID + Std Email
1. Name + College
1. Name + Std Email
1. College + Std Email
1. Student ID + Name + College
1. Student ID + Name + Email
1. Student ID + College + Std Email
1. Name + College + Std Email
1. Student ID + Name + College + Std Email

There are 13 different key(s) available to uniquely identify this relation(table). Each set above is super keys. It's all possible keys are included.

### Candidate Key
It's the minimal subset of super key, and it can help to uniquely identify data in a table.

We have 2 minimal set of super keys.
1. Student ID
1. Std Email

In the above 13 combinations, we have several unnecessary pairs of keys. **Example: Student ID + Name + College + Std Email.** It's unnecessary to use these set, where the minimal sets *Student ID or Std Email* are available. 

Hence, *Student ID and Std Email* are the 2 candidate keys in this relation.

### Primary key
It's the candidate key which is chosen to identify each tuples in a table. Primary key cannot be NULL and every tuples have a primary key.

We have **Student ID and Std Email** as our candidate keys. You can pick one of those as your primary key. It's generally chosen by a database administrator.

### Alternate Key
The remaining candidate key is alternate key. If we choose Student ID as our primary key. Then the *Std Email* will become our **Alternate Key** .

### Foreign Key
It's an attribute in a table which is used to define relationship with another relation(table).

We have to have another table.


**College table** 

| College   | Number of students | Location      |
|-----------|--------------------|---------------|
| Harvard   | 22919              | United Stats  |
| I.T. Tala | 3319               | Italy         |
| Cambridge | 21667              | United Kindom |

This table informs us a simple data of colleges. 

In **Student Records**, the **college** attribute can be made as a foreign key. It can set up referential integrity and create a relationship between tables.


**Foreign key helps to ensure data integrity** 

If someone wants to insert an entry that the college names are not provided in another table. It'll return error. Similarly, if someone want to delete a college row in the table, the rows in student records that containing that foreign keys must be delete as well.

For example, if we want to delete Cambridge from the college table. **Student ID 4, Mary** in **Student Records** must be deleted.

### Composite key
Any key with more than one attribute is composite key. For example: 
1. Name + Std Email
1. College + Std Email
1. Student ID + Name + College


Now you can identify each keys respectively. In real practice in database, primary key, and foreign key are commonly used to refer in relationships. Other types of keys are concepts which you should know.

