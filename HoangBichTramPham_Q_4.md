# 0.1 Setting up the Data


```python
# prompt: Load google drive

from google.colab import drive
drive.mount('/content/drive')
```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).



```python
%cd /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)
```

    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)



```python
%cd Datasets/
```

    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)/Datasets



```python
ls
```

    online+retail+ii.zip  [0m[01;34mretail_data[0m/  retail_data.csv


# 0.2 Read in the Data


```python
%cd retail_data
```

    /content/drive/MyDrive/6000_01/Week 5 (24 09 2024)/Datasets/retail_data



```python
ls
```

    master_df.csv  online_retail_II.xlsx  rfm_data.csv



```python
import pandas as pd
```


```python
df_0 = pd.read_excel('online_retail_II.xlsx', sheet_name = 0)
```


```python
df_1 = pd.read_excel('online_retail_II.xlsx', sheet_name = 1)
```


```python
master_df = pd.concat([df_0, df_1])
```

0.3 RFM Calculation using lifetime


```python
df = master_df.copy(deep = True)
```


```python
print("There are", df.shape[0], "rows.")
```

    There are 1067371 rows.



```python
df.isnull().sum()
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




```python
df = df.dropna(subset = ['Customer ID'])
```


```python
print("There are", df.shape[0], "rows.")
```

    There are 824364 rows.



```python
# Remove negative quantities
df = df[df['Quantity'] > 0]
```


```python
df.duplicated().sum()
df = df.drop_duplicates()
```


```python
print("There are", df.shape[0], "rows.")
```

    There are 779495 rows.



```python
df.head()
```





  <div id="df-1987d5e0-29ec-4af5-92da-13893e1398df" class="colab-df-container">
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
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-1987d5e0-29ec-4af5-92da-13893e1398df')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-1987d5e0-29ec-4af5-92da-13893e1398df button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-1987d5e0-29ec-4af5-92da-13893e1398df');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-8419ad60-0de8-48ce-8216-c1ee92674c80">
  <button class="colab-df-quickchart" onclick="quickchart('df-8419ad60-0de8-48ce-8216-c1ee92674c80')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-8419ad60-0de8-48ce-8216-c1ee92674c80 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
df['Customer ID'] = df['Customer ID'].astype(int)
```


```python
df.info()
```

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



```python
import datetime

# Convert TransactionDate to datetime format
df['TransactionDate'] = pd.to_datetime(df['InvoiceDate'])

# Create CurrentDate variable
currentDate = datetime.datetime(2024, 10, 8)
```


```python
df.set_index('InvoiceDate', inplace = True)
df = df.sort_index()
```


```python
df.head()
```





  <div id="df-ad474d84-79c7-4c82-92d2-ff3d12ab8b28" class="colab-df-container">
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
      <th>Price</th>
      <th>Customer ID</th>
      <th>Country</th>
      <th>TransactionDate</th>
    </tr>
    <tr>
      <th>InvoiceDate</th>
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
      <th>2009-12-01 07:45:00</th>
      <td>489434</td>
      <td>85048</td>
      <td>15CM CHRISTMAS GLASS BALL 20 LIGHTS</td>
      <td>12</td>
      <td>6.95</td>
      <td>13085</td>
      <td>United Kingdom</td>
      <td>2009-12-01 07:45:00</td>
    </tr>
    <tr>
      <th>2009-12-01 07:45:00</th>
      <td>489434</td>
      <td>79323P</td>
      <td>PINK CHERRY LIGHTS</td>
      <td>12</td>
      <td>6.75</td>
      <td>13085</td>
      <td>United Kingdom</td>
      <td>2009-12-01 07:45:00</td>
    </tr>
    <tr>
      <th>2009-12-01 07:45:00</th>
      <td>489434</td>
      <td>79323W</td>
      <td>WHITE CHERRY LIGHTS</td>
      <td>12</td>
      <td>6.75</td>
      <td>13085</td>
      <td>United Kingdom</td>
      <td>2009-12-01 07:45:00</td>
    </tr>
    <tr>
      <th>2009-12-01 07:45:00</th>
      <td>489434</td>
      <td>22041</td>
      <td>RECORD FRAME 7" SINGLE SIZE</td>
      <td>48</td>
      <td>2.10</td>
      <td>13085</td>
      <td>United Kingdom</td>
      <td>2009-12-01 07:45:00</td>
    </tr>
    <tr>
      <th>2009-12-01 07:45:00</th>
      <td>489434</td>
      <td>21232</td>
      <td>STRAWBERRY CERAMIC TRINKET BOX</td>
      <td>24</td>
      <td>1.25</td>
      <td>13085</td>
      <td>United Kingdom</td>
      <td>2009-12-01 07:45:00</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-ad474d84-79c7-4c82-92d2-ff3d12ab8b28')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-ad474d84-79c7-4c82-92d2-ff3d12ab8b28 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-ad474d84-79c7-4c82-92d2-ff3d12ab8b28');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-b8919743-5321-45d5-8ec5-7db4f0c99913">
  <button class="colab-df-quickchart" onclick="quickchart('df-b8919743-5321-45d5-8ec5-7db4f0c99913')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-b8919743-5321-45d5-8ec5-7db4f0c99913 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
# Install and import the lifetimes package

!pip install lifetimes

import lifetimes
```

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



```python
from lifetimes.utils import summary_data_from_transaction_data

# Calculate recency, frequency, and monetary value
rfm = summary_data_from_transaction_data(df,
                                         'Customer ID',
                                         'TransactionDate',
                                         monetary_value_col='Price',
                                         observation_period_end=currentDate)

print(rfm.head())
```

                 frequency  recency       T  monetary_value
    Customer ID                                            
    12346              7.0    400.0  5412.0       27.700000
    12347              7.0    402.0  5091.0       68.744286
    12348              4.0    363.0  5125.0       44.677500
    12349              3.0    571.0  5276.0      428.620000
    12350              0.0      0.0  4997.0        0.000000


# 0.4 Specific Questions


```python
average_recency = rfm['recency'].mean()
average_frequency = rfm['frequency'].mean()
average_monetary_value = rfm['monetary_value'].mean()

print(f"Average Recency: {average_recency}")
print(f"Average Frequency: {average_frequency}")
print(f"Average Monetary Value: {average_monetary_value}")
```

    Average Recency: 273.25420846794765
    Average Frequency: 4.63033497704472
    Average Monetary Value: 50.3174045218969



```python
# Sort the RFM dataframe by monetary_value in descending order
top_customers = rfm.sort_values('monetary_value', ascending=False).head(10)

print("Top 10 Customers based on Monetary Value:")
print(top_customers[['recency', 'frequency', 'monetary_value']])
```

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



```python
# Calculating the average recency and frequency for this top 10 group
average_recency_top_customers = top_customers['recency'].mean()
average_frequency_top_customers = top_customers['frequency'].mean()

print(f"\nAverage Recency for Top 10 Customers: {average_recency_top_customers}")
print(f"Average Frequency for Top 10 Customers: {average_frequency_top_customers}")
```

    
    Average Recency for Top 10 Customers: 307.8
    Average Frequency for Top 10 Customers: 4.9


# 0.5 RFM Scoring


```python
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


# 0.6 Segmentation


```python
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


# 0. 7 Visualizations


```python
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


    
![png](output_38_0.png)
    



    
![png](output_38_1.png)
    

