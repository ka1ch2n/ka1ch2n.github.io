---
layout: post
title: Replace data manipulation in Excel with Python
excerpt: "Use Pivot table in Pandas"
modified: 2016-09-23T14:17:25-04:00
categories: articles
tags: [Excel, Pivot Table, Pandas, Python]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---


The **pivot table** is a powerful tool to summarize and present data in **Excel**. However, in **Python**, [Pandas](http://pandas.pydata.org/)  has a function which allows you to quickly convert a DataFrame to a pivot table. 

This function is **very useful** but sometimes it can be **tricky** to remember how to use it to get the data formatted in a way you need.


# Read in the data


```python
import pandas as pd
import numpy as np
```

Read in the sales funnel data into  `DataFrame`.


```python
df = pd.read_excel("../sales-funnel.xlsx")
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Name</th>
      <th>Rep</th>
      <th>Manager</th>
      <th>Product</th>
      <th>Quantity</th>
      <th>Price</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>714466</td>
      <td>Trantow-Barrows</td>
      <td>Craig Booker</td>
      <td>Debra Henley</td>
      <td>CPU</td>
      <td>1</td>
      <td>30000</td>
      <td>presented</td>
    </tr>
    <tr>
      <th>1</th>
      <td>714466</td>
      <td>Trantow-Barrows</td>
      <td>Craig Booker</td>
      <td>Debra Henley</td>
      <td>Software</td>
      <td>1</td>
      <td>10000</td>
      <td>presented</td>
    </tr>
    <tr>
      <th>2</th>
      <td>714466</td>
      <td>Trantow-Barrows</td>
      <td>Craig Booker</td>
      <td>Debra Henley</td>
      <td>Maintenance</td>
      <td>2</td>
      <td>5000</td>
      <td>pending</td>
    </tr>
    <tr>
      <th>3</th>
      <td>737550</td>
      <td>Fritsch, Russel and Anderson</td>
      <td>Craig Booker</td>
      <td>Debra Henley</td>
      <td>CPU</td>
      <td>1</td>
      <td>35000</td>
      <td>declined</td>
    </tr>
    <tr>
      <th>4</th>
      <td>146832</td>
      <td>Kiehn-Spinka</td>
      <td>Daniel Hilton</td>
      <td>Debra Henley</td>
      <td>CPU</td>
      <td>2</td>
      <td>65000</td>
      <td>won</td>
    </tr>
  </tbody>
</table>
</div>



For convenience sake, I define the status column as a `category` and set the order we'd like to view. This isn't strictly required but helps us keep the order we want as we work through analyzing the data.


```python
df["Status"] = df["Status"].astype("category")
df["Status"].cat.set_categories(["won","pending","presented","declined"],inplace=True)
```

# Pivot the data

As we build up the pivot table, I think it's easiest to take it **one step at a time**. Add items one at a time and check each step to verify you are getting the results you expect.

The simplest pivot table must have a dataframe and an index. 

```python
pd.pivot_table(df,index=["Name"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Barton LLC</th>
      <td>740150.0</td>
      <td>35000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Fritsch, Russel and Anderson</th>
      <td>737550.0</td>
      <td>35000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Herman LLC</th>
      <td>141962.0</td>
      <td>65000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Jerde-Hilpert</th>
      <td>412290.0</td>
      <td>5000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kassulke, Ondricka and Metz</th>
      <td>307599.0</td>
      <td>7000.0</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Keeling LLC</th>
      <td>688981.0</td>
      <td>100000.0</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>Kiehn-Spinka</th>
      <td>146832.0</td>
      <td>65000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Koepp Ltd</th>
      <td>729833.0</td>
      <td>35000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kulas Inc</th>
      <td>218895.0</td>
      <td>25000.0</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Purdy-Kunde</th>
      <td>163416.0</td>
      <td>30000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Stokes LLC</th>
      <td>239344.0</td>
      <td>7500.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Trantow-Barrows</th>
      <td>714466.0</td>
      <td>15000.0</td>
      <td>1.333333</td>
    </tr>
  </tbody>
</table>
</div>



You can have multiple indexes as well. In fact, most of the pivot_table args can take multiple values via a list.


```python
pd.pivot_table(df,index=["Name","Rep","Manager"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Name</th>
      <th>Rep</th>
      <th>Manager</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Barton LLC</th>
      <th>John Smith</th>
      <th>Debra Henley</th>
      <td>740150.0</td>
      <td>35000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Fritsch, Russel and Anderson</th>
      <th>Craig Booker</th>
      <th>Debra Henley</th>
      <td>737550.0</td>
      <td>35000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Herman LLC</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>141962.0</td>
      <td>65000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Jerde-Hilpert</th>
      <th>John Smith</th>
      <th>Debra Henley</th>
      <td>412290.0</td>
      <td>5000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kassulke, Ondricka and Metz</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>307599.0</td>
      <td>7000.0</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Keeling LLC</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>688981.0</td>
      <td>100000.0</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>Kiehn-Spinka</th>
      <th>Daniel Hilton</th>
      <th>Debra Henley</th>
      <td>146832.0</td>
      <td>65000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Koepp Ltd</th>
      <th>Wendy Yule</th>
      <th>Fred Anderson</th>
      <td>729833.0</td>
      <td>35000.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Kulas Inc</th>
      <th>Daniel Hilton</th>
      <th>Debra Henley</th>
      <td>218895.0</td>
      <td>25000.0</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Purdy-Kunde</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>163416.0</td>
      <td>30000.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Stokes LLC</th>
      <th>Cedric Moss</th>
      <th>Fred Anderson</th>
      <td>239344.0</td>
      <td>7500.0</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Trantow-Barrows</th>
      <th>Craig Booker</th>
      <th>Debra Henley</th>
      <td>714466.0</td>
      <td>15000.0</td>
      <td>1.333333</td>
    </tr>
  </tbody>
</table>
</div>



This is interesting but not particularly useful. What we probably want to do is look at this by Manager and Director.
It's easy enough to do by changing the index.


```python
pd.pivot_table(df,index=["Manager","Rep"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Account</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>720237.0</td>
      <td>20000.000000</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>194874.0</td>
      <td>38333.333333</td>
      <td>1.666667</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>576220.0</td>
      <td>20000.000000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>196016.5</td>
      <td>27500.000000</td>
      <td>1.250000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>614061.5</td>
      <td>44250.000000</td>
      <td>3.000000</td>
    </tr>
  </tbody>
</table>
</div>



Now we start to get a glimpse of what a pivot table can do for us.

For this purpose, the Account and `Quantity` columns aren't really useful. Let's remove it by explicitly defining the columns we care about using the values field.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>20000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>38333</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>20000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>27500</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>44250</td>
    </tr>
  </tbody>
</table>
</div>



The `Price` column automatically averages the data but we can do a count or a sum. Adding them is simple using `aggfunc`.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],aggfunc=np.sum)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>80000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>115000</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>40000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>110000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>177000</td>
    </tr>
  </tbody>
</table>
</div>



`aggfunc` can take a list of functions. Let's try a `mean` using the numpy functions and `len` to get a count.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],aggfunc=[np.mean,len])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th>mean</th>
      <th>len</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>20000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>38333</td>
      <td>3</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>20000</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>27500</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>44250</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



If we want to see sales broken down by the `Products`, the `columns variable` allows us to define one or more columns.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],
               columns=["Product"],aggfunc=[np.sum])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">Price</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>65000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>105000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>35000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>95000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
      <td>10000.0</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>165000.0</td>
      <td>7000.0</td>
      <td>5000.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



The NaN's are a bit distracting. If we want to remove them, we could use `fill_value` to set them to `0`.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],
               columns=["Product"],aggfunc=[np.sum],fill_value=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">Price</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>65000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>105000</td>
      <td>0</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>35000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>95000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>165000</td>
      <td>7000</td>
      <td>5000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



It would be useful to add the quantity as well. Add `Quantity` to the values list.


```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price","Quantity"],
               columns=["Product"],aggfunc=[np.sum],fill_value=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="8" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">Price</th>
      <th colspan="4" halign="left">Quantity</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3" valign="top">Debra Henley</th>
      <th>Craig Booker</th>
      <td>65000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Daniel Hilton</th>
      <td>105000</td>
      <td>0</td>
      <td>0</td>
      <td>10000</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>John Smith</th>
      <td>35000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>Cedric Moss</th>
      <td>95000</td>
      <td>5000</td>
      <td>0</td>
      <td>10000</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Wendy Yule</th>
      <td>165000</td>
      <td>7000</td>
      <td>5000</td>
      <td>0</td>
      <td>7</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



What's interesting is that you can move items to the index to get a different visual representation. We can add the `Products` to the index.


```python
pd.pivot_table(df,index=["Manager","Rep","Product"],
               values=["Price","Quantity"],aggfunc=[np.sum],fill_value=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th colspan="2" halign="left">sum</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th>Product</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="7" valign="top">Debra Henley</th>
      <th rowspan="3" valign="top">Craig Booker</th>
      <th>CPU</th>
      <td>65000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Daniel Hilton</th>
      <th>CPU</th>
      <td>105000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">John Smith</th>
      <th>CPU</th>
      <td>35000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Fred Anderson</th>
      <th rowspan="3" valign="top">Cedric Moss</th>
      <th>CPU</th>
      <td>95000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Wendy Yule</th>
      <th>CPU</th>
      <td>165000</td>
      <td>7</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>7000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Monitor</th>
      <td>5000</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



For this data set, this representation makes more sense. Now, what if I want to see some totals? `margins=True` does that for us.


```python
pd.pivot_table(df,index=["Manager","Rep","Product"],
               values=["Price","Quantity"],
               aggfunc=[np.sum,np.mean],fill_value=0,margins=True)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th colspan="2" halign="left">sum</th>
      <th colspan="2" halign="left">mean</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th>Price</th>
      <th>Quantity</th>
      <th>Price</th>
      <th>Quantity</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Rep</th>
      <th>Product</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="7" valign="top">Debra Henley</th>
      <th rowspan="3" valign="top">Craig Booker</th>
      <th>CPU</th>
      <td>65000.0</td>
      <td>2.0</td>
      <td>32500.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000.0</td>
      <td>2.0</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000.0</td>
      <td>1.0</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Daniel Hilton</th>
      <th>CPU</th>
      <td>105000.0</td>
      <td>4.0</td>
      <td>52500.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000.0</td>
      <td>1.0</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">John Smith</th>
      <th>CPU</th>
      <td>35000.0</td>
      <td>1.0</td>
      <td>35000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000.0</td>
      <td>2.0</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th rowspan="6" valign="top">Fred Anderson</th>
      <th rowspan="3" valign="top">Cedric Moss</th>
      <th>CPU</th>
      <td>95000.0</td>
      <td>3.0</td>
      <td>47500.000000</td>
      <td>1.500000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>5000.0</td>
      <td>1.0</td>
      <td>5000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Software</th>
      <td>10000.0</td>
      <td>1.0</td>
      <td>10000.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Wendy Yule</th>
      <th>CPU</th>
      <td>165000.0</td>
      <td>7.0</td>
      <td>82500.000000</td>
      <td>3.500000</td>
    </tr>
    <tr>
      <th>Maintenance</th>
      <td>7000.0</td>
      <td>3.0</td>
      <td>7000.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <th>Monitor</th>
      <td>5000.0</td>
      <td>2.0</td>
      <td>5000.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>All</th>
      <th></th>
      <th></th>
      <td>522000.0</td>
      <td>30.0</td>
      <td>30705.882353</td>
      <td>1.764706</td>
    </tr>
  </tbody>
</table>
</div>



Let's move the analysis up a level and look at our pipeline at the manager level. Notice how the `Status` is ordered based on our earlier category definition.


```python
pd.pivot_table(df,index=["Manager","Status"],values=["Price"],
               aggfunc=[np.sum],fill_value=0,margins=True)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th>sum</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>Price</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Status</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Debra Henley</th>
      <th>won</th>
      <td>65000.0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>50000.0</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>70000.0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Fred Anderson</th>
      <th>won</th>
      <td>172000.0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>5000.0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>45000.0</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>65000.0</td>
    </tr>
    <tr>
      <th>All</th>
      <th></th>
      <td>522000.0</td>
    </tr>
  </tbody>
</table>
</div>



A really handy feature is the ability to pass a `dictionary` to the `aggfunc` so you can perform different functions on each of the values you select. 


```python
pd.pivot_table(df,index=["Manager","Status"],columns=["Product"],values=["Quantity","Price"],
               aggfunc={"Quantity":len,"Price":np.sum},fill_value=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">Price</th>
      <th colspan="4" halign="left">Quantity</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Status</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Debra Henley</th>
      <th>won</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>40000</td>
      <td>10000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>30000</td>
      <td>0</td>
      <td>0</td>
      <td>20000</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>70000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Fred Anderson</th>
      <th>won</th>
      <td>165000</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>0</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>30000</td>
      <td>0</td>
      <td>5000</td>
      <td>10000</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



You can provide a list of `aggfunctions` to apply to each value too:


```python
table = pd.pivot_table(df,index=["Manager","Status"],columns=["Product"],values=["Quantity","Price"],
               aggfunc={"Quantity":len,"Price":[np.sum,np.mean]},fill_value=0)
table
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="8" halign="left">Price</th>
      <th colspan="4" halign="left">Quantity</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">mean</th>
      <th colspan="4" halign="left">sum</th>
      <th colspan="4" halign="left">len</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Status</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Debra Henley</th>
      <th>won</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>40000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>40000</td>
      <td>10000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>30000</td>
      <td>0</td>
      <td>0</td>
      <td>10000</td>
      <td>30000</td>
      <td>0</td>
      <td>0</td>
      <td>20000</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>35000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>70000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Fred Anderson</th>
      <th>won</th>
      <td>82500</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>165000</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>0</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>30000</td>
      <td>0</td>
      <td>5000</td>
      <td>10000</td>
      <td>30000</td>
      <td>0</td>
      <td>5000</td>
      <td>10000</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



It can look daunting to try to pull this all together at once but as soon as you start playing with the data and slowly add the items, you can get a clear understanding for how it works.

#  Pivot Table Filtering

Once you have generated your data, it is in a `DataFrame` so you can filter on it using your normal DataFrame functions.


```python
table.query('Manager == ["Debra Henley"]')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="8" halign="left">Price</th>
      <th colspan="4" halign="left">Quantity</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">mean</th>
      <th colspan="4" halign="left">sum</th>
      <th colspan="4" halign="left">len</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Status</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Debra Henley</th>
      <th>won</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>40000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>40000</td>
      <td>10000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>presented</th>
      <td>30000</td>
      <td>0</td>
      <td>0</td>
      <td>10000</td>
      <td>30000</td>
      <td>0</td>
      <td>0</td>
      <td>20000</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>declined</th>
      <td>35000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>70000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
table.query('Status == ["pending","won"]')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="8" halign="left">Price</th>
      <th colspan="4" halign="left">Quantity</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th colspan="4" halign="left">mean</th>
      <th colspan="4" halign="left">sum</th>
      <th colspan="4" halign="left">len</th>
    </tr>
    <tr>
      <th></th>
      <th>Product</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
      <th>CPU</th>
      <th>Maintenance</th>
      <th>Monitor</th>
      <th>Software</th>
    </tr>
    <tr>
      <th>Manager</th>
      <th>Status</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">Debra Henley</th>
      <th>won</th>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>65000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>40000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>40000</td>
      <td>10000</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">Fred Anderson</th>
      <th>won</th>
      <td>82500</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>165000</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>pending</th>
      <td>0</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



This is a brief walk-through on how to use **pivot tables** on your data sets.
