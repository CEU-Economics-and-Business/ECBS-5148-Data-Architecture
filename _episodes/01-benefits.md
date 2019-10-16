---
title: "The Benefits of Good Architecture"
teaching: 130
exercises: 70
questions:
- ""
objectives:
- Reason about things that do not exist (yet).
- Separate important from unimportant features.
- Represent a mental model visually.
- Recognize and discard mental models when no longer useful.
- Identify key trade-offs in data architecture.
- "Recall existing data models: table, relational data, hierarchical, networked."
- Capture requirements in user stories.
- Create conceptual model of data product.
keypoints:
- Architecture spans the gap between "data as it is" and "data most suitable for analysis questions."
- Make plans that are fit for purpose and execute them.
- Architecture for analysis differs from architecture in production.
- Optimize for human rather than machine performance.
- As analysts you care about real-world entities, not records in your data.
---

FIXME: teach something immediately useful

FIXME: hands-on exercise for planning (mapping?)

FIXME: introduce and walk through one model in detail. tree. layers.

## Reading
[Concept maps](http://rodallrich.com/advphysiology/ausubel.pdf)

> ## Challenge
> Take a one-paragraph description of a Grimm story (favorite TV show?). Working in groups, represent the players and events on either (i) pyramid, (ii) tree, (iii) partition. Present your choice to the rest of the class. 
{: .challenge}

> ## Exercise
> Change your representation to one of the other two mental models. 
{: .challenge}

> ## Challenge
> You are the chief engineer of SpaceX, responsible for creating the first human base on Mars. You are putting in a budget request to Elon Musk. Create a one-page conceptual diagram explaining what you will need and why.
{: .challenge}

## Key tradeoffs
1. Reliable
2. Scalable
3. Maintainable

## Layers
1. Context
2. Conceptual
3. Logical
4. Physical

> ## Exercise
> You have a data processing script written in Python that can process 1 million recods in one hour. Rewriting it in C++ would make it 30 times faster, but would take 100 hours of developer time. If you minimize the sum of developer plus machine time, when would you choose to rewrite the code in C++?
{: .challenge}

FIXME: define and work with user stories here? or in last episode?

> ## Challenge
> Draw a conceptual model for a data product suitable for analyzing which firms win the most EU contracts. 
{: .challenge}

