## INTRODUCTION

This Online Retail II data set contains all the transactions occurring for a UK-based and registered, non-store online retail between 01/12/2009 and 09/12/2011. The company mainly sells unique all-occasion giftware. Many customers of the company are wholesalers. A real-time online retail transaction data set of two years.

![image.png](9445a1f0-a2e3-480a-8f88-1c8a24b115b8.png)

  ## Data Source and Description

This dataset was downloaded from Kaggle (https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) an open-source data downloaded in .csv format (Comma Separated Values). IT has columns for;

· **InvoiceNo**: Invoice number. Nominal. A 6-digit integral number uniquely assigned to each transaction. If this code starts with the letter ‘c’, it indicates a cancellation.

· **StockCode**: Product (item) code. Nominal. A 5-digit integral number uniquely assigned to each distinct product.

· **Description**: Product (item) name. Nominal.

· **Quantity**: The quantities of each product (item) per transaction. Numeric.

· **InvoiceDate**: Invoice date and time. Numeric. The day and time when a transaction was generated.

· **UnitPrice**: Unit price. Numeric. Product price per unit in sterling (Â£).

· **CustomerID**: Customer number. Nominal. A 5-digit integral number uniquely assigned to each customer.

· **Country**: Country name. Nominal. The name of the country where a customer resides.



## Problem Statement

1. Can you identify any seasonal trends in sales analysis? Were there any possible reasons behind the trends or patterns in sales?

2. What were the top-selling products with their quantities?

3. What can you deduce from customer analysis?



## Skills and Concepts Demonstrated

-Understanding the Dataset
-Load the Dataset
-Data Cleaning and Preprocessing
-Exploratory Data Analysis (EDA)
-Visualization
-Derive Insights
    



## PROJECT STRATEGY

Jupyter Notebook was used to carry out my analysis throughout this project. After downloading the dataset from (https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci), I studied the dataset to know the necessary Python libraries I’ll be making use of and I imported a retail outlet picture and the necessary libraries.

**Import Python Libraries**


```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
%matplotlib inline
```

**Install Openpyxl package**


```python
pip install openpyxl

```

    Requirement already satisfied: openpyxl in c:\users\user\anaconda3\lib\site-packages (3.0.10)
    Requirement already satisfied: et_xmlfile in c:\users\user\anaconda3\lib\site-packages (from openpyxl) (1.1.0)
    Note: you may need to restart the kernel to use updated packages.
    

**Load Online Retail || Dataset**


```python
# Loading the dataset
data = pd.read_excel("online_retail_II.xlsx", sheet_name='Year 2009-2010')

# Inspecting the dataset
data.head()
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 525461 entries, 0 to 525460
    Data columns (total 8 columns):
     #   Column       Non-Null Count   Dtype         
    ---  ------       --------------   -----         
     0   Invoice      525461 non-null  object        
     1   StockCode    525461 non-null  object        
     2   Description  522533 non-null  object        
     3   Quantity     525461 non-null  int64         
     4   InvoiceDate  525461 non-null  datetime64[ns]
     5   Price        525461 non-null  float64       
     6   Customer ID  417534 non-null  float64       
     7   Country      525461 non-null  object        
    dtypes: datetime64[ns](1), float64(2), int64(1), object(4)
    memory usage: 32.1+ MB
    

**Data Cleaning and Preprocessing**


```python
# Handling missing values

# Filling rows with missing values
data['Description'] = data['Description'].fillna('Unknown Product')

# Dropping rows with missing values
data = data.dropna(subset=['Customer ID'])

print(data.isnull().sum())
```

    Invoice        0
    StockCode      0
    Description    0
    Quantity       0
    InvoiceDate    0
    Price          0
    Customer ID    0
    Country        0
    dtype: int64
    


```python
# Removing Duplicates
data = data.drop_duplicates()
```


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 410763 entries, 0 to 525460
    Data columns (total 8 columns):
     #   Column       Non-Null Count   Dtype         
    ---  ------       --------------   -----         
     0   Invoice      410763 non-null  object        
     1   StockCode    410763 non-null  object        
     2   Description  410763 non-null  object        
     3   Quantity     410763 non-null  int64         
     4   InvoiceDate  410763 non-null  datetime64[ns]
     5   Price        410763 non-null  float64       
     6   Customer ID  410763 non-null  float64       
     7   Country      410763 non-null  object        
    dtypes: datetime64[ns](1), float64(2), int64(1), object(4)
    memory usage: 28.2+ MB
    


```python
 # Handling Outliers
# print(data.describe())
data = data[(data['Quantity'] > 0) & (data['Price']  > 0)]
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Invoice</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>Price</th>
      <th>Customer ID</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>489434</td>
      <td>85048</td>
      <td>15CM CHRISTMAS GLASS BALL 20 LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.95</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>1</th>
      <td>489434</td>
      <td>79323P</td>
      <td>PINK CHERRY LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.75</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>2</th>
      <td>489434</td>
      <td>79323W</td>
      <td>WHITE CHERRY LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.75</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>3</th>
      <td>489434</td>
      <td>22041</td>
      <td>RECORD FRAME 7" SINGLE SIZE</td>
      <td>48</td>
      <td>2009-12-01 07:45:00</td>
      <td>2.10</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>4</th>
      <td>489434</td>
      <td>21232</td>
      <td>STRAWBERRY CERAMIC TRINKET BOX</td>
      <td>24</td>
      <td>2009-12-01 07:45:00</td>
      <td>1.25</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
    </tr>
  </tbody>
</table>
</div>



**Exploratory Data Analysis (EDA)**


```python
# Total Sales per Invoice
data['Total Sales'] = data['Quantity'] * data['Price']
total_sales = data.groupby('Invoice')['Total Sales'].sum()
```


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Invoice</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>Price</th>
      <th>Customer ID</th>
      <th>Country</th>
      <th>Total Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>489434</td>
      <td>85048</td>
      <td>15CM CHRISTMAS GLASS BALL 20 LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.95</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
      <td>83.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>489434</td>
      <td>79323P</td>
      <td>PINK CHERRY LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.75</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
      <td>81.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>489434</td>
      <td>79323W</td>
      <td>WHITE CHERRY LIGHTS</td>
      <td>12</td>
      <td>2009-12-01 07:45:00</td>
      <td>6.75</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
      <td>81.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>489434</td>
      <td>22041</td>
      <td>RECORD FRAME 7" SINGLE SIZE</td>
      <td>48</td>
      <td>2009-12-01 07:45:00</td>
      <td>2.10</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
      <td>100.8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>489434</td>
      <td>21232</td>
      <td>STRAWBERRY CERAMIC TRINKET BOX</td>
      <td>24</td>
      <td>2009-12-01 07:45:00</td>
      <td>1.25</td>
      <td>13085.0</td>
      <td>United Kingdom</td>
      <td>30.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>Price</th>
      <th>Customer ID</th>
      <th>Total Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>400916.000000</td>
      <td>400916</td>
      <td>400916.000000</td>
      <td>400916.000000</td>
      <td>400916.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>13.767418</td>
      <td>2010-07-01 05:01:16.167027712</td>
      <td>3.305826</td>
      <td>15361.544074</td>
      <td>21.945330</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>2009-12-01 07:45:00</td>
      <td>0.001000</td>
      <td>12346.000000</td>
      <td>0.001000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>2010-03-26 13:28:00</td>
      <td>1.250000</td>
      <td>13985.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5.000000</td>
      <td>2010-07-09 10:26:00</td>
      <td>1.950000</td>
      <td>15311.000000</td>
      <td>12.500000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>12.000000</td>
      <td>2010-10-14 13:58:45</td>
      <td>3.750000</td>
      <td>16805.000000</td>
      <td>19.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>19152.000000</td>
      <td>2010-12-09 20:01:00</td>
      <td>10953.500000</td>
      <td>18287.000000</td>
      <td>15818.400000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>97.638385</td>
      <td>NaN</td>
      <td>35.047719</td>
      <td>1680.635823</td>
      <td>77.758075</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Distribution of Total Sales
sns.histplot(total_sales, kde=True)
plt.title('Distribution of Total Sales per Invoice')
plt.show()
```

    C:\Users\User\anaconda3\Lib\site-packages\seaborn\_oldcore.py:1119: FutureWarning: use_inf_as_na option is deprecated and will be removed in a future version. Convert inf values to NaN before operating instead.
      with pd.option_context('mode.use_inf_as_na', True):
    


    
![png](output_21_1.png)
    



```python
# Derive KPIs
total_sales = data['Total Sales'].sum()
num_invoices = data['Invoice'].nunique()


average_revenue_per_invoice = total_sales / num_invoices
median_sales_per_invoice = data.groupby('Invoice')['Total Sales'].sum().median()
high_value_threshold = data.groupby('Invoice')['Total Sales'].sum().quantile(0.95)  # Top 5%
sales_variability = data.groupby('Invoice')['Total Sales'].sum().std()
low_value_invoices = (data.groupby('Invoice')['Total Sales'].sum() < 50).mean() * 100


print(f"Average Revenue per Invoice: ${average_revenue_per_invoice:.2f}")
print(f"Median Sales per Invoice: ${median_sales_per_invoice:.2f}")
print(f"High-Value Transaction Threshold (Top 5%): ${high_value_threshold:.2f}")
print(f"Revenue Variability (Standard Deviation): ${sales_variability:.2f}")
print(f"Percentage of Low-Value Invoices: {low_value_invoices:.2f}%")

```

    Average Revenue per Invoice: $457.93
    Median Sales per Invoice: $302.90
    High-Value Transaction Threshold (Top 5%): $1230.25
    Revenue Variability (Standard Deviation): $930.92
    Percentage of Low-Value Invoices: 6.96%
    


```python
# Top 10 Selling Products
top_selling_products = data.groupby('Description')[['Quantity','Total Sales']].sum().sort_values(by='Total Sales',ascending=False).head(10)
top_selling_products
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quantity</th>
      <th>Total Sales</th>
    </tr>
    <tr>
      <th>Description</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>WHITE HANGING HEART T-LIGHT HOLDER</th>
      <td>56814</td>
      <td>151339.16</td>
    </tr>
    <tr>
      <th>REGENCY CAKESTAND 3 TIER</th>
      <td>12484</td>
      <td>143727.60</td>
    </tr>
    <tr>
      <th>Manual</th>
      <td>2568</td>
      <td>98531.99</td>
    </tr>
    <tr>
      <th>ASSORTED COLOUR BIRD ORNAMENT</th>
      <td>44431</td>
      <td>70291.03</td>
    </tr>
    <tr>
      <th>JUMBO BAG RED RETROSPOT</th>
      <td>29519</td>
      <td>51644.25</td>
    </tr>
    <tr>
      <th>POSTAGE</th>
      <td>2212</td>
      <td>48741.08</td>
    </tr>
    <tr>
      <th>ROTATING SILVER ANGELS T-LIGHT HLDR</th>
      <td>21579</td>
      <td>40156.05</td>
    </tr>
    <tr>
      <th>PAPER CHAIN KIT 50'S CHRISTMAS</th>
      <td>13839</td>
      <td>36871.55</td>
    </tr>
    <tr>
      <th>PARTY BUNTING</th>
      <td>8312</td>
      <td>35017.30</td>
    </tr>
    <tr>
      <th>EDWARDIAN PARASOL NATURAL</th>
      <td>7201</td>
      <td>34044.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
 # Time Series Analysis
data['InvoiceDate'] = pd.to_datetime(data['InvoiceDate'])
data['Month'] = data['InvoiceDate'].dt.to_period('M')
monthly_sales = data.groupby('Month')['Total Sales'].sum()

monthly_sales.plot(kind='line', title='Monthly Sales Over Time')
plt.ylabel('Total Sales')
plt.show();
```


    
![png](output_24_0.png)
    



```python
# Customer Segmentation using RFM
import datetime as dt

current_date = data['InvoiceDate'].max() + pd.Timedelta(days=1)
rfm = data.groupby('Customer ID').agg({
    'InvoiceDate': lambda x: (current_date - x.max()).days,
    'Invoice': 'nunique',
    'Total Sales': 'sum'
}).rename(columns={'InvoiceDate': 'Recency', 'Invoice': 'Frequency', 'Total Sales': 'Monetary'})
rfm.describe()


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Recency</th>
      <th>Frequency</th>
      <th>Monetary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4312.000000</td>
      <td>4312.000000</td>
      <td>4312.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>91.171846</td>
      <td>4.455705</td>
      <td>2040.406712</td>
    </tr>
    <tr>
      <th>std</th>
      <td>96.860633</td>
      <td>8.170213</td>
      <td>8911.755977</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>2.950000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>18.000000</td>
      <td>1.000000</td>
      <td>307.187500</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>53.000000</td>
      <td>2.000000</td>
      <td>701.615000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>136.000000</td>
      <td>5.000000</td>
      <td>1714.932500</td>
    </tr>
    <tr>
      <th>max</th>
      <td>374.000000</td>
      <td>205.000000</td>
      <td>349164.350000</td>
    </tr>
  </tbody>
</table>
</div>



**Visualization**


```python
# Country-based Distribution
country_sales = data.groupby('Country')['Total Sales'].sum().sort_values(ascending=False)
plt.figure(figsize=(10,10))
sns.barplot(x=country_sales.values, y=country_sales.index)
plt.title('Total Sales by Country')
plt.show()
```


    
![png](output_27_0.png)
    



```python
# Time Series chart
data['InvoiceDate'] = pd.to_datetime(data['InvoiceDate'])
data['Month'] = data['InvoiceDate'].dt.to_period('M')
monthly_sales = data.groupby('Month')['Total Sales'].sum()

# Convert Period index to string 
monthly_sales.index = monthly_sales.index.astype(str)


fig = px.line(x=monthly_sales.index, y=monthly_sales.values, title='Monthly Sales Over Time')
fig.update_layout(xaxis_title='Month', yaxis_title='Total Sales')
fig.show()

```


<div>                            <div id="6e3de916-db38-4556-9417-580bf2bd797e" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("6e3de916-db38-4556-9417-580bf2bd797e")) {                    Plotly.newPlot(                        "6e3de916-db38-4556-9417-580bf2bd797e",                        [{"hovertemplate":"x=%{x}<br>y=%{y}<extra></extra>","legendgroup":"","line":{"color":"#636efa","dash":"solid"},"marker":{"symbol":"circle"},"mode":"lines","name":"","orientation":"v","showlegend":false,"x":["2009-12","2010-01","2010-02","2010-03","2010-04","2010-05","2010-06","2010-07","2010-08","2010-09","2010-10","2010-11","2010-12"],"xaxis":"x","y":[683504.01,555802.672,504558.956,696978.471,591982.002,597833.38,636371.13,589736.17,602224.6,829013.951,1033112.01,1166460.022,310656.37],"yaxis":"y","type":"scatter"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Month"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Total Sales"}},"legend":{"tracegroupgap":0},"title":{"text":"Monthly Sales Over Time"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('6e3de916-db38-4556-9417-580bf2bd797e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# Customer Segmentation
fig = px.scatter(
                rfm, x='Recency', 
                y='Monetary',
                size='Frequency',
                color='Frequency',
                title='Customer Segmentation: RFM Analysis'
 )
fig.show()
```


<div>                            <div id="e2879edc-ff4d-4b8f-8220-4571925eca23" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e2879edc-ff4d-4b8f-8220-4571925eca23")) {                    Plotly.newPlot(                        "e2879edc-ff4d-4b8f-8220-4571925eca23",                        [{"hovertemplate":"Recency=%{x}<br>Monetary=%{y}<br>Frequency=%{marker.color}<extra></extra>","legendgroup":"","marker":{"color":[11,2,1,3,1,2,1,1,3,2,3,6,5,3,1,1,1,3,3,3,2,3,2,2,1,2,7,1,1,1,2,3,5,1,2,4,1,3,1,5,1,1,3,3,4,1,1,3,1,3,7,1,11,3,2,2,14,2,2,1,1,1,5,13,5,1,4,21,2,2,1,1,1,2,3,3,4,4,1,2,1,1,1,1,3,2,1,49,13,13,1,12,3,3,6,29,11,6,1,1,1,11,1,3,1,2,2,1,8,1,2,1,1,7,1,1,2,1,12,9,3,2,1,5,1,2,1,1,3,16,2,1,1,6,2,1,1,4,2,1,7,1,1,4,2,3,5,2,1,2,13,3,3,6,2,3,5,1,3,6,12,1,4,1,1,6,5,4,1,1,16,6,2,2,5,2,3,2,2,1,4,1,2,6,3,4,27,3,5,3,1,1,1,3,1,10,2,2,2,1,6,2,1,1,1,2,5,2,9,24,23,8,1,2,1,1,3,1,3,2,1,1,4,3,5,9,20,2,10,7,1,6,2,3,4,4,3,1,1,5,3,2,2,1,2,2,1,4,3,1,16,144,4,1,10,5,5,1,2,1,6,3,1,1,1,1,1,1,2,1,1,3,7,1,6,1,2,1,1,4,2,1,6,1,2,1,2,1,1,3,1,6,1,2,4,4,1,3,2,2,2,3,2,1,7,1,13,2,6,1,2,41,11,1,9,13,3,17,2,22,2,1,1,1,4,1,1,5,4,4,4,1,1,1,1,2,5,1,2,2,6,3,1,3,1,11,2,2,3,2,8,2,1,4,1,8,1,1,2,2,1,1,1,2,2,3,3,5,1,9,1,3,3,5,1,3,39,2,8,1,4,1,7,42,8,5,1,8,5,12,1,1,2,1,1,1,2,14,9,1,1,3,5,2,1,4,1,5,1,2,1,6,33,4,1,1,6,7,3,5,1,12,7,1,10,3,18,4,1,1,4,1,4,3,1,20,2,18,2,1,2,1,2,1,3,18,1,6,7,6,2,8,1,1,2,1,4,13,2,1,5,7,1,2,1,1,9,4,4,3,10,2,1,17,5,2,19,2,6,1,1,1,3,9,1,2,5,2,16,3,1,2,4,1,8,32,1,3,19,12,1,1,6,1,1,1,109,13,2,7,48,2,1,18,3,14,2,3,1,1,5,2,1,5,1,1,14,1,6,12,4,1,2,1,1,7,1,2,4,1,2,1,8,8,2,1,3,12,1,1,4,14,3,2,7,3,3,3,5,1,1,12,1,2,1,1,1,1,1,1,10,10,2,1,10,1,1,1,6,3,3,1,4,1,1,3,1,2,30,4,7,6,1,1,13,2,3,5,7,6,7,1,1,1,1,1,4,9,1,17,2,2,1,2,4,5,1,4,4,1,5,6,2,1,2,2,1,2,1,2,8,2,4,1,7,18,14,1,1,2,3,3,3,4,1,1,2,2,1,3,5,1,1,10,3,6,1,3,5,3,2,2,1,1,3,7,1,2,1,1,2,1,7,1,6,13,34,2,8,1,14,7,3,1,3,2,1,3,5,6,4,1,4,2,3,1,1,2,1,1,2,3,2,1,2,1,6,4,1,4,2,1,1,1,4,11,2,5,1,3,1,3,2,3,2,3,46,4,1,6,3,4,1,4,15,6,1,1,2,12,3,2,6,2,4,3,2,1,2,7,1,3,6,2,1,11,9,7,6,1,1,1,5,2,2,2,3,4,5,2,4,38,1,2,6,2,3,5,5,2,2,6,1,2,10,2,11,1,18,6,5,1,1,2,2,5,4,1,1,7,1,3,2,3,2,1,1,6,2,2,5,1,5,10,1,2,5,4,3,2,17,2,4,2,2,1,1,1,1,1,10,1,7,9,2,11,2,10,5,35,1,3,4,3,14,4,1,11,7,1,4,1,1,2,1,2,3,1,3,4,12,4,9,1,3,2,4,14,1,2,3,1,2,1,1,1,7,7,1,5,2,7,2,5,2,1,1,5,1,1,1,2,1,8,3,11,3,1,1,5,3,7,2,4,8,5,4,1,3,1,5,4,2,4,10,1,1,8,4,3,2,14,1,1,1,1,2,2,3,8,1,3,2,1,1,1,1,2,9,94,1,3,2,1,3,1,1,1,5,1,3,5,2,7,1,3,1,13,2,6,1,7,5,1,6,10,3,2,4,9,5,1,1,4,1,1,2,5,6,1,3,1,10,1,10,3,4,4,2,3,1,1,39,2,11,4,5,5,2,1,2,37,3,1,1,2,7,4,3,4,6,4,1,1,1,1,1,4,1,3,54,7,2,16,2,2,3,2,4,1,4,1,3,3,4,5,7,2,1,1,13,1,1,2,3,2,7,1,1,10,1,1,1,6,3,5,1,15,4,8,6,1,2,1,3,1,6,7,4,3,5,6,9,2,3,1,5,23,10,5,5,5,4,3,2,2,1,2,1,3,10,5,1,1,1,1,2,11,1,1,1,4,2,4,4,2,1,9,9,3,2,1,4,11,1,1,1,2,3,3,12,5,1,3,8,3,1,1,14,2,3,1,1,1,18,2,6,1,6,3,1,6,4,1,6,10,8,7,2,2,3,2,10,9,2,3,3,17,4,7,1,4,4,1,1,1,2,2,2,2,7,1,1,2,4,1,1,8,2,3,14,27,6,1,4,4,2,1,23,1,2,6,13,10,2,9,15,1,2,2,5,2,13,3,12,9,10,3,3,2,1,1,1,3,1,4,1,4,9,1,3,1,16,1,2,3,2,10,1,1,2,9,7,3,4,1,4,2,21,6,12,5,2,1,2,3,1,1,3,1,4,3,1,1,15,11,5,1,3,2,1,3,7,7,1,2,5,1,6,102,3,9,7,5,1,2,4,2,2,3,1,3,2,5,2,9,9,2,4,2,5,1,2,1,2,1,1,15,4,1,5,3,1,3,3,1,3,7,2,1,1,9,11,9,1,1,4,1,7,2,2,8,5,4,1,5,3,4,3,1,3,4,4,2,2,10,1,1,8,1,4,2,12,1,2,1,1,3,4,16,3,1,4,2,1,1,1,1,2,1,6,4,6,1,2,1,11,4,1,4,4,3,12,7,6,1,4,2,38,19,3,3,2,2,4,5,2,2,1,1,5,1,2,5,1,1,3,2,4,2,1,2,5,1,8,1,2,4,1,1,10,2,4,14,3,1,2,2,3,2,1,2,2,1,2,6,2,1,2,1,1,2,1,1,1,1,9,3,5,1,1,1,12,6,2,8,2,9,1,2,8,1,4,5,2,1,3,1,10,1,1,3,4,13,2,3,7,2,1,3,3,1,1,1,5,4,9,7,5,2,1,1,1,1,5,4,10,2,5,4,1,1,6,10,1,1,2,8,7,8,1,4,1,2,1,1,1,3,4,3,3,3,6,1,3,2,2,3,2,1,7,5,2,1,6,1,5,4,11,3,5,3,1,3,5,3,6,6,1,2,1,4,4,8,1,69,10,1,7,4,8,6,1,3,8,6,23,1,1,6,7,3,2,5,2,4,4,3,3,1,22,8,2,2,1,3,1,3,1,1,4,5,1,1,1,3,1,1,1,3,4,4,1,1,20,9,1,2,1,4,1,2,1,1,1,4,102,8,7,6,2,2,1,1,1,5,2,2,4,1,4,5,4,9,7,1,3,1,3,4,1,1,3,4,3,5,1,78,1,15,5,1,7,5,1,3,3,2,3,2,2,31,1,4,1,2,2,3,1,1,6,1,1,3,43,3,4,1,1,8,5,2,8,2,8,6,1,3,2,3,2,6,13,2,1,2,3,12,9,1,11,3,4,1,1,1,1,2,7,1,3,1,2,1,3,11,4,4,24,2,1,2,19,8,2,1,1,8,7,4,7,1,1,2,1,4,14,2,3,1,1,6,1,9,6,2,1,1,1,4,1,1,1,1,1,1,6,1,1,3,11,1,5,1,6,2,3,1,2,2,3,1,1,4,1,5,3,2,5,7,1,6,13,2,10,4,3,1,1,1,2,5,2,3,1,8,1,6,1,1,2,1,21,6,1,12,3,4,5,5,1,1,9,3,8,13,4,4,5,5,3,1,1,1,2,2,16,1,12,1,2,2,2,1,1,2,18,7,4,9,1,1,5,2,2,6,1,10,1,3,205,4,4,9,4,2,7,1,1,2,2,2,1,1,1,3,2,4,3,1,4,2,2,1,4,1,7,1,4,4,21,3,1,1,1,1,3,13,3,7,5,5,1,1,8,1,1,1,5,1,1,1,1,1,4,2,7,6,1,2,1,1,1,5,2,1,2,1,2,6,6,29,1,2,2,2,10,13,4,1,1,4,1,10,8,1,2,6,3,1,6,10,7,6,2,4,47,1,1,1,5,18,10,1,2,6,3,1,3,5,10,86,4,4,1,2,2,3,2,1,5,2,2,2,23,8,2,8,1,2,1,2,2,1,18,2,6,2,6,4,2,2,4,2,2,3,1,2,1,3,8,3,2,9,2,1,1,12,1,2,2,1,2,1,1,7,5,1,12,4,3,2,1,2,1,2,1,4,3,1,1,6,1,1,4,2,1,7,1,1,1,8,3,2,9,3,2,3,1,5,1,3,3,14,2,3,3,6,1,2,3,9,3,1,1,2,1,1,4,1,1,11,1,4,10,3,3,4,2,1,1,1,6,7,3,1,1,1,5,1,2,3,5,1,10,3,3,2,8,1,1,4,20,3,2,1,2,4,3,2,14,10,4,1,3,1,4,4,2,7,1,3,1,1,3,6,2,8,26,1,1,1,6,12,6,6,1,2,2,1,6,4,1,121,8,2,1,9,1,3,1,7,4,1,8,3,6,1,1,1,5,1,1,2,4,2,4,1,6,3,4,22,8,3,2,2,6,5,7,1,11,1,1,7,6,1,1,4,2,1,7,3,6,2,2,1,4,2,1,3,3,7,1,3,2,4,2,1,1,2,1,2,5,3,2,6,6,5,2,1,4,5,3,2,1,1,6,1,4,3,7,1,3,4,1,2,3,1,1,7,1,2,2,1,15,3,1,1,1,2,1,1,4,1,1,1,4,1,7,6,2,5,1,1,4,5,1,4,3,1,1,7,19,1,2,5,1,3,3,1,2,18,7,1,7,2,8,15,1,2,15,2,1,6,1,2,1,6,1,3,1,12,1,2,12,1,5,8,6,1,22,1,1,2,4,2,3,2,2,5,14,10,3,3,3,1,2,3,8,26,1,2,3,3,1,5,3,1,5,1,1,1,4,17,8,1,5,8,8,1,13,1,4,24,1,3,1,8,7,5,1,2,9,4,1,1,13,1,1,2,4,6,2,15,8,1,2,3,1,3,2,2,4,4,2,7,6,2,3,3,2,1,1,5,8,1,2,2,8,9,2,1,3,1,1,1,1,3,1,12,2,5,1,1,2,2,3,1,1,2,7,3,3,5,8,4,1,18,1,6,3,1,1,8,2,2,2,1,9,2,1,2,26,2,1,6,2,12,11,3,6,11,4,4,2,1,2,1,2,31,21,1,3,1,1,5,1,3,1,2,1,2,11,2,1,5,3,2,5,2,7,9,2,3,1,5,3,13,2,18,2,4,2,7,2,2,3,1,6,4,3,2,1,16,1,1,3,1,16,5,2,3,1,3,1,4,1,1,9,1,26,1,9,1,10,4,10,3,1,2,17,1,1,3,1,2,4,14,1,2,2,4,1,6,2,12,3,3,2,7,5,1,1,1,22,1,3,4,1,5,1,3,2,7,8,7,1,2,1,7,4,1,1,5,3,1,1,1,1,1,1,1,1,2,2,4,1,1,2,3,4,3,1,6,1,11,1,9,2,2,3,3,4,1,1,1,10,1,10,1,2,1,6,5,3,4,2,1,4,13,1,6,21,5,1,2,2,2,5,1,2,2,1,2,11,1,1,1,5,5,6,1,3,12,8,14,2,39,2,4,4,2,18,2,2,9,1,47,6,8,3,1,2,2,2,4,3,4,1,4,1,1,4,3,2,1,4,3,3,1,7,3,1,4,1,8,3,1,1,3,3,1,2,1,1,2,4,1,2,7,6,5,2,2,1,3,7,1,3,8,4,1,3,16,8,4,3,1,1,1,4,2,1,6,2,3,3,3,14,11,1,6,3,4,6,2,23,3,1,1,1,3,2,2,1,7,8,1,4,1,1,6,6,5,11,9,3,25,5,1,2,5,40,14,3,4,3,4,7,1,5,5,3,1,15,1,7,11,1,1,1,1,1,9,1,7,10,1,8,3,2,3,9,8,4,19,2,2,1,7,1,7,1,2,1,10,2,2,3,4,4,3,7,3,4,1,1,4,4,7,1,2,2,1,1,5,3,8,2,10,3,2,1,1,2,3,7,1,1,5,2,1,2,2,1,2,6,1,2,2,3,1,1,2,4,1,1,4,1,4,6,1,4,4,1,3,1,4,1,2,3,3,2,1,3,1,5,2,4,2,1,9,10,11,1,4,4,6,4,1,7,4,2,3,1,3,1,1,9,1,5,12,16,3,1,3,1,1,7,1,7,3,1,1,4,1,3,2,3,2,1,2,2,5,2,3,1,4,1,3,4,2,11,9,1,1,1,8,11,1,2,4,1,7,1,3,4,1,3,2,6,1,2,2,1,3,59,1,3,6,1,5,3,3,2,1,1,3,2,1,13,3,2,1,2,2,2,2,2,6,9,12,1,2,2,8,7,2,1,7,3,2,1,1,5,1,4,1,2,2,1,1,1,3,3,17,6,3,1,2,7,1,1,6,3,5,2,3,4,3,2,7,2,23,1,1,3,1,4,2,1,2,1,3,7,1,1,1,1,4,1,4,1,1,24,14,4,1,21,5,4,10,19,1,6,1,5,1,3,2,4,10,7,2,2,1,1,1,2,3,2,2,4,1,2,12,5,4,3,1,7,2,1,2,1,2,11,2,5,5,2,1,3,4,1,3,4,2,1,7,2,5,1,4,6,1,1,1,1,1,1,1,2,2,1,6,1,9,5,7,15,6,3,4,1,1,1,1,7,9,1,2,2,2,3,2,1,1,27,9,1,2,2,2,3,1,2,7,13,13,4,2,22,4,9,13,4,2,18,1,1,1,9,5,8,2,11,2,1,8,2,3,1,3,1,9,5,1,1,1,1,13,7,24,9,2,1,29,4,6,1,1,1,1,4,6,3,5,5,3,2,4,6,1,3,32,1,2,26,1,4,1,1,4,3,8,3,5,3,5,3,2,2,3,3,4,1,3,16,2,5,2,1,1,6,1,3,5,1,3,1,2,2,1,5,2,2,11,2,6,19,2,1,2,1,3,2,2,1,10,5,3,3,4,2,2,1,1,3,1,3,6,1,6,1,1,18,2,1,1,2,3,3,3,7,2,2,3,4,1,3,2,1,2,7,11,3,5,1,2,4,2,5,3,1,10,14,1,2,1,13,1,5,13,3,2,7,4,3,1,1,5,3,5,1,2,1,14,5,4,4,8,3,11,4,1,5,2,1,2,1,2,1,2,3,4,2,3,1,5,7,13,9,4,1,1,1,8,2,3,1,2,8,4,4,2,2,4,1,1,1,2,1,1,13,4,8,2,6,1,3,4,2,3,3,5,4,8,1,2,2,1,1,1,1,3,4,11,2,4,3,4,10,5,3,1,1,1,5,4,6,2,1,3,27,6,1,4,1,1,6,3,1,5,4,4,6,1,23,1,1,6,11,7,1,1,4,7,2,3,1,3,1,1,1,4,1,2,2,1,1,6,4,1,1,3,2,4,5,1,2,5,1,4,1,6,6,13,4,3,2,4,10,5,1,6,1,1,5,1,17,2,2,11,1,2,5,3,1,1,2,1,2,2,1,1,3,7,2,2,2,3,1,6,3,5,11,1,2,1,1,1,2,1,4,3,1,4,5,2,1,1,3,1,6,2,7,1,1,1,9,1,7,3,5,2,1,1,2,7,7,10,15,2,7,6,2,17,3,1,12,3,44,4,1,2,1,1,2,1,2,3,2,2,5,1,1,7,1,2,7,1,2,1,1,1,1,1,1,1,6,2,1,2,2,2,16,4,6,6,2,8,2,1,3,2,2,1,1,1,2,11,2,2,2,4,9,5,1,1,1,4,4,5,5,9,2,2,2,1,3,2,5,1,2,1,9,2,8,3,4,5,1,6,12,4,4,3,9,1,2,3,1,4,3,1,3,2,5,3,17,7,2,6,1,3,20,3,1,1,2,60,1,3,12,7,1,1,1,7,27,1,1,2,1,7,1,3,1,12,1,8,11,1,2,8,2,10,2,4,15,1,1,8,4,3,5,1,7,18,2,3,7,1,1,3,1,4,1,1,1,9,46,6,7,6,2,9,1,12,2,2,7,1,1,12,7,1,1,2,1,2,2,2,1,1,2,1,2,1,1,2,8,2,1,1,1,3,2,1,9,3,7,1,31,8,2,1,5,1,2,10,2,2,2,9,2,3,2,1,1,4,2,4,4,1,2,1,3,3,2,3,6,10,3,3,1,1,1,1,4,3,2,1,2,1,1,1,7,1,23,2,6,1,2,20,4,1,4,3,1,1,4,14,2,1,6,2,7,4,1,1,1,8,3,4,1,1,1,2,6,5,13,12,3,2,1,8,3,2,6,2,4,1,1,2,4,3,3,2,5,5,3,2,9,10,2,1,2,6,3,1,3,1,2,10,2,1,3,4,3,2,5,7,1,40,5,28,1,5,2,5,6,3,6,2,1,1,2,8,3,8,2,3,19,2,10,2,1,2,17,6,2,4,1,4,4,1,13,1,1,7,1,2,1,7,2,1,7,2,8,3,8,9,11,6,2,9,1,1,2,3,1,1,1,4,8,3,28,4,2,5,1,3,3,1,2,1,4,7,2,1,2,5,2,1,3,2,1,1,2,1,1,6,2,2,4,5,1,10,3,8,1,3,1,3,4,11,2,8,4,2,1,3,3,1,6,1,1,5,2,4,1,1,4,1,2,10,1,91,1,1,4,2,21,4,155,6,3,3,4,1,2,19,4,10,3,7,2,26,2,9,1,3,2,4,1,14,1,3,2,2,2,3,18,3,17,5,11,8,2,2,1,1,10,1,5,1,1,1,12,1,2,3,1,39,6,1,2,2,1,3,1,1,8,8,12,1,1,5,1,6,3,1,3,74,6,2,4,1,9,2,1,1,2,62,3,4,1,17,5,2,4,18,1,3,12,1,1,4,1,1,1,1,4,4,7,1,3,1,1,1,7,2,3,3,2,1,4,4,1,3,1,14,7,2,1,2,1,2,1,2,4,6,2,1,1,1,1,2,8,9,1,4,20,1,1,8,1,2,1,2,1,7,1,2,1,6,1,3,2,2,19,3,2,9,15,2,6,1,5,2,10,1,8,2,1,1,2,3,15,1,1,1,8,4,6,3,3,5,1,1,3,89,1,5,1,2,13,1,1,2,1,1,5,4,1,1,1,1,1,1,1,2,5,2,2,1,2,8,7,1,4,4,1,1,3,1,1,1,1,1,1,1,11,6,4,32,1,1,1,3,7,2,4,1,1,1,3,1,3,1,2,5,1,2,1,1,1,3,1,1,1,1,1,4,4,2,2,8,5,1,12,15,15,1,10,2,23,1,6,1,2,4,4,1,2,13,1,3,8,5,1,1,1,1,4,6,5,17,2,1,1,1,5,1,1,2,3,1,2,5,4,1,1,1,1,6,1,1,2,4],"coloraxis":"coloraxis","size":[11,2,1,3,1,2,1,1,3,2,3,6,5,3,1,1,1,3,3,3,2,3,2,2,1,2,7,1,1,1,2,3,5,1,2,4,1,3,1,5,1,1,3,3,4,1,1,3,1,3,7,1,11,3,2,2,14,2,2,1,1,1,5,13,5,1,4,21,2,2,1,1,1,2,3,3,4,4,1,2,1,1,1,1,3,2,1,49,13,13,1,12,3,3,6,29,11,6,1,1,1,11,1,3,1,2,2,1,8,1,2,1,1,7,1,1,2,1,12,9,3,2,1,5,1,2,1,1,3,16,2,1,1,6,2,1,1,4,2,1,7,1,1,4,2,3,5,2,1,2,13,3,3,6,2,3,5,1,3,6,12,1,4,1,1,6,5,4,1,1,16,6,2,2,5,2,3,2,2,1,4,1,2,6,3,4,27,3,5,3,1,1,1,3,1,10,2,2,2,1,6,2,1,1,1,2,5,2,9,24,23,8,1,2,1,1,3,1,3,2,1,1,4,3,5,9,20,2,10,7,1,6,2,3,4,4,3,1,1,5,3,2,2,1,2,2,1,4,3,1,16,144,4,1,10,5,5,1,2,1,6,3,1,1,1,1,1,1,2,1,1,3,7,1,6,1,2,1,1,4,2,1,6,1,2,1,2,1,1,3,1,6,1,2,4,4,1,3,2,2,2,3,2,1,7,1,13,2,6,1,2,41,11,1,9,13,3,17,2,22,2,1,1,1,4,1,1,5,4,4,4,1,1,1,1,2,5,1,2,2,6,3,1,3,1,11,2,2,3,2,8,2,1,4,1,8,1,1,2,2,1,1,1,2,2,3,3,5,1,9,1,3,3,5,1,3,39,2,8,1,4,1,7,42,8,5,1,8,5,12,1,1,2,1,1,1,2,14,9,1,1,3,5,2,1,4,1,5,1,2,1,6,33,4,1,1,6,7,3,5,1,12,7,1,10,3,18,4,1,1,4,1,4,3,1,20,2,18,2,1,2,1,2,1,3,18,1,6,7,6,2,8,1,1,2,1,4,13,2,1,5,7,1,2,1,1,9,4,4,3,10,2,1,17,5,2,19,2,6,1,1,1,3,9,1,2,5,2,16,3,1,2,4,1,8,32,1,3,19,12,1,1,6,1,1,1,109,13,2,7,48,2,1,18,3,14,2,3,1,1,5,2,1,5,1,1,14,1,6,12,4,1,2,1,1,7,1,2,4,1,2,1,8,8,2,1,3,12,1,1,4,14,3,2,7,3,3,3,5,1,1,12,1,2,1,1,1,1,1,1,10,10,2,1,10,1,1,1,6,3,3,1,4,1,1,3,1,2,30,4,7,6,1,1,13,2,3,5,7,6,7,1,1,1,1,1,4,9,1,17,2,2,1,2,4,5,1,4,4,1,5,6,2,1,2,2,1,2,1,2,8,2,4,1,7,18,14,1,1,2,3,3,3,4,1,1,2,2,1,3,5,1,1,10,3,6,1,3,5,3,2,2,1,1,3,7,1,2,1,1,2,1,7,1,6,13,34,2,8,1,14,7,3,1,3,2,1,3,5,6,4,1,4,2,3,1,1,2,1,1,2,3,2,1,2,1,6,4,1,4,2,1,1,1,4,11,2,5,1,3,1,3,2,3,2,3,46,4,1,6,3,4,1,4,15,6,1,1,2,12,3,2,6,2,4,3,2,1,2,7,1,3,6,2,1,11,9,7,6,1,1,1,5,2,2,2,3,4,5,2,4,38,1,2,6,2,3,5,5,2,2,6,1,2,10,2,11,1,18,6,5,1,1,2,2,5,4,1,1,7,1,3,2,3,2,1,1,6,2,2,5,1,5,10,1,2,5,4,3,2,17,2,4,2,2,1,1,1,1,1,10,1,7,9,2,11,2,10,5,35,1,3,4,3,14,4,1,11,7,1,4,1,1,2,1,2,3,1,3,4,12,4,9,1,3,2,4,14,1,2,3,1,2,1,1,1,7,7,1,5,2,7,2,5,2,1,1,5,1,1,1,2,1,8,3,11,3,1,1,5,3,7,2,4,8,5,4,1,3,1,5,4,2,4,10,1,1,8,4,3,2,14,1,1,1,1,2,2,3,8,1,3,2,1,1,1,1,2,9,94,1,3,2,1,3,1,1,1,5,1,3,5,2,7,1,3,1,13,2,6,1,7,5,1,6,10,3,2,4,9,5,1,1,4,1,1,2,5,6,1,3,1,10,1,10,3,4,4,2,3,1,1,39,2,11,4,5,5,2,1,2,37,3,1,1,2,7,4,3,4,6,4,1,1,1,1,1,4,1,3,54,7,2,16,2,2,3,2,4,1,4,1,3,3,4,5,7,2,1,1,13,1,1,2,3,2,7,1,1,10,1,1,1,6,3,5,1,15,4,8,6,1,2,1,3,1,6,7,4,3,5,6,9,2,3,1,5,23,10,5,5,5,4,3,2,2,1,2,1,3,10,5,1,1,1,1,2,11,1,1,1,4,2,4,4,2,1,9,9,3,2,1,4,11,1,1,1,2,3,3,12,5,1,3,8,3,1,1,14,2,3,1,1,1,18,2,6,1,6,3,1,6,4,1,6,10,8,7,2,2,3,2,10,9,2,3,3,17,4,7,1,4,4,1,1,1,2,2,2,2,7,1,1,2,4,1,1,8,2,3,14,27,6,1,4,4,2,1,23,1,2,6,13,10,2,9,15,1,2,2,5,2,13,3,12,9,10,3,3,2,1,1,1,3,1,4,1,4,9,1,3,1,16,1,2,3,2,10,1,1,2,9,7,3,4,1,4,2,21,6,12,5,2,1,2,3,1,1,3,1,4,3,1,1,15,11,5,1,3,2,1,3,7,7,1,2,5,1,6,102,3,9,7,5,1,2,4,2,2,3,1,3,2,5,2,9,9,2,4,2,5,1,2,1,2,1,1,15,4,1,5,3,1,3,3,1,3,7,2,1,1,9,11,9,1,1,4,1,7,2,2,8,5,4,1,5,3,4,3,1,3,4,4,2,2,10,1,1,8,1,4,2,12,1,2,1,1,3,4,16,3,1,4,2,1,1,1,1,2,1,6,4,6,1,2,1,11,4,1,4,4,3,12,7,6,1,4,2,38,19,3,3,2,2,4,5,2,2,1,1,5,1,2,5,1,1,3,2,4,2,1,2,5,1,8,1,2,4,1,1,10,2,4,14,3,1,2,2,3,2,1,2,2,1,2,6,2,1,2,1,1,2,1,1,1,1,9,3,5,1,1,1,12,6,2,8,2,9,1,2,8,1,4,5,2,1,3,1,10,1,1,3,4,13,2,3,7,2,1,3,3,1,1,1,5,4,9,7,5,2,1,1,1,1,5,4,10,2,5,4,1,1,6,10,1,1,2,8,7,8,1,4,1,2,1,1,1,3,4,3,3,3,6,1,3,2,2,3,2,1,7,5,2,1,6,1,5,4,11,3,5,3,1,3,5,3,6,6,1,2,1,4,4,8,1,69,10,1,7,4,8,6,1,3,8,6,23,1,1,6,7,3,2,5,2,4,4,3,3,1,22,8,2,2,1,3,1,3,1,1,4,5,1,1,1,3,1,1,1,3,4,4,1,1,20,9,1,2,1,4,1,2,1,1,1,4,102,8,7,6,2,2,1,1,1,5,2,2,4,1,4,5,4,9,7,1,3,1,3,4,1,1,3,4,3,5,1,78,1,15,5,1,7,5,1,3,3,2,3,2,2,31,1,4,1,2,2,3,1,1,6,1,1,3,43,3,4,1,1,8,5,2,8,2,8,6,1,3,2,3,2,6,13,2,1,2,3,12,9,1,11,3,4,1,1,1,1,2,7,1,3,1,2,1,3,11,4,4,24,2,1,2,19,8,2,1,1,8,7,4,7,1,1,2,1,4,14,2,3,1,1,6,1,9,6,2,1,1,1,4,1,1,1,1,1,1,6,1,1,3,11,1,5,1,6,2,3,1,2,2,3,1,1,4,1,5,3,2,5,7,1,6,13,2,10,4,3,1,1,1,2,5,2,3,1,8,1,6,1,1,2,1,21,6,1,12,3,4,5,5,1,1,9,3,8,13,4,4,5,5,3,1,1,1,2,2,16,1,12,1,2,2,2,1,1,2,18,7,4,9,1,1,5,2,2,6,1,10,1,3,205,4,4,9,4,2,7,1,1,2,2,2,1,1,1,3,2,4,3,1,4,2,2,1,4,1,7,1,4,4,21,3,1,1,1,1,3,13,3,7,5,5,1,1,8,1,1,1,5,1,1,1,1,1,4,2,7,6,1,2,1,1,1,5,2,1,2,1,2,6,6,29,1,2,2,2,10,13,4,1,1,4,1,10,8,1,2,6,3,1,6,10,7,6,2,4,47,1,1,1,5,18,10,1,2,6,3,1,3,5,10,86,4,4,1,2,2,3,2,1,5,2,2,2,23,8,2,8,1,2,1,2,2,1,18,2,6,2,6,4,2,2,4,2,2,3,1,2,1,3,8,3,2,9,2,1,1,12,1,2,2,1,2,1,1,7,5,1,12,4,3,2,1,2,1,2,1,4,3,1,1,6,1,1,4,2,1,7,1,1,1,8,3,2,9,3,2,3,1,5,1,3,3,14,2,3,3,6,1,2,3,9,3,1,1,2,1,1,4,1,1,11,1,4,10,3,3,4,2,1,1,1,6,7,3,1,1,1,5,1,2,3,5,1,10,3,3,2,8,1,1,4,20,3,2,1,2,4,3,2,14,10,4,1,3,1,4,4,2,7,1,3,1,1,3,6,2,8,26,1,1,1,6,12,6,6,1,2,2,1,6,4,1,121,8,2,1,9,1,3,1,7,4,1,8,3,6,1,1,1,5,1,1,2,4,2,4,1,6,3,4,22,8,3,2,2,6,5,7,1,11,1,1,7,6,1,1,4,2,1,7,3,6,2,2,1,4,2,1,3,3,7,1,3,2,4,2,1,1,2,1,2,5,3,2,6,6,5,2,1,4,5,3,2,1,1,6,1,4,3,7,1,3,4,1,2,3,1,1,7,1,2,2,1,15,3,1,1,1,2,1,1,4,1,1,1,4,1,7,6,2,5,1,1,4,5,1,4,3,1,1,7,19,1,2,5,1,3,3,1,2,18,7,1,7,2,8,15,1,2,15,2,1,6,1,2,1,6,1,3,1,12,1,2,12,1,5,8,6,1,22,1,1,2,4,2,3,2,2,5,14,10,3,3,3,1,2,3,8,26,1,2,3,3,1,5,3,1,5,1,1,1,4,17,8,1,5,8,8,1,13,1,4,24,1,3,1,8,7,5,1,2,9,4,1,1,13,1,1,2,4,6,2,15,8,1,2,3,1,3,2,2,4,4,2,7,6,2,3,3,2,1,1,5,8,1,2,2,8,9,2,1,3,1,1,1,1,3,1,12,2,5,1,1,2,2,3,1,1,2,7,3,3,5,8,4,1,18,1,6,3,1,1,8,2,2,2,1,9,2,1,2,26,2,1,6,2,12,11,3,6,11,4,4,2,1,2,1,2,31,21,1,3,1,1,5,1,3,1,2,1,2,11,2,1,5,3,2,5,2,7,9,2,3,1,5,3,13,2,18,2,4,2,7,2,2,3,1,6,4,3,2,1,16,1,1,3,1,16,5,2,3,1,3,1,4,1,1,9,1,26,1,9,1,10,4,10,3,1,2,17,1,1,3,1,2,4,14,1,2,2,4,1,6,2,12,3,3,2,7,5,1,1,1,22,1,3,4,1,5,1,3,2,7,8,7,1,2,1,7,4,1,1,5,3,1,1,1,1,1,1,1,1,2,2,4,1,1,2,3,4,3,1,6,1,11,1,9,2,2,3,3,4,1,1,1,10,1,10,1,2,1,6,5,3,4,2,1,4,13,1,6,21,5,1,2,2,2,5,1,2,2,1,2,11,1,1,1,5,5,6,1,3,12,8,14,2,39,2,4,4,2,18,2,2,9,1,47,6,8,3,1,2,2,2,4,3,4,1,4,1,1,4,3,2,1,4,3,3,1,7,3,1,4,1,8,3,1,1,3,3,1,2,1,1,2,4,1,2,7,6,5,2,2,1,3,7,1,3,8,4,1,3,16,8,4,3,1,1,1,4,2,1,6,2,3,3,3,14,11,1,6,3,4,6,2,23,3,1,1,1,3,2,2,1,7,8,1,4,1,1,6,6,5,11,9,3,25,5,1,2,5,40,14,3,4,3,4,7,1,5,5,3,1,15,1,7,11,1,1,1,1,1,9,1,7,10,1,8,3,2,3,9,8,4,19,2,2,1,7,1,7,1,2,1,10,2,2,3,4,4,3,7,3,4,1,1,4,4,7,1,2,2,1,1,5,3,8,2,10,3,2,1,1,2,3,7,1,1,5,2,1,2,2,1,2,6,1,2,2,3,1,1,2,4,1,1,4,1,4,6,1,4,4,1,3,1,4,1,2,3,3,2,1,3,1,5,2,4,2,1,9,10,11,1,4,4,6,4,1,7,4,2,3,1,3,1,1,9,1,5,12,16,3,1,3,1,1,7,1,7,3,1,1,4,1,3,2,3,2,1,2,2,5,2,3,1,4,1,3,4,2,11,9,1,1,1,8,11,1,2,4,1,7,1,3,4,1,3,2,6,1,2,2,1,3,59,1,3,6,1,5,3,3,2,1,1,3,2,1,13,3,2,1,2,2,2,2,2,6,9,12,1,2,2,8,7,2,1,7,3,2,1,1,5,1,4,1,2,2,1,1,1,3,3,17,6,3,1,2,7,1,1,6,3,5,2,3,4,3,2,7,2,23,1,1,3,1,4,2,1,2,1,3,7,1,1,1,1,4,1,4,1,1,24,14,4,1,21,5,4,10,19,1,6,1,5,1,3,2,4,10,7,2,2,1,1,1,2,3,2,2,4,1,2,12,5,4,3,1,7,2,1,2,1,2,11,2,5,5,2,1,3,4,1,3,4,2,1,7,2,5,1,4,6,1,1,1,1,1,1,1,2,2,1,6,1,9,5,7,15,6,3,4,1,1,1,1,7,9,1,2,2,2,3,2,1,1,27,9,1,2,2,2,3,1,2,7,13,13,4,2,22,4,9,13,4,2,18,1,1,1,9,5,8,2,11,2,1,8,2,3,1,3,1,9,5,1,1,1,1,13,7,24,9,2,1,29,4,6,1,1,1,1,4,6,3,5,5,3,2,4,6,1,3,32,1,2,26,1,4,1,1,4,3,8,3,5,3,5,3,2,2,3,3,4,1,3,16,2,5,2,1,1,6,1,3,5,1,3,1,2,2,1,5,2,2,11,2,6,19,2,1,2,1,3,2,2,1,10,5,3,3,4,2,2,1,1,3,1,3,6,1,6,1,1,18,2,1,1,2,3,3,3,7,2,2,3,4,1,3,2,1,2,7,11,3,5,1,2,4,2,5,3,1,10,14,1,2,1,13,1,5,13,3,2,7,4,3,1,1,5,3,5,1,2,1,14,5,4,4,8,3,11,4,1,5,2,1,2,1,2,1,2,3,4,2,3,1,5,7,13,9,4,1,1,1,8,2,3,1,2,8,4,4,2,2,4,1,1,1,2,1,1,13,4,8,2,6,1,3,4,2,3,3,5,4,8,1,2,2,1,1,1,1,3,4,11,2,4,3,4,10,5,3,1,1,1,5,4,6,2,1,3,27,6,1,4,1,1,6,3,1,5,4,4,6,1,23,1,1,6,11,7,1,1,4,7,2,3,1,3,1,1,1,4,1,2,2,1,1,6,4,1,1,3,2,4,5,1,2,5,1,4,1,6,6,13,4,3,2,4,10,5,1,6,1,1,5,1,17,2,2,11,1,2,5,3,1,1,2,1,2,2,1,1,3,7,2,2,2,3,1,6,3,5,11,1,2,1,1,1,2,1,4,3,1,4,5,2,1,1,3,1,6,2,7,1,1,1,9,1,7,3,5,2,1,1,2,7,7,10,15,2,7,6,2,17,3,1,12,3,44,4,1,2,1,1,2,1,2,3,2,2,5,1,1,7,1,2,7,1,2,1,1,1,1,1,1,1,6,2,1,2,2,2,16,4,6,6,2,8,2,1,3,2,2,1,1,1,2,11,2,2,2,4,9,5,1,1,1,4,4,5,5,9,2,2,2,1,3,2,5,1,2,1,9,2,8,3,4,5,1,6,12,4,4,3,9,1,2,3,1,4,3,1,3,2,5,3,17,7,2,6,1,3,20,3,1,1,2,60,1,3,12,7,1,1,1,7,27,1,1,2,1,7,1,3,1,12,1,8,11,1,2,8,2,10,2,4,15,1,1,8,4,3,5,1,7,18,2,3,7,1,1,3,1,4,1,1,1,9,46,6,7,6,2,9,1,12,2,2,7,1,1,12,7,1,1,2,1,2,2,2,1,1,2,1,2,1,1,2,8,2,1,1,1,3,2,1,9,3,7,1,31,8,2,1,5,1,2,10,2,2,2,9,2,3,2,1,1,4,2,4,4,1,2,1,3,3,2,3,6,10,3,3,1,1,1,1,4,3,2,1,2,1,1,1,7,1,23,2,6,1,2,20,4,1,4,3,1,1,4,14,2,1,6,2,7,4,1,1,1,8,3,4,1,1,1,2,6,5,13,12,3,2,1,8,3,2,6,2,4,1,1,2,4,3,3,2,5,5,3,2,9,10,2,1,2,6,3,1,3,1,2,10,2,1,3,4,3,2,5,7,1,40,5,28,1,5,2,5,6,3,6,2,1,1,2,8,3,8,2,3,19,2,10,2,1,2,17,6,2,4,1,4,4,1,13,1,1,7,1,2,1,7,2,1,7,2,8,3,8,9,11,6,2,9,1,1,2,3,1,1,1,4,8,3,28,4,2,5,1,3,3,1,2,1,4,7,2,1,2,5,2,1,3,2,1,1,2,1,1,6,2,2,4,5,1,10,3,8,1,3,1,3,4,11,2,8,4,2,1,3,3,1,6,1,1,5,2,4,1,1,4,1,2,10,1,91,1,1,4,2,21,4,155,6,3,3,4,1,2,19,4,10,3,7,2,26,2,9,1,3,2,4,1,14,1,3,2,2,2,3,18,3,17,5,11,8,2,2,1,1,10,1,5,1,1,1,12,1,2,3,1,39,6,1,2,2,1,3,1,1,8,8,12,1,1,5,1,6,3,1,3,74,6,2,4,1,9,2,1,1,2,62,3,4,1,17,5,2,4,18,1,3,12,1,1,4,1,1,1,1,4,4,7,1,3,1,1,1,7,2,3,3,2,1,4,4,1,3,1,14,7,2,1,2,1,2,1,2,4,6,2,1,1,1,1,2,8,9,1,4,20,1,1,8,1,2,1,2,1,7,1,2,1,6,1,3,2,2,19,3,2,9,15,2,6,1,5,2,10,1,8,2,1,1,2,3,15,1,1,1,8,4,6,3,3,5,1,1,3,89,1,5,1,2,13,1,1,2,1,1,5,4,1,1,1,1,1,1,1,2,5,2,2,1,2,8,7,1,4,4,1,1,3,1,1,1,1,1,1,1,11,6,4,32,1,1,1,3,7,2,4,1,1,1,3,1,3,1,2,5,1,2,1,1,1,3,1,1,1,1,1,4,4,2,2,8,5,1,12,15,15,1,10,2,23,1,6,1,2,4,4,1,2,13,1,3,8,5,1,1,1,1,4,6,5,17,2,1,1,1,5,1,1,2,3,1,2,5,4,1,1,1,1,6,1,1,2,4],"sizemode":"area","sizeref":0.5125,"symbol":"circle"},"mode":"markers","name":"","showlegend":false,"x":[165,3,74,43,11,11,44,203,16,24,11,61,15,98,374,269,264,49,260,45,260,57,25,17,198,58,101,15,2,51,81,38,100,227,18,7,310,21,50,28,157,318,35,21,56,138,45,38,38,99,11,292,46,49,21,51,30,23,58,84,298,7,1,9,2,17,32,31,318,98,2,368,56,46,35,99,31,81,71,63,241,92,318,59,162,158,325,10,5,14,45,32,35,35,1,212,14,119,165,67,366,14,39,1,276,256,36,196,71,220,53,44,220,95,64,277,66,112,10,24,134,130,164,51,322,263,44,14,86,14,120,84,136,211,21,84,318,7,21,44,3,280,318,17,93,28,17,21,212,58,9,10,35,57,26,155,14,140,99,17,7,40,12,318,40,10,24,49,24,196,1,52,145,21,92,156,18,171,28,374,14,275,70,21,66,45,3,73,18,14,70,162,59,61,72,9,260,150,53,30,14,21,242,231,37,189,44,191,11,3,1,1,7,116,56,273,32,57,164,70,226,53,165,269,19,4,2,247,1,24,226,5,21,19,7,23,78,19,157,78,25,10,134,8,21,165,261,95,122,176,5,1,35,80,1,95,22,91,93,5,108,2,11,86,92,121,200,171,24,303,93,32,7,234,10,165,171,245,49,99,148,46,9,170,7,264,1,57,165,106,136,78,29,101,17,71,19,4,201,170,14,137,35,94,33,247,30,157,1,186,99,63,25,205,9,3,73,5,40,18,78,318,23,311,17,81,113,63,197,8,72,19,94,329,255,61,15,144,14,110,33,9,317,11,75,1,33,21,107,30,37,17,19,17,23,94,32,211,17,99,49,58,64,212,31,16,60,15,187,4,36,8,78,36,262,207,2,26,29,228,47,30,14,16,175,16,372,11,99,15,360,1,194,292,72,9,366,18,22,158,274,320,177,36,120,49,15,1,73,7,238,16,2,49,49,201,72,14,137,21,365,22,30,234,64,56,60,53,25,361,81,11,30,128,361,40,52,22,67,215,33,308,10,8,65,7,68,74,49,15,198,10,65,164,106,367,124,21,10,4,43,28,65,5,67,320,35,220,86,234,128,37,92,9,72,25,2,214,56,257,65,187,166,49,116,186,9,45,5,95,248,47,147,93,2,2,53,37,7,60,61,127,315,18,362,53,4,1,365,50,9,4,30,24,134,1,73,51,72,49,49,8,187,73,354,177,2,366,15,1,12,304,108,21,159,2,267,88,54,95,38,127,11,22,8,73,2,21,220,67,8,25,120,261,53,35,4,21,45,24,136,16,246,205,49,50,26,178,198,36,3,5,218,66,33,254,243,17,30,260,180,294,171,57,50,79,245,23,2,138,30,22,367,79,36,87,85,150,17,19,53,38,291,254,211,213,59,57,29,1,56,308,23,66,73,1,47,87,85,68,29,37,96,38,30,66,12,9,72,15,33,25,29,78,49,2,5,2,45,19,33,16,35,15,75,42,203,236,236,239,79,359,74,50,51,28,282,12,59,21,59,24,162,36,91,8,66,123,232,28,16,87,115,325,28,21,1,70,64,68,1,2,91,37,47,172,182,177,53,98,130,39,67,108,14,45,67,50,1,7,141,51,50,80,32,68,15,64,179,26,50,53,239,313,219,15,36,73,84,100,218,8,23,117,63,214,7,29,256,275,99,108,372,183,8,42,64,85,144,18,113,150,42,207,137,21,12,183,24,59,67,303,84,10,262,66,9,10,17,108,38,277,2,324,15,260,43,46,30,82,44,4,289,99,210,79,42,91,105,88,52,10,263,19,12,141,3,290,8,4,4,366,37,122,51,115,5,278,143,11,312,23,77,64,85,53,78,7,2,374,54,270,2,15,38,61,32,56,103,63,12,270,47,26,18,44,107,23,19,302,64,310,16,14,65,51,148,33,37,4,47,70,64,3,18,15,47,9,18,45,5,42,50,25,315,134,166,177,85,85,11,71,23,121,152,36,131,2,270,28,17,294,23,38,176,229,23,10,268,88,29,35,204,44,180,164,332,16,24,232,15,124,312,49,30,10,78,72,247,276,68,18,35,130,36,2,10,17,4,123,16,10,135,19,52,36,239,21,16,22,56,19,43,30,366,68,175,40,129,107,93,26,16,74,271,80,32,44,10,9,213,57,232,186,53,82,81,51,9,262,65,82,59,42,246,44,101,8,93,29,31,56,28,273,247,23,71,46,144,12,61,32,163,51,182,49,9,9,16,163,234,73,3,310,2,40,138,229,53,205,289,131,5,362,3,58,169,36,32,226,64,2,127,151,43,228,89,123,70,64,3,94,141,24,58,282,182,24,135,72,8,80,66,10,67,71,3,30,163,163,42,65,305,54,29,16,15,257,57,217,5,58,40,212,155,35,5,122,52,2,17,35,355,2,33,15,246,1,136,99,29,165,65,25,101,22,17,23,64,163,19,140,23,106,183,281,3,11,39,12,40,18,10,14,113,151,261,74,47,25,10,268,36,368,45,327,59,15,59,142,30,99,60,67,38,73,73,52,28,232,18,38,67,8,170,213,289,30,12,47,16,45,11,53,15,35,28,71,3,23,112,71,32,59,1,22,59,218,29,142,47,29,75,51,1,2,2,10,15,19,100,16,15,15,22,155,82,10,50,9,368,29,164,148,68,98,201,116,16,12,32,207,281,112,10,122,26,100,106,186,2,1,2,191,42,114,33,211,22,361,127,54,9,8,18,23,2,26,33,213,17,98,2,143,10,74,66,25,239,150,320,302,259,39,31,9,318,49,5,28,7,148,4,98,40,42,290,10,75,358,117,11,24,25,44,373,84,367,71,87,11,31,137,64,11,42,369,80,16,187,35,11,284,369,18,3,36,331,126,9,122,95,19,24,50,309,23,119,17,7,25,15,246,5,98,141,100,56,66,11,366,241,277,54,3,17,8,46,79,47,63,109,4,302,30,24,32,28,75,46,36,59,253,45,17,33,58,30,24,323,187,21,7,47,81,40,4,50,2,45,203,85,26,70,77,50,73,39,71,86,101,134,8,9,298,67,59,332,1,296,40,95,46,17,129,68,182,127,16,31,107,59,15,100,64,106,75,45,198,322,31,43,47,45,156,44,3,99,289,149,40,49,16,46,4,43,74,57,11,1,52,66,43,15,14,9,185,49,68,203,14,66,200,58,96,60,182,67,65,45,205,99,18,74,11,259,17,61,172,64,56,68,57,3,61,372,16,46,1,25,214,38,30,161,278,86,79,177,86,319,257,29,318,87,26,136,8,60,8,212,68,200,8,7,37,30,44,60,288,225,5,22,107,4,12,60,46,10,15,23,18,32,148,10,73,70,39,130,39,147,15,101,137,189,1,79,1,10,50,4,92,91,22,36,8,15,25,31,12,68,16,287,32,18,68,135,204,64,8,25,369,52,66,227,325,215,44,72,18,1,38,43,15,372,37,147,4,73,42,8,66,26,37,295,9,309,37,10,40,5,14,12,170,11,19,72,72,23,152,319,127,231,45,64,25,1,32,115,47,43,33,31,259,166,15,52,16,106,310,12,120,56,24,47,16,47,17,232,290,26,15,26,145,70,109,53,71,88,79,61,53,8,368,124,8,30,68,154,180,19,57,18,214,7,60,33,9,89,262,15,205,26,29,292,269,10,2,103,15,26,31,79,51,316,22,30,21,38,12,177,147,16,51,10,31,15,78,120,22,73,89,313,44,45,253,50,270,10,159,52,18,224,30,149,374,24,51,65,63,66,63,1,81,269,368,185,151,3,145,24,30,228,215,17,7,23,64,224,26,212,50,36,9,28,16,61,253,155,9,71,51,78,1,60,254,71,4,15,15,54,1,43,78,80,369,46,15,113,7,226,30,290,182,49,9,28,3,61,24,121,36,196,3,45,8,71,228,2,137,42,4,115,372,320,96,61,17,18,32,80,262,2,110,26,103,257,228,164,8,23,39,295,25,186,107,232,10,242,171,38,2,143,17,373,5,74,40,75,26,257,5,23,234,5,217,31,94,130,16,2,121,15,4,217,24,8,316,266,134,43,164,78,59,54,175,10,72,66,235,63,43,40,9,53,266,60,59,26,110,78,141,190,16,8,2,31,16,24,49,129,119,269,171,77,157,54,12,320,26,305,63,254,52,326,253,79,19,1,45,7,31,158,2,31,44,10,81,3,99,73,1,37,24,11,67,232,40,175,201,29,246,17,32,263,57,106,1,79,14,255,26,198,53,214,37,299,25,49,24,105,32,54,339,368,57,280,231,15,175,15,23,77,94,339,45,17,57,361,66,371,373,59,208,325,24,12,1,16,297,248,66,78,199,18,117,291,64,228,30,3,109,2,89,116,31,9,123,137,29,3,15,2,313,5,3,204,11,31,4,255,16,30,2,22,12,5,4,177,80,231,99,15,61,4,57,15,110,156,79,12,12,3,59,46,46,49,54,8,22,115,36,65,128,130,4,1,63,3,304,15,365,200,310,98,8,99,56,219,2,151,166,66,8,215,39,8,247,19,368,84,58,57,112,115,172,155,66,21,30,18,17,192,156,29,78,93,46,68,15,212,92,59,337,114,19,205,37,31,10,24,367,25,9,103,43,18,42,16,46,303,61,28,3,56,21,183,23,110,361,17,70,99,1,4,68,30,44,10,130,75,56,26,52,59,366,14,1,12,29,254,199,25,284,2,108,245,148,32,64,222,257,37,5,10,66,58,71,302,38,191,113,47,66,276,50,57,39,182,32,197,50,148,8,17,16,166,240,15,114,16,36,1,86,235,30,61,197,16,124,14,206,26,136,43,15,1,94,29,9,301,46,290,56,2,8,4,366,201,4,191,136,246,114,1,19,49,313,73,112,198,337,31,245,43,10,135,2,37,47,313,57,197,85,61,2,9,5,184,3,239,4,1,298,162,84,8,39,45,54,30,128,46,298,86,7,260,326,18,98,36,1,59,10,17,28,128,36,32,220,10,203,31,219,56,88,23,127,11,212,89,357,204,327,114,61,70,21,15,217,99,36,5,23,103,138,177,10,276,40,32,110,226,12,163,310,70,26,254,233,39,38,53,96,81,5,25,28,74,211,16,80,231,16,285,211,369,101,37,22,21,176,9,163,315,92,36,66,25,114,15,267,18,2,359,40,4,362,156,17,16,26,1,15,23,15,19,36,24,242,9,30,2,148,12,361,40,45,173,11,148,95,8,7,242,12,231,57,14,30,259,1,275,204,33,19,60,150,99,28,26,5,22,16,254,8,255,359,19,12,2,12,35,228,28,313,18,218,123,1,211,63,81,64,8,4,284,9,26,179,51,19,246,73,26,74,32,45,63,29,12,57,93,2,86,280,278,145,19,31,73,49,14,87,8,32,365,50,8,51,3,157,56,23,8,22,1,15,297,85,47,108,192,361,87,53,40,57,31,2,15,235,109,183,86,56,54,253,30,295,1,233,59,325,260,112,138,5,225,53,192,15,2,89,191,5,57,176,17,186,81,47,36,26,23,15,36,17,189,8,72,57,71,8,94,255,11,85,2,96,8,129,15,22,147,266,78,70,302,92,112,7,248,240,54,45,54,267,32,67,99,66,103,15,53,144,67,165,35,14,372,66,10,217,128,54,89,81,21,44,5,95,22,1,71,29,178,56,224,67,129,8,89,204,8,11,373,12,121,3,22,29,172,21,64,257,12,162,162,12,112,1,66,3,68,4,92,9,253,17,49,5,219,262,32,73,14,135,23,282,45,197,40,5,4,68,19,52,42,36,5,30,305,117,5,3,4,49,87,16,14,179,10,126,36,259,72,170,14,37,2,242,267,4,60,32,9,8,367,178,283,292,280,64,53,15,92,10,273,285,29,1,138,60,11,17,5,225,8,187,56,39,318,10,15,85,299,3,86,15,24,61,234,21,92,2,46,68,238,126,37,241,1,4,72,17,4,187,11,130,170,32,47,50,2,29,371,17,261,17,110,11,23,19,18,12,2,37,7,44,2,53,32,17,31,47,93,231,8,92,5,30,196,60,180,59,66,15,15,71,52,198,318,9,11,19,317,49,54,155,42,3,36,67,15,361,1,198,229,53,163,64,26,138,261,305,21,2,306,18,16,3,25,211,22,260,140,11,92,52,9,53,98,70,25,54,74,70,334,158,64,33,38,288,30,58,26,2,54,3,15,362,16,43,58,11,180,12,59,219,299,316,14,358,7,53,5,24,84,36,72,65,8,56,17,18,158,106,4,1,141,64,50,5,31,109,127,107,57,72,37,114,74,30,200,8,44,30,11,371,46,11,359,124,105,73,28,18,51,53,8,16,32,23,30,18,4,57,254,71,9,136,3,357,10,23,38,14,130,21,17,15,56,2,112,11,73,58,18,5,17,8,95,9,148,18,56,14,1,129,23,46,22,14,59,74,22,59,227,73,93,284,9,110,115,369,116,21,361,59,123,54,29,282,113,12,106,98,100,318,16,3,112,262,33,113,66,372,28,330,3,65,42,50,68,25,172,16,186,89,262,213,60,19,5,199,63,60,68,67,128,187,56,75,73,43,145,119,21,24,66,10,47,5,15,11,12,17,30,12,73,3,218,19,71,72,84,45,18,38,106,253,31,44,32,65,2,23,31,289,172,36,49,5,29,112,264,25,31,15,5,86,25,227,16,370,5,29,11,67,219,26,128,44,213,32,12,5,18,49,17,44,82,148,304,17,358,129,53,240,156,26,22,226,107,220,197,266,166,242,7,9,11,365,226,56,19,144,106,150,32,29,70,10,67,7,318,68,173,213,16,72,49,176,80,67,49,10,10,19,256,31,208,266,8,33,15,57,1,1,51,71,51,82,2,220,60,113,304,17,67,278,21,277,177,9,50,371,37,227,155,11,8,182,235,33,4,59,9,32,22,141,11,2,33,33,112,36,205,42,63,267,15,31,59,56,302,262,61,9,68,229,80,22,86,36,11,26,12,2,21,10,26,24,198,360,40,35,89,39,15,38,262,19,25,179,37,58,43,43,117,121,8,42,74,4,45,33,337,51,58,371,85,78,91,156,56,222,40,4,47,56,4,58,176,30,365,205,257,23,29,315,77,38,47,2,164,277,169,15,25,26,29,54,44,47,359,197,16,2,59,51,254,36,36,15,4,15,37,2,306,198,266,2,5,4,12,2,79,40,7,19,28,284,39,165,29,141,213,172,31,261,128,92,19,24,227,8,8,103,40,45,66,374,18,54,19,114,8,93,59,72,59,3,51,49,2,43,8,1,72,12,45,29,33,40,19,19,16,1,29,190,68,315,213,106,68,56,14,5,89,4,101,17,322,23,29,14,29,24,78,200,64,75,310,9,42,166,5,239,64,58,198,254,213,254,12,53,53,78,4,52,88,26,183,2,70,37,325,30,105,63,65,15,73,39,129,7,192,21,92,36,36,33,112,7,190,2,138,74,191,4,14,365,16,3,7,64,1,358,271,57,287,22,8,268,58,4,47,78,208,11,176,7,9,5,59,25,19,17,21,135,17,124,86,247,175,245,29,19,98,65,3,77,15,4,71,26,24,45,369,262,18,22,158,59,31,302,19,64,5,40,12,81,12,15,58,57,23,99,18,80,32,12,79,38,28,106,28,73,65,187,283,361,212,1,23,4,178,78,68,53,9,5,82,217,18,14,183,33,45,227,19,51,240,186,33,11,10,12,80,67,73,11,11,82,81,374,232,5,1,43,273,362,360,4,9,46,17,261,198,65,28,214,15,31,30,45,32,12,373,22,10,3,73,228,59,66,19,1,17,169,30,358,59,25,33,299,42,156,61,138,210,184,284,38,24,75,17,64,156,88,110,78,7,298,52,25,25,54,40,53,148,71,88,87,12,371,61,163,127,116,183,113,10,80,81,17,15,134,214,10,297,66,162,141,367,18,58,9,58,205,227,162,82,196,39,4,172,22,166,291,63,1,218,23,159,56,24,10,52,288,31,285,28,5,94,2,100,190,255,4,52,53,227,113,4,137,217,80,4,3,199,25,84,8,46,11,7,23,232,21,92,5,19,325,182,117,64,18,37,26,117,72,32,4,47,145,2,60,135,81,21,105,372,60,31,19,38,201,74,10,92,14,25,264,36,3,26,12,17,108,30,305,366,10,75,26,260,170,284,29,31,29,17,23,28,29,11,56,361,305,4,30,12,43,2,46,44,45,309,367,180,164,42,19,108,11,1,2,2,15,12,364,11,9,73,16,30,63,332,127,64,22,190,12,284,21,312,36,31,12,22,68,11,91,259,17,54,31,67,40,1,10,70,4,71,89,103,21,56,10,56,86,46,144,7,52,217,177,5,86,45,2,47,136,57,22,8,40,59,29,184,23,3,9,22,17,29,88,4,32,1,21,21,47,44,165,80,291,185,368,17,133,23,3,98,1,15,17,8,196,158,61,31,11,36,3,110,264,44,235,31,248,44,268,80,123,50,212,364,277,24,3,114,21,137,26,199,26,197,16,186,17,56,3,88,44,305,30,14,61,4,30,74,33,1,246,1,163,21,70,87,56,253,19,145,277,185,26,26,11,14,33,8,108,61,15,368,29,148,31,26,214,222,8,269,17,178,9,303,1,281,110,214,12,8,82,196,103,81,255,264,32,21,3,374,45,85,4,54,17,113,16,108,253,58,226,373,12,85,38,176,57,15,23,3,194,19,40,233,45,73,82,238,114,24,10,15,60,283,23,15,65,9,30,1,305,21,105,134,2,66,8,373,297,53,110,129,113,71,98,1,53,11,4,3,2,2,239,157,19,233,4,21,8,59,56,29,24,2,4,32,147,30,18,112,17,19,54,91,2,12,38,106,59,18,60,228,32,57,280,3,295,2,11,31,40,232,18,257,8,161,3,31,10,25,42,64,84,16,113,107,163,182,57,100,10,11,4,63,29,9,92,289,38,84,17,46,23,19,72,28,150,33,22,297,303,18,148,186,81,7,297,14,103,322,5,31,217,25,87,8,28,12,246,31,184,7,304,10,4,11,29,1,14,373,7,10,42,56,57,234,10,257,45,359,50,1,79,1,238,47,191,151,53,116,8,58,24,43,98,8,1,5,4,21,21,5,21,11,120,46,22,4,2,33,17,9,324,35,31,275,15,175,4,73,10,10,2,3,88,157,182,30,9,276,9,373,362,11,36,187,264,136,74,4,15,179,163,9,8,80,357,207,23,29,33,268,42,36,56,81,226,59,332,7,1,9,14,176,3,291,364,228,158,8,185,107,8,1,227,7,9,138,184,17,2,8,36,1,29,46,316,31,28,12,123,15,16,227,25,253,24,18,15,16,2,200,85,11,12,17,28,51,178,9,359,46,67,35,179,145,21,15,136,219,278,14,11,21,21,57,192,10,1,177,2,23,285,17,129,73,29,270,200,161,28,3,254,29,177,44,2,22,26,82,24,22,19,305,1,9,10,16,2,61,127,33,19,9,15,82,57,229,1,80,82,63,78,21,32,186,10,1,185,112,78,232,5,47,225,54,4,333,12,5,3,149,108,39,173,267,140,234,28,45,187,109,37,9,18,282,40,56,282,295,24,5,207,22,15,368,220,259,8,10,106,23,248,239,311,196,38,158,36,99,358,267,66,261,239,113,16,74,23,107,211,319,358,17,189,135,201,18,313,166,73,149,232,4,30,26,23,1,14,120,2,14,42,187,24,151,8,51,81,50,66,15,245,50,81,184,278,217,92,23,82,100,2,10,29,134,70,308,227,2,68,245,184,226,214,28,33,40,155,30,213,18,67,296,112,18],"xaxis":"x","y":[372.86,1323.32,222.16,2671.14,300.93,343.8,317.76,488.21000000000004,3560.3,12079.99,2719.01,2563.36,1590.83,321.35,130.0,500.24,917.6999999999999,1791.15,774.62,2179.42,763.0500000000001,2246.29,503.15,1798.2,1407.7,767.98,6951.49,1938.4,258.9,143.94,1120.45,1433.33,2285.18,234.75000000000003,816.85,2048.64,931.43,853.65,205.25,1031.3,591.6600000000001,63.24,2975.06,1452.22,12346.619999999999,320.38,346.9,1370.05,241.35,390.0,19543.84,202.56,3148.31,1352.62,503.01,291.56,4428.24,763.08,1579.07,964.21,762.48,303.5,3735.51,4370.52,7205.39,297.29,6096.04,7731.99,1142.88,859.2,173.55,485.31,301.58000000000004,768.6,490.32,3634.36,13151.44,3413.09,632.68,830.09,454.36,326.65,56.73,132.8,2941.2,3070.54,211.95,20129.54,11308.48,5048.66,426.26,7267.49,7056.63,2183.2200000000003,6765.07,23691.4,2612.2000000000003,1826.78,708.1,1037.28,285.29999999999995,4405.71,452.0,626.77,357.4,45.34,5205.400000000001,393.72,1697.39,1126.0,297.0,955.2,539.25,4195.45,3084.02,580.08,1336.56,385.7,2562.58,6640.39,973.73,386.8,316.48,2096.98,1807.98,1367.89,732.94,276.5,5646.400000000001,8854.25,481.88,980.25,175.78,1575.68,916.47,468.5,129.23,6725.18,1185.18,249.24,4308.58,70.0,77.52,556.87,622.8199999999999,826.89,990.5,1847.94,637.2,1469.04,7166.0599999999995,1865.73,743.88,4048.98,886.1,2145.06,1751.89,299.02,1511.01,1264.14,2444.88,104.19999999999999,1232.95,21.0,415.79,2309.01,3472.9900000000002,3449.79,417.8,140.14,11086.14,3442.7400000000002,2481.06,1558.22,2841.07,804.88,3514.18,490.0,1241.23,141.0,2280.91,249.8,826.27,4960.18,1440.58,1764.12,14201.53,793.95,1671.1399999999999,263.75,97.05000000000001,1950.98,261.35,2356.66,162.0,3138.2400000000002,239.4,1188.16,250.6,1248.42,7575.88,764.24,2622.481,211.15,619.5,957.25,1511.7,395.1,16652.72,17080.74,11745.69,4220.21,89.14,2138.91,134.6,110.95,847.11,973.68,825.02,851.91,339.75,638.11,1648.7,722.98,4893.81,2601.19,21107.94,1339.05,2797.53,9384.85,277.93,2478.94,1356.95,1286.11,2266.76,1405.5,1698.8300000000002,233.84,829.25,6879.8,2040.92,529.25,3710.5,155.35,1713.69,406.07,1201.52,4037.77,925.41,254.55,5061.03,22457.9,2806.48,268.27,5218.709999999999,7111.650000000001,3489.3,347.8,782.95,320.08,1404.58,3078.65,949.82,168.69000000000003,1895.0,282.3,124.25,98.7,300.08,55.5,519.45,953.45,2740.91,337.4,2287.32,1230.33,788.45,728.5,398.52,444.9,181.5,266.93,863.51,426.47,471.85,381.0,552.08,1533.8899999999999,219.35,584.25,1071.82,1988.84,1092.81,325.37,1434.95,2878.96,330.35,422.45,1266.63,534.99,519.05,1687.8,524.94,540.52,1747.18,128.07999999999998,4742.0,518.63,1481.03,92.30000000000001,236.06,5996.83,3949.37,528.5600000000001,2694.75,4480.31,1315.2,3472.61,799.0,4881.1,322.64,15.58,765.36,208.0,2032.18,330.0,148.95,1950.4,1654.72,612.84,2581.6,147.27,575.1,54.22,68.25,984.62,2271.35,152.45,1755.24,668.23,3819.92,640.0,337.66999999999996,1676.95,363.38,3062.52,1653.3,948.42,1013.76,1863.77,1406.3700000000001,640.13,222.4,385.46000000000004,329.3,509.5,263.38,157.35,297.75,296.21000000000004,366.4,340.33,833.53,1018.42,508.91,1026.0,924.52,2721.48,1651.72,4217.2300000000005,223.98,811.1,1534.78,3765.2,10953.5,323.86,19333.95,597.6,1535.7,35.28,1523.97,197.31,2242.19,50291.38,1698.56,1029.1,181.65,2083.16,1593.34,13683.0,377.49,258.75,496.5,437.16,324.45,304.04,252.48000000000002,11359.48,2149.65,498.96000000000004,162.45,413.82,4689.43,467.34,150.95,1322.07,211.99,1744.96,132.24,2646.02,122.45,1048.13,4782.88,1161.2,385.55,264.15,2100.09,2417.7799999999997,474.64,2502.82,517.7,8870.880000000001,2354.27,490.08000000000004,2729.84,753.51,6627.64,546.0,261.06,93.35000000000001,844.87,355.5,884.02,920.08,497.65000000000003,14256.68,293.18,5994.94,485.98,175.25,382.66,234.0,330.12,50.550000000000004,1310.99,7814.83,1914.8,3240.14,2485.46,1229.95,618.96,2270.86,65.3,759.13,555.87,323.39,757.72,10327.2,679.8,402.36,3292.84,1805.78,30.0,279.91,442.32,307.1,3033.88,1010.73,799.13,1488.59,4287.58,503.64,319.94,5557.62,2229.72,751.5600000000001,6477.67,155.8,2771.18,127.65,1106.7,209.48,709.59,2108.37,188.4,561.25,1055.05,393.05,5185.51,522.9,129.22,418.08,1378.74,135.4,2326.35,16904.510000000002,199.08,222.65,30867.77,3722.12,91.67999999999999,122.39999999999999,2017.2,439.33000000000004,313.64,236.11,57885.45,5189.32,1976.0,1662.45,47111.18,505.8,419.70000000000005,7320.88,594.21,5272.83,465.70000000000005,610.62,72.0,526.96,3721.45,748.46,347.49,1468.8000000000002,321.88000000000005,20.6,6357.08,234.38,3199.92,1658.8999999999999,1217.6,134.9,506.75,885.33,148.62,1898.6100000000001,263.5,233.25,2240.47,90.65,681.65,275.45,3666.9500000000003,3983.65,504.85,3138.04,790.71,5267.33,405.1,24.4,805.0,4814.56,449.96999999999997,1207.34,2146.38,1624.84,778.42,1586.54,3946.14,307.12,359.38,7587.2,17.65,372.2,429.68,458.73,333.82,541.14,127.2,69.6,2212.11,6621.6,831.45,229.75,8333.72,572.3100000000001,86.10000000000001,359.69,3078.4,105.34,570.66,164.69,1646.6399999999999,412.08,136.73,1271.06,373.01000000000005,1572.99,6615.77,1233.06,1580.3,1778.79,966.6600000000001,2803.2,8308.19,285.6,758.65,2617.9,1523.35,2330.94,3264.23,81.6,132.5,76.23,516.8,88.5,805.9200000000001,9796.23,204.25,3457.83,571.34,346.5,643.0,305.9,1265.1299999999999,1036.21,112.53,429.0,635.26,558.72,1390.23,897.52,253.09,252.68,399.3,1920.71,51.410000000000004,504.95,149.75,336.59999999999997,1119.36,373.16,1239.27,722.5600000000001,2300.78,5236.77,3545.47,590.0,360.85,238.86,1300.98,264.9,893.12,1494.87,350.85,181.05,279.7,655.47,734.91,781.59,927.61,307.95,208.63,2909.68,415.57,1192.94,815.79,900.77,705.11,2278.77,1076.2,913.62,95.45,61.72,917.57,1751.75,53.65,258.34,284.86,249.58,409.17,124.75,1890.0,310.26,7207.89,4212.411,11236.95,679.4,3391.52,281.76,6251.71,4323.97,1655.17,262.4,1015.74,2699.48,1575.26,1958.6,11364.08,299.4,3196.11,603.75,1553.65,1450.5,2303.33,731.36,510.20000000000005,498.1,308.28000000000003,754.87,2297.66,4274.94,422.8,1013.58,1552.88,62.4,4585.06,1164.59,305.28000000000003,2528.9,835.46,186.94,508.55,91.14,1131.45,4101.68,872.51,1647.74,213.0,275.4,310.83,913.9000000000001,154.5,641.36,568.92,2244.7,23616.422,1198.9,487.34999999999997,429.44,1103.64,784.17,228.3,1028.66,6055.47,2225.04,217.03,572.77,1766.22,4526.06,423.93,276.76,2047.14,439.1,658.3199999999999,1132.6200000000001,637.44,46.5,702.9,1295.25,424.26,419.87,1273.16,619.08,241.89999999999998,3881.89,5655.2,2076.32,4191.8,139.8,424.71,219.87,1847.59,148.22,2312.7,1160.4099999999999,2715.0,1002.35,2422.96,931.12,705.45,7670.54,82.4,592.24,2235.12,419.75,823.0,1364.59,2215.78,550.42,627.82,2026.28,151.45,476.92,2564.98,271.47,6619.57,154.25,3350.7400000000002,3329.39,972.28,327.38,299.53,1544.79,488.5,1642.39,1180.68,176.07999999999998,135.0,2766.73,282.76,462.18,619.49,663.7,374.7,295.85,220.46,3285.05,1136.64,1182.0,2298.93,402.97999999999996,1254.72,1850.82,89.5,717.85,1822.28,938.02,632.39,785.25,3959.4900000000002,1439.6100000000001,514.14,364.93,465.15,167.08,223.28,87.6,342.55,152.39999999999998,3208.85,499.6,1629.35,4792.96,200.82,2448.2,990.0,1661.54,3038.85,15310.35,174.57999999999998,831.76,1079.29,497.74,2448.32,614.27,404.45,4834.400000000001,1801.68,147.66,1072.0,107.75,162.53,241.68,664.001,243.3,684.54,127.28,961.1,741.09,4354.12,976.8100000000001,3481.41,136.35000000000002,784.48,396.71,601.92,5013.96,298.9,466.67,596.28,717.62,522.67,400.3,232.05,167.35,1021.38,2143.53,818.29,1039.8500000000001,747.28,5854.84,283.85,734.57,224.94,112.34,658.95,1805.32,462.08,301.26,449.81,607.43,310.14,5578.5,1526.88,3486.41,1928.1299999999999,312.93,405.0,1016.47,159.92,1637.33,326.25,1393.56,1764.02,1304.88,1451.66,132.65,1475.77,152.2,3600.67,1347.54,699.2,3386.33,3321.68,306.64,325.24,1352.35,1858.04,1711.6599999999999,43.25,6638.650000000001,195.0,172.6,233.53000000000003,240.68,243.9,551.24,281.26,7700.55,258.6,529.75,337.04,11880.84,375.5,349.5,212.78,1626.08,2011.82,131443.19,266.6,2020.4,451.73,455.57,348.34,161.0,135.18,326.06,1323.47,300.38,2559.45,1263.7,329.04,1540.2,537.58,740.76,161.35,7348.3,318.29,1741.4,217.39999999999998,3260.0,994.23,213.01,740.6700000000001,1839.03,724.73,487.03000000000003,6695.6900000000005,6879.2300000000005,2022.67,434.78000000000003,75.3,1232.02,477.32,176.69,169.79999999999998,1320.1,1954.92,404.45000000000005,1353.31,164.45000000000002,3999.2300000000005,195.55,6226.43,1076.85,881.52,2165.76,3502.48,1400.2,352.821,190.35,18320.170000000002,875.97,3003.9100000000003,785.37,3094.37,1977.39,301.54,1049.66,623.98,30501.260000000002,945.79,161.13,372.06,650.99,877.22,1542.09,447.44,542.49,3127.99,913.46,3.75,239.45999999999998,188.4,96.0,126.03,836.3,289.7,1179.45,38683.02,2114.71,1121.24,21659.69,1061.84,572.73,422.2,524.65,1398.61,678.2,3448.38,61.28,1502.0,652.75,993.44,2445.79,1615.82,255.59,102.0,156.85000000000002,4795.09,557.6899999999999,316.56,324.19,260.7,620.37,1245.91,139.95000000000002,651.4,2244.29,319.75,442.73,310.72,2080.0,1931.35,1955.0,187.35000000000002,4144.44,1310.3400000000001,753.53,1001.01,314.7,273.26,346.73,933.77,662.4000000000001,4913.22,2941.14,1482.6,919.57,649.32,1508.7,3027.58,332.44,1145.33,395.5,1171.89,28019.58,3885.54,1037.43,1405.92,1574.3300000000002,1043.57,859.21,634.68,1415.45,388.55,849.56,530.47,869.64,2809.14,34095.26,165.45000000000002,312.65,42.0,136.8,282.85,1654.08,124.95,42.22,70.80000000000001,1275.81,975.87,1774.68,730.7,692.95,359.27,2651.45,4019.85,6116.92,563.08,721.28,992.41,6009.34,320.0,246.54,200.43,854.15,1314.66,446.76,9572.45,3125.0,221.10999999999999,458.25,4709.2300000000005,470.73,538.4,193.25,2347.33,438.9,996.61,384.26,85.6,123.66,4266.88,846.55,1909.31,264.13,2795.16,834.38,519.8100000000001,2510.06,568.68,331.14,2511.08,4479.82,1128.09,2960.73,682.54,609.84,759.98,142.7,3309.35,2508.32,895.76,376.46,566.7,5600.45,1410.68,2487.01,209.41,1675.81,1234.48,79.48,509.04,404.3,445.15,457.82,611.5500000000001,1443.03,7062.2,229.75,367.15000000000003,606.21,1084.68,352.15000000000003,318.04,4057.9,713.11,10396.5,5839.46,23632.72,1874.68,109.44,1366.73,689.75,1509.5,115.08000000000001,8963.49,431.98,302.6,2431.28,9653.279999999999,2946.42,338.76,7444.68,6784.01,241.3,312.05,795.3199999999999,2135.0,469.19,3378.66,917.4300000000001,15120.800000000001,22710.2,1925.9,891.02,1250.8700000000001,225.81,321.6,390.85,219.39999999999998,792.61,115.66,640.34,243.63,1070.08,4369.47,231.10999999999999,373.3,150.5,5607.43,39.95,638.0,13544.99,9530.08,3152.54,311.77,2.95,422.85999999999996,2646.7400000000002,3985.14,527.3199999999999,4722.6,214.79999999999998,2174.07,447.05,7304.8,2919.44,5252.1900000000005,977.5600000000001,622.6700000000001,458.45,183.37,362.42,436.18,466.33000000000004,1190.3700000000001,286.71,1467.48,1377.86,168.22,305.5,11123.35,4953.61,1570.81,332.7,670.98,671.34,30.599999999999998,3857.85,5259.3,2106.8,309.09000000000003,679.45,1376.05,233.4,5115.08,196549.74,553.8000000000001,3230.45,8421.47,2182.46,148.71,251.15,711.7,370.45,621.51,570.18,135.0,369.02,135.16,4456.58,339.11,1660.23,1570.73,308.35,678.77,278.34000000000003,1550.63,123.5,1160.31,79.9,132.9,293.74,356.8,7321.07,1603.6,390.05,868.33,906.3,379.2,446.1,847.76,339.34,874.22,1947.5,856.85,157.8,415.31,2919.84,3955.6800000000003,2161.19,1119.6599999999999,1192.2,1626.61,310.74,1411.91,543.89,882.5999999999999,3084.69,831.98,2031.07,770.34,3017.75,745.5,1046.84,979.8000000000001,309.91,1441.55,1573.1299999999999,1253.78,471.14,287.38,1882.3200000000002,357.15,120.0,4842.28,366.41999999999996,616.79,730.37,5400.461,350.99,363.40000000000003,601.03,1000.63,1491.73,2854.73,19381.36,824.28,322.93,1183.39,843.17,40.56,354.85,295.73,101.86,482.85,256.6,1189.3799999999999,982.94,13009.61,205.83,191.86,245.15,4120.32,628.4399999999999,704.4,1374.41,3851.0099999999998,2201.91,4020.85,4354.32,2221.37,375.34,2196.71,456.40000000000003,39667.19,10533.87,1352.04,742.9000000000001,277.94,1341.56,1058.0800000000002,2075.31,1147.02,1428.28,492.1,206.19,1478.7,109.80000000000001,716.69,1370.34,158.02,494.85,868.96,687.3100000000001,474.1,701.61,245.48000000000002,757.13,1583.28,445.05,4912.38,200.63,1077.2,1766.27,339.88,310.75,2285.04,249.88,1079.66,2443.77,1294.17,82.6,689.1,285.9,840.0500000000001,400.06,214.70999999999998,227.47000000000003,634.68,136.0,469.13,1175.78,860.08,108.0,871.26,215.46,131.25,917.06,48.96,423.26,578.14,58.5,3488.16,854.55,1731.16,135.6,266.4,93.8,4630.7,1591.74,707.79,2108.21,1157.77,2744.42,285.6,404.7,1404.04,195.0,1503.2400000000002,1696.64,1531.63,656.16,643.47,902.35,6124.52,524.65,202.85,1251.65,1656.26,6007.88,651.41,661.39,7246.27,900.1800000000001,516.28,1220.9,2143.84,280.7,90.42,182.64000000000001,873.34,6781.8,4281.05,2898.8,3777.2,966.27,120.44,221.65,189.93,548.5,1555.4,645.19,2614.78,399.9,977.3,617.72,228.07,3537.821,659.16,4573.85,262.24,256.95,111.0,2887.4100000000003,2374.5,1606.98,204.24,2093.46,250.85,400.95,266.05,711.23,225.3,660.0,1276.66,982.86,1020.47,1648.18,2283.91,120.0,749.0600000000001,175.5,225.89000000000001,1111.55,1273.47,127.2,1921.91,2704.62,261.9,51.0,2129.42,221.69,1943.51,1293.3500000000001,6019.65,1256.6000000000001,1848.8600000000001,554.87,110.95,979.85,940.69,332.42,3389.84,1647.17,151.17,203.14,199.45000000000002,773.8800000000001,1066.03,3810.64,258.65,19257.08,3226.76,376.34,2912.93,1516.65,1790.36,3118.39,285.84000000000003,982.59,2756.98,1038.03,10988.14,934.23,349.25,2610.91,1888.49,624.36,587.69,691.86,343.71,959.33,1991.49,815.69,655.5,75.6,3876.2599999999998,1631.38,640.54,2605.62,760.1,2660.0,919.7,369.94,395.69,160.0,2722.34,925.3,143.70000000000002,420.22,35.400000000000006,504.44,105.56,14.850000000000001,271.05,673.33,705.1,986.22,135.39,39.75,2993.37,3048.38,255.0,423.85,222.22000000000003,1318.3899999999999,162.07,457.89,471.84000000000003,205.35999999999999,143.85999999999999,921.5600000000001,18482.1,7553.63,1567.79,884.64,24.35,604.3299999999999,877.7,76.32000000000001,295.13,951.76,121.7,190.47000000000003,788.88,896.8000000000001,1212.21,1130.28,588.22,3598.25,2619.65,462.75,1326.04,338.65,1018.71,2124.46,215.22,106.67999999999999,1994.47,1541.98,986.08,1416.96,279.8,248396.5,270.0,4466.65,2248.6,62.5,1371.77,1666.41,246.86,1946.28,498.95,601.97,635.06,490.6,913.95,12418.01,111.63,1957.54,190.47,1127.45,347.59,1674.41,173.66,419.22,5808.55,184.91,403.39,1289.44,28615.27,376.20000000000005,1462.81,146.56,228.52,4536.64,852.53,773.6800000000001,2056.3,422.63,1123.59,992.46,244.31,1534.22,688.95,593.47,880.96,1961.02,3427.44,258.96,367.54,139.2,1085.91,3965.29,1300.31,411.0,2752.5099999999998,746.05,958.0699999999999,235.77,81.3,226.42,109.07,309.1,927.89,341.9,597.16,116.5,195.15,190.58,586.24,2814.58,6579.26,1056.42,6174.47,374.05,2389.62,507.45,8385.22,2712.85,497.25,579.85,50.48,2785.1,1331.15,593.5,1736.57,310.15,112.03,126.45,306.36,909.88,4962.27,1914.8,1380.9099999999999,192.02,117.05000000000001,4082.76,382.81,5097.13,2761.2200000000003,594.62,140.55,241.45,406.56,1170.0,307.56,307.3,159.94,90.96,110.65,476.88,1781.56,303.63,396.91,633.99,2865.05,300.31,550.5699999999999,180.45,3415.38,1502.98,536.15,110.03999999999999,504.14,686.76,446.59,170.5,178.85,888.91,154.21,2849.52,495.76,184.34,767.47,1230.15,157.70000000000002,2895.2000000000003,2698.17,385.2,7699.7,995.29,1676.57,322.69,94.8,485.86,297.0,1810.82,769.9200000000001,915.28,299.86,3867.2400000000002,211.79999999999998,3512.62,150.0,437.4,1039.55,310.56,7832.91,1445.21,318.06,3295.7400000000002,901.0699999999999,1563.03,5874.65,2478.731,180.75,351.9,1254.94,77.52000000000001,6460.32,2241.78,1230.42,2056.69,1511.7,726.89,816.6,334.45,116.18,1397.08,256.7,425.85,2600.41,185.06,2956.9500000000003,120.6,832.8,691.6,552.35,202.35000000000002,571.76,837.0600000000001,13980.83,1614.45,1774.16,2195.14,1331.38,13.919999999999998,860.11,702.96,605.05,1355.24,68.44,2664.69,104.10000000000001,1404.68,152121.22,1021.4,2845.7,1503.77,1773.97,1123.75,4345.66,359.35,629.4,843.74,620.96,359.4,308.2,223.91000000000003,612.8100000000001,2873.77,666.13,1770.0,1612.56,1065.12,4273.79,2322.43,531.49,182.1,746.56,107.62,3884.42,179.4,758.97,515.0,9529.86,685.65,167.27,163.77,1325.0,203.2,458.26,9365.47,240.21000000000004,1651.23,881.24,1518.81,353.65,906.23,3327.29,30.300000000000004,192.45,31.929999999999996,1190.82,429.66,150.68,270.8,237.78,183.20000000000002,1058.1399999999999,563.1,1387.4499999999998,1704.11,342.4,304.17,314.67,210.5,36.13,946.72,442.28999999999996,230.4,338.31,129.48000000000002,199.10999999999999,3127.9700000000003,1027.03,7681.71,277.4,360.0,416.6,429.78,1610.82,2354.97,450.85,111.25,198.47,826.91,281.05,5287.64,2414.18,617.83,579.89,2129.45,521.78,365.1,1461.63,2449.75,8821.55,3957.04,397.76,579.67,19960.3,7.49,109.47,179.52,2333.3199999999997,8389.42,2533.37,98.7,2349.5,2543.55,549.12,113.64999999999999,1766.46,1285.04,2153.48,83284.38,1597.76,1254.41,168.73,2118.12,621.54,737.0500000000001,473.49,187.51999999999998,1744.61,817.46,566.89,2428.08,8018.77,2871.74,546.25,1460.92,93.35000000000001,965.2,347.54,218.53,565.9,318.65,9729.09,83.0,1771.04,660.27,2018.6999999999998,1160.41,554.67,795.98,620.85,498.15000000000003,509.75,1163.39,209.4,639.56,191.74,934.19,2853.4900000000002,2456.86,266.63,5496.39,449.49,304.97,312.68,3412.78,80.28,824.48,208.3,166.63,823.4,502.52,889.44,1388.47,2121.18,304.18,5579.87,1033.81,3686.61,2085.61,518.14,1089.44,383.96999999999997,718.04,204.62,3475.7200000000003,431.11,119.92,312.9,1203.53,487.75,167.16,1468.82,569.22,81.15,1494.6,345.71,988.04,948.93,5284.97,816.9300000000001,510.72,2376.1020000000003,639.93,283.19,991.7900000000001,310.43,1298.99,834.4399999999999,588.05,937.07,4811.16,313.7,797.9,377.71,1853.7,549.05,606.4100000000001,6001.49,2572.38,683.9200000000001,100.4,178.98000000000002,845.01,383.07,106.47999999999999,1687.49,389.49,426.24,5711.9400000000005,379.7,2062.2000000000003,2965.91,662.2,549.28,1269.0900000000001,2193.56,160.5,250.26,59.400000000000006,690.3000000000001,2753.41,1293.72,302.04,610.6,280.51,2095.0,266.25,620.06,1893.93,2577.54,60.0,9839.55,1362.63,1136.89,787.99,2815.17,71.95,368.75,1182.3799999999999,7377.46,1253.18,239.87,454.66,1839.52,732.91,961.9000000000001,816.56,6145.37,2146.95,1309.01,228.96,491.17,220.44,496.15999999999997,1290.03,284.4,6211.42,348.59,148.33,24.75,94.92,369.34000000000003,1574.51,653.3100000000001,2553.06,9178.26,247.5,233.35,207.04,6650.17,4659.47,3484.5299999999997,2533.09,308.0,650.16,225.15,93.6,2310.38,797.96,1634.64,55942.74,2005.3400000000001,492.47999999999996,129.75,3961.42,711.18,434.6,140.22000000000003,1439.72,1388.81,107.24,1909.13,767.91,4892.68,240.5,373.02,277.93,1246.95,105.5,382.2,343.7,1162.0,547.6,1770.23,111.15,2336.0,284.4,664.66,8703.210000000001,181.35,270.15000000000003,613.08,678.11,2242.81,1786.57,3203.74,275.8,6251.26,1352.96,59.400000000000006,4009.66,1002.9300000000001,139.32,60.959999999999994,1211.75,246.5,1174.55,1788.8,1293.72,7777.84,1385.38,625.2,468.65,2233.8,1216.66,289.87,1376.0900000000001,659.48,3103.26,508.20000000000005,586.1,216.95000000000002,1203.77,170.88,257.06,310.74,465.15999999999997,131.25,1234.88,6798.72,588.5,449.42,743.0,2854.98,1522.27,223.39,214.44,1941.1200000000001,1134.85,668.97,954.75,153.24,149.57,1525.5800000000002,193.29000000000002,1269.05,1254.21,1900.64,171.0,399.75,337.71000000000004,294.17,226.86,1256.63,126.89,38.25,1656.93,208.79999999999998,202.82999999999998,429.7,502.45,5281.53,1225.93,962.97,335.7,63.419999999999995,837.53,274.8,150.35,2209.98,368.4,583.56,147.4,5373.5,1993.34,3537.6,592.56,1061.69,3186.01,102.0,231.32,340.6,1285.3300000000002,210.85,2168.27,2422.02,246.5,146.14000000000001,7395.19,12633.18,158.65,399.95,3548.34,116.26,349.87,1058.25,152.41,210.58,12992.33,2368.34,171.81,2268.44,344.61,4098.04,16847.18,151.4,430.78,3126.36,1116.72,311.37,2174.12,162.25,541.47,259.09,3358.3999999999996,247.38,855.71,114.4,4615.72,135.32999999999998,1079.1000000000001,5146.45,158.09,652.7,1635.42,1243.75,162.4,7267.83,202.68,1109.4,153.52,1133.8,536.88,1459.02,70.74000000000001,227.87,2207.74,5105.32,3241.92,1431.78,597.23,774.84,30.1,137.15,923.03,1375.39,4076.9700000000003,95.96,547.34,1139.1,608.4000000000001,579.6,1496.6,217.9,357.6,1723.45,238.8,307.34,488.32,1199.31,8183.56,1932.64,40.0,738.57,2310.83,1604.21,692.3,1405.83,433.71999999999997,3147.36,10974.23,317.11,828.5500000000001,389.18,4143.05,2136.02,1702.1399999999999,391.72,566.9,2782.02,2240.9,295.05,596.67,4335.96,311.1,100.35,719.82,574.23,2265.2,639.76,11411.92,3458.35,105.78000000000002,443.16,526.97,169.0,1354.35,553.88,612.76,1068.2,1399.3700000000001,226.04,1212.13,906.15,283.73,867.6,1764.86,632.9399999999999,152.15,45.300000000000004,3478.09,4501.63,218.35,648.4,1473.24,3173.9,1563.54,318.24,330.79,173.95,55.980000000000004,90.0,190.57,266.93,434.5,37.5,4331.41,721.06,1170.1100000000001,246.75,46.2,140.39000000000001,1441.04,969.19,194.6,898.87,420.25,2400.05,654.09,453.93,658.02,1913.55,456.70000000000005,293.79,5165.9,140.49,3022.79,740.0,92.6,321.91,4212.65,544.6,107.55000000000001,605.7,125.3,1387.95,923.85,327.68,1767.12,8560.550000000001,208.62,156.87,1553.17,465.45000000000005,4330.461,1380.44,1557.96,2579.04,3628.87,2225.94,577.8900000000001,13916.34,143.05,1223.1100000000001,370.63,516.0,14687.06,32451.6,321.45,1006.54,280.88,138.84,1017.54,45.6,715.5,307.95,124.08000000000001,1399.36,848.3299999999999,3640.96,3582.62,221.82999999999998,1610.71,1995.65,933.84,1433.96,777.0,1369.8600000000001,2463.02,613.6800000000001,1535.37,537.1999999999999,530.18,1513.84,4660.75,913.62,14482.03,758.67,1607.06,1557.38,3400.33,689.11,144.14999999999998,2766.4,340.5,2029.34,954.59,3882.17,890.94,943.97,1957.98,1108.82,80.4,1539.24,385.03000000000003,40519.92,1518.35,548.8,714.51,107.01,433.3,137.99,771.87,109.73,96.55,2609.16,340.48,17703.63,273.48,2508.77,438.46,1498.8,1222.28,2033.64,286.26,244.44000000000003,1346.29,5224.81,94.53,171.47,506.02,33.099999999999994,413.0,313.91999999999996,6878.88,108.52,1449.1399999999999,330.77,1137.8,329.03000000000003,979.62,244.60000000000002,1572.47,871.78,1027.15,260.85,1279.6,535.65,128.2,305.28000000000003,151.44,4129.26,56.25,549.29,335.99,145.01,3813.0800000000004,114.69,962.5,758.62,1432.29,1700.24,810.08,6.300000000000001,205.99,364.45,1874.83,407.07,248.65,214.07999999999998,401.64,452.23,369.5,127.08,102.82,134.44,106.54,293.53,594.0,239.9,2190.66,282.79,330.23,2945.38,405.0,271.28999999999996,1839.1399999999999,646.27,2304.18,553.85,5376.2,304.95,1990.8,190.75,4242.1,617.6,492.36,357.77,542.89,410.95,451.28999999999996,289.48,269.21000000000004,2119.26,426.53000000000003,2568.32,404.55,460.95,703.22,1702.98,1769.33,680.62,1266.77,2180.64,329.22,4144.82,5954.29,324.51,2458.9,5735.09,1572.8600000000001,186.3,216.7,582.11,368.31,1205.64,150.55,854.61,761.4100000000001,694.54,617.07,5193.92,20.4,583.39,169.21,2878.46,2146.39,1786.78,231.42,1359.25,4860.35,1302.46,2486.0,1563.3,20009.84,453.45,980.91,1333.7,289.23,13542.92,487.03,232.96,1113.74,82.2,41184.3,1965.72,1985.6299999999999,473.62,217.73,1797.92,654.32,456.4,1103.35,1067.85,1082.6000000000001,340.84000000000003,2094.23,235.78,107.14,818.7900000000001,777.34,162.8,188.44,2122.89,1327.98,1591.73,62.7,1718.66,541.95,89.65,1709.83,109.31,1992.13,3269.11,115.73,457.7,750.5600000000001,3595.77,252.5,736.9300000000001,2300.4,283.79,385.40000000000003,1637.14,106.75,1033.8,1359.52,1743.6,1739.92,1437.43,263.49,180.56,770.08,2617.33,447.05,1264.3500000000001,2435.67,841.3,582.64,1938.46,4762.07,2476.25,2049.23,1393.42,327.6,460.64,549.33,892.92,422.19,4376.25,1297.45,1658.3700000000001,595.38,316.92,1523.49,4292.35,8356.83,145.06,1920.83,1396.05,987.48,6450.88,664.1999999999999,8403.22,1527.25,301.9,237.42,422.7,910.35,475.74,832.4200000000001,335.54,2485.89,3005.9500000000003,212.25,2023.91,310.3,141.70000000000002,2342.57,3309.2599999999998,796.9,3096.14,628.32,884.59,7210.59,1992.98,79.9,435.83,1386.1200000000001,6848.87,4983.95,744.34,1237.2,798.85,2052.69,1915.36,414.7,5995.69,3354.0,739.1,115.31,5019.17,105.80000000000001,2508.4700000000003,10497.300000000001,351.3,315.98,308.59999999999997,323.25,93.3,2016.71,629.36,942.75,4539.23,248.65,2136.72,521.64,534.53,1062.48,2052.81,2776.19,1884.16,17403.21,543.5699999999999,603.37,975.5,1355.29,40.8,1325.8,498.5,1087.05,321.24,1762.99,417.05,713.0,885.71,1857.26,1074.47,1722.09,1429.03,333.18,1186.4,136.65,63.66,1078.0,1680.2,3824.79,1056.63,821.35,401.94,128.31,295.09000000000003,3588.34,967.44,2284.34,876.9399999999999,6739.01,340.94,312.62,755.15,348.16,823.53,1488.17,896.06,186.66,717.57,1993.18,318.34000000000003,351.75,1591.24,1818.79,188.69,551.46,4195.5,238.05,1361.96,1139.28,2779.51,205.8,176.9,328.95,864.48,129.85,136.31,1957.7,117.72,951.51,2408.96,126.05000000000001,1313.67,487.5,193.65,1367.32,107.33,2567.64,381.66,560.55,1194.92,2560.0,460.08,171.15,1491.4,176.34,2806.0,850.86,4590.53,230.9,146.16,3003.26,1682.03,6874.97,310.83,1664.92,373.46999999999997,2037.7,417.21000000000004,263.73,2753.52,3517.69,961.93,1878.92,285.54,741.3199999999999,613.33,224.51,2595.51,328.77,1137.42,3176.13,3323.75,723.66,416.35,204.10000000000002,209.49,892.1500000000001,946.83,212.17,1766.78,1220.83,549.8100000000001,141.53,1111.15,157.2,934.58,309.49,1256.67,104.0,632.28,247.85,353.1,2252.93,774.4499999999999,473.69,765.59,788.1,186.05,467.5,1052.26,538.81,2441.61,2036.35,137.8,123.13,222.20000000000002,2755.92,2750.83,205.65,374.01,681.97,231.93,3629.76,130.32,673.24,457.2,259.83,424.96000000000004,496.49,1906.48,35.7,397.62,504.99,75.0,526.27,28904.670000000002,309.5,638.49,817.09,200.26,1161.89,1003.0699999999999,319.14,301.25,310.70000000000005,216.3,637.93,282.68,162.24,2971.21,352.65,471.19,178.13,1193.81,746.1800000000001,332.17,251.71,24.05,2535.69,4630.16,2208.15,71.78999999999999,1402.91,746.24,2171.13,2869.16,712.62,178.48000000000002,1664.08,966.03,789.6,154.72,350.35,1825.56,453.38,1923.54,134.36,209.4,303.7,205.41000000000003,801.66,385.56,780.46,731.25,3729.59,2212.89,443.05,1191.23,258.55,2182.41,142.71,240.95,1777.5,842.16,1554.92,475.05,327.68,1427.57,1140.11,433.36,2497.86,306.19,11535.29,426.85,1004.88,1531.55,48.349999999999994,1053.74,2007.84,635.9,378.28999999999996,23.35,395.07,2343.2,205.75,424.03,137.7,292.21,376.70000000000005,350.52,4819.32,104.25999999999999,30.15,8746.9,7069.27,1315.0,95.29,10864.19,2004.6200000000001,1629.79,4698.79,5535.02,175.0,2206.9700000000003,223.85999999999999,3334.82,87.8,599.35,1020.9000000000001,1331.03,3695.91,2827.05,850.34,540.87,380.0,156.6,295.05,297.95,2220.09,341.59,998.83,1361.28,153.12,605.9,3694.23,741.11,1397.59,329.48,172.8,1339.89,434.3,235.25,432.45,60.48,357.66,2359.16,1286.6200000000001,1655.64,2057.77,673.6,291.5,1712.1200000000001,1579.97,225.4,885.79,1320.95,130.05,106.80000000000001,1892.52,406.4,1596.71,473.0,3802.04,749.8100000000001,490.47,179.4,323.40000000000003,389.12,435.6,160.39,678.8,327.84,382.14,306.58000000000004,1397.69,136.2,10776.42,1110.2,2633.74,8207.37,690.83,611.9000000000001,932.76,151.05,165.0,109.2,98.73,555.7,3934.25,171.13,608.86,769.78,880.62,663.31,637.75,151.5,320.25,80489.21,1921.19,157.63,127.45,178.83,638.6999999999999,701.62,98.30000000000001,191.9,994.6,4091.16,4518.43,1110.83,664.16,29480.061,533.41,6026.14,3823.78,1217.45,621.5600000000001,7619.21,368.27,323.8,1248.48,3127.85,3006.08,2174.13,605.79,4258.2699999999995,654.96,147.35,1052.14,705.14,1121.45,170.3,3783.89,259.68,3970.0,2956.56,417.59999999999997,463.43,226.91,173.06,2334.97,1999.02,11366.64,2121.43,4157.88,207.5,65500.07,1389.25,1072.97,163.65,228.0,352.85,420.75,2481.2,1220.48,755.51,1152.63,1706.99,3698.0499999999997,249.65,905.96,1361.23,220.97,1069.56,22554.22,174.16,623.0,7732.37,391.95,784.73,533.78,51.839999999999996,1017.53,1334.54,2251.85,513.08,856.1999999999999,2062.41,1802.02,841.16,452.78000000000003,582.61,630.75,1002.29,812.74,532.5,1912.65,5781.4400000000005,406.02,1200.15,182.59,113.35,181.39,1884.71,225.6,1051.71,392.13,316.1,881.59,91.8,218.45999999999998,1302.51,485.5,1357.56,530.5,570.76,5716.14,139.85,2136.2400000000002,9374.86,614.34,253.88,628.09,421.45000000000005,1130.2,1408.96,323.09000000000003,352.8,4292.59,1184.6,1610.62,772.83,662.33,495.72,409.25,130.95,76.74000000000001,384.56,450.15999999999997,533.11,7493.77,209.89000000000001,2139.0,136.01,321.78,2101.61,641.13,234.19,134.0,465.84000000000003,434.2,947.98,285.41,1202.77,236.56,249.89000000000001,1306.96,900.63,103.95,496.26,424.71000000000004,204.26,678.55,1360.31,1923.57,851.33,722.0500000000001,252.59,255.25,1904.93,310.46999999999997,921.5,536.04,1460.3600000000001,2137.72,2515.76,355.45,255.03,611.08,6088.05,125.65,1169.6100000000001,1199.08,629.4,262.32,2536.07,1152.1100000000001,530.86,286.39,196.25,4283.68,433.85,3377.78,33.0,482.76,101.36,4202.66,1222.04,548.73,433.75,2535.21,628.22,3996.7999999999997,790.74,778.49,1483.83,560.39,251.04,207.85,71.4,749.0,127.0,365.5,926.12,496.0,424.55,1007.74,394.37,1748.15,5891.01,5459.04,3015.55,904.6,256.55,165.0,1041.21,3862.19,1950.9499999999998,666.6500000000001,107.0,406.3,2651.23,1077.82,828.21,1012.22,622.36,853.49,230.17000000000002,81.36,242.06,400.8,360.61,552.4,5416.349999999999,1367.6,2703.03,935.62,1182.22,189.0,800.18,1152.87,54.15,433.07,1593.83,1421.3,2393.5,2984.32,302.65000000000003,530.9,180.06,176.9,630.77,1954.99,13.52,583.71,858.47,11013.82,488.24,1264.05,1188.33,1146.33,4287.5,1462.95,465.6,232.85,128.60000000000002,294.74,2506.54,645.8100000000001,2177.39,194.7,228.34,454.54,4601.69,3084.58,133.18,1364.68,273.35,203.05,1402.51,401.66999999999996,306.0,669.91,660.48,2728.18,2644.01,2253.22,6285.27,221.53,128.45,1105.53,3961.44,3737.38,235.88,100.2,1248.09,2675.8,363.66,591.0699999999999,582.17,885.3199999999999,424.3,474.03,481.58,5255.24,174.95,410.42,336.59999999999997,227.97,197.57,2305.59,1923.99,389.78,314.74,432.18,631.87,1026.98,1155.0300000000002,31.85,1220.9099999999999,9730.09,212.1,798.03,231.68,1099.81,4036.77,8915.99,727.58,696.25,880.97,3120.91,5102.86,1485.36,143.10000000000002,1357.95,231.90000000000003,327.0,640.76,591.87,4832.9,713.03,560.71,6691.13,856.01,406.68,1635.64,867.11,400.2,320.95,178.68,159.68,638.81,596.1899999999999,119.5,432.96,795.47,1452.8,225.45000000000002,321.3,198.41000000000003,229.46,263.76,2118.7000000000003,569.88,1281.83,2726.77,75.28,610.74,549.88,562.98,327.75,521.66,135.04000000000002,356.71,853.3199999999999,160.65,2004.01,3940.13,615.27,292.62,111.15,595.7,251.72,1729.43,567.5600000000001,2401.07,186.15,63.6,96.6,2226.96,144.9,1269.76,642.14,1250.86,433.99,100.86,131.15,501.97999999999996,1081.8600000000001,1269.8899999999999,5750.08,3940.08,369.98,1354.82,1013.13,679.44,3982.66,806.95,130.6,2736.64,1754.24,11920.43,871.14,200.18,38.92,181.89000000000001,68.03,529.15,198.29,490.21999999999997,258.93,189.78,417.3,1239.11,120.95,221.59,1504.26,268.28,205.14,970.25,491.81,601.4499999999999,204.3,141.12,465.4,114.1,502.49,25.2,118.85999999999999,797.8499999999999,623.08,188.46,1140.58,407.94,237.81,3000.5,1050.8,1110.78,974.95,271.75,1896.57,467.78999999999996,338.13,455.84000000000003,336.62,393.64,131.15,111.5,2135.46,1151.04,3999.8900000000003,4511.76,237.64000000000001,1025.1,2181.71,2376.51,3318.35,124.45,157.32999999999998,29.1,1695.15,1008.05,922.59,644.52,1311.64,512.77,312.35,152.35,104.7,475.42,59.900000000000006,838.27,328.13,497.04,81.12,5395.79,119.0,9935.93,1006.53,1227.54,1066.3,299.74,1066.94,2509.84,1285.69,674.16,612.381,1401.08,36.25,1502.0,1740.8999999999999,273.31,768.25,500.2,156.6,632.27,383.59000000000003,711.8100000000001,920.46,7510.24,3417.13,1077.24,879.42,979.1999999999999,236.97,3010.13,520.58,231.34,260.45,250.44,14174.25,10.95,645.0,19726.76,836.88,110.69999999999999,171.15,126.35000000000001,1415.81,25381.65,149.3,168.65,381.15,97.5,1476.3,516.87,2525.44,114.85,2461.68,118.95,15874.3,2704.63,552.9,283.44000000000005,7021.66,646.44,3159.46,560.14,1471.52,10405.03,164.18,206.60000000000002,2273.95,485.6,1059.41,1615.39,420.54,3040.51,14562.91,832.54,469.40999999999997,1363.23,266.8,331.35,317.76,369.68,1034.32,325.74,137.16,293.35,2371.74,14523.67,2348.94,52422.3,1030.41,975.5600000000001,4247.11,213.08,1605.8400000000001,659.4,617.1800000000001,2542.79,102.6,57.8,3180.28,1679.8,321.65000000000003,224.81,607.0,1047.0,489.86,1144.3,1272.8,389.26000000000005,370.40999999999997,775.64,177.69,209.57,160.55,336.5,658.18,2422.2000000000003,747.85,100.91000000000001,54.699999999999996,309.16,1141.84,287.25,718.51,6493.14,1289.64,4191.92,206.07999999999998,84541.17,2542.83,645.6,313.35,963.49,182.62,329.42,3265.45,554.85,394.14,739.2,2970.14,1240.25,725.21,970.12,65.34,34.8,1445.47,288.33,1176.84,1011.7,119.86,170.1,120.05,944.82,767.54,727.85,627.96,2239.48,2154.88,2270.0,565.43,142.77,34.0,259.17,167.62,1040.85,537.9,392.38,131.83,1024.55,374.45,100.0,205.23999999999998,496.26000000000005,171.7,5266.7699999999995,348.75,903.87,304.38,325.3,7712.71,1271.68,135.95,4067.12,948.03,167.4,176.71,1699.35,11433.77,519.95,148.3,4168.46,1504.7,2075.7400000000002,1439.72,409.98,52.05,297.07,2300.01,1393.66,633.02,160.75,87.30000000000001,333.81,296.07,782.12,1511.94,4376.41,3346.41,392.2,489.59999999999997,385.19,3071.36,1795.22,1130.21,1838.72,725.15,3958.5,254.0,296.36,641.4,470.16,732.55,1265.58,842.4,506.67,2547.28,1112.2,439.7,3132.96,3141.4,198.6,418.87,710.4,1758.52,1013.54,126.0,835.33,163.47,412.92,3372.92,561.59,563.1,1319.6399999999999,1895.19,2735.63,830.63,2600.46,2578.91,300.66,17885.32,1378.16,20329.68,301.65,1585.5,313.05,731.2,1982.45,819.01,2751.64,2557.35,432.68,304.2,317.08,2685.2,489.65000000000003,2234.86,1412.85,980.7,5393.88,442.0,4145.33,1319.67,296.1,260.38,10245.49,456.90000000000003,620.85,533.56,293.5,1198.09,1257.18,163.2,10300.98,376.92,299.83,1572.97,307.21000000000004,2006.32,102.0,1092.06,1362.75,438.65000000000003,2475.32,468.88,2166.42,741.69,4134.91,2304.51,4939.24,426.9,101.1,1743.3700000000001,229.2,326.74,600.34,793.9300000000001,358.15,351.66,157.85999999999999,382.06,2786.57,728.75,3773.11,1555.83,441.92,631.34,129.75,71.15,741.02,156.37,299.61,141.85,1210.83,3095.95,141.6,187.3,575.37,2060.1,330.56,197.70000000000002,346.56,679.02,93.49,112.86,271.44,736.28,171.25,815.11,368.35,147.3,661.04,1191.44,178.07,3207.76,770.03,3761.38,184.66,383.78000000000003,414.72,437.87,262.64,5925.4800000000005,373.16,1757.98,405.48,1609.37,591.96,75.76,400.46999999999997,124.78,2907.65,180.60999999999999,334.62,2162.93,449.25,701.96,133.34,388.84,3867.5,338.46000000000004,689.6700000000001,3786.7000000000003,175.9,29562.02,430.69,114.92,596.09,129.69,2657.91,457.96000000000004,51208.87,1552.35,543.47,1596.29,757.24,208.97,1803.2,7614.24,717.39,2990.41,923.99,2585.2,321.72,15785.87,805.3000000000001,424.18,227.15,599.78,1213.8,913.05,235.87,4621.32,167.1,3424.02,999.48,366.0,291.56,324.08,2325.92,703.27,1091.44,545.05,1292.39,1968.75,147.12,149.7,216.39999999999998,381.09000000000003,1580.3799999999999,291.76,1142.47,110.05,261.7,64.49,3020.64,225.78,43.87,396.39,536.28,20137.24,1574.13,170.4,289.5,574.2,244.08,638.2,294.0,128.7,2239.1,2534.74,2993.88,114.69,344.90000000000003,540.35,144.29,26286.75,1128.72,74.99,360.35,60117.6,947.26,627.5400000000001,788.46,159.17000000000002,1618.93,392.6,465.42,140.75,61.300000000000004,1697.85,564.55,643.16,261.76,4558.01,865.49,223.42,590.4399999999999,2126.09,161.94,1035.1,2527.17,321.79,1223.31,1159.99,201.27,239.12,138.18,171.71,999.51,766.87,288.42,207.32999999999998,896.77,206.95,252.88,225.99,1595.1399999999999,419.38,482.33,639.24,338.41,124.25,509.01,244.37,353.51,524.83,215.22,17642.89,1896.84,410.79,115.3,476.90000000000003,165.0,175.85,190.48000000000002,216.41,894.53,1177.72,2074.07,192.05,433.77,451.3,335.52,546.34,1142.37,97.39999999999999,141.48,548.22,4536.29,116.1,287.91,1417.01,189.63,326.09,222.64,500.92,337.66,2282.28,10877.18,271.05,176.5,4381.78,190.95,1188.46,603.9,1185.46,4369.56,912.15,186.60000000000002,3667.83,3473.8,1122.4,2455.68,314.1,975.63,1019.44,3171.54,151.49,1699.25,389.40000000000003,590.78,463.53,704.75,955.8000000000001,10705.8,428.45,321.9,360.85,7111.18,2346.19,3374.2200000000003,1723.04,808.81,1965.52,527.94,451.56,399.61,349164.35,105.8,1019.16,165.88,577.8100000000001,1828.24,193.48000000000002,405.85,420.67,76.32000000000001,9.700000000000001,731.38,1068.05,180.7,92.2,102.65,209.05,399.29,222.95999999999998,220.13,445.0,567.71,847.03,145.99,536.41,882.47,2169.25,2748.19,112.85000000000001,941.88,1226.09,426.69,246.22,533.41,186.88,213.9,365.82,184.41,371.51,728.84,175.5,2538.7599999999998,1897.69,1206.0,9905.56,589.52,58.5,176.1,875.45,4512.93,754.38,821.49,291.13,329.58,724.89,1279.55,313.49,461.16,321.59999999999997,963.59,3188.2000000000003,101.16,770.56,230.1,344.75,447.53000000000003,332.1,314.65999999999997,544.47,360.08,147.45,298.3,1780.34,1933.15,323.29,479.91999999999996,1493.1200000000001,1616.48,383.33,7516.31,7539.84,6650.83,295.0,3526.81,1480.8600000000001,4782.400000000001,179.6,913.11,270.92,588.75,913.9,1372.72,515.78,568.64,3757.92,162.72,839.73,21964.14,1015.42,317.88,267.9,199.85,110.7,2262.52,2059.67,1979.6000000000001,7318.91,428.06,603.25,251.1,269.37,1464.73,168.6,161.4,680.85,1307.4,153.0,741.85,1320.66,1069.67,240.3,231.33999999999997,307.55,120.32,619.37,461.68,427.0,1296.43,2345.71],"yaxis":"y","type":"scattergl"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Recency"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Monetary"}},"coloraxis":{"colorbar":{"title":{"text":"Frequency"}},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"legend":{"tracegroupgap":0,"itemsizing":"constant"},"title":{"text":"Customer Segmentation: RFM Analysis"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e2879edc-ff4d-4b8f-8220-4571925eca23');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# Top 10 Selling Products
fig = px.bar(
    x=top_selling_products['Total Sales'],  
    y=top_selling_products.index,         
    orientation='h', 
    title='Top 10 Products by Sales'
)
fig.update_layout(
    xaxis_title='Total Sales', 
    yaxis_title='Product Description'
)
fig.show()

```


<div>                            <div id="7ca74bbc-d12a-42d3-9bd0-f8fd9fc63ec2" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("7ca74bbc-d12a-42d3-9bd0-f8fd9fc63ec2")) {                    Plotly.newPlot(                        "7ca74bbc-d12a-42d3-9bd0-f8fd9fc63ec2",                        [{"alignmentgroup":"True","hovertemplate":"x=%{x}<br>y=%{y}<extra></extra>","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"h","showlegend":false,"textposition":"auto","x":[151339.16,143727.6,98531.99,70291.03,51644.25,48741.08,40156.049999999996,36871.55,35017.3,34044.75],"xaxis":"x","y":["WHITE HANGING HEART T-LIGHT HOLDER","REGENCY CAKESTAND 3 TIER","Manual","ASSORTED COLOUR BIRD ORNAMENT","JUMBO BAG RED RETROSPOT","POSTAGE","ROTATING SILVER ANGELS T-LIGHT HLDR","PAPER CHAIN KIT 50'S CHRISTMAS ","PARTY BUNTING","EDWARDIAN PARASOL NATURAL"],"yaxis":"y","type":"bar"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Total Sales"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Product Description"}},"legend":{"tracegroupgap":0},"title":{"text":"Top 10 Products by Sales"},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('7ca74bbc-d12a-42d3-9bd0-f8fd9fc63ec2');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# Creating heatmap for monthly sales by country

# Convert Period to string
data['Month'] = data['InvoiceDate'].dt.to_period('M').astype(str)

# Group data for the heatmap
heatmap_data = data.groupby(['Country', 'Month'])['Total Sales'].sum().unstack()

# Create the heatmap
fig = px.imshow(
    heatmap_data,
    labels=dict(x="Month", y="Country", color="Total Sales"),
    x=heatmap_data.columns,
    y=heatmap_data.index,
    color_continuous_scale="Viridis",
    title="Monthly Sales by Country"
)
fig.update_layout(xaxis_title="Month", yaxis_title="Country")
fig.show()


```


<div>                            <div id="8e8c9bbf-f3ea-41a1-93f2-5fe32f62e229" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("8e8c9bbf-f3ea-41a1-93f2-5fe32f62e229")) {                    Plotly.newPlot(                        "8e8c9bbf-f3ea-41a1-93f2-5fe32f62e229",                        [{"coloraxis":"coloraxis","name":"0","x":["2009-12","2010-01","2010-02","2010-03","2010-04","2010-05","2010-06","2010-07","2010-08","2010-09","2010-10","2010-11","2010-12"],"y":["Australia","Austria","Bahrain","Belgium","Brazil","Canada","Channel Islands","Cyprus","Denmark","EIRE","Finland","France","Germany","Greece","Iceland","Israel","Italy","Japan","Korea","Lithuania","Malta","Netherlands","Nigeria","Norway","Poland","Portugal","RSA","Singapore","Spain","Sweden","Switzerland","Thailand","USA","United Arab Emirates","United Kingdom","Unspecified","West Indies"],"z":[[271.1,null,1029.66,429.39,630.95,2371.15,3214.78,686.12,176.0,785.83,2989.15,18245.52,617.15],[1998.3400000000001,null,1118.8600000000001,925.13,1388.54,622.04,268.1,null,1043.15,1940.17,1236.7,2873.3,null],[null,null,null,null,null,488.21000000000004,null,null,null,null,317.76,null,null],[447.6,1634.02,3209.0,884.52,2570.39,98.15,3800.34,1914.23,1713.62,1190.91,1475.72,5252.9800000000005,346.1],[null,null,null,null,null,null,null,null,null,268.27,null,null,null],[null,null,null,null,null,null,null,null,null,null,834.89,381.77000000000004,null],[989.18,838.75,null,1065.12,926.82,5378.52,845.0,null,1363.14,6156.34,4341.19,2278.73,363.53],[3541.6800000000003,76.52000000000001,750.49,2879.19,1151.7,515.61,489.8,null,null,993.1800000000001,551.33,397.59999999999997,null],[1437.66,7870.6,19809.899999999998,7595.18,748.0,null,2695.5099999999998,591.6600000000001,2643.03,1725.25,448.6,4059.96,1281.5],[18170.46,65031.41,20206.46,22989.46,20668.08,18430.91,22934.34,31987.23,22919.36,36322.78,43929.86,27717.73,4733.78],[549.08,801.42,null,null,1255.13,837.11,534.14,956.23,null,864.68,1166.69,414.98,null],[6521.69,8465.63,9024.16,8637.79,7405.18,13912.55,9172.54,11399.15,16570.32,14531.33,15207.57,18968.74,6290.42],[9830.27,12562.11,9124.25,16698.08,23683.231,9108.36,15668.43,20274.29,9943.119999999999,17708.68,21499.98,30240.51,5684.08],[610.95,4798.35,null,522.73,277.93,2476.22,302.4,null,721.7099999999999,780.06,3084.02,761.3,null],[null,null,null,null,null,null,null,null,null,null,611.53,null,711.79],[null,null,null,null,null,null,null,1950.98,null,null,null,1248.42,null],[422.35,39.67,null,734.22,1525.85,2373.45,267.45,null,null,1380.56,5208.22,2673.16,427.8],[null,2.95,193.72,110.4,null,null,594.68,1253.3,null,1895.0,1533.8899999999999,23.6,4114.48],[null,null,null,null,null,null,null,null,null,168.69000000000003,null,949.82,null],[null,null,null,null,null,null,null,1706.23,null,null,null,1525.39,1661.06],[null,null,1636.38,null,null,null,null,null,3737.12,null,null,null,null],[15204.73,29230.77,17255.39,24447.98,6888.78,22416.53,24224.51,5532.64,25028.69,45037.14,17265.85,36058.74,192.60000000000002],[null,27.82,null,null,null,null,null,null,112.57000000000001,null,null,null,null],[485.31,null,null,13916.34,null,null,null,null,152.4,973.69,null,4629.32,3787.12],[371.82,null,null,318.86,null,null,556.16,374.75,527.4399999999999,349.47,null,821.14,248.16],[2821.58,117.72,458.83000000000004,2519.05,85.53999999999999,757.2,3668.46,288.1,463.21999999999997,2964.58,2978.23,5056.43,1664.52],[null,null,931.43,null,null,null,null,null,null,null,null,null,null],[null,null,null,null,1252.95,null,null,null,null,2784.82,null,null,null],[7950.18,4179.59,554.04,1671.77,833.98,2854.58,2332.2,1134.81,1299.99,3770.45,14066.13,6126.21,794.72],[285.29999999999995,1680.15,8202.58,1314.18,17698.12,3556.7599999999998,914.3000000000001,2590.78,2702.94,2899.56,9369.41,1933.91,null],[589.4,null,477.68,951.62,1939.76,9825.16,2362.47,743.99,2053.05,2474.44,8629.69,13570.73,303.4],[null,null,null,null,null,null,1956.84,1113.7,null,null,null,null,null],[141.0,2289.59,null,null,null,416.31,896.0699999999999,508.1,null,null,132.8,402.59999999999997,null],[517.7,null,1310.3000000000002,1201.52,null,null,null,null,2047.28,null,509.92,1713.69,null],[610346.63,415120.502,409265.826,587165.941,500798.551,501394.56,538672.61,502350.0,506470.04,679069.921,875722.88,977832.812,277434.16000000003],[null,1035.1,null,null,252.51999999999998,null,null,2379.88,null,1978.15,null,300.93,null],[null,null,null,null,null,null,null,null,536.41,null,null,null,null]],"type":"heatmap","xaxis":"x","yaxis":"y","hovertemplate":"Month: %{x}<br>Country: %{y}<br>Total Sales: %{z}<extra></extra>"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"scaleanchor":"y","constrain":"domain","title":{"text":"Month"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"autorange":"reversed","constrain":"domain","title":{"text":"Country"}},"coloraxis":{"colorbar":{"title":{"text":"Total Sales"}},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"title":{"text":"Monthly Sales by Country"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('8e8c9bbf-f3ea-41a1-93f2-5fe32f62e229');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
# Boxplot for Total Sales by Country
fig = px.box(data, x='Total Sales', title='Total Sales Distribution')
fig.update_layout(xaxis_title='Total Sales')
fig.show()


```



var gd = document.getElementById('0d5bcfc6-2aee-4dca-88f9-46bbc30c85fb');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## INSIGHTS GAINED THROUGH ANALYSIS


1. **Sales Trends and Revenue Analysis**

**Insight 1: Monthly Sales Trends**
- **Observation:** Sales culminate during June, March and more significantly during November with a sum of 1,166,460 before the holiday season while a decline of 855,803.6 occurs in December, followed by steady growth throughout the year.
- **Actionable Decision:** Allocate additional marketing budget and resources (such as inventory, staffing, and logistics)during November prior to the holiday season as customers start purchasing their gift items, groceries and other products in bulk which are useful to them before seasonal events like Christmas and New Year. Plan clearance sales in January to mitigate the seasonal drop in sales.

**Insight 2: Revenue Concentration**
- **Observation:** The top 10% of customers contribute to approximately 50% of the revenue.
- **Actionable Decision:** Focus on retaining high-value customers through loyalty programs, personalized offers, and exclusive deals. Segment and prioritize customer outreach based on revenue contribution.


2. **Customer Behavior**

**Insight 3: Customer Segmentation (RFM Analysis)**
- **Observation:** A small group of customers (e.g 5%) are highly frequent buyers with significant spending while many others show high recency, indicating they haven't purchased recently.
- **Actionable Decision:** Offer exclusive benefits (e.g. free shipp, free giftsing, discounts) for loyal customers. For customers with high recency but low frequency or monetary value, implement win-back campaigns such as email reminders or limited-time offers.

**Insight 4: High Returns from Certain Customers**
- **Observation:** Some customers have a disproportionately high rate of returns affecting sales. The majority of customers have low-value purchase influence which affects profitability.
- **Actionable Decision** Use feedback loops (e.g. surveys) to idenify reasons (e.g. product quality, inaccurate descriptions) for churn and address te descriptions). Introduce stricter return policies or provide education these customers through targeted marketing to encourage  higher spending over product use.
  

3. **Product Insights**

**Insight 5: Top Selling Products** 
- **Observation:** A small amount of products accounts for the majority of sale with WHITE HANGING HEART T-LIGHT HOLDER product having the highest revenue of a total sum of 151,339.2 and the highest total demand of 56814 in the year 2010. REGENCY CAKE STAND 3 TIER happens to be the top 2 selling  product with a total revenue of 143,727 and has a total demand of 12484 as the 2nd highest demanded product throughout the year while MANUAL happens to be the top 3  selling product with the total revenue of 98,532 and has a demand of 2568 throughout the year.
- **Actionable Decision:** For seasonal products, ensure inventory is stocked up. For consistently popular products like White Hanging Heart T-Light Holder and negotiate better pricing with suppliers or bundle them with slower-moving items like Pads To Match All Cushions.

**Insight 6: Low-Performing Products**
- **Observation** Several products have negligible sales or very low return rates with PADS TO MATCH ALL CUSHIONS product having a total sales of 0.014 and only 14 demands throughout the year.
- **Actionable Decision:** Discontinue underperforming products to free up inventory space. Investigate if the low performance is due to poor visibility, high pricing, or a lack of demand.

  
5. **Geographical Insights**

**Insight 7: Revenue Concentration by Country**
- **Observation:** The UK dominates sales, contributing 80% of revenue. Eire and Netherlands follow as the next largest markets.  
- **Actionable Decision:** Invest in marketing campaigns in the UK to maintain market share. Focus on expanding operations in Eire and Netherlands by addressing potential barriers (e.g., shipping costs, localization).

**Insight 8: High Returns in Specific Regions**
- **Observation:** Eire shows a higher return rate compared to other countries.
- **Actionable Decision:** Investigate whether language barriers or unclear product descriptions are causing misunderstandings. Provide localized customer support and clearer return policies in Eire.


6. **Operational Insights**

**Insight 9: Inventory Management**
- **Observation:** Some products have low turnover rates, indicating overstocking, while others frequently go out of stock.
- **Actionable Decision:** Optimize inventory by stocking up on fast-moving items like WHITE HANGING HEART T-LIGHT HOLDER inventory and reducing orders for slow movers like PADS TO MATCH ALL CUSHIONS. Historical sales data can be used to predict future demand and improve forecasting accuracy.

**Insight 10: Order Processing Efficiency**
- **Observation:** International orders take longer to fulfill, leading to potential customer dissatisfaction.
- **Actionable Decision:** Improve supply chain processes for international shipments (e.g., partner with local distributors) and provide accurate delivery estimated at checkout to manage customer expectations.

7. **Anomaly Detection**

**Insight 11: Unusual Order Patterns**
- **Observation:** A significant spike in sales in March resulted from a bulk order by a corporate client.
- **Actionable Decision:** Target similar corporate clients for bulk purchases and offer bulk discounts to encourage repeat orders from this segment.


8. **Revenue Growth Oppurtunities**

**Insight 12: Target EmergingGermany**
- **Observation:** Smaller markets like France and the Germany show steady growth.
- **Actionable Decision:** Invest in marketing and operational strategies to capitalize on opportunities in these emerging markets.

## Summary of Key Recommendations for Stakeholders:

* Leverage RFM segmentation to design targeted marketing campaigns that resonate with different customer segments, thereby maximizing engagement and driving sales.

* Focus on retaining high-value customers by offering personalized incentives and enhancing their shopping experiences, based on their unique preferences and behaviors.

* Utilize sales insights to optimize inventory levels, ensuring that we are adequately stocked with top-performing products while minimizing excess inventory costs.

* Boost holiday season campaigns and prepare inventory to capitalize on high sales months.

* Introduce loyalty and retention programs to nurture high-value customers and win back inactive ones.

* Discontinue underperforming products and focus resources on popular, high-demand items.

* Expand internationally by addressing barriers in high-potential markets like Eire and Netherlands.

* Optimize inventory management to reduce costs associated with overstocking and frequent stockouts.

* Improve supply chain efficiency for international orders to enhance customer satisfaction.

* Target high-value customers or expand into high-growth regions.

* Improve inventory for top-selling products or enhance service for regions with high returns.

* Streamline fulfillment processes to reduce delays. 