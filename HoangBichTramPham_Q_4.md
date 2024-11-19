---
jupyter:
  colab:
    authorship_tag: ABX9TyPb9pjPDWdg5SK8Db/kOmOS
  kernelspec:
    display_name: Python 3
    name: python3
  language_info:
    name: python
  nbformat: 4
  nbformat_minor: 0
---

::: {.cell .markdown id="ndkkMOxl6kvQ"}
# 0.1 Setting up the Data {#01-setting-up-the-data}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":1283,\"status\":\"ok\",\"timestamp\":1728422898944,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="CaIq265B2OdV" outputId="90415d4e-6a1b-437e-a355-e858f2190c59"}
``` python
# prompt: Load google drive

from google.colab import drive
drive.mount('/content/drive')
```

::: {.output .stream .stdout}
    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":11,\"status\":\"ok\",\"timestamp\":1728422898944,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="Wr7Egmrj7DDk" outputId="2c63bef8-5d09-40ae-92b3-830f85391ba4"}
``` python
%cd /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)
```

::: {.output .stream .stdout}
    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":9,\"status\":\"ok\",\"timestamp\":1728422898944,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="FrTKfQFv7Hav" outputId="d70e428b-babe-4413-a152-5138df1e3334"}
``` python
%cd Datasets/
```

::: {.output .stream .stdout}
    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)/Datasets
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":5,\"status\":\"ok\",\"timestamp\":1728422898944,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="Vz8OsByY7I7J" outputId="78da4a64-9668-4354-eb59-ed520f40d2d3"}
``` python
ls
```

::: {.output .stream .stdout}
    online+retail+ii.zip  retail_data/  retail_data.csv
:::
:::

::: {.cell .markdown id="mbhhsSjs7MAw"}
# 0.2 Read in the Data {#02-read-in-the-data}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":211,\"status\":\"ok\",\"timestamp\":1728422899153,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="PtiyOq4R7OXn" outputId="a22705f4-4f93-438c-9f84-de82e2bbceb3"}
``` python
%cd retail_data
```

::: {.output .stream .stdout}
    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)/Datasets/retail_data
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":6,\"status\":\"ok\",\"timestamp\":1728422899153,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="1SksTfAv7Sh8" outputId="925bfaf6-c6c3-4d16-990c-20184e5bee28"}
``` python
ls
```

::: {.output .stream .stdout}
    master_df.csv  online_retail_II.xlsx  rfm_data.csv
:::
:::

::: {.cell .code id="u8v1JAK97Upo"}
``` python
import pandas as pd
```
:::

::: {.cell .code id="Za6cd_d97XJm"}
``` python
df_0 = pd.read_excel('online_retail_II.xlsx', sheet_name = 0)
```
:::

::: {.cell .code id="JLgywz-w7YeL"}
``` python
df_1 = pd.read_excel('online_retail_II.xlsx', sheet_name = 1)
```
:::

::: {.cell .code id="1kfTRdWq7ZvP"}
``` python
master_df = pd.concat([df_0, df_1])
```
:::

::: {.cell .markdown id="tSOtfl7k-e4Q"}
0.3 RFM Calculation using lifetime
:::

::: {.cell .code id="9ldzJN5s8-G8"}
``` python
df = master_df.copy(deep = True)
```
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":4,\"status\":\"ok\",\"timestamp\":1728423052927,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="VBsAlKXE9Eoj" outputId="ad9ec935-637c-415a-8850-5799d3236634"}
``` python
print("There are", df.shape[0], "rows.")
```

::: {.output .stream .stdout}
    There are 1067371 rows.
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":335}" executionInfo="{\"elapsed\":13,\"status\":\"ok\",\"timestamp\":1728423053185,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="AeNuue-g9GAG" outputId="b838ca08-9b50-4a23-c4fb-123be6524e0a"}
``` python
df.isnull().sum()
```

::: {.output .execute_result execution_count="62"}
```{=html}
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Invoice</th>
      <td>0</td>
    </tr>
    <tr>
      <th>StockCode</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Description</th>
      <td>4382</td>
    </tr>
    <tr>
      <th>Quantity</th>
      <td>0</td>
    </tr>
    <tr>
      <th>InvoiceDate</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Price</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Customer ID</th>
      <td>243007</td>
    </tr>
    <tr>
      <th>Country</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>
```
:::
:::

::: {.cell .code id="_I53EBfh9H1v"}
``` python
df = df.dropna(subset = ['Customer ID'])
```
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":10,\"status\":\"ok\",\"timestamp\":1728423053186,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="5Zc0ykQh9K0g" outputId="773de89a-cef8-46a4-c3c5-de86ea0e7b22"}
``` python
print("There are", df.shape[0], "rows.")
```

::: {.output .stream .stdout}
    There are 824364 rows.
:::
:::

::: {.cell .code id="AQ9lBnQr9MwZ"}
``` python
# Remove negative quantities
df = df[df['Quantity'] > 0]
```
:::

::: {.cell .code id="TbP_Vs_V9PvG"}
``` python
df.duplicated().sum()
df = df.drop_duplicates()
```
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":5,\"status\":\"ok\",\"timestamp\":1728423054307,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="dFst9Lyz9STL" outputId="2dc39d4a-4842-43a6-c45f-713ed43058ca"}
``` python
print("There are", df.shape[0], "rows.")
```

::: {.output .stream .stdout}
    There are 779495 rows.
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":206}" executionInfo="{\"elapsed\":14,\"status\":\"ok\",\"timestamp\":1728423054622,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="Hf4_AmMM9UBG" outputId="ff5c8fc2-109f-4896-8132-c1c9fdf6c2c4"}
``` python
df.head()
```

::: {.output .execute_result execution_count="68"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .code id="lyiIOZXI9Vqh"}
``` python
df['Customer ID'] = df['Customer ID'].astype(int)
```
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":11,\"status\":\"ok\",\"timestamp\":1728423054623,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="2Z1nSpf09XZ7" outputId="236729e2-89a4-4348-c8f7-2cb584a52f06"}
``` python
df.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    Index: 779495 entries, 0 to 541909
    Data columns (total 8 columns):
     #   Column       Non-Null Count   Dtype         
    ---  ------       --------------   -----         
     0   Invoice      779495 non-null  object        
     1   StockCode    779495 non-null  object        
     2   Description  779495 non-null  object        
     3   Quantity     779495 non-null  int64         
     4   InvoiceDate  779495 non-null  datetime64[ns]
     5   Price        779495 non-null  float64       
     6   Customer ID  779495 non-null  int64         
     7   Country      779495 non-null  object        
    dtypes: datetime64[ns](1), float64(1), int64(2), object(4)
    memory usage: 53.5+ MB
:::
:::

::: {.cell .code id="xGfosk7b9fQ3"}
``` python
import datetime

# Convert TransactionDate to datetime format
df['TransactionDate'] = pd.to_datetime(df['InvoiceDate'])

# Create CurrentDate variable
currentDate = datetime.datetime(2024, 10, 8)
```
:::

::: {.cell .code id="Jmf9El6L-KqJ"}
``` python
df.set_index('InvoiceDate', inplace = True)
df = df.sort_index()
```
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":238}" executionInfo="{\"elapsed\":263,\"status\":\"ok\",\"timestamp\":1728423054879,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="HLEDYI8M-Rm3" outputId="c0f03c4c-cbdc-4e93-d0fe-2612f4f289e3"}
``` python
df.head()
```

::: {.output .execute_result execution_count="73"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":2924,\"status\":\"ok\",\"timestamp\":1728423057795,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="0V9m-9D59se9" outputId="d69bb081-19a1-4300-e1c9-99f98c4ef2a1"}
``` python
# Install and import the lifetimes package

!pip install lifetimes

import lifetimes
```

::: {.output .stream .stdout}
    Requirement already satisfied: lifetimes in /usr/local/lib/python3.10/dist-packages (0.11.3)
    Requirement already satisfied: numpy>=1.10.0 in /usr/local/lib/python3.10/dist-packages (from lifetimes) (1.26.4)
    Requirement already satisfied: scipy>=1.0.0 in /usr/local/lib/python3.10/dist-packages (from lifetimes) (1.13.1)
    Requirement already satisfied: pandas>=0.24.0 in /usr/local/lib/python3.10/dist-packages (from lifetimes) (2.2.2)
    Requirement already satisfied: autograd>=1.2.0 in /usr/local/lib/python3.10/dist-packages (from lifetimes) (1.7.0)
    Requirement already satisfied: dill>=0.2.6 in /usr/local/lib/python3.10/dist-packages (from lifetimes) (0.3.9)
    Requirement already satisfied: python-dateutil>=2.8.2 in /usr/local/lib/python3.10/dist-packages (from pandas>=0.24.0->lifetimes) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.10/dist-packages (from pandas>=0.24.0->lifetimes) (2024.2)
    Requirement already satisfied: tzdata>=2022.7 in /usr/local/lib/python3.10/dist-packages (from pandas>=0.24.0->lifetimes) (2024.2)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.10/dist-packages (from python-dateutil>=2.8.2->pandas>=0.24.0->lifetimes) (1.16.0)
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":577,\"status\":\"ok\",\"timestamp\":1728423058364,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="VWRynp7L9ym9" outputId="5b6bb910-79b3-4f5e-ff8a-e86de727ef1d"}
``` python
from lifetimes.utils import summary_data_from_transaction_data

# Calculate recency, frequency, and monetary value
rfm = summary_data_from_transaction_data(df,
                                         'Customer ID',
                                         'TransactionDate',
                                         monetary_value_col='Price',
                                         observation_period_end=currentDate)

print(rfm.head())
```

::: {.output .stream .stdout}
                 frequency  recency       T  monetary_value
    Customer ID                                            
    12346              7.0    400.0  5412.0       27.700000
    12347              7.0    402.0  5091.0       68.744286
    12348              4.0    363.0  5125.0       44.677500
    12349              3.0    571.0  5276.0      428.620000
    12350              0.0      0.0  4997.0        0.000000
:::
:::

::: {.cell .markdown id="IKOJ0hV5-ZaS"}
# 0.4 Specific Questions {#04-specific-questions}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":9,\"status\":\"ok\",\"timestamp\":1728423058364,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="995ixFclBfjS" outputId="6648b54c-87d1-4994-9b9d-c96024f66d1e"}
``` python
average_recency = rfm['recency'].mean()
average_frequency = rfm['frequency'].mean()
average_monetary_value = rfm['monetary_value'].mean()

print(f"Average Recency: {average_recency}")
print(f"Average Frequency: {average_frequency}")
print(f"Average Monetary Value: {average_monetary_value}")
```

::: {.output .stream .stdout}
    Average Recency: 273.25420846794765
    Average Frequency: 4.63033497704472
    Average Monetary Value: 50.3174045218969
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":167,\"status\":\"ok\",\"timestamp\":1728423210586,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="5yCQLiVhBnzg" outputId="50fc635f-161e-4efd-d94b-423824b8605a"}
``` python
# Sort the RFM dataframe by monetary_value in descending order
top_customers = rfm.sort_values('monetary_value', ascending=False).head(10)

print("Top 10 Customers based on Monetary Value:")
print(top_customers[['recency', 'frequency', 'monetary_value']])
```

::: {.output .stream .stdout}
    Top 10 Customers based on Monetary Value:
                 recency  frequency  monetary_value
    Customer ID                                    
    12536           16.0        1.0     8322.120000
    14096           97.0       16.0     2081.990000
    14063          293.0        7.0     1915.387143
    12744          540.0        7.0     1881.391429
    15480          560.0        3.0      817.083333
    12378          433.0        1.0      656.440000
    12669          106.0        2.0      615.595000
    16072          261.0        2.0      554.765000
    16521          233.0        1.0      545.780000
    15502          539.0        9.0      478.392222
:::
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":935,\"status\":\"ok\",\"timestamp\":1728423273544,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="F75VyM0nDTdV" outputId="5247f95d-8c85-48b9-a05d-22af53c7fa8e"}
``` python
# Calculating the average recency and frequency for this top 10 group
average_recency_top_customers = top_customers['recency'].mean()
average_frequency_top_customers = top_customers['frequency'].mean()

print(f"\nAverage Recency for Top 10 Customers: {average_recency_top_customers}")
print(f"Average Frequency for Top 10 Customers: {average_frequency_top_customers}")
```

::: {.output .stream .stdout}

    Average Recency for Top 10 Customers: 307.8
    Average Frequency for Top 10 Customers: 4.9
:::
:::

::: {.cell .markdown id="r1ECYphq-wOl"}
# 0.5 RFM Scoring {#05-rfm-scoring}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":236,\"status\":\"ok\",\"timestamp\":1728423290704,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="M_Ntgdes-_L-" outputId="44604a4c-d132-4273-8f4e-51313a0340a5"}
``` python
# Assign scores for Recency, Frequency, and Monetary Value
quantiles = rfm[['frequency', 'recency', 'monetary_value']].quantile([0.2, 0.4, 0.6, 0.8]).to_dict()

def r_score(x):
    if x <= quantiles['recency'][0.2]:
        return 1
    elif x <= quantiles['recency'][0.4]:
        return 2
    elif x <= quantiles['recency'][0.6]:
        return 3
    elif x <= quantiles['recency'][0.8]:
        return 4
    else:
        return 5

def fm_score(x, c):
    if x <= quantiles[c][0.2]:
        return 1
    elif x <= quantiles[c][0.4]:
        return 2
    elif x <= quantiles[c][0.6]:
        return 3
    elif x <= quantiles[c][0.8]:
        return 4
    else:
        return 5

rfm['r_quartile'] = rfm['recency'].apply(lambda x: r_score(x))
rfm['f_quartile'] = rfm['frequency'].apply(lambda x: fm_score(x, 'frequency'))
rfm['m_quartile'] = rfm['monetary_value'].apply(lambda x: fm_score(x, 'monetary_value'))

# Combine the scores into a single RFM score
rfm['rfm_score'] = rfm.r_quartile.map(str) + rfm.f_quartile.map(str) + rfm.m_quartile.map(str)

print(rfm.head(10))
```

::: {.output .stream .stdout}
                 frequency  recency       T  monetary_value  r_quartile  \
    Customer ID                                                           
    12346              7.0    400.0  5412.0       27.700000           4   
    12347              7.0    402.0  5091.0       68.744286           4   
    12348              4.0    363.0  5125.0       44.677500           4   
    12349              3.0    571.0  5276.0      428.620000           4   
    12350              0.0      0.0  4997.0        0.000000           1   
    12351              0.0      0.0  5062.0        0.000000           1   
    12352              8.0    356.0  5079.0      174.532500           4   
    12353              1.0    204.0  5095.0       24.300000           3   
    12354              0.0      0.0  4919.0        0.000000           1   
    12355              1.0    353.0  5254.0       54.650000           3   

                 f_quartile  m_quartile rfm_score              segment  
    Customer ID                                                         
    12346                 4           3       443                Other  
    12347                 4           4       444  Potential Loyalists  
    12348                 4           3       443                Other  
    12349                 3           5       435  Potential Loyalists  
    12350                 1           1       111       Lost Customers  
    12351                 1           1       111       Lost Customers  
    12352                 5           5       455            Champions  
    12353                 2           3       323                Other  
    12354                 1           1       111       Lost Customers  
    12355                 2           4       324                Other  
:::
:::

::: {.cell .markdown id="g1amFQbn-0et"}
# 0.6 Segmentation {#06-segmentation}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":253,\"status\":\"ok\",\"timestamp\":1728423320957,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="T2UPMGXD_Tkv" outputId="3c8c65fe-1c9b-4262-8eba-68a8fc1587f8"}
``` python
def segment_customers(rfm_score):
  if rfm_score in ['545', '555', '554', '454', '455', '445']:
    return 'Champions'
  elif rfm_score in ['345', '355', '354', '444', '435']:
    return 'Potential Loyalists'
  elif rfm_score in ['451', '452', '453', '541', '542']:
    return 'At Risk'
  elif rfm_score in ['111', '112', '121', '122', '123', '132', '211', '212', '221']:
    return 'Lost Customers'
  else:
    return 'Other'

rfm['segment'] = rfm['rfm_score'].apply(lambda x: segment_customers(x))

print(rfm.head(10))
```

::: {.output .stream .stdout}
                 frequency  recency       T  monetary_value  r_quartile  \
    Customer ID                                                           
    12346              7.0    400.0  5412.0       27.700000           4   
    12347              7.0    402.0  5091.0       68.744286           4   
    12348              4.0    363.0  5125.0       44.677500           4   
    12349              3.0    571.0  5276.0      428.620000           4   
    12350              0.0      0.0  4997.0        0.000000           1   
    12351              0.0      0.0  5062.0        0.000000           1   
    12352              8.0    356.0  5079.0      174.532500           4   
    12353              1.0    204.0  5095.0       24.300000           3   
    12354              0.0      0.0  4919.0        0.000000           1   
    12355              1.0    353.0  5254.0       54.650000           3   

                 f_quartile  m_quartile rfm_score              segment  
    Customer ID                                                         
    12346                 4           3       443                Other  
    12347                 4           4       444  Potential Loyalists  
    12348                 4           3       443                Other  
    12349                 3           5       435  Potential Loyalists  
    12350                 1           1       111       Lost Customers  
    12351                 1           1       111       Lost Customers  
    12352                 5           5       455            Champions  
    12353                 2           3       323                Other  
    12354                 1           1       111       Lost Customers  
    12355                 2           4       324                Other  
:::
:::

::: {.cell .markdown id="3cHV13iB-8pn"}
# 0. 7 Visualizations {#0-7-visualizations}
:::

::: {.cell .code colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":1000}" executionInfo="{\"elapsed\":877,\"status\":\"ok\",\"timestamp\":1728423333927,\"user\":{\"displayName\":\"Hoang Bich Tram Pham\",\"userId\":\"02504502517984396301\"},\"user_tz\":300}" id="wMI3yaQb-97A" outputId="bc35fd03-cd3f-48e6-9388-f1d8ef584b49"}
``` python
import matplotlib.pyplot as plt

# Bar Chart: Distribution of customers across segments
segment_counts = rfm['segment'].value_counts()
plt.figure(figsize=(10, 6))
plt.bar(segment_counts.index, segment_counts.values)
plt.xlabel('Customer Segments')
plt.ylabel('Number of Customers')
plt.title('Distribution of Customers Across Segments')
plt.show()

# Scatter Plot: Frequency vs. Monetary Value
plt.figure(figsize=(10, 6))
plt.scatter(rfm['frequency'], rfm['monetary_value'])
plt.xlabel('Frequency')
plt.ylabel('Monetary Value')
plt.title('Frequency vs. Monetary Value')

# Highlight customers with the highest RFM scores
high_rfm_customers = rfm[rfm['rfm_score'].isin(['555', '554', '545', '455', '454'])]
plt.scatter(high_rfm_customers['frequency'], high_rfm_customers['monetary_value'], color='red', label='High RFM')
plt.legend()
plt.grid(True, axis='y')  # Add grid lines only for the y-axis
plt.show()
```

::: {.output .display_data}
![](vertopal_e65140429dde425fabd2ce77953181b6/9d80cb79678635a2afe7324ec416047a8dd74f00.png)
:::

::: {.output .display_data}
![](vertopal_e65140429dde425fabd2ce77953181b6/0c5e2aaafa0e0900a5ab01702473e7b60fc5dd0f.png)
:::
:::
