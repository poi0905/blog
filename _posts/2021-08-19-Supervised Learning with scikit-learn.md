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

2. [Regression](#Regression)

3. [Fine-tuning your model](#3)



<a name="前言"/>
# 前言

***

這篇文章記錄我在[datacamp](https://learn.datacamp.com)學習**Supervised Learning with scikit-learn**的課程。

什麼是ML?

簡單來說就是給機器資料，讓他有能力去做決定，

而這個決定並不是依賴明確的編碼而產生的過程。

Supervised learning 與 Unsupervised learning 最大的差別在於要不要主動去貼標這件事。


<a name="Classification"/>
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


<a name="Regression"/>
# Regression

***

- regression 的 target value(y) 是連續型的變數，如GDP。
- 以下範例為預測年紀的程式碼
```python
# Import numpy and pandas
import numpy as np
import pandas as pd
# Read the CSV file into a DataFrame: df
df = pd.read_csv('gapminder.csv')
# Create arrays for features and target variable(先僅用出生率做出一個單變數回歸)
X = df['fertility'].values
y = df['life'].values
# Print the dimensions of y and X before reshaping
print("Dimensions of y before reshaping: ", y.shape)
print("Dimensions of X before reshaping: ", X.shape)
# Reshape X and y
y_reshaped = y.reshape(-1,1)  # 假設原先的array有8個elements，reshape(2,4)將變成2rows,4columns的matrix
X_reshaped = X.reshape(-1,1)  # reshape填入-1即代表自動計算(給定另一個的數字的情況下)
# Print the dimensions of y_reshaped and X_reshaped
print("Dimensions of y after reshaping: ", y_reshaped.shape)
print("Dimensions of X after reshaping: ", X_reshaped.shape)
```
```
Dimensions of y before reshaping:  (139,)
Dimensions of X before reshaping:  (139,)
Dimensions of y after reshaping:  (139, 1)
Dimensions of X after reshaping:  (139, 1)
```
```python
# Import LinearRegression
from sklearn.linear_model import LinearRegression
# Create the regressor: reg
reg = LinearRegression()
# Create the prediction space(做出一個等差數列，再轉成matrix，將用來做為測試集)
prediction_space = np.linspace(min(X_fertility), max(X_fertility)).reshape(-1,1)    
# Fit the model to the data
reg.fit(X_fertility, y)
# Compute predictions over the prediction space: y_pred
y_pred = reg.predict(prediction_space)
# Print R^2 (regression用R square來看)
print(reg.score(X_fertility, y))
```
```python
# Import necessary modules
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
# Create training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state=42)
# Create the regressor: reg_all
reg_all = LinearRegression()
# Fit the regressor to the training data
reg_all.fit(X_train, y_train)
# Predict on the test data: y_pred
y_pred = reg_all.predict(X_test)
# Compute and print R^2 and RMSE
print("R^2: {}".format(reg_all.score(X_test, y_test)))
rmse = np.sqrt(mean_squared_error(y_test, y_pred))
print("Root Mean Squared Error: {}".format(rmse))
```
```
    R^2: 0.838046873142936
    Root Mean Squared Error: 3.2476010800377213
```

*Cross-validation*

- k folds = k-fold CV
- more folds = more computationally expensive
- 好處是不會剛好在切訓練集時特別衰
```python
# Import the necessary modules
from sklearn.linear_model import LinearRegression 
from sklearn.model_selection import cross_val_score 
# Create a linear regression object: reg
reg = LinearRegression()
# Compute 5-fold cross-validation scores: cv_scores
cv_scores = cross_val_score(reg, X, y, cv=5)
# Print the 5-fold cross-validation scores
print(cv_scores)
print("Average 5-Fold CV Score: {}".format(np.mean(cv_scores)))
```
```
    [0.81720569 0.82917058 0.90214134 0.80633989 0.94495637]
    Average 5-Fold CV Score: 0.8599627722793232
```

*Regularized regression*

- goals: penalizing large coefficients by *Reqularization* because large coefficients can lead to overfitting
- **Example 1: ridge regression**
    - Loss function = OLS loss function + Alpha*sum(coefficients)^2
    - **(need to select Alpha, like KNN's k)** 
    - 下例將顯示不同Alpha時獲得不同的score
```python
# Import necessary modules
from sklearn.linear_model import Ridge
from sklearn.model_selection import cross_val_score
# Setup the array of alphas and lists to store scores
alpha_space = np.logspace(-4, 0, 50)
ridge_scores = []
ridge_scores_std = []
# Create a ridge regressor: ridge
ridge = Ridge(normalize=True)  # 原本會是Ridge(alpha=0.1, normalize=True)，但這裡不寫alpha留到迴圈寫，為的是顯示出不同Alpha時不同的情況
# Compute scores over range of alphas
for alpha in alpha_space:
    # Specify the alpha value to use: ridge.alpha
    ridge.alpha = alpha
    # Perform 10-fold CV: ridge_cv_scores
    ridge_cv_scores = cross_val_score(ridge, X, y, cv=10)
    # Append the mean of ridge_cv_scores to ridge_scores
    ridge_scores.append(np.mean(ridge_cv_scores))
    # Append the std of ridge_cv_scores to ridge_scores_std
    ridge_scores_std.append(np.std(ridge_cv_scores))
# Display the plot
display_plot(ridge_scores, ridge_scores_std)
```
- 下面程式碼作為補充
```python
def display_plot(cv_scores, cv_scores_std):
    fig = plt.figure()
    ax = fig.add_subplot(1,1,1)
    ax.plot(alpha_space, cv_scores)
    std_error = cv_scores_std / np.sqrt(10)
    ax.fill_between(alpha_space, cv_scores + std_error, cv_scores - std_error, alpha=0.2)
    ax.set_ylabel('CV Score +/- Std Error')
    ax.set_xlabel('Alpha')
    ax.axhline(np.max(cv_scores), linestyle='--', color='.5')
    ax.set_xlim([alpha_space[0], alpha_space[-1]])
    ax.set_xscale('log')
    plt.show()
```
![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/Ridge.jpg)
- **Example 2: Lasso regression**
    - Loss function = OLS loss function + Alpha*abs(coefficients)
    - **can be used to select important features of a dataset**
    - shrinks the coefficients of less important features to exactly 0
```python
 # Import Lasso
from sklearn.linear_model import Lasso
# Instantiate a lasso regressor: lasso
lasso = Lasso(alpha=0.4, normalize=True)
# Fit the regressor to the data
lasso.fit(X, y)
# Compute and print the coefficients
lasso_coef = lasso.fit(X, y).coef_
print(lasso_coef)
# output:    [-0.         -0.         -0.          0.          0.          0.
#     -0.         -0.07087587]
# Plot the coefficients
plt.plot(range(len(df_columns)), lasso_coef)
plt.xticks(range(len(df_columns)), df_columns.values, rotation=60)
plt.margins(0.02)
plt.show()
```
![image](https://raw.githubusercontent.com/poi0905/blog/master/assets/img/posts/lasso_feature_selection.jpg)
*'child_mortality' is the most important feature when predicting life expectancy.*

<a name="3"/>
# Fine-tuning your model

***

- Metrics for classification
    - KNN 
```python
# Import necessary modules
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
# Create training and test set
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=42)
# Instantiate a k-NN classifier: knn
knn = KNeighborsClassifier(n_neighbors=6)
# Fit the classifier to the training data
knn.fit(X_train, y_train)
# Predict the labels of the test data: y_pred
y_pred = knn.predict(X_test)
# Generate the confusion matrix and classification report
print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))
```
    - Logistic regression (預設的 threshold=0.5，超過0.5判定為1，相反則為0。)
```python
# Import the necessary modules
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix, classification_report
# Create training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.4, random_state=42)
# Create the classifier: logreg
logreg = LogisticRegression()
# Fit the classifier to the training data
logreg.fit(X_train, y_train)
# Predict the labels of the test set: y_pred
y_pred = logreg.predict(X_test)
# Compute and print the confusion matrix and classification report
print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))
```
    - 製作ROC curve
```python
# Import necessary modules
from sklearn.metrics import roc_curve
# Compute predicted probabilities: y_pred_prob
y_pred_prob = logreg.predict_proba(X_test)[:,1]
# Generate ROC curve values: fpr, tpr, thresholds
fpr, tpr, thresholds = roc_curve(y_test, y_pred_prob)
# Plot ROC curve
plt.plot([0, 1], [0, 1], 'k--')
plt.plot(fpr, tpr)
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curve')
plt.show()
```