# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of twelve (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. It is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features in the future, hoping to create a multi-faceted system for a Student Portal Web Application.

<br>

## Entity Relationship Diagram (ERD)
ER diagram is a representation of the connection between entities. By creating one, we can define the relationship of each entities in a much more sensible and convenient manner with the symbols and principles help.
<p>The following markers used in the ERD are defined as:</p><b>PK</b> - Primary key only<br><b>FK</b> - Foreign key only<b><br>PK/FK</b> - Primary and Foreign key<br><b>2 PK/FK</b> - Composite key


![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/ERD.svg)

This database and its data is an insource that I created using the following tools:

<a href="https://lucid.app/" target="_blank">Lucid Chart Diagram tools</a> &nbsp;&nbsp; <a href="http://online.visual-paradigm.com/" target="_blank">Online Visual-Paradigm tools</a> &nbsp;&nbsp; <a href="https://www.mockaroo.com/" target="_blank">Mackaroo Mockup data</a> &nbsp;&nbsp; <a href="https://www.convertcsv.com/csv-to-sql.htm" target="_blank">sql converter</a> &nbsp;&nbsp; <a href="https://fakenumber.net/phone-number/philippines" target="_blank">fake number generator</a> &nbsp;&nbsp; <a href="https://www.dcode.fr/values-separation" target="_blank">Varchar decoder</a> &nbsp;&nbsp; <a href="https://www.random.org/strings/?mode=advanced" target="_blank">Random string generator</a>

### Entity Names and Description

| Entity Name | Description |
| --- | ----------- |
| users | ```is responsible for storing necessary data upon the creation of a user and when its modified.``` |  
| users_detail | ```is responsible for storing necessary data upon the creation and modification of a user that has identified itself as a student.``` |
| user_class | ```is responsible for storing necessary data of a classifier for users which is used to assign each user a user level to identify to when interacting with the system.``` |  
| schools | ```is responsible for storing necessary data upon the creation of a school and when its modified.``` |
| articles | ```is responsible for storing necessary data upon the creation of an article and when its modified.``` |  
| subjects | ```is responsible for storing necessary data upon the creation of a subject and when its modified.``` |
| author_subscriptions | ```is responsible for storing necessary data upon user subscription of the authors and when its modified.``` |  
| articles_comment | ```is responsible for storing necessary data upon the creation and modification of user comment within an article (creates social engagement among users).``` |
| articles_reply | ```is responsible for storing necessary data upon the creation and modification of user replies within a comment.``` |  
| cities | ```is responsible for storing necessary data of a city.``` |
| states | ```is responsible for storing necessary data of a state.``` |  
| countries | ```is responsible for storing necessary data of a country.``` |

<br>

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


* ***Establish Tables and Relationships (Create Table, Primary key, Foreign key, etc.)*** - A good database system should be able to create relational connection between its tables

   1. **`Query 1: `**

      ```SQL
         CREATE TABLE IF NOT EXISTS `articles` (
         `article_id` int(11) NOT NULL,
         `author_id` int(11) NOT NULL,
         `subj_id` int(11) NOT NULL,
         `title` text DEFAULT NULL,
         `content` longtext DEFAULT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
         `is_active` tinyint(1) NOT NULL DEFAULT 1,
            PRIMARY KEY (`article_id`),
            FOREIGN KEY (`author_id`) REFERENCES users (`user_id`), 
            FOREIGN KEY (`subj_id`) REFERENCES subjects (`subj_Id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `subjects` (
         `subj_id` int(11) NOT NULL,
         `name` varchar(50) DEFAULT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
         `is_active` tinyint(1) NOT NULL DEFAULT 1,
            PRIMARY KEY (`subj_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `articles_comment` (
         `art_comm_id` int(11) NOT NULL,
         `user_id` int(11) NOT NULL,
         `article_id` int(11) NOT NULL,
         `comment` mediumtext DEFAULT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
            PRIMARY KEY (`art_comm_id`),
            FOREIGN KEY (`user_id`) REFERENCES users (`user_id`), 
            FOREIGN KEY (`article_id`) REFERENCES articles (`article_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `articles_reply` (
         `art_reply_id` int(11) NOT NULL,
         `user_id` int(11) NOT NULL,
         `art_comm_id` int(11) NOT NULL,
         `reply` mediumtext DEFAULT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
            PRIMARY KEY (`art_reply_id`),
            FOREIGN KEY (`user_id`) REFERENCES users (`user_id`), 
            FOREIGN KEY (`art_comm_id`) REFERENCES articles_comment (`art_comm_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `author_subscription` (
         `user_id` int(11) NOT NULL,
         `author_id` int(11) NOT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
         `is_active` tinyint(1) NOT NULL DEFAULT 1,
            PRIMARY KEY (`user_id`),
            FOREIGN KEY (`author_id`) REFERENCES users (`user_id`), 
            FOREIGN KEY (`user_id`) REFERENCES users (`user_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


         CREATE TABLE IF NOT EXISTS `schools` (
         `school_id` int(11) NOT NULL,
         `name` varchar(50) DEFAULT NULL,
         `email` varchar(80) DEFAULT NULL,
         `landline_no` varchar(60) DEFAULT NULL,
         `saddress` varchar(80) DEFAULT NULL,
         `city_id` int(11) NOT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
            PRIMARY KEY (`school_id`),
            FOREIGN KEY (`city_id`) REFERENCES cities (`city_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `cities` (
         `city_id` int(11) NOT NULL,
         `name` varchar(30) DEFAULT NULL,
         `state_id` int(11) NOT NULL,
            PRIMARY KEY (`city_id`),
            FOREIGN KEY (`state_id`) REFERENCES states (`state_id`) 
         ) ENGINE=InnoDB DEFAULT CHARSET=latin1;

         CREATE TABLE IF NOT EXISTS `states` (
         `state_id` int(11) NOT NULL,
         `name` varchar(30) DEFAULT NULL,
         `country_id` int(11) NOT NULL DEFAULT 1,
            PRIMARY KEY (`state_id`),
            FOREIGN KEY (`country_id`) REFERENCES countries (`country_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=latin1;

         CREATE TABLE IF NOT EXISTS `countries` (
         `country_id` int(11) NOT NULL,
         `sortname` varchar(3) NOT NULL,
         `name` varchar(150) NOT NULL,
         `phonecode` int(11) NOT NULL,
            PRIMARY KEY (`country_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

         CREATE TABLE IF NOT EXISTS `users` (
         `user_id` int(11) NOT NULL,
         `u_cl_id` int(11) NOT NULL,
         `uname` varchar(50) DEFAULT NULL,
         `pword` varchar(50) DEFAULT NULL,
         `rec_code` varchar(20) DEFAULT NULL,
         `email` varchar(80) NOT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
         `is_active` tinyint(1) NOT NULL DEFAULT 1,
         `guest_ip` varchar(20) DEFAULT NULL,
         `latest_log` timestamp NULL DEFAULT NULL,
            PRIMARY KEY (`user_id`),
            FOREIGN KEY (`u_cl_id`) REFERENCES users_class (`u_cl_id`), 
            FOREIGN KEY (`user_id`) REFERENCES users_detail (`user_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `users_detail` (
         `user_id` int(11) NOT NULL,
         `fname` varchar(30) DEFAULT NULL,
         `lname` varchar(30) DEFAULT NULL,
         `contact_no` varchar(16) DEFAULT NULL,
         `saddress` varchar(80) DEFAULT NULL,
         `city_id` int(11) NOT NULL,
         `school_id` int(11) DEFAULT NULL,
         `modified_at` timestamp NULL DEFAULT NULL,
         `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
         `is_active` tinyint(1) NOT NULL DEFAULT 1,
            PRIMARY KEY (`user_id`),
            FOREIGN KEY (`city_id`) REFERENCES cities (`city_id`), 
            FOREIGN KEY (`school_id`) REFERENCES schools (`school_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

         CREATE TABLE IF NOT EXISTS `user_class` (
         `u_cl_id` int(11) NOT NULL,
         `label` varchar(20) DEFAULT NULL,
         `priv_pword` varchar(50) NOT NULL,
            PRIMARY KEY (`u_cl_id`)
         ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
      ```

      <br>

* ***User Management (Create User, Drop User, Grant Privilege)*** - A good database system should be able to distinguish user privileges according to user levels (user class) to designate users to their respective roles in the system and secure the database from possible attack vectors.

   2. **`Query 2: Student Privileges`**

      ```SQL
         -- student privileges
         GRANT SELECT ON studentportal.schools TO 'student'@'localhost';
         GRANT SELECT ON studentportal.cities TO 'student'@'localhost';
         GRANT SELECT ON studentportal.states TO 'student'@'localhost';
         GRANT SELECT ON studentportal.countries TO 'student'@'localhost';
         GRANT SELECT ON studentportal.articles TO 'student'@'localhost';
         GRANT SELECT ON studentportal.subjects TO 'student'@'localhost';
         GRANT SELECT ON studentportal.articles_comment TO 'student'@'localhost';
         GRANT SELECT ON studentportal.articles_reply TO 'student'@'localhost';
         GRANT SELECT ON studentportal.author_subscription TO 'student'@'localhost';
         GRANT SELECT (uname, pword, rec_code, email, modified_at, created_at) ON studentportal.users TO 'student'@'localhost';
         GRANT SELECT (fname, lname, contact_no, saddress, city_id, school_id) ON studentportal.users_detail TO 'student'@'localhost';

         GRANT UPDATE (uname, pword, is_active) ON studentportal.users TO 'student'@'localhost';
         GRANT UPDATE (fname, lname, contact_no, saddress, city_id, school_id) ON studentportal.users_detail TO 'student'@'localhost';
         GRANT UPDATE (comment, modified_at) ON studentportal.articles_comment TO 'student'@'localhost';
         GRANT UPDATE (reply, modified_at) ON studentportal.articles_reply TO 'student'@'localhost';
         GRANT UPDATE (is_active, modified_at) ON studentportal.author_subscription TO 'student'@'localhost';

         GRANT INSERT ON studentportal.articles_comment TO 'student'@'localhost';
         GRANT INSERT ON studentportal.articles_reply TO 'student'@'localhost';
         GRANT INSERT ON studentportal.author_subscription TO 'student'@'localhost';
      ```
      This is important because users should have distinct 'access level' to the database, and students have the access it needed to be able to interact with the system without jeopardizing the security;

   3. **`Query 3: Author Privileges`**

      ```SQL
         -- author privileges
         GRANT SELECT ON studentportal.schools TO 'author'@'localhost';
         GRANT SELECT ON studentportal.cities TO 'author'@'localhost';
         GRANT SELECT ON studentportal.states TO 'author'@'localhost';
         GRANT SELECT ON studentportal.countries TO 'author'@'localhost';
         GRANT SELECT ON studentportal.articles TO 'author'@'localhost';
         GRANT SELECT ON studentportal.subjects TO 'author'@'localhost';
         GRANT SELECT ON studentportal.articles_comment TO 'author'@'localhost';
         GRANT SELECT ON studentportal.articles_reply TO 'author'@'localhost';
         GRANT SELECT ON studentportal.author_subscription TO 'author'@'localhost';
         GRANT SELECT (uname, pword, rec_code, email, modified_at, created_at) ON studentportal.users TO 'author'@'localhost';
         GRANT SELECT (fname, lname, contact_no, saddress, city_id) ON studentportal.users_detail TO 'author'@'localhost';

         GRANT UPDATE (uname, pword, is_active) ON studentportal.users TO 'author'@'localhost';
         GRANT UPDATE (fname, lname, contact_no, saddress, city_id) ON studentportal.users_detail TO 'author'@'localhost';
         GRANT UPDATE (comment, modified_at) ON studentportal.articles_comment TO 'author'@'localhost';
         GRANT UPDATE (reply, modified_at) ON studentportal.articles_reply TO 'author'@'localhost';
         GRANT UPDATE (subj_id, title, content, modified_at) ON studentportal.articles TO 'author'@'localhost';

         GRANT INSERT ON studentportal.articles TO 'author'@'localhost';
         GRANT INSERT ON studentportal.articles_comment TO 'author'@'localhost';
         GRANT INSERT ON studentportal.articles_reply TO 'author'@'localhost';

         GRANT DELETE ON studentportal.articles_comment TO 'author'@'localhost';
         GRANT DELETE ON studentportal.articles_reply TO 'author'@'localhost';
      ```
      This is important because users should have distinct 'access level' to the database, and authors have the access it needed to be able to interact with the system without jeopardizing the security;

   4. **`Query 4: Admin/Super_User Privileges`**
      ```SQL
         -- admin/super_user
         -- grant all privileges including the ability to grant other users their privilege
         GRANT ALL PRIVILEGES ON studentportal.* TO 'admin'@'localhost' WITH GRANT OPTION;   
      ```
      The admin should have all the privileges pointed to a particular database, and should be able to grant privileges to other users to manage them accordingly.

      <br>

* ***Views*** - A good database system should have a summary table (view) of most frequently accessed tables to present data that are already summarized.

   5.   **`Query 5: Create View that Ranks Articles Based On the Number of Comments`**

         ```SQL
            CREATE VIEW popArtBasedOnComm AS

            SELECT art.title AS Article_Title, full_name(ud.fname, ud.lname) AS Author, subj.name AS Subject, art.content AS Content, COUNT(ac.art_comm_id) AS Total_Comments, art.modified_at AS Last_Modified 
            FROM articles art
            LEFT JOIN subjects subj  USING (subj_id) 
            LEFT JOIN articles_comment ac USING (article_id) 
            LEFT JOIN users_detail ud ON art.author_id = ud.user_id
            WHERE art.is_active = TRUE 
            GROUP BY subj.name ORDER BY Total_Comments DESC, art.title, subj.name ASC;
         ```

   6. **`Query 6: Create View that Ranks Subjects Based On the Number of Articles Created`**

      ```SQL
         CREATE VIEW popSubjBasedOnArt AS

         SELECT subj.name AS Subject, subj.created_at AS Date_Created, COUNT(art.article_id) AS Total_Articles 
         FROM subjects subj 
         LEFT JOIN articles art USING(subj_id) 
         GROUP BY subj.name 
         ORDER BY Total_Articles DESC, subj.name ASC;

      ```     
   7. **`Query 7: Create View that Ranks Comments Based On the Number of Replies`**    
      ```SQL
         CREATE VIEW popCommBasedOnReplies AS

         SELECT us.uname AS Username, art.title AS Article, ac.comment AS Comment, subj.name AS Subject, COUNT(ar.reply) AS Total_Replies, ac.created_at AS Date_Created 
         FROM articles_comment ac 
         LEFT JOIN articles_reply ar USING(art_comm_id) 
         LEFT JOIN users us ON ac.user_id = us.user_id 
         LEFT JOIN articles art USING (article_id) 
         LEFT JOIN subjects subj USING (subj_id) 
         GROUP BY ac.comment 
         ORDER BY Total_Replies DESC, us.uname ASC;
      ```

      <br>

* ***Reports*** - A good database system should be able to accept requests successfully, and process it to create consistent and accurate reports to send back to the end-users.
   
   5. **`Query 5: Create Summary Report from Views`** - record count.

      ```SQL
         -- this is a summary report of a View
         SELECT SUM(Total_Comments) AS Grand_Total
         , AVG(Total_Comments) AS Average
         , MAX(Total_Comments) AS Maximum
         , MIN(Total_Comments) AS Minimum 
         FROM popartbasedoncomm
      ```
      This will enhance how user-friendly the reports are by providing the end-users an improved simplification of data set.

   6. **`Query 6: Create Summary Report from 2 Joined Views and 1 Table`** - This is an improved version of query #5 where instead of 1, 3 views are summarized.

      ```SQL
		   SELECT 
         -- articles info
         SUM(Total_Articles) AS Articles_Grand_Total
         , AVG(Total_Articles) AS Articles_Average
         , MAX(Total_Articles) AS Articles_Maximum
         , MIN(Total_Articles) AS Articles_Minimum
         -- comments info
         ,SUM(Total_Comments) AS Comments_Grand_Total
         , AVG(Total_Comments) AS Comments_Average
         , MAX(Total_Comments) AS Comments_Maximum
         , MIN(Total_Comments) AS Comments_Minimum
         -- replies info
         ,SUM(Total_Replies) AS Replies_Grand_Total
         , AVG(Total_Replies) AS Replies_Average
         , MAX(Total_Replies) AS Replies_Maximum
         , MIN(Total_Replies) AS Replies_Minimum 
         
         FROM popsubjbasedonart 
         LEFT JOIN popartbasedoncomm USING(Subject)
         LEFT JOIN popcommbasedonreplies USING(Subject)
      ```
      This is much more important since it improved the retrieval of data sets by supplying more summarized data into the data table.

   <br>

* ***Triggers*** - A good database system should have triggers in place to perform actions that always happen before or after an insertion, updation, and delition.

   7. **`Query 7: `**
   
      ```SQL
         -- create triggers for 8 tables that has modified_at field. This will update all fields based on the current timestamp of when the session is ran.
         CREATE TRIGGER up_artc_ma 
            AFTER UPDATE ON articles_comment 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_artr_ma 
            AFTER UPDATE ON articles_reply 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_art_ma 
            AFTER UPDATE ON articles 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_as_ma 
            AFTER UPDATE ON author_subscription 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_sch_ma 
            AFTER UPDATE ON schools 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_subj_ma 
            AFTER UPDATE ON subjects 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_us_ma 
            AFTER UPDATE ON users 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
            
         CREATE TRIGGER up_usd_ma 
            AFTER UPDATE ON users_detail 
            FOR EACH ROW SET OLD.modified_at = CURRENT_TIMESTAMP;
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

   8. **`Query 8: `** 
      ```SQL
         DELIMITER //

         CREATE TRIGGER trg_upd_del_comm 
            AFTER DELETE ON articles_comment
            FOR EACH ROW BEGIN
            -- when the deletion of a comment is successful, all replies within that comment are also deleted.
            DELETE FROM articles_reply WHERE art_comm_id = OLD.art_comm_id;
            
            END //
            
         DELIMITER ;
      ```
      <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            SET @id = 100008;
            -- select the initial state of the replies before delition of comments
            SELECT * FROM articles_reply WHERE art_comm_id = @id;
            -- call this stored proc which disables all foreign key constraints associated to the query
            CALL exec_const_qry("DELETE FROM articles_comment WHERE art_comm_id = @id");
            -- select the new state of the replies after delition of comments. All replies within the deleted comment should be deleted also
            SELECT * FROM articles_reply WHERE art_comm_id = @id;
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/tr2-1.png)
      </details>

      <br>

   9. **`Query 9: `** 
      ```SQL
         CREATE TRIGGER trg_upd_u_ud 
            AFTER DELETE ON users
            FOR EACH ROW
            -- to save the user data even if the account is deleted
            UPDATE users_detail SET is_active = FALSE;
      ```
      <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            SET @id = 10000103;
            -- check the initial is_active state of the user_detail
            SELECT * FROM users_detail WHERE user_id = @id;
            -- call this stored proc which disables all foreign key constraints associated to the query
            CALL exec_const_qry("DELETE FROM users WHERE user_id = @id");
            -- check the new is_active state of the user_detail after a user associated to that details is deleted
            SELECT * FROM users_detail WHERE user_id = @id;
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/tr3-1.png)
      </details>

      <br>

* ***Stored Fuctions*** - A good database system should have stored functions prepared to do the redundant common tasks like concatinating multiple columns or returning a scalar value (single value) from a query.

   10.   **`Query: 10`** 
         ```SQL
            CREATE FUNCTION full_name(
               fname CHAR(30),
               lname CHAR(30)
            )
            -- DETERMINISTIC means that you insure that the result will always be the same
            RETURNS CHAR(60) DETERMINISTIC
            RETURN CONCAT(fname, ' ', lname);
         ```
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- calling it the traditional way
            SELECT user_id, full_name(fname, lname) FROM `users_detail` LIMIT 5 OFFSET 1;
            -- calling it within a stored proc which converts it into a dynamic query
            CALL exec_qry("SELECT full_name(fname, lname) FROM users_detail WHERE id =");
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func1-1.png)
         </details>

   <br>

   11.   **`Query 11`** 
         ```SQL
            CREATE FUNCTION full_address(
               street VARCHAR(100),
               city VARCHAR(50),
               state VARCHAR(50),
               country VARCHAR(50)
            )
            RETURNS VARCHAR(250) DETERMINISTIC
            RETURN CONCAT(street, ", " , city, " City", ", ", state, ", ", country);
         ```
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- call the function and supply the needed parameters
            SELECT schools.name, full_address(schools.saddress, cities.name, states.name, countries.name) FROM schools INNER JOIN cities ON schools.city_id = cities.city_id INNER JOIN states ON cities.state_id = states.state_id INNER JOIN countries ON states.country_id = countries.country_id;
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func2-1.png)
         </details>

   <br>

   12.   **`Query: 12`**
         ```SQL
            CREATE FUNCTION file_extension(
               tbl VARCHAR(50)
            )
               RETURNS VARCHAR(250) NOT DETERMINISTIC
               RETURN CONCAT(tbl,'_',CURDATE(),"_",HOUR(CURRENT_TIME),"_",MINUTE(CURRENT_TIME),"_",SECOND(CURRENT_TIME),'_copy');
         ```
         <details>
         <summary>Show more...</summary>
         **`Query for the calling program:`**
         ```SQL
            SET @tb = 'users';

            -- use this to create a unique filename every time (see Query #9)
            SELECT CONCAT(file_extension(@tb), '.csv');
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func3-1.png)
         </details>

   <br>

* ***General Queries*** - A good database system should be able to perform all kinds of techniques that a RDBMS has provided.

<details>
   <summary>functions, clauses, ...</summary>

   **`Aggregate Functions`** <br>
   `COUNT()`, `COUNT(DISTINCT)`, `SUM()`, `AVG()`, `MIN()`, `MAX()`<br><br>
   **`Mathematical Functions`** <br>
   `CEILING()`, `FLOOR()`, `ABS()`, `POW()`, `ROUND()`, `MOD()`, `RAND()`<br><br>
   **`Window (Non Aggregate) Functions`** <br>
   `DENSE_RANK()`, `RANK()`, `NTILE()`, `FIRST_VALUE()`, `LAST_VALUE()`, `ROW_NUMBER`<br><br>
   **`Date and Time Functions`** <br>
   `CURRENT_TIMESTAMP`, `CURDATE()`, `DAY()`, `HOUR()`, `MINUTE()`, `SECOND()` <br><br>
   **`Information Functions`** <br>
   `USER()`, `CURRENT_USER()`, `SESSION_USER`  <br><br>
   **`JOIN Clauses`** <br>
   `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN` <br><br>
   **`Others`** <br>
   `CONCAT()`
</details>

   9. **`Query 9: Verify User`** - This stored procedure is responsible for verifying a user based on the input (username, password, email) they give and then returns username and user_id if verified.
      ```SQL
         CREATE PROCEDURE verifyUser(
            IN v_uname VARCHAR(50),
            IN v_pword VARCHAR(80),
            IN v_email VARCHAR(80),
            OUT ov_uname VARCHAR(50),
            OUT ov_uid INT(11)
         )
         BEGIN 

            SET @aid = 10000;
            -- select user... if verified, return username and userid (to use as session_data for the application)
            SELECT uname, user_id 
            INTO ov_uname, ov_uid 
            FROM users 
            WHERE u_cl_id != @aid AND uname = v_uname AND pword = v_pword AND email = v_email 
            LIMIT 1;
               
         END //

         DELIMITER ;
      ```
      Verifying a user is important and is a standard of any application thus, stored procedure is a suitable method since it insures consistency on the results, and it improves transmission speed.
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- SET the needed data for convenience. In this case, a correct one.
         SET @uname = 'username123';
         SET @pword = 'password123';
         SET @pword = 'email@email.com';

         -- CALL the procedure in which it should return 2 data from users table
         CALL verifyUser(@uname, @pword, @email, @s_uname, @s_uid);

         -- SELECT the OUT parameter to get the ouput
         SELECT @s_uname, @s_uid;
      ```
      </details>

      <br>
   
   10.   **`Query 10: Retrieve Personal Information`** - This select statement is responsible for getting the full name, full address, contact number, & email of a user.

         ```SQL
            SET @aid = 10000; -- admin id
            SET @uid = 10000105; -- student id (input)

            SELECT full_name(ud.fname, ud.lname) 
            AS Full_Name
            ,full_address(ud.saddress, ct.name, sts.name, ctr.name) 
            AS Full_Address
            ,ud.contact_no 
            AS Contact_No
            ,us.email 
            AS Email

            FROM users us

            LEFT JOIN users_detail ud 
            USING(user_id)
            LEFT JOIN cities ct 
            USING(city_id)
            LEFT JOIN states sts 
            USING(state_id)
            LEFT JOIN countries ctr 
            USING(country_id)

            WHERE us.u_cl_id != @aid 
            AND us.user_id = @uid

            ORDER BY ud.fname 
            ASC;
         ```
         This is important since in order to present user data effectively to the end-users, we have to accept a single request to point to a query that will return all the necessary data about them.

      <br>

   11.   **`Query 11: Retrieve School Information`**

         ```SQL
            SET @scid = 3003; -- school id (input)

            SELECT 
            sc.name
            AS School_Name
            ,sc.email 
            AS Email
            ,full_address(sc.saddress, ct.name, sts.name, ctr.name)
            AS Full_Address

            FROM schools sc

            LEFT JOIN cities ct 
            USING(city_id)
            LEFT JOIN states sts 
            USING(state_id)
            LEFT JOIN countries ctr 
            USING(country_id)

            WHERE sc.school_id = @scid

            ORDER BY sc.name 
            ASC;
         ```
         Since this is an educational system, this query is important and the data sets the are retrieved from it

      <br>

   12.   **`Query 12: Retrieve Article Information`**

         ```SQL
            SELECT art.title AS Article_Title, full_name(ud.fname, ud.lname) AS Author, subj.name AS Subject, COUNT(ac.art_comm_id) AS Total_Comments FROM subjects subj INNER JOIN articles art USING (subj_id) INNER JOIN articles_comment ac USING (article_id) INNER JOIN users_detail ud ON art.author_id = ud.user_id GROUP BY subj.name ORDER BY Total_Comments DESC, art.title, subj.name ASC;
         ```

   
   2. Article Information - queries that are only associated with retrieving information about the articles

      **`Query 8: Retrieve Article Information`**
      ```SQL
         SET @article_title = "the"; -- article title (input)
         SET @act = CONCAT(@article_title,"%");

         SELECT 
         art.title
         AS Title
         ,art.content 
         AS Content
         ,subj.name
         AS Subject
         ,full_name(auth.fname, auth.lname)
         AS Full_Name
         ,art.created_at

         FROM articles art

         LEFT JOIN subjects subj 
         USING(subj_id)
         LEFT JOIN users us 
         ON art.author_id = us.user_id
         LEFT JOIN users_detail auth 
         USING(user_id)

         WHERE art.title LIKE @act -- where first characters 

         ORDER BY art.title 
         ASC;
      ```


* ***Stored Procedure***
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
            CALL getTableIncrement('users_detail', @ai);
            
            -- here is where the primary key originated
            INSERT INTO users_detail 
               ( fname, lname, contact_no, saddress, city_id, school_id ) 
            VALUES
               ( fn, ln, ct, sadd, city_id, schid );

            -- here is where the @ai value is used as primary/foreign key
            INSERT INTO users
               ( user_id, u_cl_id, uname, pword, rec_code, email ) 
            VALUES
               (@ai, ucl, un, pw, rec,em);

            -- create users (admin, author, and student) to insure that when created it has a Db account to refer from
            CREATE user IF NOT EXISTS 'admin'@'localhost';
            CREATE user IF NOT EXISTS 'author'@'localhost';
            CREATE user IF NOT EXISTS 'student'@'localhost';

            -- select the label from user_class
            SELECT label INTO @lb FROM user_class WHERE u_cl_id = ucl;

            -- perform prepared statements that is contained within another stored proc "exec_qry":
            -- create user as guest with their ip attached as their host
            CALL exec_qry(CONCAT("CREATE user IF NOT EXISTS @lb@localhost"));
            
            -- check privilege and react according to the findings:         
            -- use that selected value here to determine if the original number of privileges has been updated (decreased)
            CALL exec_qry(CONCAT("SELECT COUNT(user) INTO @c1 FROM mysql.columns_priv WHERE user = @lb"));
            CALL exec_qry(CONCAT("SELECT COUNT(user) INTO @c2 FROM mysql.tables_priv WHERE user = @lb"));
            
            -- if so, then run this stored proc to rerun the original privileges that I initially set for them
            IF (@c1 < 14 || @c2 < 10) THEN
               CALL grantPrivUsers();
            END IF;
               
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
               SIGNAL SQLSTATE '45500'
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
            IN v_uname VARCHAR(50),
            IN v_pword VARCHAR(80),
            IN v_email VARCHAR(80),
            OUT ov_uname VARCHAR(50),
            OUT ov_uid INT(11)
         )
         BEGIN 

            SET @aid = 10000;
            -- select user... if verified, return username and userid (to use as session_data for the application)
            SELECT uname, user_id 
            INTO ov_uname, ov_uid 
            FROM users 
            WHERE u_cl_id != @aid 
               AND uname = v_uname 
               AND pword = v_pword 
               AND email = v_email 
            LIMIT 1;
               
         END //

         DELIMITER ;
      ```
      <details>
      <summary>Show more...</summary>

      **`Query for the calling program:`**
      ```SQL
         -- SET the needed data for convenience. In this case, a correct one.
         SET @uname = 'username123';
         SET @pword = 'password123';
         SET @pword = 'email@email.com';

         -- CALL the procedure in which it should return 2 data from users table
         CALL verifyUser(@uname, @pword, @email, @s_uname, @s_uid);

         -- SELECT the OUT parameter to get the ouput
         SELECT @s_uname, @s_uid;
      ```
      `Result: `
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp8-1.png)
      </details>

      <br>

   9. **`Query: 9:`**
      ```SQL
         DELIMITER //

         CREATE PROCEDURE copyToCSV(
            IN tbl VARCHAR(50),
            IN lim INT(11)
         )
         BEGIN

            -- fileExtension() is a stored function that concatenate dates in order to produce a unique string (see Query #16)
            SET @sql = CONCAT("SELECT * FROM ", tbl," LIMIT ", lim," OFFSET 1 INTO OUTFILE 'C:/CSV/",fileExtension(tbl),".csv' FIELDS ENCLOSED BY '`' TERMINATED BY ';' ESCAPED BY '`' LINES TERMINATED BY '\r\n'");
            
            -- limit should be less or equal to the total row of the table
            IF (lim > 0) THEN
               PREPARE stmt FROM @sql;
               EXECUTE stmt;
               DEALLOCATE PREPARE stmt;
            END IF;
            
         END //

         DELIMITER ;
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
            CALL exec_qry('SHOW GRANTS FOR root@localhost');

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

   12.   **`Query: 12`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE returnCustError(
               IN err VARCHAR(100)
            )

            SIGNAL SQLSTATE '45000'
               SET MESSAGE_TEXT = err //

            DELIMITER ;
         ```
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- call proc and insert the error msg you want to return
            CALL returnCustError("Custom error: User not found");
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp12-1.png)
         </details>

         <br>
   
   13.   **`Query: 13`**
         ```SQL
            -- student privileges
            GRANT SELECT ON studentportal.schools TO 'student'@'localhost';
            GRANT SELECT ON studentportal.cities TO 'student'@'localhost';
            GRANT SELECT ON studentportal.states TO 'student'@'localhost';
            GRANT SELECT ON studentportal.countries TO 'student'@'localhost';
            GRANT SELECT ON studentportal.articles TO 'student'@'localhost';
            GRANT SELECT ON studentportal.subjects TO 'student'@'localhost';
            GRANT SELECT ON studentportal.articles_comment TO 'student'@'localhost';
            GRANT SELECT ON studentportal.articles_reply TO 'student'@'localhost';
            GRANT SELECT ON studentportal.author_subscription TO 'student'@'localhost';
            GRANT SELECT (uname, pword, rec_code, email, modified_at, created_at) ON studentportal.users TO 'student'@'localhost';
            GRANT SELECT (fname, lname, contact_no, saddress, city_id, school_id) ON studentportal.users_detail TO 'student'@'localhost';

            GRANT UPDATE (uname, pword, is_active) ON studentportal.users TO 'student'@'localhost';
            GRANT UPDATE (fname, lname, contact_no, saddress, city_id, school_id) ON studentportal.users_detail TO 'student'@'localhost';
            GRANT UPDATE (comment, modified_at) ON studentportal.articles_comment TO 'student'@'localhost';
            GRANT UPDATE (reply, modified_at) ON studentportal.articles_reply TO 'student'@'localhost';
            GRANT UPDATE (is_active, modified_at) ON studentportal.author_subscription TO 'student'@'localhost';

            GRANT INSERT ON studentportal.articles_comment TO 'student'@'localhost';
            GRANT INSERT ON studentportal.articles_reply TO 'student'@'localhost';
            GRANT INSERT ON studentportal.author_subscription TO 'student'@'localhost';

            -- author privileges
            GRANT SELECT ON studentportal.schools TO 'author'@'localhost';
            GRANT SELECT ON studentportal.cities TO 'author'@'localhost';
            GRANT SELECT ON studentportal.states TO 'author'@'localhost';
            GRANT SELECT ON studentportal.countries TO 'author'@'localhost';
            GRANT SELECT ON studentportal.articles TO 'author'@'localhost';
            GRANT SELECT ON studentportal.subjects TO 'author'@'localhost';
            GRANT SELECT ON studentportal.articles_comment TO 'author'@'localhost';
            GRANT SELECT ON studentportal.articles_reply TO 'author'@'localhost';
            GRANT SELECT ON studentportal.author_subscription TO 'author'@'localhost';
            GRANT SELECT (uname, pword, rec_code, email, modified_at, created_at) ON studentportal.users TO 'author'@'localhost';
            GRANT SELECT (fname, lname, contact_no, saddress, city_id) ON studentportal.users_detail TO 'author'@'localhost';

            GRANT UPDATE (uname, pword, is_active) ON studentportal.users TO 'author'@'localhost';
            GRANT UPDATE (fname, lname, contact_no, saddress, city_id) ON studentportal.users_detail TO 'author'@'localhost';
            GRANT UPDATE (comment, modified_at) ON studentportal.articles_comment TO 'author'@'localhost';
            GRANT UPDATE (reply, modified_at) ON studentportal.articles_reply TO 'author'@'localhost';
            GRANT UPDATE (subj_id, title, content, modified_at) ON studentportal.articles TO 'author'@'localhost';

            GRANT INSERT ON studentportal.articles TO 'author'@'localhost';
            GRANT INSERT ON studentportal.articles_comment TO 'author'@'localhost';
            GRANT INSERT ON studentportal.articles_reply TO 'author'@'localhost';

            GRANT DELETE ON studentportal.articles_comment TO 'author'@'localhost';
            GRANT DELETE ON studentportal.articles_reply TO 'author'@'localhost';

            -- admin/super_user
            -- grant all privileges including the ability to grant other users their privilege
            GRANT ALL PRIVILEGES ON studentportal.* TO 'admin'@'localhost' WITH GRANT OPTION;
         ```
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gr1-1.png)
         </details>

         <br>
   
* ***Triggers*** 
   
   4. ```SQL
       SELECT * FROM TAGURU
       ```

* ***Functions*** 

   4. ```SQL
       SELECT * FROM TAGURU
       ```
       
* ***Transactions*** 
   1. **`Query: _`**
      ```SQL
         -- means that queries next to it are now insde a transaction
         START TRANSACTION;

         -- the effects of these queries are temporary and reflected only in session.
         -- outside this session, nothing is changed yet
         INSERT INTO articles_comment
            (user_Id, article_id, comment)
            VALUES(10000105, 1001, 'Wow this article is so great. I love africa.');
         -- savepoint is basically saving this part of the transaction (first insertion)
         SAVEPOINT ins_a;

         INSERT INTO articles_comment
            (user_Id, article_id, comment)
            VALUES(10000106, 1001, 'This article is not great. I hate africa.');
         -- save second insertion
         SAVEPOINT ins_b;

         -- select the changes so far before doign a rollback. It should return 2 data set
         SELECT * FROM articles_comment;

         -- Rollback means to go back or redo the changes back to a previous state
         -- in this case, go back to the first insertion state
         ROLLBACK TO ins_a;

         -- select the changes after the rollback. The table should only have 1 data set
         SELECT * FROM articles_comment;
      ```
      <details>
         <summary>Show more...</summary>
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trans1-1.png)
      </details>

      <br>

   2. **`Query: _`**
      ```SQL
            DELIMITER //

            CREATE PROCEDURE testTransaction(
               -- use this 2 variables to output the 2 test results
               OUT result1 VARCHAR(100),
               OUT result2 VARCHAR(100)
            )

            BEGIN

               START TRANSACTION;

               -- wrong credentials
               SELECT COUNT(*) INTO @cc1 FROM users WHERE uname = 'qwesdaw' AND pword = 'qweoasdiqwe';
               -- update the latest log of the user (base on the credentials submitted) into the current datetime
               UPDATE users SET latest_log = CURRENT_TIMESTAMP WHERE uname = 'qwesdaw' AND pword = 'qweoasdiqwe';

               IF @cc1 > 0 THEN
                  COMMIT;
                  SET result1 = 'verified';
               ELSE
                  ROLLBACK;
                  SET result1 = 'wrong credentials';
               END IF;

               -- do the same thing but with correct credentials
               SELECT COUNT(*) INTO @cc2 FROM users WHERE uname = 'admin101' AND pword = 'admin101';
               UPDATE users SET latest_log = CURRENT_TIMESTAMP WHERE uname = 'admin101' AND pword = 'admin101';

               IF @cc2 > 0 THEN
                  COMMIT;
                  SET result2 = 'verified';
               ELSE
                  ROLLBACK;
                  SET result2 = 'wrong credentials';
               END IF;

               -- select the users table to check for changes
               SELECT latest_log FROM users WHERE uname = 'admin101' AND pword = 'admin101';

            END //

            DELIMITER ;
      ```
      <details>
         <summary>Show more...</summary>
         **`Query for the calling program:`**
         ```SQL
            -- call procedure
            CALL testTransaction(
               @result1,
               @result2
            );

            -- check the test results
            SELECT @result1, @result2;
         ```
         `Result: `
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trans2-1.png)
      </details>

      <br>

* ***Db User Management***
<pre style="height: 500px">
   1. CREATE USER
   2. GRANT PRIVILEGE
   3. DROP USER / FLUSH PRIVILEGES
</pre>
