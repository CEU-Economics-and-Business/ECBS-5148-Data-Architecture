---
title: "Data Modeling"
teaching: 220
exercises: 110
questions:
- ""
objectives:
- Recognize tidy data. 
- Create logical model for simple relational data and represent it in UML.
- Create and query a simple database in SQLite.
- Understand and apply normal forms 1-3 to simple relational data.
- Model many-to-one relationships.
- Model many-to-many relations through a link table.
- Create a star schema for transactional data.
- Separate attributes into dimensions and facts.
- Define table and document schema in SQL and Data Package.
keypoints:
- Normalized data can be maintained with fewer errors.
- Denormalization helps analysis speed.
- Denormalize data the momemnt before analysis.
---

FIXME: lesson is too long

## Reading
- [Data model](https://en.wikipedia.org/wiki/Data_model)
- [tidy data](http://www.jstatsoft.org/v59/i10/paper)

## Tidy data

```

blockdiag {
    messy -> tidy -> normalized -> denormalized;
}

```

Initially data is messy not because of performance trade-offs, but because of design errors. We fix these errors to create tidy data. Then we normalize data to improve maintainablity. Finally, we can denormalize data to improve performance. Note that denormalized data is very far from messy data.

## The Entity--Relationship model

Relying on Silberschatz, Korth and Sudarshan, 2011. "Database System Concepts" 6th Edition, McGraw Hill, Chapters 7--8.

FIXME: decide on language: records, rows, observations, features, columns, attributes, variables.

![]({{ "/fig/messy-data.png" | relative_url }})


> ## Exercise
> Look at the data displayed above. Identify six problems with this dataset.
> > ## Solution
> > 1. Columns not titled properly.
> > 2. Different types of data in a single column.
> > 3. Course title abbreviated.
> > 4. Names inconsistently recorded.
> > 5. Rows do not correspond to records.
> > 6. Information encoded in color/formatting.
> {: .solution}
{: .challenge}

![]({{ "/fig/StudentList.png" | relative_url }})

> ## Exercise
> For the data sources listed on the course homepage, answer the following questions.
> 1. What entities are represented in the data?
> 2. What is a record in the dataset?
> 3. If multiple entities are represented, how are they related? Explain in words.
> 
> For example, for the "Government Compensation in California Data" (1) employees, employers, regions, (2) one employee at one employer in a given year. (3) Employers have many employees. Employees may have many employers but we don't know. Employers may operate across several regions.
> 
> Or for the "Street Tree Census" (1) trees, street blocks, regions. (2) Unique tree trunks observed at a point in time. (3) Each tree is at a particular point with a precise coordinate. This then belongs to a street block, which belongs to regions at various levels of the hierarchy: ZIP code, city, borough. 
> 
> Do this for the "Habitatges d'ús turístic de la ciutat de Barcelona" and the "National Bridge Inventory."
> > ## Solution
> > ### "Habitatges d'ús turístic de la ciutat de Barcelona"
> > 1. Apartments or other places of accommodation. Streets and addresses.
> > 2. An apartment with a unique ID.
> > 3. Each apartment has a unique street address. Each adress can have many apartments. Addresses belong to streets, which belong to districts, etc.
> >
> > ### "National Bridge Inventory"
> > 1. Bridges. Roads. Regions. Organization.
> > 2. A bridge in a given year.
> > 3. Each bridge may serve a road. Roads can pass through many bridges. A bridge is owned by an organization. The point of the bridge belongs to a region.
> {: .solution}
{: .challenge}

### Conceptual data model
[An online tool](http://interactive.blockdiag.com/) to create block diagrams ("lines and boxes").

#### Government Compensation in California
```

blockdiag {
    employee1 -> employer1 -> region1;
    employee2 -> employer1;
    employer1 -> region2;
    employee3 -> employer2 -> region2;
}

```

```

blockdiag {
    employee -> employer [label = "m:1"];
    employer -- region [label = "m:m"];
}


#### Street Tree Census

blockdiag {
    tree1 -> street1 -> region1;
    tree2 -> street1;
    tree3 -> street2 -> region1;
}

```

```

blockdiag {
    tree -> street [label = "m:1"];
    street -> region [label = "m:1"];
}

```
#### Habitatges de Barcelona
```

blockdiag {
   apartment1 -> address1 -> street1;
   apartment2 -> address1 -> street1;
   apartment3 -> address2 -> street1;
}

```

{% assign table_data = site.data.sis.roster %}
{% assign table_caption = "Data from `roster.csv`" %}
{% include table.html %}

> ## Exercise
> Draw a block diagram representing the conceptual model of the student roster.
{: .challenge}

## Normalization

### First normal form
> All attributes are atomic. = All cells are single valued.

"Course Title" and "Student Name" clearly violate this condition.


{% assign table_data = site.data.sis.roster-1NF %}
{% assign table_caption = "Data from `roster-1NF.csv`" %}
{% include table.html %}

Strictly speaking, `ECBS5148` is not atomic. Also, programs may be further split up into "MS in Business Analytics", "full-time".

> ## Exercise
> Look at the [2015 Street Tree Census](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/uvpi-gqnh). Which of the columns violate the first normal form?
>> ## Solution
>> `problems` is a list. We can create a separate `problem` table, with a foreign key pointing to the tree with which there are problems. This would be a many-to-one relation.
>> `address` can be broken into number and street.
> {: .solution}
{: .challenge}

{% assign table_data = site.data.modeling.problem %}
{% assign table_caption = "Data from `problem` table" %}
{% include table.html %}

### Second normal form
> None of the columns depend functionally on a proper subset of primary keys.

What are the keys in this data table? A combination of (Course Code, Student ID), or any combination of columns with the same information. 

Clearly, "Course Title" depends only on "Course Code", not on "Student ID." Similarly, "Student First Name" depends only on "Student ID."

{% assign table_data = site.data.sis.roster-1NF-error %}
{% assign table_caption = "Updating student name in a row" %}
{% include table-error.html %}


{% assign table_data = site.data.sis.roster-2NF %}
{% assign table_caption = "Data from `roster-2NF.csv`" %}
{% include table.html %}

Yeah, but where is everything else?

{% assign table_data = site.data.sis.student-2NF %}
{% assign table_caption = "Data from `student-2NF.csv`" %}
{% include table.html %}

{% assign table_data = site.data.sis.course-2NF %}
{% assign table_caption = "Data from `course-2NF.csv`" %}
{% include table.html %}

### Third normal form
> No column depends on anything but the keys.

In `student-2NF.csv`, "Department" is a function of "Program."

{% assign table_data = site.data.sis.student-2NF-error %}
{% assign table_caption = "Data from `student-2NF-error.csv`" %}
{% include table-error.html %}

{% assign table_data = site.data.sis.roster-3NF %}
{% assign table_caption = "Data from `roster-3NF.csv`" %}
{% include table.html %}

{% assign table_data = site.data.sis.student-3NF %}
{% assign table_caption = "Data from `student-3NF.csv`" %}
{% include table.html %}

{% assign table_data = site.data.sis.course-3NF %}
{% assign table_caption = "Data from `course-3NF.csv`" %}
{% include table.html %}

{% assign table_data = site.data.sis.program-3NF %}
{% assign table_caption = "Data from `program-3NF.csv`" %}
{% include table.html %}

> ## Exercise
> Look at the [2015 Street Tree Census](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/uvpi-gqnh). After creating tables that satisfy the first normal form, 
which of the columns violate the third normal form?
>> ## Solution
>> Suppose there are two tables, a `problem` table with columns `tree_id` and `problem` (see example), and a 
`tree` table listing all other columns for each tree. In the `tree` table, `address` determines a lot of columns: `postcode`, `zip_city`, `borough` etc. These could be separated out in an `address` table. 
> {: .solution}
{: .challenge}


> ## Challenge
> Programs may have a "full-time" and a "part-time" stream. Courses and students belong to streams. Create a third normal form from the data with these assumptions.
{: .challenge}

FIXME: add notation. UML? primary keys? foreign keys?

> ## Challenge
> Draw the Entity-Relation diagram of the student database.
{: .challenge}

```

blockdiag {
    roster;
    course -> roster [label = "m:1"];
    student -> roster [label = "registers"];
    student -> program [label = "belongs to"];
    program -> department [label = "offered by"];
}

```

FIXME: draw proper UML diagrams

## Putting this to practice

With the shell command `csvsql -i sqlite *-3NF.csv` (we will learn about the tool `csvkit` later), we created the following SQL schema for the third normal form of the student database. We have replaced `FLOAT` with `INTEGER`, because we know IDs cannot take fractional values.
```
CREATE TABLE course (
	"Course Code" VARCHAR NOT NULL, 
	"Course Title" VARCHAR NOT NULL, 
	"Department" VARCHAR NOT NULL
);
CREATE TABLE program (
	"Program" VARCHAR NOT NULL, 
	"Department" VARCHAR NOT NULL
);
CREATE TABLE roster (
	"Course Code" VARCHAR NOT NULL, 
	"Student ID" INTEGER NOT NULL, 
	"Reg Mode" VARCHAR NOT NULL
);
CREATE TABLE student (
	"Student ID" INTEGER NOT NULL, 
	"Student First Name" VARCHAR NOT NULL, 
	"Student Last Name" VARCHAR NOT NULL, 
	"Program" VARCHAR NOT NULL, 
	"Year" INTEGER NOT NULL
);
```
{: .language-sql}

We can insert data with `csvsql --db sqlite:///sis.sqlite --insert *-3NF.csv`.

```
BEGIN TRANSACTION;
CREATE TABLE IF NOT EXISTS course (
	course_code VARCHAR NOT NULL, 
	course_title VARCHAR NOT NULL, 
	department VARCHAR NOT NULL
);
INSERT INTO course VALUES('ECBS5148','Data Architecture for Analysts','Department of Economics and Business');
INSERT INTO course VALUES('ECBS6001','Advanced Macroeconomics','Department of Economics and Business');
CREATE TABLE IF NOT EXISTS program (
	program VARCHAR NOT NULL, 
	department VARCHAR NOT NULL
);
INSERT INTO program VALUES('MS in Business Analytics (full time) - Dual','Department of Economics and Business');
INSERT INTO program VALUES('PhD in Economics - Dual','Department of Economics and Business');
INSERT INTO program VALUES('PhD in Cognitive Science – Dual','Department of Cognitive Science');
INSERT INTO program VALUES('MA in Economics - Dual','Department of Economics and Business');
CREATE TABLE IF NOT EXISTS roster (
	course_code VARCHAR NOT NULL, 
	student_id INTEGER NOT NULL, 
	registration_mode VARCHAR NOT NULL
);
INSERT INTO roster VALUES('ECBS5148',1000001,'Grade');
INSERT INTO roster VALUES('ECBS5148',1000002,'Grade');
INSERT INTO roster VALUES('ECBS5148',1000003,'Audit');
INSERT INTO roster VALUES('ECBS5148',1000004,'Grade');
INSERT INTO roster VALUES('ECBS6001',1000003,'Grade');
INSERT INTO roster VALUES('ECBS6001',1000005,'Grade');
CREATE TABLE IF NOT EXISTS student (
	student_id INTEGER NOT NULL, 
	first_name VARCHAR NOT NULL, 
	last_name VARCHAR NOT NULL, 
	program VARCHAR NOT NULL, 
	year INTEGER NOT NULL
);
INSERT INTO student VALUES(1000001,'Sean','Richmond','MS in Business Analytics (full time) - Dual',1);
INSERT INTO student VALUES(1000002,'Dana','McPherson','MS in Business Analytics (full time) - Dual',1);
INSERT INTO student VALUES(1000003,'Csongor','Barta','PhD in Economics - Dual',2);
INSERT INTO student VALUES(1000004,'Hanna','Csatár','PhD in Cognitive Science – Dual',3);
INSERT INTO student VALUES(1000005,'Lyudmila','Vinogradova','MA in Economics - Dual',2);
COMMIT;
```
{: .language-sql}

The following is a document from the _Offene Register_ data about German corporations in [YAML format](https://en.wikipedia.org/wiki/YAML). Don't worry about the specific format now, try to understand what the document says.
```
all_attributes: 
    _registerArt: HRB
    _registerNummer: 150148
    additional_data: 
        AD: true
        CD: true
        DK: true
        HD: false
        SI: true
        UT: true
        "VÖ": false
    federal_state: Hamburg
    native_company_number: "Hamburg HRB 150148"
    registered_office: Hamburg
    registrar: Hamburg
company_number: K1101R_HRB150148
current_status: "currently registered"
jurisdiction_code: de
name: "olly UG (haftungsbeschränkt)"
officers: 
 -  name: "Oliver Keunecke"
    other_attributes: 
        city: Hamburg
        firstname: Oliver
        flag: "vertretungsberechtigt gemäß allgemeiner Vertretungsregelung"
        lastname: Keunecke
    position: "Geschäftsführer"
    start_date: "2018-02-06"
    type: person
registered_address: "Waidmannstraße 1, 22769 Hamburg."
retrieved_at: "2018-11-09T18:03:03Z"
```
{: .source}

You can also [explore the database](https://db.offeneregister.de/openregister-ef9e802).

> ## Challenge
> Create an SQL schema for the Offene Register data. It should be able to store all the information in the dataset. Write the `CREATE TABLE` statements.
>> ## Solution
>> ```
>> CREATE TABLE company (
>>     registrar: TEXT NOT NULL,
>>     company_number: TEXT NOT NULL PRIMARY KEY,
>>     current_status: TEXT,
>>     name: TEXT,
>>     registered_address: TEXT,
>>     retrieved_at: FLOAT
>> );
>> CREATE TABLE registrar (
>>     registrar_id: TEXT NOT NULL PRIMARY KEY,
>>     federal_state: TEXT,
>>     jurisdiction_code: TEXT,
>> );
>> CREATE TABLE person (
>>     person_id: INTEGER NOT NULL PRIMARY KEY,
>>     name: TEXT,
>>     city: TEXT,
>>     firstname: TEXT,
>>     flag: TEXT,
>>     lastname: TEXT,
>>     type: TEXT
>> );
>> CREATE TABLE officer (
>>     company_number: TEXT NOT NULL,
>>     person_id: INTEGER NOT NULL,
>>     position: TEXT,
>>     start_date: INTEGER,
>>     end_date: INTEGER,
>> );
>> ```
>> {: .language-sql}
> {: .solution} 
{: .challenge}

## Denormalization
[cost of a join](https://www.brianlikespostgres.com/cost-of-a-join.html)

We can denormalize our database like
```
SELECT * FROM roster 
    LEFT JOIN course ON roster.course_code = course.course_code 
    LEFT JOIN student ON student.student_id = roster.student_id;
```
{: .language-sql}


> ## Challenge
> Write the `SQL` statement that denormalizes the Offene Register data.
>> ## Solution
>> ```
>> SELECT * FROM officer 
>>     JOIN company ON officer.company_number = company.company_number
>>     JOIN person ON person.person_id = officer.person_id
>>     JOIN registrar ON registrar.registrar = company.registrar;
>> ```
>> {: .language-sql}
> {: .solution}
{: .challenge}
