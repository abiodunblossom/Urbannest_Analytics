# UrbanNest Retail Analytics

## Project Purpose

This project explores UrbanNest retail order data and prepares it for analysis in Python and PostgreSQL. The intended business analysis covers revenue, product categories, customer segments, sales channels, and delivery performance.

## Process Flow

1. **Load and inspect the source data** - Read `data/UrbanNest_Retail_Dirty_Customer_Order_Dataset.csv` into pandas, then review its shape, columns, data types, sample records, and missing values.
2. **Create a working copy** - Copy the source DataFrame to `urbannest_df_copy` so cleaning and derived fields do not alter the original data.
3. **Clean text values** - Trim whitespace from text columns, standardize known gender values, treat `Unknown` gender as missing, and normalize city spelling and capitalization.
4. **Standardize categories** - Normalize capitalization and known variations in order status, payment method, sales channel, delivery status, and return flag.
5. **Calculate and position subtotal** - Create `subtotal` as `quantity * unit_price`, then place it immediately after `unit_price`. Format price values for display without changing the numeric columns.
6. **Prepare order dates** - Convert `order_date` to datetime, coercing invalid values to `NaT`. Derive `order_month` as a full month name and `order_year` as a numeric year, then place both columns immediately after `order_date`.
7. **Protect phone numbers** - Convert `phone_number` to a string and mask its middle digits, leaving the first three and last two visible.
8. **Review the cleaned data** - Inspect the DataFrame's data types, review the largest missing-value counts, and preview the first five records.
9. **Load data into PostgreSQL** - Connect to the local PostgreSQL database and write the cleaned `urbannest_df_copy` DataFrame to the `urbannest_data` table.
10. **Query the database** - Run the example queries in `queries/urbannest_queries.sql` after the table has been created.

## Current Notebook Coverage

`urbannest_retail.ipynb` implements the loading, inspection, cleaning, derived-column, phone-masking, quality-review, and PostgreSQL export steps above. Grouped business-performance summaries by product category, customer segment, sales channel, and delivery performance remain analysis opportunities; they are not currently produced by the notebook.

## Running the Project

1. Open the project folder in VS Code and select a Python environment with Jupyter, pandas, SQLAlchemy, and `psycopg2-binary` installed.
2. Open `urbannest_retail.ipynb` and run the cells from top to bottom.
3. To run the database export, start PostgreSQL, create the `urbannest_db` database, and update the connection settings in the export cell for your local setup.
4. Open `queries/urbannest_queries.sql` in a PostgreSQL client and run the desired query after the table has been created.

## Database Safety

The notebook's export uses `if_exists="replace"`, so rerunning it drops and recreates the existing `urbannest_data` table. Back up any table data that must be retained before running the export. Do not commit real database passwords; use environment variables or a local, untracked configuration for credentials.
