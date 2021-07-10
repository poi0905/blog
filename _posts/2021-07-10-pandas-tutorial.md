---
layout: post
title: Python中pandas的常用語法
summary: 這篇文章將分享我在datacamp上學習pandas時的筆記(還是怕自己以後忘記...)
featured-img: pandas
categories: [教學]
---

# 前言

***

最近在一個以資料科學為主的程式教學平台[datacamp](https://learn.datacamp.com)學習，

寫這篇文章一方面分享學習筆記，

也讓自己未來在操作時有地方翻語法。


# Data Manipulation with pandas

***

## Transforming Data

一開始從最基礎的檢視dataframe(df)開始。
- df.head() # 看前五rows
- df.info() # 跳出各col是int/float/object/...
- df.shape  # 看幾x幾
- df.describe() # mean/median...
- df.values # 2D Numpy array
- df.coulmns # 看cols的名稱
- df.index # 看rows的名稱
- sort_values("col_a", ascending="True/False") # 針對col_a排序
    - 可以裝成list, eg. sort_values(["a","b"],ascending=[True,False])
- df["col_a"] # print出col_a
    - df[["col_a", "col_b"]]
- df["new"] = df["old1"] + df["old2"] : 增加col
- dogs["height_cm"] > 60 # True/False
    - dogs[dogs["height_cm"] > 60] # print出高度大於60
    - dogs[(dogs["height_cm"] > 60) & (dogs["color"] == "tan")] #　多個條件
- 篩選欄位的值
```
colors = ["brown", "black", "tan"] # 只要這些顏色
condition = dogs["color"].isin(colors)
dogs[condition] # only brown, black, tan dog
```

## Aggregating Data

進入到敘述性統計
- df["column"].mean() # 取那個col的平均，類似的有max/min/median
- df["column"].agg(function) # agg是用來套入function
```
# Import NumPy and create custom IQR function
import numpy as np
def iqr(column):
   return column.quantile(0.75) - column.quantile(0.25)
# Update to print IQR and median of temperature_c, fuel_price_usd_per_l, & unemployment
print(sales[["temperature_c", "fuel_price_usd_per_l", "unemployment"]].agg([iqr, np.median]))
```
|                      | temperature_c        | fuel_price_usd_per_l | unemployment         |
|----------------------|----------------------|----------------------|----------------------|
| iqr                  |    16.583            |     0.073            |     0.565            |
| median               |    16.967            |     0.743            |     8.099            |
- df["column"].cumsum() # 累加column的值
- df["column"].cummax() # 依照每一列記錄下當前的max
|                  | date             | weekly_sales     | cum_weekly_sales | cum_max_sale     |
|------------------|------------------|------------------|------------------|------------------|
| 0                | 2010-02-05       | 24924.50         | 24924.50         | 24924.50         |
| 1                | 2010-03-05       | 21827.90         | 46752.40         | 24924.50         |
| 2                | 2010-04-02       | 57258.43         | 104010.83        | 57258.43         |
| 3                | 2010-05-07       | 17413.94         | 121424.77        | 57258.43         |
| 4                | 2010-06-04       | 17558.09         | 138982.86        | 57258.43         |
- df.drop_duplicates(subset="column") # 針對column把重複項刪除
- df.drop_duplicates(subset=["column1","column2"]) # 兩個col都一樣才刪
- df["column"].value_counts() # 算各項出現幾次
    - df["column"].value_counts(normalize=True) # 算出比例
    - df["column"].value_counts(sort=True) # sort
- df.groupby("color")["weight"].mean() # 算出在不同顏色下不同的平均重量
- df.groupby("color")["weight"].agg([min, max, sum]) # 一次有三個數值
- df.groupby(["color", "breed"])["weight"].mean() 
- df.pivot_table(values="kg", index="color", aggfunc=[np.mean, np.median]) # 跑出像是**樞紐分析表**的東西(aggfunc預設是平均)
- df.pivot_table(values="kg", index="color", columns="breed") # 空值會是NaN，若想空值補零就加 fill_value=0，多加margins=True會多出一列與一行，顯示各row/column的平均值


## Slicing and Indexing



## Creating and Visualizing DataFrames


