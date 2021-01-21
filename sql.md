# Coursera: Introduction to SQL

## Command Lines:
start mysql
/Applications/MAMP/Library/bin/mysql -u root -p

## Operations:
CREATE DATABASE People DEFAULT CHARACTER SET utf8;

USE People;

CREATE TABLE Users (
  user_id PRIMARY KEY INT UNSIGNED NOT NULL AUTO_INCREMENT,
  name VARCHAR(128),
  email VARCHAR(128),
  PRIMARY KEY (user_id),
  INDEX (name)
);

DESCRIBE Users;

INSERT INTO Users (name, email) VALUES ("Tester","Tester@email.com");

DELETE FROM Users WHERE email="Tester@email.com";

UPDATE Users SET name="tester01" WHERE email="Tester@email.com";

SELECT * FROM Users ORDER BY email;

SELECT * FROM Users WHERE name LIKE "%e%";

LIMIT :
	the first n rows, or first n from a starting row
	the first row is 0
	happens after ORDER BY

SELECT * FROM Users ORDER BY email DESC LIMIT 2;
SELECT * FROM Users ORDER BY email LIMIT 1,2;

SELECT COUNT(*) FROM Users;

## Data Types:
### Strings:
- CHAR: allocates entire space, known length
  ex: 20,20,20,10
- VARCHAR: allocates a varibale amount of space (less space)
  ex: 5, 100, 23
- Chaaracter set: for paragraph or html
  TINYTEXT: 255 
  TEXT: 65K
  MEDIUMTEXT: 16M
  LONGTEXT: 4G
- Generally not used with indexed and sort
### Binary Fields
- Character = 8-32 bits (depending on character set)
- bytes = 8 bits
  - BYTES: 255
  - VARBINARY: 65K bytes
- small images
- no indexing or sorting

### Binary Large Object 
- Large raw data/files/images, pdfs, movies
- Con: slow down the database backup
- No translation, indexing or character set
- TINYBLOB: 255
- BLOB: 65K
- MEDIUMBLOB: 16M
- LONGBLOB: 4G
### Nmbers
### Integers
- easy to process and little storage space
 - CPUS can compare integers with a sinlge instructins
- TINYINT: (-128, 128)
- SMALLINT: (-32768, 32768)
- INT: 2 billion
- BIGINT: 10**18 ish

### Floating Point Numbers
- respsent a wide range of values, but acc is limited
- FLOAT 32-bit, 7 digit acc
- DOUBLE 64bit, 14 digit acc

### Dates
- no go below 
- TIMESTAMP: YYYY-MM-DD HH:MM:SS
	- \# of seconds since 1970, stored in a 32-bit number
	- absolut sort of length
	- problem until 2023, 2 billion seconds(64-bit then)
- DATETIME: YYYY-MM-DD HH:MM:SS
	- any year, more space
- DATE: YYYY-MM-DD
- TIME: HH:MM:SS
- buitin MySQL function NOW()

### AUTO_INCREMENT 
join tables together need a primary int key for each row

### Index
when too much records
hashes (exact matches) or trees (prefix matches or sorting) 
- good for individual row loopup and sorting/grouping results 
- works best with exact mthces or prefix loopups 
- can suggst HASH or BTREE 

#### Keys
- primiary keys: find row in table (little space, exact match, no duplicate, fast for int fields)
- foreign keys: reference to primiary key in other table

## Building a Data Model
- Basic Rule: do not put the same string data in twice - use a relshtionship instead. 
- there should only be one copy of that thing in the database

### Database Normalizaiton (3NF)
- do not replicate data. Reference data instead
- Use int for keys and references
- Add a speical key column to each table, which you will make references to 

### Int Reference Pattern
- AUTO_INCREMENT, we use int columns in one table to ference rows in another table.

### Three kinds of keys
- primary key, generally an int auto-increment
- logical key, what the outside world uses for lookup (ex: title)
- foreign key, generally an int key pointing to a row in another table

### Primary Key Rules:
- never use your logical key as the primary key
- logical keys can and do change, albeit slowly
- relationships that are based on matching string fields are less efficient that ints.

### Foreign Keys
- when a table has a column contaiing a key that points to the primary key of another table
- when all primary keys are ints, then all foreign eys are ints. 

```
CREATE TABLE Album(
	album_id INTEGER NOT NULL AUTO_INCREMENT,
	title VARCHAR(255),
	artist_id INTEGER,
	PRIMARY KEY(album_id),
	INDEX USING BTREE (title),
	
	CONSTRAINT FOREIGN KEY(artist_id)
	 REFERENCES Artist (artist_id)
	 ON DELETE CASCADE ON UPDATE CASCADE
)ENGINE=InnoDB;
```
### Join
- Use Join with On, get all the combanations of the rows between two tables, filtering with the on clause. 
- If only use JOIN, give all combanations of all rows between 2 tabels. 
```
SELECT album.title, artist.name FROM album JOIN artist ON album.artist_id = artist.artist_id
```
- the speed using SQL is much faster

### ON DELETE CASCADE 
If the primary key is deleted, then the rows containng that foreign key in other table will be deleted 
- Default/ RESTRICT, Do not allow changes that break the parent rows 
- CASCADE, adjust child rows by removing or updating to maintain conssitency
- SET NULL, set the foreign key columns in the child rows to null. 

