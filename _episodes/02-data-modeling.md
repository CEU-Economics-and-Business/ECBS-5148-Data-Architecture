---
title: "Data Modeling"
teaching: 100
exercises: 100
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
- ""
---

## Reading
- [Data model](https://en.wikipedia.org/wiki/Data_model)
- [tidy data](http://www.jstatsoft.org/v59/i10/paper)

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
> Or for the "Street Tree Census" (1) trees, street blocks, regions. (2) Unique tree trunks. (3) Each tree is at a particular point with a precise coordinate. This then belongs to a street block, which belongs to regions at various levels of the hierarchy: ZIP code, city, borough. 
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
{% blockdiag %}
blockdiag {
    employee1 -> employer1 -> region1;
    employee2 -> employer1;
    employer1 -> region2;
    employee3 -> employer2 -> region2;
}
{% endblockdiag %}

{% blockdiag %}
blockdiag {
    employee -> employer [label = "m:1"];
    employer -- region [label = "m:m"];
}
{% endblockdiag %}

#### Street Tree Census
{% blockdiag %}
blockdiag {
    tree1 -> street1 -> region1;
    tree2 -> street1;
    tree3 -> street2 -> region1;
}
{% endblockdiag %}

{% blockdiag %}
blockdiag {
    tree -> street [label = "m:1"];
    street -> region [label = "m:1"];
}
{% endblockdiag %}

#### Habitatges de Barcelona
{% blockdiag %}
blockdiag {
   apartment1 -> address1 -> street1;
   apartment2 -> address1 -> street1;
   apartment3 -> address2 -> street1;
}
{% endblockdiag %}

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

> ## Challenge
> Programs may have a "full-time" and a "part-time" stream. Courses and students belong to streams. Create a third normal form from the data with these assumptions.
{: .challenge}

FIXME: add notation. UML? primary keys? foreign keys?

> ## Challenge
> Draw the Entity-Relation diagram of the student database.
{: .challenge}

{% blockdiag %}
blockdiag {
    roster;
    course -> roster [label = "m:1"];
    student -> roster [label = "registers"];
    student -> program [label = "belongs to"];
    program -> department [label = "offered by"];
}
{% endblockdiag %}

FIXME: draw proper UML diagrams

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
{: .challenge}

FIXME: This is physical layer.

FIXME: stick to student data instead?

FIXME: do joins

FIXME: why denormalize?
