---
title: "Data Integration and Profiling"
teaching: 200
exercises: 0
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
- Create hiearchical groups.
keypoints:
- ""
---

FIXME: teach regex?

> ## Challenge
> Load the Verboten Bücher dataset in Open Refine. Split books with multiple authors into a separate author and relationship table. 
{: .challenge}


FIXME: is dplyr too much?

FIXME: create offeneregister dump of people only

> ## Challenge
> Load the `geschaftsfuhrer.csv` dataset in Open Refine. Merge people with similar names in the same city and given them a unique identifier. 
{: .challenge}
