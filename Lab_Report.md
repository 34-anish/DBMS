
 
Student table which is created using  following query:
![fc17c72b4761eaee820d1ef1a90980e1.png](../_resources/fc17c72b4761eaee820d1ef1a90980e1.png)
```
DROP TABLE IF EXISTS Student;
CREATE TABLE Student(
	Id int,
	Username varchar(20),
	Email varchar(50),
	First_name varchar(20),
	Last_name varchar(20),
	Full_name varchar(20),
	DOB DATE NOT NULL,
	Age int,
	PRIMARY KEY (ID)
);	
```

![22bd6e1b747486f42e71fd3c75aef7e3.png](../_resources/22bd6e1b747486f42e71fd3c75aef7e3.png)
```
-- 3.1.	Function that returns the full name of the user with the given ID
CREATE OR REPLACE FUNCTION fullname(uid INTEGER)
	RETURNS character varying
	AS
	$$
	DECLARE
		fname CHARACTER VARYING;
		lname CHARACTER VARYING;
	BEGIN 
		SELECT First_name ,Last_name INTO fname,lname FROM Student WHERE ID = uid;
		
		RETURN fname || ' ' || lname;
	END;
	$$ LANGUAGE plpgsql;

SELECT fullname (1);
```
![e5d2b1840f65a39772d1585ed47f7e74.png](../_resources/e5d2b1840f65a39772d1585ed47f7e74.png)
```
-- 	3.2.	Function that returns the number of users
DROP FUNCTION IF EXISTS countuser();
CREATE OR REPLACE FUNCTION countuser()
	RETURNS int
	AS
	$$
	DECLARE
		total int;
	BEGIN 
		SELECT COUNT(*) INTO total FROM Student;
		
		RETURN total;
	END;
	$$ LANGUAGE plpgsql;

SELECT countuser() AS total_user;
```

![29ab34bc73bae0adda323d207aaef0b9.png](../_resources/29ab34bc73bae0adda323d207aaef0b9.png)
```--3.3. Function that returns the age of the user with the given ID

DROP FUNCTION IF EXISTS age(integer);
CREATE OR REPLACE FUNCTION age(uid INTEGER)
	RETURNS int
	AS
	$$
	DECLARE
		student_age int;
	BEGIN 
		SELECT  date_part('year',AGE(dob)) Into student_age FROM Student WHERE ID = uid;

		RETURN  student_age;
	END;
	$$ LANGUAGE plpgsql;

SELECT age(3);
SELECT * FROM Student;
```

![d1187e83d40bcb4f636846657bd149ae.png](../_resources/d1187e83d40bcb4f636846657bd149ae.png)
The disadvantage of the function is immediate effect is not seen
![b40c62369861b159bb1277b89eb532a8.png](../_resources/b40c62369861b159bb1277b89eb532a8.png)
![e217ea934f76d507a7b0400b3f397d89.png](../_resources/e217ea934f76d507a7b0400b3f397d89.png)
Due to trigger it can be seen that Avaya's tuple already has full name without inserting it
![36918cc2f5764dc6339916e11d3b255c.png](../_resources/36918cc2f5764dc6339916e11d3b255c.png)
Similarly the trigger for age works accordingly
AS the data with id 12 and 13  are redundat lets update it and hold the log information in our table user_logs
![385794c8401b0a98bd0b82444c2b7e81.png](../_resources/385794c8401b0a98bd0b82444c2b7e81.png)
```
UPDATE Student SET last_name ='Manandhar' where id =13;
UPDATE Student SET first_name ='Anish' where id =13;```