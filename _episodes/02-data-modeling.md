---
title: "Data Modeling"
teaching: 200
exercises: 0
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

FIXME: start with excel?

FIXME: add .csv examples

| Course Title | Student ID |
|--------------|------------|
{% for row in site.data.sis.roster %}| {{ row["Course Title"] }} | {{ row["Student ID"] }} | 
{% endfor %}

> ## Exercise
> (FIXME: find a messy data). Identify the xx problems with this dataset.
{: .challenge}

> ## Exercise
> For each of the data sources listed on the course homepage, answer the following questions.
> 1. What entities are represented in the data?
> 2. What is a record in the dataset?
> 3. If multiple entities are represented, how are they related? Explain in words.
{: .challenge}

FIXME: select 3-4 datasets for this exercise. Do 2 as example, and leave 1-2 for challenge.

## The Entity--Relationship model

Relying on Silberschatz, Korth and Sudarshan, 2011. "Database System Concepts" 6th Edition, McGraw Hill, Chapters 7--8.

FIXME: decide on language: records, rows, observations, features, columns, attributes, variables.

FIXME: work out a simple student information system example?

Entities: student, faculty, course, stream?

### Primary key

### First normal form
All attributes are atomic. = All cells are single valued.

Strictly speaking, `ECBS5148` is not atomic.

### Second normal form
None of the columns depend functionally on a proper subset of primary keys.

### Third normal form
No column depends on anything but the keys.

"[every] non-key [attribute] must provide a fact about the key, the whole key, and nothing but the key".

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
