# Discovery Components

BigConnect Discovery performs analytics on its ingested data sources or other external data sources using various analytical tools and outputs analytical results in charts and reports. To utilize this system, you must understand its overall structure shown below:

![](../../../.gitbook/assets/image%20%28330%29.png)

### Data Preparation

Data Preparation refines data from source data to be ingested into BigConnect. See [Discovery Data Preparation](../data-preparation/) for more details.

![BigConnect Discovery Data Preparation](../../../.gitbook/assets/prep.png)

![BigConnect Discovery Data Preparation](../../../.gitbook/assets/prep2.jpg)

### Data Storage

Data Storage manages data ingested into the Druid engine for analysis and visualization. See [Discovery Data Management](../discovery-data-management/) for more details.

![](../../../.gitbook/assets/datasource.jpg)

![](../../../.gitbook/assets/image%20%28270%29.png)

### Data analysis and visualization

Each module below allows users to perform visualization-based exploration and analysis of stored data.

#### Workspace

Workspace provides an interface to manage its workbooks, workbenches, and notebooks used in an organization according to user access. See [Discovery Workspaces](../discovery-workspaces.md) for more details.

![BigConnect Discovery Workspaces](../../../.gitbook/assets/workspace.jpg)

#### Workbook, dashboard, chart

Workbook supports working on, sharing, and making a presentation with dashboards and charts using a PowerPoint-like interface. See [Discovery Workbook](../discovery-workbooks/)s for more details.

![BigConnect Discovery Workbook Dashboard](../../../.gitbook/assets/dashboard.jpg)

![BigConnect Discovery Workbook Dashboard](../../../.gitbook/assets/image%20%2836%29.png)

![BigConnect Discovery Workbook Dashboard](../../../.gitbook/assets/image%20%28226%29.png)

#### Notebook

Notebooks enables advanced analytics based on machine learning by using Jupyter Notebooks. See [Discovery Notebooks](../discovery-notebooks/) for more details.

#### Workbench

The Workbench enables SQL data analytics. See [Discovery Workbench](../discovery-workbench/) for more details.

![BigConnect Discovery Data Workbench](../../../.gitbook/assets/sql.jpg)

### Data Monitoring

This function monitors data use based on data query statistics and query logs. See [Discovery Data Monitoring](../discovery-data-management/discovery-data-monitoring.md) for more details.

![BigConnect Discovery Data Monitoring](../../../.gitbook/assets/monitoring.jpg)



