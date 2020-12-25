# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of twelve (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. It is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features in the future, hoping to create a multi-faceted system for a Student Portal Web Application.

<br>

## Entity Relationship Diagram (ERD)

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database is an insource that I created using the following tools:

<a href="https://lucid.app/" target="_blank">Lucid Chart Diagram tools</a> &nbsp;&nbsp; <a href="http://online.visual-paradigm.com/" target="_blank">Online Visual-Paradigm tools</a> &nbsp;&nbsp; <a href="https://www.mockaroo.com/" target="_blank">Mackaroo Mockup data</a> &nbsp;&nbsp; <a href="https://www.convertcsv.com/csv-to-sql.htm" target="_blank">sql converter</a> &nbsp;&nbsp; <a href="https://fakenumber.net/phone-number/philippines" target="_blank">fake number generator</a> &nbsp;&nbsp; <a href="https://www.dcode.fr/values-separation" target="_blank">Varchar decoder</a> &nbsp;&nbsp; <a href="https://www.random.org/strings/?mode=advanced" target="_blank">Random string generator</a>

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
   1. **`Query 1: `**
       ```SQL
         DELIMITER //

         CREATE PROCEDURE selectUser(
         IN uid INT(11),
         OUT un VARCHAR (30),
         OUT em VARCHAR(80),
         OUT sadd VARCHAR(80),
         OUT stat BOOLEAN
         )
         -- SELECT but not show the values that is received from this statement and assign it to different variables
         SELECT users.uname, users.email, users_detail.saddress, users_detail.is_active 
            INTO un, em, sadd, stat 
            FROM users 
         INNER JOIN users_detail 
            ON users.user_id = users_detail.user_id 
         WHERE users.user_id = uid //

         DELIMITER ;
       ```
       <details>
       <summary>Show more...</summary>

        **`Query for the calling program:`**
        ```SQL
         -- call procedure
         CALL selectUser(
            10000418,
            @un,
            @em,
            @sadd,
            @stat
         );

         -- SELECT the variables one more time. This time, we are selecting it with the intention of showing the returned value
         SELECT @un, @em, @sadd, @stat;
        ```
        `Result:`
        ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-1.PNG)
        </details>
       
       <br>

   2. **`Query 2: `**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE insertUser(
         -- users table
         IN ucl int(11),
         IN un varchar(50),
         IN pw varchar(50),
         IN rec varchar(20),
         IN em VARCHAR(80),
         -- users_detail table
         IN fn varchar(30),
         IN ln varchar(30),
         IN ct varchar(16),
         IN sadd varchar(80),
         IN city_id int(11),
         IN schid int(11)    
         )
         -- use BEGIN - END if there are multiple logic/schema in one session
         BEGIN

            -- assign the next increment value to a variable
            SELECT `AUTO_INCREMENT` 
               INTO @ai
               FROM  INFORMATION_SCHEMA.TABLES
            WHERE TABLE_SCHEMA = 'studentportal'
               AND TABLE_NAME = 'users_detail';
            
            INSERT INTO users_detail 
               ( fname, lname, contact_no, saddress, city_id, school_id ) 
            VALUES
               ( fn, ln, ct, sadd, city_id, schid );
                  
            -- assign the @ai variable in here
            INSERT INTO users
               ( user_id, u_cl_id, uname, pword, rec_code, email ) 
            VALUES
               (@ai, ucl, un, pw, rec, em);

         END //

         DELIMITER ;
      ```
       <details>
       <summary>Show more...</summary>

       **`Query for the calling program:`**
      ```SQL
       -- check the total rows before calling the procedure to get the initial number
         SELECT COUNT(user_id) 
            FROM users_detail;
         SELECT COUNT(user_id) 
            FROM users;
      ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-1.png)

       ```SQL
         -- call the procedure using dummy data as parameters
         CALL insertUser(
            10001,
            'username123',
            'password123',
            'MyCoD3',
            'email@email.com',
            'John',
            'Doe',
            '63-909-555-4117',
            'km 11 Bayview, Sasa',
            5,
            3005
         );

         -- check the total rows again after the procedure is called which should now have 1 row added to it
         SELECT COUNT(user_id) 
            FROM users_detail;
         SELECT COUNT(user_id)
            FROM users;
       ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-2.png) 
      </details>

        <br>

   3. **`Query 3: `**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE insertCityIllegaly(
            nm VARCHAR(50),
            em VARCHAR(80),
            ln VARCHAR(16),
            sadd VARCHAR(80),
            cid INT(11)
         )
         BEGIN

            -- disable the keycheck for foreign keys since we are inserting an illegal city_id. If not disabled, it will throw a constraint restriction
            SET foreign_key_checks = 0;
               
            -- insert a false city_id illegally    
            INSERT INTO schools 
               (name, email, landline_no, saddress, city_id)
            VALUES 
               (nm, em, ln, sadd, cid);
                     
            -- set keycheck back to normal        
            SET foreign_key_checks = 1;
            
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**

      ```SQL
         CALL insertCityIllegaly(
            'False University',
            'false@false.com',
            '911',
            'False street',
            -- this id does not exist in the city table so its an illegal insertion
            91111111
         );

         -- select the row that was just added and it should be there.
         SELECT * 
            FROM schools 
         WHERE city_id = 91111111;
      ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp3-1.png) 
      <details>

   4. **`Query 4: `**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE updateUsersMod(
            IN tbn VARCHAR(50),
            IN uid INT(11)
         )

         BEGIN

            IF tbn = 'users' THEN
               UPDATE users 
               SET `modified_at` = CURRENT_TIMESTAMP 
               WHERE user_id = uid;
            ELSEIF tbn = 'users_detail' THEN
               UPDATE users_detail 
               SET `modified_at` = CURRENT_TIMESTAMP 
               WHERE user_id = uid;
            -- create and throw a custom error when condition is met
            ELSE
               SIGNAL SQLSTATE '45000'
               SET MESSAGE_TEXT = 'Custom error: Table is not users table';
            END IF;
            
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- check the initial state
         SELECT modified_at 
            FROM users 
         WHERE user_id = 10000000;

         CALL updateUsersMod(
            'users',
            10000000
         );

         -- check the updated state
         SELECT modified_at 
            FROM users 
         WHERE user_id = 10000000;
      ```
         `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp4-1.png)
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp4-2.png) 
      </details>

   5. **`Query 5: `**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE copyToCSV()

         BEGIN

            SELECT COUNT(user_id) 
               INTO @cu 
               FROM users;
            
            IF @cu >= 263 THEN
               SELECT * 
                  FROM users
               
               INTO OUTFILE 'C:/CSV/users_copy.csv' 
               FIELDS ENCLOSED BY '"' 
               TERMINATED BY ';' 
               ESCAPED BY '"' 
               LINES TERMINATED BY '\r\n';
            ELSE
               SIGNAL SQLSTATE '45001'
               SET MESSAGE_TEXT = 'Custom error: Table has not reached specified row number';
            END IF;
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- first count the total rows in the users table to make sure
         SELECT 
            COUNT(user_id), CURRENT_TIMESTAMP 
            FROM users;

         -- call the procedure
         CALL copyToCSV();
      ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp5-1.png)
      </details>

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