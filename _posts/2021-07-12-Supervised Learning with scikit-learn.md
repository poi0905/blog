---
layout: post
title: 使用scikit-learn來執行監督式學習
summary: datacamp學習筆記
featured-img: sklearn
categories: [python]
---

# 前言

***

這篇文章記錄我在[datacamp](https://learn.datacamp.com)學習**Supervised Learning with scikit-learn**的課程。

什麼是ML?

簡單來說就是給機器資料，讓他有能力去做決定，

而這個決定並不是依賴明確的編碼而產生的過程。

Supervised learning 與 Unsupervised learning 最大的差別在於要不要主動去貼標這件事。

# Supervised Learning with scikit-learn

***

## Classification

*EDA(Exploratory data analysis)*

- load the dataset
```python
from sklearn import datasets
iris = datasets.load_iris()
```
- visual EDA
```python
x = iris.data
y = iris.target
df = pd.DataFrame(X, columns=iris.feature_names)
_ = pd.plotting.scatter_matrix(df, c=y, figsize=[8, 8], s=150, marker="D")
```
