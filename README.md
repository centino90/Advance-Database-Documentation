# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of eleven (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. This database is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features into this database in the future.

<br>

## Entity Relationship Diagram (ERD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database is an insource that I created using the following tools:

> [Lucid Chart Diagram tools](https://lucid.app/) &nbsp;&nbsp; [Online Visual-Paradigm tools](http://online.visual-paradigm.com/) &nbsp;&nbsp; [Mackaroo Mockup data](https://www.mockaroo.com/) &nbsp;&nbsp; [sql converter](https://www.convertcsv.com/csv-to-sql.htm) &nbsp;&nbsp; [fake number generator](https://fakenumber.net/phone-number/philippines) &nbsp;&nbsp; [Varchar decoder](https://www.dcode.fr/values-separation) &nbsp;&nbsp; [Random string generator](https://www.random.org/strings/?mode=advanced) &nbsp;&nbsp;

### Entity Names and Description

**`users`** - is responsible for storing necessary data upon the creation of a user and when its modified.

**`users_detail`** - is responsible for storing necessary data upon the creation and modification of a user that has identified itself as a student.

**`user_class`** - is responsible for storing necessary data of a classifier for users which is used to assign each user a user level to identify to when interacting with the system.

**`schools`** - is responsible for storing necessary data upon the creation of a school and when its modified.

**`articles`** - is responsible for storing necessary data upon the creation of an article and when its modified.

**`subjects`** - is responsible for storing necessary data upon the creation of a subject and when its modified.

**`author_subscriptions`** - is responsible for storing necessary data upon user subscription of the authors and when its modified.

**`articles_comment`** - is responsible for storing necessary data upon the creation and modification of user comment within an article (creates social engagement among users).

**`articles_reply`** - is responsible for storing necessary data upon the creation and modification of user replies within a comment.

**`cities`** - is responsible for storing necessary data of a city.

**`states`** - is responsible for storing necessary data of a state.

**`countries`** - is responsible for storing necessary data of a country.

<br />

## Functional Dependency Diagram (FDD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/FDD.svg)

This diagram shows that each entities had undergone normalization removing all dependencies that can cause anomalies.

<br />

## Complex Queries associated with the database

Here are a list of queries with their sample output from the DBRMS: 
<br>

><p>Note:<br>Click if you notice a dropdown button labeled <b>Show more...</b> <br> It will show the corresponding result of the query.
</p>


* ***Stored Procedures***
    1. **`SETUP PROCEDURE: `**
       ```SQL
       DELIMITER //
       CREATE PROCEDURE insertStudent(
         IN fn varchar(15),
         IN ln varchar(15),
         IN ct varchar(16),
         IN sadd varchar(50),
         IN city_id int(7),
         IN schid int(9)
       )

       BEGIN

        INSERT INTO students 
          ( fname, lname, contact_no, haddress, zip, school_id ) 
            VALUES
              ( fn, ln, ct, sadd, city_id, schid )

       END //
       DELIMITER ;
       ```
       <details open>
       <summary>Show more...</summary>

       **`Query for the calling program:`**
       ```SQL
       -- first, select all data from students table. It should be empty.
        SELECT * FROM students;
       ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-1.PNG)

       ```SQL
        -- then call the procedure with a random data assigned to it
        CALL insertStudent('John', 'Doe', +63-909-555-4117, 'km 11, Bayview, Sasa, Davao City', 1000000001, 100000005);

        -- then select for the last time to show all the data in students table after the procedure was called
        SELECT * FROM students;
       ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-2.PNG)
        </details>
        
        *---end of item*

    2. **`SETUP PROCEDURE: `**
       ```SQL
       DELIMITER //
       CREATE PROCEDURE alterStudents(
         OUT time_stamp CURRENT_TIMESTAMP
       )

       BEGIN

        INSERT INTO students 
          ( fname, lname, contact_no, haddress, zip, school_id ) 
            VALUES
              ( fn, ln, ct, hadd, z, schid )

       END //
       DELIMITER ;
       ```
       <details>
       <summary>Show more...</summary>

        **`Query for the calling program:`**
        ```SQL
        -- first, select all data from students table. It should be empty.
          SELECT * FROM students
        ```
        ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-1.PNG)

        ```SQL
          -- then call the procedure with a random data assigned to it
          CALL insertStudent('John', 'Doe', 639154485321, 'km 11, Bayview, Sasa, Davao City', 8000, 1011);

          -- then select for the last time to show all the data in students table after the procedure was called
          SELECT * FROM students
        ```
        `Result:`
        ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-2.PNG)
        </details>

        *---end of item*

    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```

* ***Triggers*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```

* ***Functions*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```
       
* ***Transactions*** 
    1. ```SQL
       SELECT * FROM TAGURU
       ```
    2. ```SQL
       SELECT * FROM TAGURU
       ```
    3. ```SQL
       SELECT * FROM TAGURU
       ```
    4. ```SQL
       SELECT * FROM TAGURU
       ```