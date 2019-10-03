---
title: "Data Integration and Profiling"
teaching: 115
exercises: 85
questions:
- ""
objectives:
- Understand the extract-transform-load workflow.
- Use dplyr for basic data transformations.
- Split multi-valued columns into rows.
- Reshape data long from variable name patterns.
- Compare different methods of Entity Resolution.
- Normalize strings referring to people and corporations using regular expressions.
- Apply Levenshtein and other fuzzy distance metrics.
- Use OpenRefine for Entity Resolution.
- Use string functions in OpenRefine to normalize text data.
- Create hiearchical groups.
keypoints:
- There is always a tradeoff between false positives and false negatives in entity resolution.
- Normalize records before comparing them to minimize the necessary number of comparisons.
---

FIXME: teach regex?

## Make data tidy

We will use [`dplyr`](https://r4ds.had.co.nz/tidy-data.html) for this purpose.

> ## Exercise
> Read `gdp-wide.csv` and reshape it in long format using `dplyr`. Save as .csv.
{: .challenge}

> ## Challenge
> Read `WDI.csv`. Make it tidy. Save as .csv.
{: .challenge}

> ## Challenge
> Load the Verboten Bücher dataset in Open Refine. Split books with multiple authors into a separate author and relationship table. 
{: .challenge}


FIXME: is dplyr too much?

FIXME: create offeneregister dump of people only

> ## Exercise
> Suppose you have a dataset of `n` records that you want to disambiguate. It takes 1ms to compare any pair of records and decide whether they belong to the same entity. You can also classify each record into one of 10 equal-sized groups, but this is slow: it takes 1s per record. There are no matches outside groups, so if you create groups, you only need to compare records within groups.
> 
> Draw a diagram of the time needed for disambiguation as a function of `n`, with and without grouping. For what values of `n` do you want to create groups? Explain. 
{: .challenge}

> ## Challenge
> Load the `geschaftsfuhrer.csv` dataset in Open Refine. Merge people with similar names in the same city and given them a unique identifier. 
{: .challenge}
