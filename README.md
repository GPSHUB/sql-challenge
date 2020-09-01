# sql-challenge
Employee Database: A Mystery in Two Parts - Giam Sigaud

DATA ENGINEERING (AKA MODELING) 

Video (https://www.youtube.com/watch?v=dR5lPbGLY84) on how to use https://app.quickdatabasediagrams.com/#/d/aAhUb9

** Foreign key from secondary table gets dragged to connect to the primary key in primary table on the ERD (entity relationship diagram). This reminds me of old school Microsoft Access connectors.
*** Quick DB automatically adds the " >- table2.col " when I connect the keys

ERD Terminology: https://www.youtube.com/watch?v=QpdhBUYk7Kk
Entity: (AKA: Table) Object like person, place, or thing, to be tracked in the DB. Ex: Customer, Order, Product.
	Entity shown as rows in DB
Attributes: (AKA: Columns) Entity property or traits. Ex: Entity = Customer & Attributes = First Name, Last Name.
	Attributes shown as columns in DB
Relationships: The lines that connect the Entities via the attributes. 
	ERD Cardinality is expressed by line type:
		One: 		---|- 
		Many: 		---<-
		One & ONLY one: ---||- One & Only one customer can be assigned to each order
		Zero or One: 	---O|-
		One or Many: 	---|<- One order must contain at least one product or many products
		Zero or Many: 	---O<- Customer can order 1 or infinite (many) orders


DATA ENGINEERING STEP ONE: REVIEW CSVS

Table Header Review: 
departments: dept_no, dept_name
dept_emp: emp_no, dept_no
dept_manager: dept_no, emp_no
employees: emp_no, emp_title_id, birth_date, first_name, last_name, sex, hire_date
salaries: emp_no, salary
titles: title_id, title

Looking for:
1. where to place primary and foreign keys on each file
2. how to connect each report to deliver answer to question


DATA ENGINEERING STEP TWO: * Use the information you have to create a table schema for each of the six CSV files. 
Remember to specify data types, primary keys, foreign keys, and other constraints.

Need to: 
1. Update datatypes and constraints: 
	a. main data types: https://www.postgresql.org/docs/9.5/datatype.html
		i. INT = Integer
			a. can follow any data type description with PK (primary key) or FK (foreign key) to designate key status.
		ii. VARCHAR = Variable Character 
			a. can follow VARCHAR with (#) to limit cell value count for the entire column
		iii. DATE - date
		iv. DEC -decimal
		v. BOOLEAN = same

DATA ENGINEERING STEP THREE: 

 * For the primary keys check to see if the column is unique, otherwise create a 
	[composite key](https://en.wikipedia.org/wiki/Compound_key). 
	Which takes to primary keys in order to uniquely identify a row.

departments / dept_no PK: 
	all rows unique = no need for compound key
dept_emp / emp_no FK / dept_no FK: 
	emp_no has 300024 unique from 331603 total indicating 31579 are duplicates = NOT PK so No COMPOUND KEY
	dept_no has 9 unique from 331603 total indicating 331594 are duplicates = NOT PK so No COMPOUND KEY
dept_manager / dept_no FK / emp_no FK:
	dept_no has 9 unique from 24 total indicating 15 are duplicates = NOT PK so No COMPOUND KEY
	emp_no has 24 unique from 24 total indicating none are duplicates = no need for compound key
employees / emp_no PK
	emp_no has 300024 unique from 300024 total indicating 0 are duplicates = no need for compound key
salaries / emp_no FK
	emp_no has 300024 unique from 300024 total indicating 0 are duplicates = no need for compound key
titles / title_id FK
	emp_no has 7 unique from 7 total indicating 0 are duplicates = no need for compound key

  * Be sure to create tables in the correct order to handle foreign keys.

