---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

{% include gh_variables.html %}

## Instructor
Miklós Koren is professor of economics at CEU. He is the founder of the Business Analytics MSc program
and the CEU MicroData research group. His research focuses on international trade and economic
development. He publishes regularly in leading international academic journals, and he has participated in
numerous international research projects, including a large-scale Starting Grant of the European Research
Council. He is a recipient of the Peter Kenen Fellowhsip and the Nicholas Káldor Prize. Professor Koren
received his Ph.D. from Harvard University in 2005. He also holds an M.A. from Central European
University (2000) and a B.A. from Corvinus University Budapest (1999). Before coming to CEU, he worked
at the Federal Reserve Bank of New York and at Princeton University.

> ## Prerequisites
> Students should have taken Data Engineering 1: Different Shapes of Data.
> 
{: .prereq}

## Outline

> Data architecture spans the gap between “data as it is” and “data most suitable for analysis.”

FIXME: is this helpful distinction?

1. Context: Understand user requirements and technical constraints.
2. Conceptual model: Map out the ideal shape of data for matching requirements.
3. Logical model: 
4. Physical model: select technologies and tools.

## The data science workflow
1. Collect and discover
2. Profile and clean
3. Normalize and integrate
4. Share and reuse
5. Explore and visualize
6. Denormalize and analyze
7. Deploy and scale

We will discuss 2-4. But that's where most of the work is done anyway. In the [CRISP-DM model](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining), all of this is covered in Steps 2 and 3.

## Hadley Wickham's version
1. Import
2. Tidy
3. Transform
4. Visualize
5. Model
6. Communicate

(source: [R for Data Science](https://r4ds.had.co.nz/introduction.html))

Design choice: Start from the end with plenty of scaffolding. Move backwards and take away the scaffolding. This is like running 5k, 10k, 20k before attempting a marathon. (Sorry for the mix of metaphors.) Otherwise we would be like a coaching plan for a marathon: "Run the first km. Then run the other 41."

1. Tidy data ready for analysis
2. Denormalize from well designed and maintained relational data
3. Choose data structures for performance
4. Prepare data for sharing and reuse
    - serialization
    - packaging
5. Find and fix common data quality errors
6. Design choices

## Not covered
- Data security
- Analysis
- Production
- Visualization

{% include links.md %}
