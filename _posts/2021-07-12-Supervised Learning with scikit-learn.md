---
layout: post
title: 使用scikit-learn來執行監督式學習
summary: datacamp學習筆記
featured-img: sklearn
categories: [python]
---

# 目錄

***

0. [前言](#前言)

1. [Classification](#Classification)



<a name="前言"/>
# 前言

***

這篇文章記錄我在[datacamp](https://learn.datacamp.com)學習**Supervised Learning with scikit-learn**的課程。

什麼是ML?

簡單來說就是給機器資料，讓他有能力去做決定，

而這個決定並不是依賴明確的編碼而產生的過程。

Supervised learning 與 Unsupervised learning 最大的差別在於要不要主動去貼標這件事。


# Classification

***

*EDA(Exploratory data analysis)*

- load the dataset
```python
from sklearn import datasets
iris = datasets.load_iris()
print(iris.keys())  # to see the keys of iris
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

```python
# Import necessary modules
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
# Create feature and target arrays
X = digits.data
y = digits.target
# Split into training and test set
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state=42, stratify=y)
# Create a k-NN classifier with 7 neighbors: knn
knn = KNeighborsClassifier(n_neighbors=7)
# Fit the classifier to the training data
knn.fit(X_train, y_train)
# y_pred = knn.predict(X_test)
# Print the accuracy
print(knn.score(X_test, y_test))
```

*Generate plot to see which k is the best*

- larger k = smoother decision boundary = less complex model
- larger k = more complex model = can lead to overfitting
- use model complexity to see "overfitting" and "underfitting"
- review: [enumerate()](https://www.runoob.com/python/python-func-enumerate.html)
```python
# Setup arrays to store train and test accuracies
neighbors = np.arange(1, 9)
train_accuracy = np.empty(len(neighbors))
test_accuracy = np.empty(len(neighbors))
# Loop over different values of k
for i, k in enumerate(neighbors):
    # Setup a k-NN Classifier with k neighbors: knn
    knn = KNeighborsClassifier(n_neighbors=k)
    # Fit the classifier to the training data
    knn.fit(X_train, y_train) 
    # Compute accuracy on the training set
    train_accuracy[i] = knn.score(X_train, y_train)
    # Compute accuracy on the testing set
    test_accuracy[i] = knn.score(X_test, y_test)
# Generate plot
plt.title('k-NN: Varying Number of Neighbors')
plt.plot(neighbors, test_accuracy, label = 'Testing Accuracy')
plt.plot(neighbors, train_accuracy, label = 'Training Accuracy')
plt.legend()
plt.xlabel('Number of Neighbors')
plt.ylabel('Accuracy')
plt.show()
```
![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/knn_with_different_k.jpg)