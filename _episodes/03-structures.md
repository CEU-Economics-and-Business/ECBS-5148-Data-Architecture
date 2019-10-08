---
title: "Data Structures and Speed"
teaching: 135
exercises: 65
questions:
- ""
objectives:
- Classify search algorithms according to algorithmic complexity.
- Explain the basic idea behind hash tables.
- Build a binary tree from simple ordered data.
- Express the algorithmic complexity of simple algorithms written in pseudocode.
- Compare different data structures.
- Describe linked lists, binary trees and hash tables.
- Contrast worst-case and average algorithmic complexities.
- Understand O() notation.
keypoints:
- ""
---

## Algorithmic complexity

```
for item = 1 to 5
    sleep for 1 second
```
{: .source}

```
for item = 1 to 5
    for other_item = 1 to 2
        sleep for 1 second
```
{: .source}

> ## Exercise
> How long does the following pseudo-code take to run?
> ```
> for item in A B C
>     for other_item in D E
>         sleep for 1 second
> ```
> {: .source}
> > ## Solution
> > The outer loop runs 3 times. The inner loop runs 2 times. Each run takes 1 second, so total runtime is 6 seconds.
> {: .solution}
{: .challenge}

```
for item = 1 to 100
    for other_item = 1 to 100
        if item > other_time
            break loop and stop
        else
            sleep for 1 second
```
{: .source}

```
for item in {1..10} 
do
    for other_item in {1..10} 
    do
        if [ $item -gt $other_item ]
        then
            break
        else
            sleep 1
        fi
    done
done
```
{: .language-bash}


> ## Exercise
> How long does the following pseudo-code run?
> ```
> for item = 1 to N
>     for other_item = 1 to item
>         sleep for 1 second
> ```
> {: .source}
> > ## Solution
> > If the outer index has value i, the inner loop runs i times. The outer loop iterates over i, so the sleep command runs 1 + 2 + 3 + ... + N times. We know from Euler that this sum is N (N + 1) / 2, so the program takes as many seconds.
> {: .solution}
{: .challenge}

[Big-O notation](https://en.wikipedia.org/wiki/Big_O_notation)

indexing. tree search, 

## Searching for a value

[Linked list](https://en.wikipedia.org/wiki/Linked_list)

[Binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm)

[Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)

[Tree data structures](https://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/)

[Hash table](https://en.wikipedia.org/wiki/Hash_table)

### Indexing

{% assign table_data = site.data.structure.indexable %}
{% assign table_caption = "A database to be indexed" %}
{% include table.html %}


{% assign table_data = site.data.structure.id-index %}
{% assign table_caption = "A sorted index of `id`" %}
{% include table.html %}

{% assign table_data = site.data.structure.word-index %}
{% assign table_caption = "A sorted index of `word`" %}
{% include table.html %}


### Bisection search

{% assign table_data = site.data.structure.bisection %}
{% assign table_caption = "A sorted list of values" %}
{% include table.html %}

```
function bisection(what, sorted_list)
    if length(sorted_list) == 0
        return 'Not found.'
    middle_index = floor(length(sorted_list) / 2)
    if what == sorted_list[middle_index]
        return middle_index
    if what < sorted_list[middle_index]
        return bisection(what, sorted_list[1:middle_index - 1])
    else
        return bisection(what, sorted_list[middle_index + 1:end])
```
{: .source}

> ## Exercise
> Take the following binary search tree. For each value stored in it, calculate how many comparison steps are needed to find them in the tree. What is the worst-case and the average number of steps?
> ![](https://upload.wikimedia.org/wikipedia/commons/d/da/Binary_search_tree.svg)
> > ## Solution
> >
> > | Value | Steps |
> > |-------|-------|
> > | 1 | 3 |
> > | 3 | 2 |
> > | 4 | 4 |
> > | 6 | 3 |
> > | 7 | 4 |
> > | 8 | 1 |
> > | 10 | 2 |
> > | 13 | 4 |
> > | 14 | 3|
> > 
> > Elements: 9. Worst-case: 4 steps. Average: 2.9 steps.
> {: .solution}
{: .challenge}

> ## Exercise
> There are twice as many elements in this tree as before. Recalculate the worst-case and the average search times. How did they change?
{: .challenge}

> ## Exercise
> Suppose you categorize your files into folders. To maintain a clear folder structure, each folder contains at most seven files or subfolders. Given your logical structure, you always know for sure which subfolder to look into when searching a file.
> 
> You have 15,000 files to organize. What is the worst-case number of steps for finding a file?
{: .challenge}

With bisection search, binary trees or similar data structures, search time is O(log n). This is very good, but we can do even faster!

### Hash tables

What if we had an index where we kne exactly where to look for keys?

{% assign table_data = site.data.structure.positional-index %}
{% assign table_caption = "A positional index of `id`" %}
{% include table.html %}

But how to this with text data? Create a function that returns a numeric value in a bounded range for every piece of text. The output of the function should cover the entire range fairly equally and should be easy to calculate. This is a *hash function*.

See, for example:
```
def hash(word):
    '''
    Return a hash based on the sum of characters in the word. 
    a = 1, b = 2, etc
    The hash is an integer between 1 and 20.
    '''
    sum_of_digits = 0
    for char in word.lower():
        sum_of_digits += ord(char) - ord('a') + 1
    return (sum_of_digits) % 20 + 1
```
{: .language-python}

{% assign table_data = site.data.structure.word-hash-index %}
{% assign table_caption = "A hash index of `word`" %}
{% include table.html %}

#### Finding "love"
```
"love" = 12 + 15 + 22 + 5 = 54
54 mod 20 = 14
14 + 1 = 15
```
{: .source}
We should be looking for "love" in the 15th position.

#### Finding "hate"
```
"hate" = 8 + 1 + 20 + 5 = 34
34 mod 20 = 14
14 + 1 = 15
```
{: .source}
The word "hate" also has the same hash ("hash collision"), so it should be in the 15th position. But it is not.

FIXME: distributed data structures and sharding?

FIXME: use grep to illustrate? check git bash

> ## Exercise
> [Watch this clip](https://www.youtube.com/watch?v=IN4CiToDGNg&t=22m48s) of searching for a lost transit pass in the lost-and-found office. What is the complexity of this search algorithm? (Hint: it is not O(n).)
> ### English transcript
> > You have lost your pass, yes? Sit.  
> > This is not it.  
> > Let's see. This neither.  
> > These are from yesterday.  
> > No. Not here. It wasn't left here.  
> 
> > ## Solution
> > Because each time a pass is checked, it is put back into the pile, this process is slower than O(n). If checked passes went to a new pile, this would be searching in a linked list with O(n) time. However, each pass can be picked up twice or more, slowing down the process. The likelihood of checking again increases with n. In the worst case, the pass is not found in finite time. The average number of check required is O(n log n).
> >
> > This relates to the [coupon collector's problem](https://en.wikipedia.org/wiki/Coupon_collector%27s_problem). 
> {: .solution}
{: .challenge}



## Comparing two values

Suppose we have two lists of student names and want to select all students who appear on both. In SQL, this is called an `INNER JOIN`.

```
for one_student in Anna Cecilia Balazs Daniel
    for other_student in Greg Daniel Anna Pavel
        if one_student == other_student
            yield one_student
```
{: .source}
The complexity of this algorithm is O(nk), with n and k the number of students in each list. (Don't worry, this not how `INNER JOIN` is implemented.)

> ## Exercise
> We have written a naive double-loop join algorithm in Python to match 1,000 firms with those in a list of a 200 politically connected firms. The code takes 10 minutes to run. After a data update, we will have 500 politically connected firms, and 20,000 candidate firms to match with them. How long will this matching run?
> > ## Solution
> > The naive algorithm makes 1000 x 200 = 200,000 comparisons in 10 minutes, so 1,000 comparisons take 3 seconds. On the bigger data, we would do 20000 x 500 = 10 million comparisons. This takes 500 minutes, that is, 8 hours and 20 minutes.
> {: .solution}
{: .challenge}

comparison O(n2)
