---
layout: post
title: Pandas常用語法
summary: 資料科學中最基礎的功夫!
featured-img: pandas
categories: [Python]
---

# 前言

***

最近在一個以資料科學為主的程式教學平台[datacamp](https://learn.datacamp.com)學習，

寫這篇文章一方面分享學習筆記，

也讓自己未來在操作時有地方翻語法。


# Data Manipulation with pandas

***

## Transforming Data

*Intro of df*
- df.head()  # 看前五rows
- df.info()  # 跳出各col是int/float/object/...
- df.shape   # 看幾x幾
- df.describe()  # mean/median...
- **df.values**  # 2D Numpy array，**丟入模型常用**
- df.coulmns  # 看cols的名稱
- df.index  # 看rows的名稱
- df.sort_values("col_a", ascending="True/False")  # 針對col_a排序
    - 可以裝成list, eg. sort_values(["a","b"],ascending=[True,False])
- df["col_a"]  # print出col_a
    - df[["col_a", "col_b"]]
- df["new"] = df["old1"] + df["old2"]  # 增加col

*Subsetting*
- df["height_cm"] > 60  # True/False
    - df[df["height_cm"] > 60]  # print出高度大於60
    - df[(dogs["height_cm"] > 60) & (dogs["color"] == "tan")]  # 多個條件
- 篩選欄位的值
```python
colors = ["brown", "black", "tan"]  # 只要這些顏色
condition = dogs["color"].isin(colors)
dogs[condition]  # only brown, black, tan dog
```

## Aggregating Data

*Summary statistics*
- df["column"].mean()  # 取那個col的平均，類似的有max/min/median
- df["column"].agg(function)  # agg是用來套入function
```python
# Import NumPy and create custom IQR function
import numpy as np
def iqr(column):
   return column.quantile(0.75) - column.quantile(0.25)
# Update to print IQR and median of temperature_c, fuel_price_usd_per_l, & unemployment
print(sales[["temperature_c", "fuel_price_usd_per_l", "unemployment"]].agg([iqr, np.median]))
```
```
              temperature_c   fuel_price_usd_per_l  unemployment
    iqr           16.583               0.073            0.565
    median        16.967               0.743            8.099
```

- df["column"].cumsum()  # 累加column的值
- df["column"].cummax()  # 依照每一列記錄下當前的max
```
             date     weekly_sales  cum_weekly_sales  cum_max_sales
    0  2010-02-05      24924.50          24924.50       24924.50
    1  2010-03-05      21827.90          46752.40       24924.50
    2  2010-04-02      57258.43         104010.83       57258.43
    3  2010-05-07      17413.94         121424.77       57258.43
    4  2010-06-04      17558.09         138982.86       57258.43
    5  2010-07-02      16333.14         155316.00       57258.43
```
- df.drop("col1", axis=0)  # axis=0表示刪除row，axis=1表示刪除column，此處為把col1刪掉
- df.drop_duplicates(subset="column")  # 針對column把重複項刪除
- df.drop_duplicates(subset=["column1","column2"])  # 兩個col都一樣才刪
- df["column"].value_counts()  # 算各項出現幾次
    - df["column"].value_counts(normalize=True)  # 算出比例
    - df["column"].value_counts(sort=True)  # sort
- df.groupby("color")["weight"].mean()  # 算出在不同顏色下不同的平均重量
- df.groupby("color")["weight"].agg([min, max, sum])  # 一次有三個數值
- df.groupby(["color", "breed"])["weight"].mean() 

*Pivot tables*
- df.pivot_table(values="kg", index="color", aggfunc=[np.mean, np.median])  # 跑出像是**樞紐分析表**的東西(aggfunc預設是平均)
- df.pivot_table(values="kg", index="color", columns="breed")  # 空值會是NaN，若想空值補零就加 fill_value=0，多加margins=True會多出一列與一行，顯示各row/column的平均值


## Slicing and Indexing

*Setting Index*
- df.index = list  # 把df的index換成list裡面的element
    - pd_ind = pd.set_index("name")  # 設定index，此處把name當作index
    - df.sort_index(level="column1", ascending=False)  # 對index中的column1做sort(descending)
    - pd_ind.loc[["name1", "name2"]]  # 設定index的好處就是能快速找到name1和name2的rows
    - pd_ind1 = pd.set_index(["breed", "color"])  # 也可以同時有兩個
        - pd_ind1 = pd.set_index(["breed", "color"], ascending=[True, False])  # sort
        - pd_ind1.loc[("breed1", "Brown"), ("breed2", "Tan")]  # 用tuple來找交集的rows
- pd_ind.reset_index()  # 重新回到原本pd的樣子
    - pd_ind.reset_index(drop=True)  # 多把name那個col刪掉

*Slicing the df*
- pd.loc["index_x":"index_y"]  # 這也是為什麼要先sort過，再來slice，這會得到index_x到index_y的資料
- df.loc[("country1", "city1"):("country2", "city2")]  # 仍然針對index來做篩選，值得注意適用在多個index的情況下
```
                          date  avg_temp_c
    country  city                         
    Pakistan Lahore 2000-01-01      12.792
             Lahore 2000-02-01      14.339
             Lahore 2000-03-01      20.309
             Lahore 2000-04-01      29.072
             Lahore 2000-05-01      34.845
    ...                    ...         ...
    Russia   Moscow 2013-05-01      16.152
             Moscow 2013-06-01      18.718
             Moscow 2013-07-01      18.136
             Moscow 2013-08-01      17.485
             Moscow 2013-09-01         NaN
```
- df.iloc[:, "col_x":"col_y"]  # 針對column來slice
- df.iloc[2:5, 1:4]  # 對row與column同時來slice

*pivot tables & index*
- df.mean(axis="index")  # 替index算平均
- df.mean(axis="columns")  # 替columns算平均


## Creating and Visualizing DataFrames

*Using matplotlib.pyplot to visualize*
- df["column1"].hist()  # histograms(**要先group完!!!**)
    - df["column1"].hist(bins=k)  # 設定bar的數量
    - df["column1"].hist(alpha=0.7)  # 設定透明度，0為完全透明，1為完全不透明
    - df[df["sex"]=="F"]["cm"].hist()  # 篩選出女生高度
    - plt.legend(["F","M"])  # 使用時機: 在一張圖同時有兩個變量時給予顏色(此例為兩變量為男性與女性)
- df.plot(kind="bar", title="test")  # bar plot
- df.plot(x="date", y="kg", kind="line")  # line plots:顯現時間趨勢
- df.plot(x="cm", y="kg", kind="scatter")  # scatter plot:適合用在檢視兩變數關係
- plt.show()  # print

*Dealing with missing data*
- df.isna()  # return True/False, True 代表缺失
    - df.isna().any()  # 看整個column中，有沒有任何缺失值
    - df.isna().sum()  # 算整個column中，缺失值共有幾個
    - df.isna().sum().plot(kind="bar")
- df.dropna()  # 直接不要那行資料
- df.fillna(0)  # NaN填0

*Two Ways of Creating DataFrames*
- From a list of dicts
    - row by row
```python
list_of_dicts = [
    {"name": "A", "grade": 90},
    {"name": "B" "grade": 85},
    {"name": "C", "grade": 80}
]
```
    - pd.DataFrame(list_of_dicts)
- From a dict of lists
    - col by col
```python
dict_of_lists = {
    "name": ["A", "B", "C"],
    "grade": [90, 85, 80]
}
```
    - key = column name
    - value = list of column values
    - pd.DataFrame(dict_of_lists)

*Reading and writing CSVs*
```python
# CSV to DataFrame
import pandas as pd
newfile = pd.read_csv("newfile.csv")
print(newfile)
# DataFrame to CSV
newfile.to_csv("newfile.csv")
```