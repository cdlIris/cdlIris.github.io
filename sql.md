# Coursera: Introduction to SQL

## Command Lines:
start mysql
/Applications/MAMP/Library/bin/mysql -u root -p

## Operations:
CREATE DATABASE People DEFAULT CHARACTER SET utf8;

USE People;

CREATE TABLE UserS (
  name VARCHAR(128),
  email VARCHAR(128)
);

DESCRIBE Users;

INSERT INTO Users (name, email) VALUES ("Tester" "Tester@email.com");

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
  - # of seconds since 1970, stored in a 32-bit number
  - absolut sort of length
  - Y3K problem until 2023, 2 billion seconds(64-bit then)
- DATETIME: YYYY-MM-DD HH:MM:SS
  - any year, more space
- DATE: YYYY-MM-DD
- TIME: HH:MM:SS
- buitin MySQL function NOW()
