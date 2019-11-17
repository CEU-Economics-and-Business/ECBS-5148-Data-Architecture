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
- Use head, less and grep to explore data stored in text files.
- Explore .csv files with csvkit.
- Explore JSON files with jq.
keypoints:
- Always check your character encoding of your "plain text" data. Then immediately convert it in UTF-8.
- Save data in human readable format to facilitate sharing and maintenance.
---

# Reading
1. [Chapter 4 (pages 107-112)](https://ceulearning.ceu.edu/mod/resource/view.php?id=288212) of Kleppmann 2016. 
2. Joel Spolsky's [essay on Unicode](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)

# Setup
1. If you use Windows, install [Git Bash](https://git-scm.com/download/win) and the [Software Carpentry Installer](https://github.com/swcarpentry/windows-installer/releases/download/v0.3/SWCarpentryInstaller.exe), following [these instructions](https://www.youtube.com/watch?v=339AEqk9c-8). If you use Mac or Linux, you don't need to download these.
2. [Download jq](https://stedolan.github.io/jq/download/), a command-line JSON processor.
3. Make sure you have a [text editor](https://carpentries.github.io/workshop-template/#editor) you are comfortable working with.

# Exploring files
> ## Exercise
> Open `DE.sqlite` in the DB Browser for SQLite. Export the following three tables to .csv: `seller`, `buyer`, `lot` using "File", "Export", "Tables to CSV".
{: .challenge}

```
bash-5.0$ pwd
/Users/koren/Downloads/data-architecture
bash-5.0$ ls -l
total 22392
-rw-r--r--@ 1 koren  staff  4038665 Nov 17 17:33 buyer.csv
-rw-r--r--@ 1 koren  staff  4738428 Nov 17 17:33 lot.csv
-rw-r--r--@ 1 koren  staff  2680572 Nov 17 17:33 seller.csv
```
{: .language-bash}

If you are not in the folder where you exported the tables, navigate there using `cd`.

The command `ls` not only shows us the files in the folder, but also their size. We can display file sizes in human readable form by turning on the `h` option of `ls`,

```
bash-5.0$ ls -lh
total 22392
-rw-r--r--@ 1 koren  staff   3.9M Nov 17 17:33 buyer.csv
-rw-r--r--@ 1 koren  staff   4.5M Nov 17 17:33 lot.csv
-rw-r--r--@ 1 koren  staff   2.6M Nov 17 17:33 seller.csv
```
{: .language-bash}

It is good to review the size of a file before attempting to open it.

# Fixed width
FIXME: add fixed width example (Comercio Exterior?)

## Columnar 


# Comma Separated Values
The [CSV format](https://en.wikipedia.org/wiki/Comma-separated_values#Standardization) is a standard plain text format for tabular data.

```
ID_AWARD,ID_LOT_AWARDED,CAE_NAME,ISO_COUNTRY_CODE
8447168,,European Commission-Directorate General for Energy,LU
8447171,,"European Commission, Directorate-General for Environment",BE
8447173,,Gemeinde Unterföhring,DE
8448129,,Verwaltungsgemeinschaft Altfraunhofen,DE
8448343,,Verbandsgemeindewerke St. Goar-Oberwesel,DE
8448585,,Barmherzige Brüder gemeinnützige Krankenhaus GmbH,DE
8448586,,Barmherzige Brüder gemeinnützige Krankenhaus GmbH,DE
8448609,1,IT.NRW,DE
8448610,2,IT.NRW,DE
```

The first row ("header"), contains the column names. There are not other metadata, notably we do not know the data types in each column.

Subsequent rows are a comma-separated list of cells.

Note that the third row, because the column `CAE_NAME` contains a comma, is escaped with double quotes. Otherwise, quotes are unnecessary for strings.

> ## Exercise
> Open `bash`. Find the folder with the _Verbbanten Bücher_ dataset and create a short documentation file `README.md`.
{: .challenge}

Because one observation is one line, it is easy to count the number of observations in a .csv file. (An exception is if a long text field includes line breaks.)

```
bash-5.0$ wc -l buyer.csv 
   58003 buyer.csv
```
{: .language-bash}

The file `buyer.csv` includes 58003 lines, one of which is the header. It hence has 58002 observations.

> ## Exercise
> What is the number of observations in the `seller` table?
>> ## Solution
>> ```
>> bash-5.0$ wc -l seller.csv 
>>    61702 seller.csv
>> ```
>> {: .language-bash}
>> There are 61,701 observations in `seller.csv`.
> {: .solution}
{: .challenge}

We can explore large .csv files by checking their first few lines. The bash command `head` can do exactly this.

```
bash-5.0$ head seller.csv 
ID_AWARD,ID_LOT_AWARDED,WIN_NAME,WIN_COUNTRY_CODE
8447168,,Dialogika GmbH,DE
8447171,,Ecologic Institute gemeinnützige GmbH,DE
8448828,,Josef Meier GmbH &amp; Co. KG Hoch- und Tiefbau,DE
8450085,,Ingenieurbüro Grohmann GmbH,DE
8450155,,Harnisch Creative Planning GmbH,DE
8450240,,Rupprecht Consult Forschung und Beratung GmbH,DE
8452251,1,Architekten Hermann Kaufmann ZT GmbH,AT
8452254,4,Brückner Dietz GmbH,DE
8455072,3,Heraeus Medical GmbH,DE
```
{: .language-bash}

We can set how many lines to show with the `-n` option. This can be used to create a smaller (non-random!) sample to explore with our favorite spreadsheet editor.

```
bash-5.0$ head -n2 seller.csv 
ID_AWARD,ID_LOT_AWARDED,WIN_NAME,WIN_COUNTRY_CODE
8447168,,Dialogika GmbH,DE
```
{: language-bash}

> ## Exercise
> Create a 1000-row sample from `seller.csv`. Recall that you can redirect the output of a bash command to a file with `> output.txt` (or whatever file name you give). Make sure your file name ends in .csv so that you can open it with a spreadsheet editor.
>> ## Solution
>> ```
>> bash-5.0$ head -n1000 seller.csv > seller-1000.csv
>> bash-5.0$ ls -lh
>> total 22496
>> -rw-r--r--@ 1 koren  staff   3.9M Nov 17 17:33 buyer.csv
>> -rw-r--r--@ 1 koren  staff   4.5M Nov 17 17:33 lot.csv
>> -rw-r--r--  1 koren  staff    50K Nov 17 18:00 seller-1000.csv
>> -rw-r--r--@ 1 koren  staff   2.6M Nov 17 17:33 seller.csv
>> ```
>> {: .language-bash}
> {: .solution}
{: .challenge}

# Character encoding

### Libre Office gives you complete control over your .csv files
![Libre Office gives you complete control over your .csv files]({{ "/fig/libre-office-open-csv.png" | relative_url }})

### Make sure you select the corrent encoding
![Make sure you select the corrent encoding]({{ "/fig/libre-office-wrong-encoding.png" | relative_url }})

### You can also open a .csv file in a text editor
![You can also open a .csv file in a text editor]({{ "/fig/sublime-text-csv.png" | relative_url }})

> ## Exercise
> Download the [_Verbannten Bücher_ dataset](https://www.berlin.de/berlin-im-ueberblick/geschichte/berlin-im-nationalsozialismus/verbannte-buecher/suche/index.php/index/all.csv?q=) in .csv. Open it in Excel. What is wrong? Display the contents in the shell using `head`.
>> ## Solution
>> ```
>> bash-5.0$ head all.csv 
>> "id";"ssflag";"pagenumberinocrdocument";"authorfirstname";"authorlastname";"title";"firsteditionpublisher";"firsteditionpublicationplace";"firsteditionpublicationyear";"secondeditionpublisher";"secondeditionpublicationplace";"secondeditionpublicationyear";"additionalinfos";"ocrresult"
>> "70526";"2";"0";"Adler";"Alfred";"Praxis und Theorie der Individualpsychologie";"Bergmann";"München";"1930";"";"";"";"";"Adler, Alfred: Praxis und Theorie der Individualpsychologie. München: Bergmann 1930. "
>> "70527";"2";"2";"Bruno";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Bruno: Sämtliche Schriften."
>> "70528";"2";"2";"Felix";"Adler";"Der Moralunterricht der Kinder";"Dümmler";"Berlin";"1894";"";"";"";"";"Adler, Felix: Der Moralunterricht der Kinder. Berlin: Dümmler 1894."
>> "70529";"2";"2";"Friedrich (Wolfgang)";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Friedrich (Wolfgang): Sämtliche Schriften."
>> "70530";"2";"2";"Georg";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Georg: Sämtliche Schriften."
>> "70531";"2";"2";"Max";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Max: Sämtliche Schriften."
>> "70532";"2";"2";"Otto";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Otto: Sämtliche Schriften."
>> "70533";"2";"2";"Viktor";"Adler";"Sämtliche Schriften";"";"";"";"";"";"";"";"Adler, Viktor: Sämtliche Schriften."
>> "70534";"2";"2";"";"Adolf, Gustav (Pseud.)";"Syphilis. Einstundenspiel aus der Nachkriegszeit";"Jensen";"Swinemünde";"1921";"";"";"";"";"Adolf, Gustav (Pseud.): Syphilis. Einstundenspiel aus der Nachkriegszeit. Swinemünde: Jensen 1921."
>> ```
>> This is actually semicolon-separated, not comma separated.
> {: .solution}
{: .challenge}

# JSON (Javascript Object Notation) 
Export the seller table from `DE.sqlite` in JSON using "File", "Export", "Table to JSON". 

```
bash-5.0$ ls -lh
total 64440
-rw-r--r--@ 1 koren  staff   879K Nov 17 19:13 all.csv
-rw-r--r--@ 1 koren  staff   3.9M Nov 17 17:33 buyer.csv
-rw-r--r--  1 koren  staff    10M Nov 17 19:21 buyer.json
-rw-r--r--@ 1 koren  staff   4.5M Nov 17 17:33 lot.csv
-rw-r--r--  1 koren  staff    50K Nov 17 18:00 seller-1000.csv
-rw-r--r--@ 1 koren  staff   2.6M Nov 17 17:33 seller.csv
-rw-r--r--  1 koren  staff   9.4M Nov 17 19:21 seller.json
```
{: .language-bash}

Note that `seller.json` is much larger than `seller.csv`, because of the special boilerplate characters `[], {}` and extra indentation for better viewing.

```
bash-5.0$ head seller.json 
[
    {
        "ID_AWARD": 8447168,
        "ID_LOT_AWARDED": "",
        "WIN_COUNTRY_CODE": "DE",
        "WIN_NAME": "Dialogika GmbH"
    },
    {
        "ID_AWARD": 8447171,
        "ID_LOT_AWARDED": "",
```
{: .language-bash}

In JSON, you can have string and numeric _literals_, _lists_ (arrays) and _dictionaries_ (objects). (See [full definition](http://www.json.org/).)

```
["apple", "banana", "plum"]
```

You have to use double quotes to write strings, single quotes are not acceptable.

```
{"name": "apple", "type": "fruit", "quantity": 2, "unit": "kg"}
```

```
[
  { "name": "apple", 
    "type": "fruit", 
    "quantity": 2, 
    "unit": "kg"},
  { "name": "carrot", 
    "type": "vegetable", 
    "quantity": 1, 
    "unit": "kg"}
]
```
You can use whitespace to make JSON more readable, but pay attention to limiting commas. 

> ## Exercise
> What is wrong with the following JSON string? You can put it in a [JSON validator](https://codebeautify.org/jsonvalidator) to check.
> ```
> [
>  "fruits": [
>   {name: "apple", color: 'red'},
>   {name: "banana", color: 'yellow'},
>   ]
> ```
>> ## Solution
>> 1. Outside parantheses should be curly to create an _object_, not an _array_.
>> 2. Dictionary keys (`name`, `color`) are not quoted.
>> 3. Do not use single quotes.
>> 4. Last element in an array cannot be followed by comma.
>> 5. No closing bracket for object.
> {: .solution}
{: .challenge}

> ## JSON on the web
> JSON has become the de facto standard for data sharing between web applications, maybe because of its link to JavaScript, which is the standard frontend language. It is easy to parse and process.
{: .discussion}

## A JSON document
## A binary tree
## JSON lines

# YAML
[YAML](https://yaml.org/) is essentially JSON for humans. It replaces brackets and other boilerplate with human-readable whitespace.

```
- apple
- banana
- plum
```

There is no need to quote strings.

```
name: apple
type: fruit
quantity: 2
unit: kg
```

You can validate [YAML online](https://codebeautify.org/yaml-validator).

```
- name: apple 
  type: fruit 
  quantity: 2 
  unit: kg
- name: carrot 
  type: vegetable 
  quantity: 1
  unit: kg
```

> ## Benefits of YAML
> 1. Human readable and editable
> 2. Can be processed sequentially
{: .discussion}

Although is much more human readable than JSON, you do not see much data being exchanged in this format. One exception is configuration settings which are often directly edited by humans. For example, if you are writing a document in [(R)Markdown](https://bookdown.org/yihui/rmarkdown/html-document.html), you may have a header like
```
---
title: Habits
author: John Doe
date: March 22, 2005
output: html_document
---
```
This is a YAML-formatted configuration header.

## XML

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

## Query JSON and XML documents

XPath and jq.

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
