# Discovery

BigConnect Discovery is a 4th-generation OLAP-based business intelligence \(BI\) solution that combines OLAP, visualization, and machine learning technologies for even non-experts to quickly and easily derive higher-level value from data.

### 4th-generation BI solution

The figure below shows BI trends from the 1st to 4th generation.

![Generation of BI products](../../.gitbook/assets/bi.png)

The mainstream products in the current BI market belong to the 2nd and 3rd generations, and 4th generation products are beginning to come under the spotlight. As a 4th generation BI solution, BigConnect Discovery supports self & ad-hoc data discovery and guarantees rapid response to big data.

### Built on Big OLAP

BigConnect Discovery combines data of various dimensions for large-sized fact data to produce a single big OLAP cube \(data mart\).

![Big OLAP Cube](../../.gitbook/assets/image%20%28595%29.png)

The use of a big OLAP cube offers the following advantages:

* Minimizes the number of data marts
  * Lower ETL cost for data mart production
  * Influence of structural change can be minimized
  * Satisfies diverse demands by saving all fact data
* Distributed architecture allows storing of large-scale data and ensures fast data processing
* With a dynamic schema approach, schema changes do not require schema redefining
* Data can be processed at the record level in real time as tables are saved with no data loss

### Architecture

BigConnect Discovery is an end-to-end solution that supports the entire process of data discovery, from preparation of large-scale data to data visualization and exploration and to advanced analytics. The figure below is a summary of Discovery’s architecture and key features.

![](../../.gitbook/assets/single_solution.png)



## Components

BigConnect Discovery performs analytics on its ingested data sources or other external data sources using various analytical tools and outputs analytical results in charts and reports. To utilize this system, you must understand its overall structure shown below:

![](../../.gitbook/assets/image%20%28543%29.png)

### Data Preparation

Data Preparation refines data from source data to be ingested into BigConnect. See [Discovery Data Preparation](data-preparation/) for more details.

![BigConnect Discovery Data Preparation](../../.gitbook/assets/prep.png)

![BigConnect Discovery Data Preparation](../../.gitbook/assets/prep2.jpg)

### Data Storage

Data Storage manages data ingested into the Druid engine for analysis and visualization. See [Discovery Data Management](discovery-data-management/) for more details.

![](../../.gitbook/assets/datasource.jpg)

![](../../.gitbook/assets/image%20%28441%29.png)

### Data analysis and visualization

Each module below allows users to perform visualization-based exploration and analysis of stored data.

#### Workspace

Workspace provides an interface to manage its workbooks, workbenches, and notebooks used in an organization according to user access. See [Discovery Workspaces](discovery-workspaces.md) for more details.

![BigConnect Discovery Workspaces](../../.gitbook/assets/workspace.jpg)

#### Workbook, dashboard, chart

Workbook supports working on, sharing, and making a presentation with dashboards and charts using a PowerPoint-like interface. See [Discovery Workbook](discovery-workbooks/)s for more details.

![BigConnect Discovery Workbook Dashboard](../../.gitbook/assets/dashboard.jpg)

![BigConnect Discovery Workbook Dashboard](../../.gitbook/assets/image%20%2853%29.png)

![BigConnect Discovery Workbook Dashboard](../../.gitbook/assets/image%20%28374%29.png)

#### Notebook

Notebooks enables advanced analytics based on machine learning by using Jupyter Notebooks. See [Discovery Notebooks]() for more details.

#### Workbench

The Workbench enables SQL data analytics. See [Discovery Workbench](discovery-workbench/) for more details.

![BigConnect Discovery Data Workbench](../../.gitbook/assets/sql.jpg)

### Data Monitoring

This function monitors data use based on data query statistics and query logs. See [Discovery Data Monitoring](discovery-data-management/discovery-data-monitoring.md) for more details.

![BigConnect Discovery Data Monitoring](../../.gitbook/assets/monitoring.jpg)



