# Sigma Computing Dashboard

Sigma Computing is a modern business intelligence tool built for the cloud, like Amazon Redshift. What's cool is that it lets you look at your raw data directly, without the usual hassle of old-school BI. It's got a live, interactive setup that feels a lot like a spreadsheet, so more people on the team can dive in and understand the numbers without needing to be tech wizards.

What makes Sigma stand out is how fast it is, how quickly people can learn to use it, how well it handles lots of data, and how it helps keep everything organized and secure. It's also great for getting the data experts and the business folks on the same page.
![image](https://github.com/user-attachments/assets/d33e240c-efe6-4cd6-8bc6-a8785f30cb8e)

## Case: Wine Retail Analytics 🍷
For this project, we're using Sigma Computing's interactive dashboards to give a US liquor distributor clear, useful insights into how profitable each of their products and brands are – looking at the details for each one.

Our main goal here is to provide this distributor with actionable information about which products and brands are making money and which aren't. Based on that, we'll also suggest which ones they might want to consider dropping from their offerings.

## Content
1. Introduction (inputs, KPIs) 
3. Exploratory Data Analysis (EDA)
4. Extract, transform and load (ETL)
5. Sigma Computing Visualizations
6. Key Findings

## Introduction
### KPIs
1. Efficiently ingest the relevant csv files into a suitable database.
2. Transform the data to calculate profits ($) and margins (%).
3. Create a report for Annie outlining:
  1. Top 10 products with highest based on profit ($) and margin (%).
  2. Top 10 brands with highest based on profit ($) and margin (%).
  3. Which brands / products should she drop as a wholesales because they are loosing money.


### Inputs
6 different datasets:
- Purchases
- Beginning Inventory
- Purchase Prices
- Vendor Invoices
- Ending Inventory
- Sales
The datasets are located in https://www.pwc.com/us/en/careers/university-relations/data-and-analytics-case-studies-files.html.


## Potential schema outline
![Screenshot 2025-04-08 202006](https://github.com/user-attachments/assets/688b7973-84dd-4a1c-82e0-19d5a62a0699)


## Exploratory Data Analysis
We first tried to load the datasets into Sigma Computing, but the free version's 200MB limit prevented this. So, as a next step, we used Pandas to get a general overview of all the data. This helped us locate the key information we needed, like product IDs, date and time details, purchase and selling prices, and the links (primary and foreign keys) to connect different tables.
  
  import pandas as pd
  
  #pd.read_csv for opening the files
  df_sales = pd.read_csv("SalesFINAL12312016.csv")
  
  #.info() to have an overview, like datatypes, number of rows
  df_sales.info()

  #.head() to actually see the top 5 rows
  df_sales.head()

  #and some other methods to explore the important columns
  len(df_sales['Description'].unique()) #this column is actually the product id
  len(df_sales['PurchasePrice'].unique())

Finally we cound the relevant information and started the transformations.


## Extract, Transform, Load 
We began by selecting only the necessary columns from the relevant tables. Next, we removed any duplicate entries. After that, we joined the purchase and sales data together. Finally, we calculated the average purchase price, sales price, and quantity sold, grouping these averages by the ID columns for brand, description, and size. Also, we rounded the numbers to two decimal places to keep the averages from being too long.

Then, we saved this cleaned-up data into a new CSV file and uploaded it to Sigma Computing.

Once the dataset was easly uploaded, we still did some calculus:

### Calculating [Profit per unit] column
[profit_margin_annies_numbers.csv/SalesPrice] - [profit_margin_annies_numbers.csv/PurchasePrice]
![image](https://github.com/user-attachments/assets/a2888062-8383-4336-be17-7223aa9ba7a4)

### Calculating [Top 10] column
Rank([Margin percent], "desc") <= 10
![image](https://github.com/user-attachments/assets/ea4d3935-9615-4a44-8e87-af0e3ebaa6e6)

### Calculating [Margin percent] column
[Profit  per unit] / [profit_margin_annies_numbers.csv/SalesPrice]
![image](https://github.com/user-attachments/assets/6b1a124d-5588-49ca-aa8a-91c811d014f1)


## Sigma Computing Visualizations
Link to the online workbook:
https://app.sigmacomputing.com/base-labs/workbook/sigma_practical_case_wine_store-3oUwcjF5jHN9xD7Pg8Jmoe/edit?:nodeId=-AWdonS-jk

### Sales Quantity Tooltip
We also added a tooltip to the average quantity sold column. This way, you can easily see how many products are actually being sold. Because even if the profit per bottle looks great, selling only one a month doesn't necessarily translate to big profits overall.
![image](https://github.com/user-attachments/assets/89058bf2-5083-4fc9-845f-800086ecd31d)

### Top Products Controller
Each bar chart features a control in the top right corner that displays the number of 'wished' products, filtered to show only the top items.
![image](https://github.com/user-attachments/assets/899976d0-d5f3-4bb5-b0b2-4c8fba1ce9ae)

![image](https://github.com/user-attachments/assets/566800e3-0f8c-466f-985f-9bbb15ef44eb)


## Key Findings
Despite some products having numerous sales, as illustrated in the first row,
![image](https://github.com/user-attachments/assets/2874a44d-847c-42e6-927a-81a4c343305c)

it's important to consider that selling even a single unit of a high-value product, as shown in the subsequent image, can yield a more significant benefit to the company.
![image](https://github.com/user-attachments/assets/4d41b92c-4b53-42db-8fe0-fdb14e2b4a72)

### Top profit products
![image](https://github.com/user-attachments/assets/ba2fc69f-e375-4e52-97b8-cf14f0be76a1)


### Unprofitable Products
We've identified over 80 unprofitable products that the store should seriously consider discontinuing after a closer look.
![image](https://github.com/user-attachments/assets/4684d3f1-2398-4819-b4be-ea59d2014815)
