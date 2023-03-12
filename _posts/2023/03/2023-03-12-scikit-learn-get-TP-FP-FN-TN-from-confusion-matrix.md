---
title: "How to get True Positive, False Positive, True Negative and False Negative from confusion matrix in scikit learn"
date: 2023-03-12
categories: [matplotlib, python]
tags: [matplotlib, python]
last_modified_at: 2023-03-12
permalink: How-to-get-TP-FP-TN-and-FN-from-cm-in-sklearn
---

In machine learning, we often use classification models to predict the class labels of a set of samples. 

The predicted labels may or may not match the true labels, depending on the accuracy of the model. 

To measure the accuracy of a classification model, we use metrics such as precision, recall, and F-score. 

To calculate these metrics, we need to know the number of True Positive (TP), True Negative (TN), False Positive (FP), and False Negative (FN) samples. 

In this article, we will discuss how to obtain these values in scikit-learn, a popular Python library for machine learning.

What are True Positive, True Negative, False Positive, and False Negative?

Before we dive into the implementation details, let's briefly define these terms:

- True Positive (TP): The number of samples that are correctly predicted as positive (belonging to the positive class).
- True Negative (TN): The number of samples that are correctly predicted as negative (not belonging to the positive class).
- False Positive (FP): The number of samples that are incorrectly predicted as positive (not belonging to the positive class but predicted as such).
- False Negative (FN): The number of samples that are incorrectly predicted as negative (belonging to the positive class but predicted as not belonging to it).

These values are often used to calculate other classification metrics such as precision, recall, and F-score, which provide a more comprehensive evaluation of the model's performance.

Obtaining True Positive, True Negative, False Positive, and False Negative in Scikit-Learn

In scikit-learn, we can obtain these values using the confusion matrix function from the metrics module. 

The confusion matrix is a table that summarizes the predictions of a classification model by comparing the predicted labels with the true labels. It has four cells, each representing the number of TP, TN, FP, and FN samples, respectively.

In scikit-learn, the confusion matrix is generated using the [confusion_matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)  function from the `sklearn.metrics` module. The function takes two arguments: the true labels and the predicted labels. It returns a two-dimensional array with the following layout:

## Compute true positive, true negative, false positive and false negative from confusion matrix in Binary Classification

Here's an example of how to obtain the confusion matrix for a binary classification problem in scikit-learn:

```python
from sklearn.metrics import confusion_matrix

y_true = [1, 0, 1, 1, 0, 1]
y_pred = [1, 0, 0, 1, 1, 1]

tn, fp, fn, tp = confusion_matrix(y_true, y_pred).ravel()

print("True Negative:", tn)
print("False Positive:", fp)
print("False Negative:", fn)
print("True Positive:", tp)

```

In this example, `y_true` represents the true labels of a binary classification problem, and `y_pred` represents the predicted labels. 

The `confusion_matrix` function returns a matrix(2 X2) with the number of true negatives, false positives, false negatives, and true positives.

```python
array([[1, 1],
       [1, 3]])
```

 The `ravel` function is used to convert the matrix into a 1-dimensional array so that we can easily extract the values we're interested in using indexing.

The output of this code will be:

```python
True Negative: 1
False Positive: 1
False Negative: 1
True Positive: 3

```

This means that the model correctly predicted 1 negative sample (True Negative), incorrectly predicted 1 positive sample as negative (False Negative), incorrectly predicted 1 negative sample as positive (False Positive), and correctly predicted 3 positive samples (True Positive).

## Compute true positive, true negative, false positive and false negative from confusion matrix in multilabel classification

This is an example of multi-class classification, we have three classes in y_true and y_pred arrays i.e. animal, fruit and reptile(sorted alphabetically)

```python
y_true = ["animal", "fruit", "animal", 
          "animal", "fruit", "reptile"]
y_pred = ["fruit", "fruit", "animal", 
          "animal", "fruit", "animal"]
```

```python
confusion_matrix(y_true, y_pred, labels=["animal", 
                                         "fruit", 
                                         "reptile"])
```

**Out:**

The output of confusion_matrix returns a 3 X 3 matrix

```python
array([[2, 1, 0],
       [0, 2, 0],
       [1, 0, 0]])
```

The output of the confusion matrix looks like this with the labels

|         | animal | fruit | reptile |
| :-----: | :----: | :---: | :-----: |
| animal  |   2    |   1   |    0    |
|  fruit  |   0    |   2   |    0    |
| reptile |   1    |   0   |    0    |

In the multi-class classification problem, we won’t get TP, TN, FP, FN values directly as in the binary classification problem. We need to compute that for each class.

Now let's compute the TP, TN, FP, FN for class animal :

TP = 2, cell (animal, animal)

FP = 1, sum of cell(fruit, animal) and cell(reptile, animal)

TN = 2, sum of cell(fruit, fruit), cell(fruit, reptile), cell(reptile, fruit) and cell(reptile, reptile) 

FN = 1, sumof cell(fruit, reptile) and cell(animal, reptile)

**Confusion Matrix for Class Animal:**

|   TN(2)   |   FP(1)   |
| :-------: | :-------: |
| **FN(1)** | **TP(2)** |

Similarly we can compute it for other classes fruit and reptile

However, You could also compute the confusion matrix of each classes automatically using scikit learn [sklearn.metrics.**multilabel_confusion_matrix**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.multilabel_confusion_matrix.html) as shown here

```python
multilabel_confusion_matrix(y_true, y_pred,)
```

**Out:**

```python
array([[[2, 1],
        [1, 2]],

       [[3, 1],
        [0, 2]],

       [[5, 0],
        [1, 0]]])
```



The first array is same as the animal confusion matrix we computed manually above, And the other two arrays are the confusion matrix for fruit and reptiles respectively

