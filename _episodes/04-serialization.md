---
title: "Data Serialization for Ease of Sharing"
teaching: 130
exercises: 70
questions:
- ""
objectives:
- Understand character encodings and Unicode points.
- Load and save text file with different character encodings.
- Compare popular serialization formats fixed width, CSV, JSON, XML, YAML, JSONlines, Parquet.
- Explain the tradeoffs in data serialization.
- Use hexadecimal transformation of numbers.
- Understand base64 encoding.
- Download data in various formats using wget.
- Explore .csv files with csvkit.
- Explore JSON files with jq.
- Write shell scripts to automate data manipulation.
keypoints:
- Always check your character encoding of your "plain text" data. Then immediately convert it in UTF-8.
- Save data in human readable format to facilitate sharing and maintenance.
---

## Reading
1. [Unicode](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)

> ## Exercise
> Open `bash`. Find the folder with the _Verbbanten Bücher_ dataset and create a short documentation file `README.md`.
{: .challenge}

> ## Exercise
> Open the _Verbannten Bücher_ dataset in Excel. What is wrong? Display the contents in the shell using `head`.
{: .challenge}

FIXME: code examples for shell script

> ## Exercise
> Write a shell script to download all years of wage the for the [California Superior Court](https://publicpay.ca.gov/Reports/RawExport.aspx) and unzip them in a separate folder.
{: .challenge}

> ## Challenge
> Download the Comercio Exterior data for 2018 using bash and wget. 
{: .challenge}

> ## Exercise
> Represent the entites in the following nursery rhyme in JSON, XML and YAML. Give them a unique numeric identifier starting from 1. (You don't have to type out all entitis, but do think about the numbering.)
> > As I was going to St Ives,  
> > Upon the road I met seven wives;  
> > Every wife had seven sacks,  
> > Every sack had seven cats,  
> > Every cat had seven kits:  
> > Kits, cats, sacks, and wives,  
> > How many were going to St Ives?  
> 
{: .challenge}

> ## Exercise 
> Convert the JSON lines file in _Offene Register_ to proper JSON using `jq`.
> > ## Solution
> > ```
> > cat offeneregister-1000.jsonl | jq -cs '.' > offeneregister-1000.json
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

```
~/D/t/c/2/d/d/okf ❯ head -n5 offeneregister-1000.jsonl | jq ".officers[] | {name: .name, city: .other_attributes.city, position: .position}"
{
  "name": "Oliver Keunecke",
  "city": "Hamburg",
  "position": "Geschäftsführer"
}
...
{
  "name": "Wolfgang Adamiok",
  "city": "Mommenheim",
  "position": "Geschäftsführer"
}
```
{: .language-bash}

> ## Exercise
> Using `jq`, create a list of JSON objects ("JSON lines"), with each record corresponding to a person-entry in the _Offene Register_, with the following fields: name, city, firm identifier.
> > ## Solution
> > ```
> > jq ". as $parent | .officers[] | {name: .name, firm: $parent.company_number, city: .other_attributes.city, position: .position}"
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

```
jq -r '(map(keys) | add | unique) as $cols | map(. as $row | $cols | map($row[.])) as $rows | $cols, $rows[] | @csv' officer.json > officer.csv
```
{: .language-bash}

FIXME: JSON and jq seem enough for this class

> ## Exercise
> Use csvkit to select all managers living in Hamburg from `geschaftsfuhrer.csv`. 
{: .challenge}

> ## Challenge
> Use csvkit to create a JSON file of all unique first and last names by city in `geschaftsfuhrer.csv`. Adding an extra column `number_occur`, capturing the number of times an identical row occurs in the .csv file. 
{: .challenge}
