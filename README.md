# Database Documentation

## The database

This database is made to manage the data needed for an Educational Blog Site. At the moment, it is consist of twelve (12) entities which are responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles. It is operated in a RDBMS (Relational Database Management System) called MySQL using the PhpMyAdmin platform. Multiple complex queries (showing entity relationships' various links and operations) are documented using this setup.

>I am planning to add more features in the future, hoping to create a multi-faceted system for a Student Portal Web Application.

<br>

## Table of Contents

* [ERD](https://github.com/centino90/Advance-Database-Documentation/#The-database)
   *  Diagram
   *  Entities and Description
* [FDD](https://github.com/centino90/Advance-Database-Documentation/#Functional-Dependency-Diagram-FDD)
* [Complex Queries](https://github.com/centino90/Advance-Database-Documentation/#Complex-Queries-associated-with-the-database)
   *  Establish Tables and Relationships
   *  User Management
   *  Views
   *  Reports
   *  Triggers
   *  Stored Functions
   *  Transactions
   *  Stored Procedures
   *  General Queries
* [End of file](https://github.com/centino90/Advance-Database-Documentation/#end-of-file)
   

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


* ***<h4>Establish Tables and Relationships (Create Table, Primary key, Foreign key, etc.)</h4>*** - A good database system should be able to create relational connection between its tables

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
      <details>
      <summary>Show more...</summary>

      `Result: `

      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/ct1-1.PNG)
      </details>
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

      <details>
      <summary>Show more...</summary>

      `Result: `
      
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gr1-1.PNG)
      </details>

      <br>

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

      <details>
      <summary>Show more...</summary>

      `Result: `
      
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gr2-1.PNG)
      </details>

      <br>

   4. **`Query 4: Admin/Super_User Privileges`**
      ```SQL
         -- admin/super_user
         -- grant all privileges including the ability to grant other users their privilege
         GRANT ALL PRIVILEGES ON studentportal.* TO 'admin'@'localhost' WITH GRANT OPTION;   
      ```
      The admin should have all the privileges pointed to a particular database, and should be able to grant privileges to other users to manage them accordingly.

      <details>
      <summary>Show more...</summary>

      `Result: `
      
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gr3-1.PNG)
      </details>

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

         Creating a view is important to represent a summarized data meaningfully into another set of container that behaves like a table

         <details>
         <summary>Show more...</summary>

         `Result: `
         
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/cv1-1.PNG)
         </details>

   <br>

   6. **`Query 6: Create View that Ranks Subjects Based On the Number of Articles Created`**

         ```SQL
            CREATE VIEW popSubjBasedOnArt AS

            SELECT subj.name AS Subject, subj.created_at AS Date_Created, COUNT(art.article_id) AS Total_Articles 
            FROM subjects subj 
            LEFT JOIN articles art USING(subj_id) 
            GROUP BY subj.name 
            ORDER BY Total_Articles DESC, subj.name ASC;

         ```

         Creating a separate view for ranked subjects based on articles created is meaningful and is easier to present summarized data sets.

         <details>
         <summary>Show more...</summary>

         `Result: `
         
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/cv2-1.PNG)
         </details>

   <br>

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

         Creating a separate view for ranked comments based on replies is meaningful and is easier to present summarized data sets.

         <details>
         <summary>Show more...</summary>

         `Result: `
         
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/cv3-1.PNG)
         </details>

   <br>

* ***Reports*** - A good database system should be able to accept requests successfully, and process it to create consistent and accurate reports to send back to the end-users.
   
   8. **`Query 8: Create Summary Report from Views`** - Create a report from a View.

      ```SQL
         -- this is a summary report of a View
         SELECT SUM(Total_Comments) AS Grand_Total
         , AVG(Total_Comments) AS Average
         , MAX(Total_Comments) AS Maximum
         , MIN(Total_Comments) AS Minimum 
         FROM popartbasedoncomm
      ```

      This will enhance how user-friendly the reports are by providing the end-users an improved simplification of data set.

      <details>
      <summary>Show more...</summary>

      `Result: `
      
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/rep1-1.PNG)
      </details>

   <br>

   9. **`Query 9: Create Summary Report 2`** - This is an improved version of query #8 where instead of 1, 3 views are summarized.

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

      <details>
      <summary>Show more...</summary>

      `Result: `
      
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/rep2-1.PNG)
      ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/rep2-2.PNG)
      </details>
      
      
   <br>

* ***Triggers*** - A good database system should have triggers in place to perform actions that always happen before or after an insertion, updation, and deletion.

   10. **`Query 10: Trigger modification date to current_timestamp`**
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

         This is essential to minimize the coding everytime we update something by rows.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- check the initial state of the field before update
            SELECT modified_at 
            FROM articles 
            WHERE article_id = 1021;

            -- perform update to trigger the trigger
            UPDATE articles
            SET title = 'The History of Africa'
            WHERE article_id = 1021;

            -- check the field after update
            SELECT modified_at 
            FROM articles 
            WHERE article_id = 1021;
         ```
         `Result:`

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg1-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg1-2.PNG)
         </details>

   <br>

   11. **`Query 11: Trigger delete replies associated to deleted comment`** 
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

         This is essential to automatically remove the replies associated in that comment without hassling to code it everytime we delete a comment.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            SET @id = 100027;
            -- select the initial state of the replies before delition of comments
            SELECT * FROM articles_reply WHERE art_comm_id = @id;
            -- call this stored proc which disables all foreign key constraints associated to the query (see query #20)
            CALL exec_const_qry("DELETE FROM articles_comment WHERE art_comm_id = @id");
            -- select the new state of the replies after delition of comments. All replies within the deleted comment should be deleted also
            SELECT * FROM articles_reply WHERE art_comm_id = @id;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg2-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg2-2.PNG)
         </details>

   <br>

   12. **`Query 12: Trigger update is_active associated to deleted user_id`** 
         ```SQL
            CREATE TRIGGER trg_upd_u_ud 
               AFTER DELETE ON users
               FOR EACH ROW
               -- to save the user data even if the account is deleted
               SET is_active = FALSE;
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

         This is basically the same from the previous one, but it keeps the data instead of deleting it so its crucial for keeping datas.

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg3-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trg3-2.PNG)
      </details>

   <br>

* ***Stored Fuctions*** - A good database system should have stored functions prepared to do the redundant common tasks like concatinating multiple columns or returning a scalar value (single value) from a query.

   13.   **`Query 13: Concatinate Fname and Lname`** 
         ```SQL
            CREATE FUNCTION full_name(
               fname CHAR(30),
               lname CHAR(30)
            )
            -- DETERMINISTIC means that you insure that the result will always be the same
            RETURNS CHAR(60) DETERMINISTIC
            RETURN CONCAT(fname, ' ', lname);
         ```

         Every iteration of concatinating a full name is made less hassle and effective by this function.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            SELECT user_id, full_name(fname, lname) FROM `users_detail` LIMIT 5 OFFSET 1;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func1-1.PNG)
         </details>

   <br>

   14. **`Query 14: Concatinate full address`** 
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

         Every iteration of concatinating multiple variables such as a full address is made faster and effective by this function.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- call the function and supply the needed parameters
            SELECT schools.name, full_address(schools.saddress, cities.name, states.name, countries.name) FROM schools INNER JOIN cities ON schools.city_id = cities.city_id INNER JOIN states ON cities.state_id = states.state_id INNER JOIN countries ON states.country_id = countries.country_id;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func2-1.PNG)
         </details>

   <br>

   15.   **`Query 15: Concatinate file name and extension`**
         ```SQL
            CREATE FUNCTION file_extension(
               tbl VARCHAR(50)
            )
               RETURNS VARCHAR(250) NOT DETERMINISTIC
               RETURN CONCAT(tbl,'_',CURDATE(),"_",HOUR(CURRENT_TIME),"_",MINUTE(CURRENT_TIME),"_",SECOND(CURRENT_TIME),'_copy');
         ```

         Creating a unique file name and extension is made faster and effective by this function.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            SET @tb = 'users';

            -- use this to create a unique filename every time (see Query #9)
            SELECT CONCAT(file_extension(@tb), '.csv');
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/func3-1.PNG)
         </details>

   <br>

* ***Transactions*** - A good database system should have ACID (Atomicity, Consitency, Isolation, Durability) properties in place to manage operations (transactions) that are essential to the end-users.

   16. **`Query 16: Transaction with Rollback`**
         ```SQL
            START TRANSACTION;

            -- the effects of these queries are temporary and reflected only in session.
            -- outside this session, nothing is changed yet
            INSERT INTO articles_comment
               (user_id, article_id, comment)
               VALUES(10000105, 1001, 'Wow this article is so great. I love africa.');
            -- savepoint is basically saving this part of the transaction (first insertion)
            SAVEPOINT ins_a;

            INSERT INTO articles_comment
               (user_id, article_id, comment)
               VALUES(10000106, 1001, 'This article is not great. I hate africa.');
            -- save second insertion
            SAVEPOINT ins_b;

            -- select the changes so far before doign a rollback. It should return 2 data set
            SELECT * FROM articles_comment WHERE art_comm_id >= 100882;

            -- Rollback means to go back or redo the changes back to a previous state
            -- in this case, go back to the first insertion state
            ROLLBACK TO ins_a;

            -- select the changes after the rollback. The table should only have 1 data set
            SELECT * FROM articles_comment WHERE art_comm_id >= 100882;
         ```

         This is very important when it comes to dealin with sensitive operations that are happening in real time. Transaction and rollback keeps 'transactions' within session and its changes do not affect the global state, yet.

         <details>
         <summary>Show more...</summary>

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trans1-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trans1-2.PNG)
         </details>

   <br>

   17. **`Query 17: Transaction with Commit inside Stored Procedure`**
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

         Commits allow transactions to be permanent in session and global.

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

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/trans2-1.PNG)
         </details>

   <br>

* ***Stored Procedures*** - A good database system should have stored procedures stored within the database to further automate the processes and operations when interacting with the system.

   18. **`Query 18: Verify User`** - This stored procedure is responsible for verifying a user based on the input (username, password, email) they give and then returns username and user_id if verified.
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
               SET @email = 'email@email.com';

               -- CALL the procedure in which it should return 2 data from users table
               CALL verifyUser(@uname, @pword, @email, @s_uname, @s_uid);

               -- SELECT the OUT parameter to get the ouput
               SELECT @s_uname, @s_uid;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp1-1.PNG)
         </details>

   <br>

   19. **`Query 19: Insert New User Via Stored Procedure`**
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
               -- do this to insure that the db user is created
               CALL exec_qry(CONCAT("CREATE user IF NOT EXISTS @lb@localhost"));
               
               -- check privilege and react according to the findings:         
               -- use that selected value here to determine if the original number of privileges has been updated (decreased)
               CALL exec_qry(CONCAT("SELECT COUNT(user) INTO @c1 FROM mysql.columns_priv WHERE user = @lb"));
               CALL exec_qry(CONCAT("SELECT COUNT(user) INTO @c2 FROM mysql.tables_priv WHERE user = @lb"));
               
               -- if so, then run this stored proc to rerun the original privileges that I initially set for them
               IF (@c1 < 19 || @c2 < 11) THEN
                  CALL grantPrivUsers();
               END IF;
                  
            END //

            DELIMITER ;
         ```

         inserting a new user is very common and happens everytime a new user goes to the application, putting it inside stored procedure is a good choice where we can do if..else statements to further restrict the input.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
         -- check the total rows before calling the procedure to get the initial number
            SELECT COUNT(user_id) 
               FROM users_detail;
            SELECT COUNT(user_id) 
               FROM users;

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
            -- in addition, also check if the current user was added to the userlist of all mysql account
            SELECT * 
               FROM mysql.user 
               WHERE host = 'localhost';

            -- also check the previliges it has 
            SHOW GRANTS FOR 'student'@'localhost'
         ```
         `Result:`

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-2.PNG) 
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-3.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-4.PNG) 
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-5.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp2-6.PNG) 
         </details>

   <br>

   20. **`Query: 20: Create .CSV file based on selected data from a table`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE copyToCSV(
               IN tbl VARCHAR(50),
               IN lim INT(11)
            )
            BEGIN

               -- fileExtension() is a stored function that concatenate dates in order to produce a unique string (see Query #12)
               SET @sql = CONCAT("SELECT * FROM ", tbl," LIMIT ", lim," OFFSET 1 INTO OUTFILE 'C:/CSV/",fileExtension(tbl),".csv' FIELDS ENCLOSED BY '`' TERMINATED BY ';' ESCAPED BY '`' LINES TERMINATED BY '\r\n'");
               
               -- limit should not be less or equal to 0 to be able to perform the prepared stmt
               IF (lim > 0) THEN
                  PREPARE stmt FROM @sql;
                  EXECUTE stmt;
                  DEALLOCATE PREPARE stmt;
               END IF;
               
            END //

            DELIMITER ;
         ```

         This is important to insure the uniqueness of all outgoing .csv files.

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

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp3-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp3-2.PNG)
         </details>

   <br>

   21. **`Query 21: Execute Parameterized Query that Disables Foreign Constraints momentarily`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE exec_const_qry(
               IN p_sql varchar(500)
            )
            
            BEGIN

            -- disable foreign key constraint
            SET FOREIGN_KEY_CHECKS = 0;

            SET @tquery = p_sql;
            PREPARE stmt FROM @tquery;
            EXECUTE stmt;
            DEALLOCATE PREPARE stmt;

            -- enable foreign key constraint
            SET FOREIGN_KEY_CHECKS = 1;

            END //
            
            DELIMITER ;
         ```

         This is very handy helper to disable foreign key constraint when querying.

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- deleting a user_detail should not be permitted since it is connected to a user somewhere
            -- but through this procedure it can be done
            -- initial count
            SELECT COUNT(*) FROM users_detail;
            
            CALL exec_const_qry('DELETE FROM users_detail where user_id = 10000461');
            
            -- after count
            SELECT COUNT(*) FROM users_detail;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp4-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp4-2.PNG)
         </details>

   <br>

   22. **`Query 22: Retrieve Current Increment of a Table`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE getTableIncrement(
               IN tbl VARCHAR(50),
               OUT ai VARCHAR(11)
            )

            BEGIN
            -- this can be used to assign the next increment value to a variable to use it as reference (since the next primary key is predictable due to it being incremented automatically by 1)
               SET @tbl = tbl;
               SELECT `AUTO_INCREMENT`
               INTO ai FROM INFORMATION_SCHEMA.TABLES
               WHERE TABLE_SCHEMA = 'studentportal' AND TABLE_NAME = tbl;

            END //
               
            DELIMITER ;
         ```

         This comes in handy when we to get the current increment count of a table to insert, update, or delete data
            
         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            CALL getTableIncrement('articles_reply', @ai1);
            CALL getTableIncrement('articles', @ai2);
            CALL getTableIncrement('articles_comment', @ai3);

            SELECT @ai1, @ai2, @ai3;
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp5-1.PNG)
         </details>

   <br>

   23.   **`Query 23: Trigger a Customer Error by Calling it within a Store Procedure`**
         ```SQL
            DELIMITER //

            CREATE PROCEDURE returnCustError(
               IN err VARCHAR(100)
            )

            SIGNAL SQLSTATE '45000'
               SET MESSAGE_TEXT = err //

            DELIMITER ;
         ```

         This will come handy when we want to force a request to fail and return an error instead of an empty row;

         <details>
         <summary>Show more...</summary>

         **`Query for the calling program:`**
         ```SQL
            -- call proc and insert the error msg you want to return
            CALL returnCustError("Custom error: User not found");
         ```
         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/sp6-1.PNG)
         </details>

   <br>

* ***General Queries*** - A good database system should be able to perform all kinds of techniques that the RDBMS has provided.
   
   24.   **`Query 24: Retrieve Personal Information`** - This select statement is responsible for getting the full name, full address, contact number, & email of a user.

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

         <details>
         <summary>Show more...</summary>

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen1-1.PNG)
         </details>

   <br>

   25. **`Query 25: Retrieve School Information`**

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

         <details>
         <summary>Show more...</summary>

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen2-1.PNG)
         </details>

   <br>

   26. **`Query 26: Retrieve Article Information base on Title Match`**

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

         This is important when searching through about an article with partial data and just match it to return possible matches

         <details>
         <summary>Show more...</summary>

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen3-1.PNG)
         </details>

   <br>
   
   27. **`Query 27: Show all Stored Routines (Procedure, Function, Trigger)`**

         ```SQL
               -- show all triggers
               SELECT TRIGGER_NAME 
               FROM INFORMATION_SCHEMA.triggers 
               WHERE TRIGGER_SCHEMA = 'studentportal';

               -- show functions and procedures
               SELECT * FROM INFORMATION_SCHEMA.ROUTINES;
         ```

         This is important when you want to check all routines you have in database

         <details>
         <summary>Show more...</summary>

         `Result: `

         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen4-1.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen4-2.PNG)
         ![image](https://github.com/centino90/Advance-Database-Documentation/blob/main/img/stored_procedures/gen4-3.PNG)
         </details>

   <br>

   28. **`Query 28: Store all initial user privileges`**

      ```SQL
         DELIMITER //

         CREATE PROCEDURE grantPrivUsers()

         BEGIN 
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
         END //

         DELIMITER ;
      ```

=============================
============ [Go back](https://github.com/centino90/Advance-Database-Documentation/#The-database)
=============================
# End of file