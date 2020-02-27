# BigConnect Discovery Data Science

This tutorial will teach you the basics of data science with BigConnect Discovery. The main points addressed below are:

* Using Jupyter Notebooks with data from BigConnect Discovery
* Training a ML model for sales forecast

BigConnect Discovery has seamless integration with Jupyter Notebooks. You can create notebooks for each Workbook to address various use cases like custom data transformation, data science and whatever scenario you can think of.

## Prerequisites

1.Follow the **BigConnect Discovery Basics** tutorial, **Preparing data** section to create the **salesdata** datasource.

## Tutorial

1.The first step is to connect Jupyter Notebook to BigConnect Discovery. Login to BigConnect Discovery and choose **MANAGEMENT / Notebook / Notebook Server** from the menu. Click on **Add a server** on the Notebook page:

![](../../.gitbook/assets/image%20%2894%29.png)

2. Choose **jupyter** for the **Type**, enter **http://localhost:9888** for the **URL** and **BDL Jupyter Server** for **Name** and click **Done**.

![](../../.gitbook/assets/image%20%28548%29.png)

This sets up the connection to the Jupyter Notebook server provided by BDL.

3. Head to the **WORKSPACE / Admin Workspace** from the menu and connect the notebook server to our workspace. Click on the three dots on the right hand side of the top pane and choose **Set notebook server**:

![](../../.gitbook/assets/image%20%28250%29.png)

4. Select **BDL Jupyter Server** and click **Done**.

![](../../.gitbook/assets/image%20%28146%29.png)

5. A new button named **Notebook** will be shown on the bottom pane of the Admin Workspace. Click on it to create a new notebook

![](../../.gitbook/assets/image%20%28116%29.png)

6. Choose **Datasource** as the source type, select our previously created sa**l**esdata datasource and click **Next**.

7. Choose **PYTHON** for the **Development language** \(_R is not installed in the Sandbox_\), enter **Sales Forecast** for **Name** and click **Done**

![](../../.gitbook/assets/image%20%28578%29.png)

8. You will be taken to the notebook details page. Click on the **Detail** link to open the actual Jupyter Notebook.

![](../../.gitbook/assets/image%20%2850%29.png)

9. The Jupyter Notebook will be opened in a new tab. It already contains the necessary code to load data from our datasource.

![](../../.gitbook/assets/image%20%2840%29.png)

10. Copy the following piece of code into the analyze empty field and run the notebook:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn import linear_model
from statsmodels.tsa.stattools import acf, pacf
from statsmodels.tsa.arima_model import ARIMA
from sklearn.model_selection import train_test_split

df_sales = pd.json_normalize(dataset)
df_sales.head()
```

![](../../.gitbook/assets/image%20%28530%29.png)

11. Add another cell with the following code and run the notebook again:

```python
df_sales['OrderDate'] = pd.to_datetime(df_sales['OrderDate'], format='%y-%m-%d')
df_sales = df_sales.groupby('OrderDate').Sales.sum().reset_index()
train, test = train_test_split(df_sales, test_size=0.2)

regr = linear_model.LinearRegression()
regr.fit(train.OrderDate.values.reshape(-1, 1), train.Sales.values.reshape(-1, 1))

# Make predictions using the testing set
y_pred = regr.predict(test.OrderDate.values.astype(float).reshape(-1, 1))
df_new = test.copy()
df_new['pred'] = y_pred

ax = df_sales.plot(x='OrderDate', y='Sales', color='black', style='.')
df_new.plot(x='OrderDate', y='pred', color='orange', linewidth=3, ax=ax, alpha=0.5)
```

![](../../.gitbook/assets/image%20%28171%29.png)

12. Add another cell with the following code and run the notebook again to save the trained model to a file and run the notebook again:

```python
from joblib import dump, load
dump(regr, 'my_model.joblib') 
!ls
```

The output should be similar to:

![](../../.gitbook/assets/image%20%28523%29.png)

The trained model is not saved on the local disk, as the **my\_model.joblib** file.

You can use it in a [Data Collector](../../data-pipelines/data-pipelines/) pipeline to apply it on new data as it arrives.

