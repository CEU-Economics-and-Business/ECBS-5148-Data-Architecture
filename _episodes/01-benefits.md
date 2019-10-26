---
title: "Data Architecture for Reprodicuble Analysis"
teaching: 130
exercises: 70
questions:
- Who needs the data and why? What legal and technological constraints do we face?
- What is the best shape of the data for answering the analysis questions?
- How do we model how different parts of the data hang together?
- In what format do we store and ship the data? What are the requirements on physical infrastructure?
objectives:
- Reason about things that do not exist (yet).
- Separate important from unimportant features.
- Represent a mental model visually.
- Create concept map of data products.
- Identify key trade-offs in data architecture.
keypoints:
- Architecture spans the gap between "data as it is" and "data most suitable for analysis questions."
- Make plans that are fit for purpose and execute them.
- Architecture for analysis differs from architecture in production.
- Optimize for human rather than machine performance.
- As analysts you care about real-world entities, not records in your data.
---

{% include mermaid.html %}

## Reading
1. Chapter 1 of Brown (2018), [Software Architecture for Developers, Volume 1](https://leanpub.com/software-architecture-for-developers/).
2. Chapter 1 of Kleppmann (2016), [Designing Data-Intensive Applications](https://dataintensive.net/buy.html).
3. [Concept maps](http://rodallrich.com/advphysiology/ausubel.pdf)
4. [Data lakes](https://martinfowler.com/bliki/DataLake.html)

## Concept maps

A lot of information can be communicated effectively with "lines and boxes." Boxes typically correspond to concepts or things ("nouns"), lines represent the relation between these concepts or actions between them ("verbs").

### Visualizing frontal instruction
<div class="mermaid">
graph TD
    t(Teacher) --> m(Material)
    m --> s1(Student)
    m --> s2(Student)
    m --> s3(Student)
</div>

### Visualizing live coding
<div class="mermaid">
graph TD
    subgraph S
        s1(Student) --> m1(Code)
        s2(Student) --> m2(Code)
        s3(Student) --> m3(Code)
    end
    subgraph T
        t(Teacher) --> m(Code)
    end
    m -.- m1 -.- m2 -.- m3
</div>

> ## Exercise
> Label each line with a verb.
{: .challenge}

> ## Challenge
> Take a one-paragraph description of a Grimm story. Working in groups, represent the players and events on a concept map.  
{: .challenge}

> ## Challenge (optional)
> You are the chief engineer of SpaceX, responsible for creating the first human base on Mars. You are putting in a budget request to Elon Musk. Create a one-page conceptual diagram explaining what you will need and why.
{: .challenge}

## The life cycle of data
We will be thinking about how data moves through your organization.

> ## Data Architecture
> Data architecture spans the gap between “data as it is” and “data most suitable for analysis.”
{: .callout}

<div class="mermaid">
graph LR
    a(Data as it is) --> b(Data suitable for analysis)
</div>

1. Collect and discover
2. Profile and clean
3. Normalize and integrate
4. Share and reuse
5. Explore and visualize
6. Structure and analyze
7. Deploy and scale

> ## Hadley Wickham's version
> Hadley Wickham splits data analysis into the following stages. This is helpful, but we want to dig deeper in steps 1-3.
> 1. Import
> 2. Tidy
> 3. Transform
> 4. Visualize
> 5. Model
> 6. Communicate
> 
> (source: [R for Data Science](https://r4ds.had.co.nz/introduction.html))
{: .callout}

<div class="mermaid">
graph LR
	Raw --> Tidy
    subgraph Architecture
        Tidy --> Normalized
        Normalized --> Structured
    end
	Structured --> Analysis
</div>

### Not covered
- Data security
- Analysis
- Production
- Visualization

### Two views of the data life cycle

![Fowler (2015)](https://martinfowler.com/bliki/images/dataLake/contrast.png)

> ## Lake vs warehouse
> Who wouldn't want a "lake" instead of a "warehouse"? But this is, for our purposes, an irrelevant debate. In an overly structured organization with huge IT department and tons of internally generated data, moving towards a "data lake" might be productive. For small analytics teams, gathering data from wherever they can find them, moving towards a "data warehouse" might be more productive. 
> 
> In this course, we strive for a middle ground ("data reservoirs", huh). Learning to plan and structure (like in a DW), and to use tools to work directly on raw data (like in a DL).
{: .discussion}

## Visualizing data architecture

### Stages of planning
1. **Context**: Who needs the data and why? What legal and technological constraints do we face?
2. **Conceptual**: What is the best shape of the data for answering the analysis questions?
3. **Logical**: How do we model how different parts of the data hang together?
4. **Physical**: In what format do we store and ship the data? What are the requirements on physical infrastructure?

### Levels of detail
1. **Context**: Who needs the data and why? What legal and technological constraints do we face?
2. **Data Product**: A usable collection of data and software fit for the analysis question.
3. **Data Set**: A reusable set of data covering one topic (typically) from one source. 
4. **Data Object**: A flat file, database, API storing or giving access to the data.


<div class="mermaid">
graph TD
	Context --> c2["Data Product"]

	subgraph "Data Product"
        c2 --> c3a["Data Set"]
        c2 --> c3b["Data Set"]
        c2 --> c3c["Data Set"]
	end

	subgraph "Data Set"
        c3b --> c4a["Data Object"]
        c3b --> c4b["Data Object"]
        c3b --> c4c["Data Object"]
	end
</div>

> ## Example: Analyze German procurement tenders
> 1. **Context**: As a small business owner, I want to identify firms similar to mine that are winning procurement contracts in Germany so that I can better compete with them. I need to find similar and high-performing firms and don't have a lot of money for proprietary tools and products. My data handling should be GDPR-compliant.
> 2. **Data Product** ("system"): A searchable and queriable database to identify best-performing bidding firms and display their key information. Consists of data collected from Tenders Electronic Daily and OpenCorporates. Includes a standard, open-source search tool.
> 3. **Data Set** ("component"): 
>    * _Contract Award Notices_ from Tenders Electronic Daily. Gather from EU website in CSV format and tidy up so that firms can be matched to OpenCorporates. Key variables to pay attention to: buying agency, selling firm, amount of contract award, data awarded, product (CPV) codes. Can be limited to German buying agencies to save space and effort. Open data with a CC license.
>    * _Offene Register_. A dataset derived from OpenCorporates data, listing German firms with their names, addresses, their officers. Key variables: company name, address, unique identifier. Firms moving across German states change their identifiers so these records should be linked. Open data with a CC license.
>   * _Search tool_. A software tool (Elastic Search? Lucene?) to faciliate searching among firm names and headquarter cities.
> 4. **Data Object** ("code"):
>   * _Contract Award Notices_ in CSV format, one per year. About 500MB per year. Cells contain multivalued fields (multiple winners in one cell, for example) that have to be cleaned up. Firms are not identified with numerical identifier.
>   * _OffeneRegister_ dump in JSON-lines format. Several GB in size.
{: .callout}

> ## Exercise
> What would change if our analysis goal was to estimate the home bias in procurement? We want to study whether German agencies favor German firms and if so, by how much.
>> ## Solution
>> This is statistical analysis of aggregates, not a search of particularly successful firms. We do not need a search component, but we will need more data from other countries on procurement (say, France). We also need to identify different cities in the procurement data and measure the distance between them. There may be more procurement between Stuttgart and Strasbourg because they are close by. 
> {: .solution}
{: .challenge}

> ## Exercise
> Draw a concept map at the context level (enumerating users, constraints and data products) for our analysis of home bias in German procurement.
{: .challenge}

## Key tradeoffs in data architecture
1. Reliable
2. Scalable
3. Maintainable

> ## Exercise
> You have a data processing script written in Python that can process 1 million recods in one hour. Rewriting it in C++ would make it 30 times faster, but would take 100 hours of developer time. If you minimize the sum of developer plus machine time, when would you choose to rewrite the code in C++?
{: .challenge}

> ## Challenge
> Draw a conceptual model for a data product suitable for analyzing which firms win the most EU contracts. 
{: .challenge}

