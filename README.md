# Database Documentation

## The database

This database is made to manage the data needed for a Student Portal System. At the moment, it is consist of nine (9) entities and is responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles(although I am planning to include more features to this database system in the future). This database will be operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. With that, multiple complex queries and methods to manage all of the relationships between entities will be used.

<br />

## Entity Relationship Diagram (ERD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database is an insource based on my plan to create a Student Portal Web Application in the future. The ERD shown in this documentation is made using Lucid Chart's general purpose diagram tool.

### Entity Names and Description

**`users`** - is responsible for storing necessary data upon the creation of a user and when its modified.

**`students`** - is responsible for storing necessary data upon the creation and modification of a user that has identified itself as a student.

**`authors`** - is responsible for storing necessary data upon the creation and modification of a user that has identified itself as an author.

**`schools`** - is responsible for storing necessary data upon the creation of a school and when its modified.

**`articles`** - is responsible for storing necessary data upon the creation of an article and when its modified.

**`subjects`** - is responsible for storing necessary data upon the creation of a subject and when its modified.

**`author_subscriptions`** - is responsible for storing necessary data upon user subscription of the authors and when its modified.

**`articles_engagement`** - is responsible for storing necessary data upon the creation and modification of user comment within an article (creates social engagement among users).

**`articles_reply`** - is responsible for storing necessary data upon the creation and modification of user replies within a comment.

<br />

## Functional Dependency Diagram (FDD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/FDD.svg)

This diagram shows that each entities had undergone normalization removing all dependencies that can cause anomalies.

<br />

## Complex Queries associated with the database

Here are a list of queries with their sample output from the DBRMS:

* ***Stored Procedures***
    1.  ```
       SELECT * FROM TAGURU
        ```
    2.  ```
       SELECT * FROM TAGURU
        ```
    3.  ```
       SELECT * FROM TAGURU
        ```
    4.  ```
       SELECT * FROM TAGURU
        ```

* ***Triggers*** 
    1.  ```
       SELECT * FROM TAGURU
        ```
    2.  ```
       SELECT * FROM TAGURU
        ```
    3.  ```
       SELECT * FROM TAGURU
        ```
    4.  ```
       SELECT * FROM TAGURU
        ```

* ***Functions*** 
    1.  ```
       SELECT * FROM TAGURU
        ```
    2.  ```
       SELECT * FROM TAGURU
        ```
    3.  ```
       SELECT * FROM TAGURU
        ```
    4.  ```
       SELECT * FROM TAGURU
        ```   
        
* ***Transactions*** 
    1.  ```
       SELECT * FROM TAGURU
        ```
    2.  ```
       SELECT * FROM TAGURU
        ```
    3.  ```
       SELECT * FROM TAGURU
        ```
    4.  ```
       SELECT * FROM TAGURU
        ```        