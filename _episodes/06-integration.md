---
title: "Data Integration for Broader Information"
teaching: 0
exercises: 0
questions:
- ""
objectives:
- Apply the ISO-8601 date-time format.
- Apply interval algebra on temporal data.
- Apply spatial relations to spatial data.
- Use wget to download data from the web.
- Use for loops in bach to automate data transformations.
- Compare different methods of Entity Resolution.
- Normalize strings referring to people and corporations using regular expressions.
- Apply Levenshtein and other fuzzy distance metrics.
keypoints:
- There is always a tradeoff between false positives and false negatives in entity resolution.
- Normalize records before comparing them to minimize the necessary number of comparisons.
---

## Data warehouses vs data lakes

### Extract, Transform, Load

> ## Exercise
> Write a shell script to download all years of wage the for the [California Superior Court](https://publicpay.ca.gov/Reports/RawExport.aspx) and unzip them in a separate folder.
{: .challenge}

> ## Challenge
> Download the Comercio Exterior data for 2018 using bash and wget. 
{: .challenge}


### Views
We can create a view of our data warehouse
```
CREATE VIEW denormalized_roster AS
SELECT * FROM roster 
    LEFT JOIN course ON roster.course_code = course.course_code 
    LEFT JOIN student ON student.student_id = roster.student_id;
```
{: .language-sql}

We create a network of firms selling the same CPV product in EU tenders.
```
CREATE VIEW main_product AS
SELECT "ID_AWARD", "ID_LOT_AWARDED", cpv FROM product
    WHERE main_cpv = 1;
```
{: .language-sql}

```
CREATE VIEW firm_product AS
SELECT DISTINCT "WIN_NAME", cpv FROM seller 
    INNER JOIN main_product ON main_product."ID_LOT_AWARDED" = seller."ID_LOT_AWARDED" AND main_product."ID_AWARD" = seller."ID_AWARD";
```
{: .language-sql}


```
CREATE VIEW competing_firm AS
SELECT left.WIN_NAME as left_firm, right.WIN_NAME as right_firm FROM firm_product AS left 
    INNER JOIN firm_product AS right ON left.cpv = right.cpv;
```
{: .language-sql}




> ## Exercise
> Suppose you have a dataset of `n` records that you want to disambiguate. It takes 1ms to compare any pair of records and decide whether they belong to the same entity. You can also classify each record into one of 10 equal-sized groups, but this is slow: it takes 1s per record. There are no matches outside groups, so if you create groups, you only need to compare records within groups.
> 
> Draw a diagram of the time needed for disambiguation as a function of `n`, with and without grouping. For what values of `n` do you want to create groups? Explain. 
{: .challenge}


FIXME: add data integration. correspondance between different classifications. spatial and temporal merge.
potential integration exercises:
    - cars hitting trees in NYC
    - taxi accidents in NYC
    - merge firms in TED with Offene Register
        - merge iso countries?
    - merge AEAT with WDI country data (https://github.com/korenmiklos/per-shipment-costs-replication/blob/master/code/03_create_country_variables.do)

FIXME: changing schema?

FIXME: can practice JOIN on iso country code, hierarchical keys with CPV codes

> ## Challenge
> Load the `officer.csv` dataset in Open Refine. Merge people with similar names in the same city and given them a unique identifier. 
{: .challenge}
