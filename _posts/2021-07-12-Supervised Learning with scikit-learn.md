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
- target variable 也就是y值，換句話說就是要預測的值。
- 當features的值是binary時，可以用**Seaborn's countplot**來進行EDA。
```python
plt.figure()
# x=feature, hue=y, palette=color of the bar
sns.countplot(x='education', hue='party', data=df, palette='RdBu')
# 0 represent no, 1 represent yes
plt.xticks([0,1], ['No', 'Yes'])
plt.show()  # outcome的x軸會是education，分別是No跟Yes，且在No跟Yes上分別各有兩個bars，indicate出兩parties各有多少人數(party只有民主黨跟共和黨)
```

*classification with KNN*

- by using features to predict which parties would the voter vote to
```python
from sklearn.neighbors import KNeighborsClassifier
# Create arrays for the features and the response variable
y = df['party'].values
X = df.drop('party', axis=1).values
# Create a k-NN classifier with 6 neighbors
knn = KNeighborsClassifier(n_neighbors=6)
# Fit the classifier to the data
knn.fit(X,y)
# Predict the labels for the training data X
y_pred = knn.predict(X)
# Predict and print the label for the new data point X_new
new_prediction = knn.predict(X_new)
print("Prediction: {}".format(new_prediction))
```

*Measuring model performance*