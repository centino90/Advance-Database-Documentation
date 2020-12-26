# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of twelve (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. It is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features in the future, hoping to create a multi-faceted system for a Student Portal Web Application.

<br>

## Entity Relationship Diagram (ERD)
ER diagram is a representation of the connection between entities. By creating one, we can define the relationship of each entities in a much more sensible and convenient manner with the symbols and principles help.
<p>The following markers used in the ERD are defined as:</p><b>PK</b> - Primary key<br><b>FK</b> - Foreign key<b><br>PK/FK</b> - Composite key


![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database and its data is an insource that I created using the following tools:

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

FDD is a representation of each entities' functional dependency. This diagram helps us remove dependencies that can cause insertion, deletion, & updation anomalies in the database.

![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/FDD.svg)

The diagram shows that each entities had undergone normalization removing all dependencies that can cause anomalies.

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
         IN gip VARCHAR(20),
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

            -- call a procedure that gets the current increment value of the table supplied as parameter
            -- this is based on a proc that I already made (see query #11)
	         CALL getTableIncrement('users_detail', @ai);

            -- here is where the primary key originated
            INSERT INTO users_detail 
               ( fname, lname, contact_no, saddress, city_id, school_id ) 
            VALUES
               ( fn, ln, ct, sadd, city_id, schid );

            -- here is where the @ai value is used as primary/foreign key
            INSERT INTO users
               ( user_id, u_cl_id, uname, pword, rec_code, email, guest_ip ) 
            VALUES
               (@ai, ucl, un, pw, rec,em, gip);

            SET @gip = gip;
            -- perform prepared statements that is contained within another stored proc "exec_qry (See query #10)"
            -- create user as guest with their ip attached as their host
            CALL exec_qry(CONCAT("CREATE user user@",@gip));
            -- grant priveleges that is common to all users (selecting data from articles and subjects table)
            CALL exec_qry(CONCAT("GRANT SELECT ON studentportal.articles TO user@",@gip));
            CALL exec_qry(CONCAT("GRANT SELECT ON studentportal.subjects TO user@",@gip));
               
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
            '192.168.5.5', -- this can be based on the user_ip_address or browser info that you get from a user of the application
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
         -- in addition, also check if the current user was added to the userlist of all mysql account
         SELECT * 
            FROM mysql.user 
            WHERE host = '192.168.5.5';
         -- also check the previliges it has 
         SHOW GRANTS FOR 'user'@'192.168.5.5'
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
      </details>

      <br>
      
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
               SET MESSAGE_TEXT = 'Custom error: The credentials you submitted did not reflect the internal requirements (users or users_detail), try again';
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
      </details>

      <br>

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
            -- convert the data table into csv and setup the direction of which the converted table will be sent   
               INTO OUTFILE 'C:/CSV/users_copy.csv' 
               FIELDS ENCLOSED BY '"' 
               TERMINATED BY ';' 
               ESCAPED BY '"' 
               LINES TERMINATED BY '\r\n';
            -- throw an error if the condition is not met
            ELSE
               SIGNAL SQLSTATE '45001'
               SET MESSAGE_TEXT = 'Custom error: Table has not reached specified row count (263), try again';
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
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp5-2.png)
      </details>

      <br>

   6. **`Query 6: `**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE showRoutines(
            IN name VARCHAR(30)
         )

         BEGIN
            
            IF name = 'trigger' THEN
               SELECT TRIGGER_NAME 
               FROM INFORMATION_SCHEMA.triggers 
               WHERE TRIGGER_SCHEMA = 'studentportal';
            ELSEIF name = 'procedure' THEN
               SHOW PROCEDURE STATUS 
               WHERE Db = 'studentportal';
            ELSEIF name = 'function' THEN
               SHOW FUNCTION STATUS 
               WHERE Db = 'studentportal';
            ELSE
               SIGNAL SQLSTATE '45002'
               SET MESSAGE_TEXT = 'Custom error: The "routine" you submitted is not found, try again';
            END IF;
            
         END //

         DELIMITER ;
      ```   
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- call the procedure to check each routines with either one of these parameters (procedure, trigger, function)
         CALL showRoutines('procedure');
      ```
      `Result: `
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp6-1.png)
      </details>

      <br>

   7. **`Query 7:`**
      ```SQL
         DROP PROCEDURE IF EXISTS selectTable;

         DELIMITER //

         CREATE PROCEDURE selectTable(
            IN tb VARCHAR(30),
            -- use this parameter type to return a value to the caller
            INOUT updatedTb VARCHAR(30)
         )

         BEGIN
            -- update the returned value to the one that is just submitted
            SET updatedTb = tb;
            
            -- use Prepared Statement to perform a dynamic query
            SET @sql = CONCAT("SELECT * FROM ", tb);
            PREPARE stmt FROM @sql;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;
            
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- set the extension (label) to be appended before the table name
         SET @ext = 'table: ';

         -- call procedure in which INOUT parameter can have a 'pre-procedure' or initial value unlike either IN or OUT
         CALL selectTable('articles', @table);

         -- select the INOUT parameter to return a value just like OUT
         SELECT CONCAT(@ext, @table) AS new_table;
      ```
      `Result: `
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp7-1.png)
      </details>

      <br>

   8. **`Query: 8`**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE verifyUser(
            IN un VARCHAR(50),
            IN pw VARCHAR(50),
            OUT is_verified BOOLEAN
         )

         BEGIN 
            -- assign the selected field into a variable
            SELECT user_id
               INTO @uid
               FROM users
               WHERE uname = un AND pword = pw;
            
            -- if the select operation returned null or did not find a match, verification is failed
            IF @uid IS NULL THEN
               -- or you can use the SIGNAL SQLSTATE to create and throw an SQL error
               SET is_verified = FALSE;
            -- else verification is success and return the datatable of that user
            ELSE
               SET is_verified = TRUE;
               SELECT * FROM users WHERE user_id = @uid;
            END IF;
            
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- SET the needed data for convenience. In this case, a correct one.
         SET @uname = 'admin101';
         SET @pword = 'admin101';

         -- CALL the procedure in which it should return all the data from users table
         CALL verifyUser(@uname, @pword, @is_verified);

         -- SELECT the OUT parameter to get the ouput
         SELECT @is_verified;
      ```
      `Result: `
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp8-1.png)
      </details>

      <br>

   9. **`Query: 9:`**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE copyToCSV(
            IN tb VARCHAR(50),
            IN lim INT(11)
         )
         BEGIN

            -- limit should be less or equal to the total row of the table
            IF (lim > 0) THEN
               -- Use prepared statement to supply dynamic table_name for all tables in just 1 query
               PREPARE stmt FROM 
               CONCAT("SELECT * FROM ", tb," LIMIT ? OFFSET 1
               -- use this format to makesure the uniqueness of .csv filenames       
               INTO OUTFILE
                  'C:/CSV/",tb,"_",CURDATE(),"_",HOUR(CURRENT_TIME),"_",MINUTE(CURRENT_TIME),"_",SECOND(CURRENT_TIME),"_copy.csv' 
               FIELDS ENCLOSED BY '`' 
               TERMINATED BY ';'
               ESCAPED BY '`' 
               LINES TERMINATED BY '\r\n'");
               EXECUTE stmt USING lim;
            END IF;
            
         END
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- call procedure in which it should send a .csv file to our directory "C:/CSV/"
         -- if its less or equal to 0 it should not create a csv file. If its over the row_count then it should include all data in that table
         CALL copyToCSV("users", 0);
         CALL copyToCSV("users", 9999);
      ```
      `Result: `
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp9-1.png)
      </details>

      <br>

   10.   **`Query: 10`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE exec_qry(
               IN p_sql varchar(500)
            )

            BEGIN
            SET @tquery = p_sql;
            PREPARE stmt FROM @tquery;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;
            END //

            DELIMITER ;
         ```
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- this proc can receive even complex queries (see query #2)
            -- call query 3 times with different query targets

            -- basic select
            CALL exec_qry('SELECT * FROM articles');
            -- select with limit and offset
            CALL exec_qry('SELECT * FROM users LIMIT 10 OFFSET 1');
            -- shows all preveliges of root@localhost
            CALL exec_qry(CONCAT('SHOW GRANTS FOR root@localhost'));

         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp10-1.png)
         </details>

         <br>

   11.   **`Query 11: `**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE getTableIncrement(
               IN tbl VARCHAR(50),
               OUT ai VARCHAR(11)
            )

            BEGIN
            -- assign the next increment value to a variable to use it as reference (since the next primary key is predictable due to it being incremented automatically by 1) to the user_id from users_detail table
               SET @tbl = tbl;
               SELECT `AUTO_INCREMENT`
               INTO ai FROM INFORMATION_SCHEMA.TABLES
               WHERE TABLE_SCHEMA = 'studentportal' AND TABLE_NAME = tbl;

            END //
               
            DELIMITER ;
         ```            
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- this proc can receive even complex queries (see query #2)
            -- call query 3 times with different query targets

            -- basic select
            CALL exec_qry('SELECT * FROM articles');
            -- select with limit and offset
            CALL exec_qry('SELECT * FROM users LIMIT 10 OFFSET 1');
            -- shows all preveliges of root@localhost
            CALL exec_qry('SHOW GRANTS FOR root@localhost');

         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp11-1.png)
         </details>

         <br>

* ***Triggers*** 
   1. **`Query 8: `**
      ```SQL
      -- create triggers for 8 tables that has modified_at field. This will update all fields based on the current timestamp of when the session is ran.
      CREATE TRIGGER up_artc_ma 
         BEFORE UPDATE ON articles_comment 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_artr_ma 
         BEFORE UPDATE ON articles_reply 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_art_ma 
         BEFORE UPDATE ON articles 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_as_ma 
         BEFORE UPDATE ON author_subscription 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_sch_ma 
         BEFORE UPDATE ON schools 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_subj_ma 
         BEFORE UPDATE ON subjects 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_us_ma 
         BEFORE UPDATE ON users 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
         
      CREATE TRIGGER up_usd_ma 
         BEFORE UPDATE ON users_detail 
         FOR EACH ROW SET NEW.modified_at = CURRENT_TIMESTAMP;
       ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- check the initial state of the field before update
         SELECT modified_at 
         FROM articles 
         WHERE article_id = 1001;

         -- perform update to trigger the trigger
         UPDATE articles
         SET title = 'The History of Africa'
         WHERE article_id = 1001;

         -- check the field after update
         SELECT modified_at 
         FROM articles 
         WHERE article_id = 1001;
      ```
       `Result:`
       ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/tr1-1.png)
      </details>

      <br>

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