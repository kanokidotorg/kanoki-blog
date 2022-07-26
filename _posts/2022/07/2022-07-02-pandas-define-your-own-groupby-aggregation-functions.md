---
title: "Pandas define your own groupby aggregation functions"
date: "2022-07-02"
categories: [ pandas, python]
tags: [ pandas, python]

---

The .agg method does aggregation as it sounds and you can pass in the names of aggregation methods, Python aggregations, Numpy reduce functions and you can also define your own function.

The beauty of .agg is you can do multiple aggregation at the same time. 

So let's group this data by Student and compute the max, min, grade(based on condition) and difference between max and min scores for each student 



|      | Student | Score |
| ---- | ------- | ----- |
| 0    | 1       | 50    |
| 1    | 5       | 42    |
| 2    | 2       | 24    |
| 3    | 3       | 61    |
| 4    | 4       | 75    |
| 5    | 1       | 98    |
| 6    | 2       | 37    |
| 7    | 3       | 42    |
| 8    | 4       | 90    |
| 9    | 5       | 43    |
|      |         |       |

Create a custom function grade that takes a series of score as parameter and computes the mean and returns grade A if mean score of student  is greater than 50 otherwise returns grade B

```python
def grade(x):
    mean_score = x.mean()
    return 'A' if mean_score >= 50 else 'B'
```
Now pass multiple aggregation function name along with our custom function grade and a lambda function to compute the difference between max and min score

```python
df.groupby('col').value.agg(max_score = 'max',
                      min_score = 'min',
                      grade = grade,
                      diff_max_min = lambda x: x.max()-x.min())
```

**Output:**

|      |     student    |   max_score   | min_score | grade | diff_max_min |
| ----------- | :-----------: | :-----------:| :-----------: | :-----------:|:-----------:|
| 0      | 1         | 98 | 50 | A | 48 |
| 1      | 2         | 37 | 24 | B | 13 |
| 2      | 3         | 61 | 42 | A | 19 |
| 3      | 4         | 90 | 75 | A | 15 |
| 4      | 5         | 43 | 42 | B | 1 |
