# Database Documentation

## The database

This database is made to manage the data needed for a Student Portal System. At the moment, this database is consist of nine (9) entities and is responsible for users data management; creation, updation, & deletion management of user (students & authors) transactions and articles, although I am planning to include more features to this database system in the future. This database will be operated in a RDBMS (Relational Database Management System) called MySQL. With that, multiple complex queries and methods to manage all of the relationships between entities will be used.

<br />

## Entity Relationship Diagram

<img src="https://raw.githubusercontent.com/centino90/Advance-Database-Documentation/3d6b5b4dab9c31c4fb25daf66279319192273609/img/ERD.svg"/>

This database is an insource based on my plan to create a Student Portal Web Application in the future. The ERD shown in this documentation is made using Lucid Chart's general purpose diagram tool. The platform used to perform MySQL operations is PhpMyAdmin.

### Table Name and Description

#### Users
`- is responsible for storing necessary data upon the creation of a user and when its modified.`

#### Students
`- is responsible for storing necessary data upon the creation and modification of a user that has identified itself as a student.`

#### Authors
`- is responsible for storing necessary data upon the creation and modification of a user that has identified itself as an author.`

#### Schools
`- is responsible for storing necessary data upon the creation of a school and when its modified`

#### Articles
`- is responsible for storing necessary data upon the creation of an article and when its modified`

#### Subjects
`- is responsible for storing necessary data upon the creation of a subject and when its modified`

#### Author_subscriptions
`- is responsible for storing necessary data upon user subscription of the authors and when its modified`

#### Articles_engagement
`- is responsible for storing necessary data upon the creation and modification of user comment within an article (creates social engagement among users)`

#### Articles_reply
`- is responsible for storing necessary data upon the creation and modification of user replies within a comment`


