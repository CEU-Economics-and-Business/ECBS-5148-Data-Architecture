---
title: "Data Structures for Scalability"
teaching: 170
exercises: 85
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
- If your algorithm is worse than O(n), think harder.
- Indexing takes unordered lists of data and converts them into data structures suitable for efficient lookup.
- "Hash tables > search trees > unordered lists"
---

# Reading
1. [Algorithmic complexity and big-O notation](https://skerritt.blog/big-o/)
2. [Tree data structures](https://www.freecodecamp.org/news/all-you-need-to-know-about-tree-data-structures-bceacb85490c/)
3. [Binary search](https://medium.com/karuna-sehgal/a-simplified-interpretation-of-binary-search-246433693e0b) (Ignore the Java and JavaScript code examples.)
4. [Indexing in SQLite](https://medium.com/@JasonWyatt/squeezing-performance-from-sqlite-indexes-indexes-c4e175f3c346)
5. [Review logarithmic functions](https://github.com/kiss-oliver/ba-pre-session-2019/blob/master/02_functions_of_one_variable/02_slides.pdf)

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
> > If the outer index has value i, the inner loop runs i times. The outer loop iterates over i, so the sleep command runs 1 + 2 + 3 + ... + N times. We know from Euler that this sum is N (N + 1) / 2, so the program takes as many seconds. In Big-O notation, this is O(n^2).
> {: .solution}
{: .challenge}

We use the `sleep` command to illustrate that what matters is the number of times a loop runs, not the runtime of a single run.

> ## Why Big-O notation
> We care about what happens when our problem becomes "very large." As the size of the problem grows without bound, just throwing more resources at it won't help. For large-enough n, an O(n) problem will take more time on a supercomputer than an O(log n) problem will take on your laptop.
> Big-O notation helps uncover the big-picture challenges that defy easy solution.
{: .callout}

### Less than linear runtime

How many times does the following loop run?
```
# a loop to keep breaking a stick in halves
while length_of_stick > LIMIT
    length_of_stick = length_of_stick / 2
```
{: .source}

> ## Challenge (optional)
> Give a simple algorithm to find all divisors of a positive integer `n`. What is its algorithmic complexity?
>> ## Solution
>> [Solution](https://www.geeksforgeeks.org/find-divisors-natural-number-set-1/)
> {: .solution}
{: .challenge}

## Searching for a value

### List (data structure)

{% assign table_data = site.data.structure.indexable %}
{% assign table_caption = "A data table as a list of rows" %}
{% include table.html %}

> ## Exercise
> What is the algorithmic complexity of finding a word in the data table above?
>> ## Solution
>> We have to loop through all the rows to check them against the search word. In the worst case, this takes `n` steps, so the complexity is `O(n)`.
> {: .solution}
{: .challenge}

If the list is sorted, a bisection (binary) search can drastically improve search time.

### Ordered list (data structure)

{% assign table_data = site.data.structure.bisection %}
{% assign table_caption = "A sorted list of values" %}
{% include table.html %}

#### Bisection search
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
> For each value in the table above, calculate how many comparison steps does a _bisection search_ take? What is the average and the worst number of steps?
{: .challenge}

### Binary tree (data structure)

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


> ## Quadtree (optional)
> Not everything can be sorted on the real line. Take geo-coordinates, for example. Is (47.6698, 17.6588) bigger than (47.4816, 19.1300)? These points can be arranged in a [quadtree](https://en.wikipedia.org/wiki/Quadtree), in which every node has at most _four_ children, rather than two as in a binary tree. A point to the North-West would become the first child, a point to the North-East the second, etc.
> 
> ![Source: https://commons.wikimedia.org/wiki/User:David_Eppstein/](https://upload.wikimedia.org/wikipedia/commons/8/8b/Point_quadtree.svg)
> 
> Show that looking for a point in a quadtree has `O(log n)` complexity.
> 
> [Searching for points in three dimensions](https://www.youtube.com/watch?v=G67eMq1YwmI)
{: .callout}

[Linked list](https://en.wikipedia.org/wiki/Linked_list)

[Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)

[Hash table](https://en.wikipedia.org/wiki/Hash_table)

> ## Exercise
> Suppose you categorize your files into folders. To maintain a clear folder structure, each folder contains at most seven files or subfolders. Given your logical structure, you always know for sure which subfolder to look into when searching a file.
> 
> You have 15,000 files to organize. What is the worst-case number of steps for finding a file?
{: .challenge}

With bisection search, binary trees or similar data structures, search time is O(log n). This is very good, but we can do even faster!

### Hash tables

> ## Hash function
> 1. Uniformity
> 2. Defined range
> 3. Pre-image resistant
> 4. Collision resistant
> 5. One-way function
{: .callout}

FIXME: exercises about hash functions?

> In a well-dimensioned hash table, the average cost (number of instructions) for each lookup is independent of the number of elements stored in the table.

```
hash function -> cryptographic hash function
```
[cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function)

## Indexing

> **Indexing** takes unordered lists of data and converts them into data structures suitable for efficient lookup.

{% assign table_data = site.data.structure.id-index %}
{% assign table_caption = "A sorted index of `id`" %}
{% include table.html %}

{% assign table_data = site.data.structure.word-index %}
{% assign table_caption = "A sorted index of `word`" %}
{% include table.html %}

FIXME: create simple indexing exercise



### Hash tables

What if we had an index where we knew exactly where to look for keys?

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

> ## Challenge (optional)
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

FIXME: [create index in sqlite](https://www.sqlitetutorial.net/sqlite-index/)



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
