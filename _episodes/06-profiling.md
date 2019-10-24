---
title: "Data Profiling for Better Quality"
teaching: 130
exercises: 70
questions:
- How do I explore my data for common data entry errors?
- How do I extract numeric information from malformatted data?
- How do I normalize mistyped text data?
objectives:
- Use string functions in OpenRefine to normalize text data.
- Explore data in OpenRefine with facets and filters.
- Use OpenRefine to split and reshape data.
- Use simple regular expressions to exract numerical information from textual data.
- Save, edit and replay changes in OpenRefine on different datasets.
- Reshape data long from variable name patterns.
keypoints:
- OpenRefine gives an interactive overview of your data as it is.
- Experiment with various text replace functions, clustering and regular expressions.
- Save all your steps in a .json file.
- If your data is large, conduct cleaning on a random sample.
---

## Reading
1. [Falsehoods about names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)
2. [Beider-Morse phonetic matching](https://stevemorse.org/phonetics/bmpm2.htm)
3. [Dates are difficult](https://www.i-programmer.info/babbages-bag/391-dates-are-difficult.html)

## Review and transform textual data (25 mins)

1. [Thomas Padilla's tutorial](http://thomaspadilla.org/dataprep/). 
2. Create project.
3. Text faceting, multiple editing.
4. Go through various clustering options, including Levenshtein distance. These still don't match "Titan" to "Titan Books". 
5. Illustrate GREL transformations `value.replace('.', '')` and `value.replaceChars('.', '')`. 
6. Also, simple regex to extract publication dates, `value.find(/\d{4}/)[0]`. Convert `toNumber()`. Check with Numeric facet.
7. Export project.
8. Undo/redo. Export .json script.

> ## Exercise (10 mins)
> Cluster and edit the Author-Persons column. How many distinct authors are there?
>> ## Solution
>> Edit cells / Transform / Cluster and edit. Select "fingerprint" first, then "ngram-fingerprint", then "Beider-Morse". Only then switch to Nearest neighbor and select "levenshtein" with radius of 1.0. With these steps, you can reduce the number of distinct authors from about 5,400 to about 4,000.
> {: .solution}
{: .challenge}

[The Carpentries](https://librarycarpentry.org/lc-open-refine/)



## Reshape data

> ## Gotcha
> When using "Edit cells / Fill down" to fill in missing values for multiple columns, always start from the last column and work leftwards. OpenRefine defines records based on the first columns and so these shoul be changed last. 
{: .callout}

> ## Code example
> Read `gdp-wide.csv` and reshape it in long format using OpenRefine. Select rows matching "World" and save as .csv.

> ## Challenge
> Read `WDI-3indicators.csv`. Make it tidy. Save as three separate .csv files.
{: .challenge}

> ## Code example
> Open the TED sample for Germany in OpenRefine. Split the name of winners into separate rows.

> ## Exercise
> Split the address of winners into separate rows. Do it for all address components.
{: .challenge}

> ## Code example
> Select relevant columns: `ID_AWARD`, `WIN_*`. Save as `award-winner.csv`.
> Save steps in JSON file. Illustrate going back in time.
> Replay same JSON on larger file.
>
> Now replace numbers in `WIN_TOWN`.

> ## Exercise 
> Save the JSON file of all the steps we have done, including removing the numbers from `WIN_TOWN`. Replay these changes on the larger sample.
{: .challenge}

> ## Challenge
> Edit the original JSON file to make the same edits for Contracting Agencies (`CAE_*`). Split into rows, fill down, remove numbers from city. Create an `award-cae.csv`.
{: .challenge}



