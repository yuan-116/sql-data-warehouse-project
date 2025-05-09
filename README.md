# Sales Data Warehouse Project | SQL
This project showcases end-to-end data warehousing skills, from designing to building a data warehouse from scratch.


The data architecture for this project follows Medallion Architecture **Bronze**, **Silver**, and **Gold** layers:

![Data Architecture](docs/Data%20Warehouse%20Architecture%20.png)


---

##  Project Overview

This project involves:
1. **Data Architecture**: Designing a modern data warehouse using Medallion Architecture (Bronze, Silver, Gold).
2. **ETL Pipelines**: Extracting, transforming, and loading data from source systems into the warehouse.
3. **Data Modeling**: Developing fact and dimension tables optimized for analytical queries.

---

# Detailed Project Structure

![data_flow](docs/data_flow_diagram.png)

## 🟤 Bronze Layer - Raw Data Ingestion

The Bronze Layer represents the initial raw ingestion of CRM and ERP source files into the data warehouse.

### Key Actions:
- Created raw staging tables in the `bronze` schema.
- Implemented a `bronze.load_bronze` stored procedure:
  - Truncates tables.
  - Loads CSV data using `BULK INSERT`.
  - Logs load durations and batch times.

### Tables:
- `bronze.crm_cust_info`
- `bronze.crm_prd_info`
- `bronze.crm_sales_details`
- `bronze.erp_cust_az12`
- `bronze.erp_loc_a101`
- `bronze.erp_px_cat_g1v2`

### Purpose:
- Preserve raw source data without transformation.
- Serve as the base for further processing in the Silver Layer.

![data_integration](docs/data_integration.png)

---

## ⚪ Silver Layer - Data Cleansing and Standardization

The Silver Layer refines and standardizes the raw data ingested from the Bronze Layer.

### Key Actions:
- Created cleaned and standardized tables in the `silver` schema.
- Implemented a `silver.load_silver` stored procedure:
  - Standardized gender, marital status, and country names.
  - Corrected invalid dates and missing sales or price fields.
  - Applied data cleaning and normalization.

### Tables:
- `silver.crm_cust_info`
- `silver.crm_prd_info`
- `silver.crm_sales_details`
- `silver.erp_cust_az12`
- `silver.erp_loc_a101`
- `silver.erp_px_cat_g1v2`

### Purpose:
- Improve data consistency, quality, and usability.
- Prepare analysis-ready datasets for modeling in the Gold Layer.


---

## 🟡 Gold Layer - Business-Ready Star Schema

The Gold Layer consolidates cleansed data into a final star schema ready for analytics and reporting.

### Key Actions:
- Created analytical views in the `gold` schema:
  - `gold.dim_customers`
  - `gold.dim_products`
  - `gold.fact_sales`
- Built a complete Star Schema:
  - **Fact Table**: `fact_sales`
  - **Dimension Tables**: `dim_customers`, `dim_products`
- Integrated CRM and ERP data to enrich customer and product profiles.

### 1. **gold.dim_customers**
- **Purpose:** Stores customer details enriched with demographic and geographic data.
- **Columns:**

| Column Name      | Data Type     | Description                                                                                   |
|------------------|---------------|-----------------------------------------------------------------------------------------------|
| customer_key     | INT           | Surrogate key uniquely identifying each customer record in the dimension table.               |
| customer_id      | INT           | Unique numerical identifier assigned to each customer.                                        |
| customer_number  | NVARCHAR(50)  | Alphanumeric identifier representing the customer, used for tracking and referencing.         |
| first_name       | NVARCHAR(50)  | The customer's first name, as recorded in the system.                                         |
| last_name        | NVARCHAR(50)  | The customer's last name or family name.                                                     |
| country          | NVARCHAR(50)  | The country of residence for the customer (e.g., 'Australia').                               |
| marital_status   | NVARCHAR(50)  | The marital status of the customer (e.g., 'Married', 'Single').                              |
| gender           | NVARCHAR(50)  | The gender of the customer (e.g., 'Male', 'Female', 'n/a').                                  |
| birthdate        | DATE          | The date of birth of the customer, formatted as YYYY-MM-DD (e.g., 1971-10-06).               |
| create_date      | DATE          | The date and time when the customer record was created in the system|

---

### 2. **gold.dim_products**
- **Purpose:** Provides information about the products and their attributes.
- **Columns:**

| Column Name         | Data Type     | Description                                                                                   |
|---------------------|---------------|-----------------------------------------------------------------------------------------------|
| product_key         | INT           | Surrogate key uniquely identifying each product record in the product dimension table.         |
| product_id          | INT           | A unique identifier assigned to the product for internal tracking and referencing.            |
| product_number      | NVARCHAR(50)  | A structured alphanumeric code representing the product, often used for categorization or inventory. |
| product_name        | NVARCHAR(50)  | Descriptive name of the product, including key details such as type, color, and size.         |
| category_id         | NVARCHAR(50)  | A unique identifier for the product's category, linking to its high-level classification.     |
| category            | NVARCHAR(50)  | The broader classification of the product (e.g., Bikes, Components) to group related items.  |
| subcategory         | NVARCHAR(50)  | A more detailed classification of the product within the category, such as product type.      |
| maintenance_required| NVARCHAR(50)  | Indicates whether the product requires maintenance (e.g., 'Yes', 'No').                       |
| cost                | INT           | The cost or base price of the product, measured in monetary units.                            |
| product_line        | NVARCHAR(50)  | The specific product line or series to which the product belongs (e.g., Road, Mountain).      |
| start_date          | DATE          | The date when the product became available for sale or use, stored in|

---

### 3. **gold.fact_sales**
- **Purpose:** Stores transactional sales data for analytical purposes.
- **Columns:**

| Column Name     | Data Type     | Description                                                                                   |
|-----------------|---------------|-----------------------------------------------------------------------------------------------|
| order_number    | NVARCHAR(50)  | A unique alphanumeric identifier for each sales order (e.g., 'SO54496').                      |
| product_key     | INT           | Surrogate key linking the order to the product dimension table.                               |
| customer_key    | INT           | Surrogate key linking the order to the customer dimension table.                              |
| order_date      | DATE          | The date when the order was placed.                                                           |
| shipping_date   | DATE          | The date when the order was shipped to the customer.                                          |
| due_date        | DATE          | The date when the order payment was due.                                                      |
| sales_amount    | INT           | The total monetary value of the sale for the line item, in whole currency units (e.g., 25).   |
| quantity        | INT           | The number of units of the product ordered for the line item (e.g., 1).                       |
| price           | INT           | The price per unit of the product for the line item, in whole currency units (e.g., 25).      |

### Purpose:
- Deliver a high-quality, structured data model optimized for business intelligence, data analysis.

![data_model](docs/data_model.png)

---

#  Tools and Technologies

- SQL Server
- ETL Pipelines (Stored Procedures, BULK INSERT)
- Star Schema Design
- Medallion Architecture (Bronze, Silver, Gold)

