# TASK 7: Python-SQL Integration for Basic Sales Reporting

## 🎯 Objective

The objective of this task was to demonstrate the core data analysis workflow by integrating SQL for data aggregation with Python for visualization and reporting. The goal was to calculate basic sales summary statistics (total quantity and total revenue per product) from a self-created SQLite database.

## 🛠️ Tools and Methodology

* **Primary Tools:** Python (Jupyter Notebook)
* **Libraries:** `sqlite3`, `pandas`, `matplotlib`
* **Database:** SQLite (built-in)

### Methodology

The process followed a four-stage sequential pipeline:

1.  **Connection & Setup:** Established a connection to the `sales_data.db` file (creating the file locally).
2.  **Data Ingestion:** Used SQL `CREATE TABLE` to define the structure and `INSERT INTO` (with Python's `executemany`) to populate the table with sample data. A `DELETE FROM` statement was included for repeatable execution.
3.  **Analysis:** Executed a single SQL query using `SUM()` and `GROUP BY` to aggregate sales data by product.
4.  **Reporting:** Used **Pandas (`pd.read_sql_query`)** to pull the aggregated results directly into a DataFrame for display and visualization using Matplotlib.

## 📁 Deliverables

* **Jupyter Notebook (.ipynb):** Contains the complete code flow from database creation to chart generation.
* **`sales_data.db`:** The created SQLite database file.

---
## WorkFlow

* **Importing the required Libraries and establishing connection with the database file**
```python
  import sqlite3
  import pandas as pd
  import matplotlib.pyplot as plt
  conn = sqlite3.connect("sales_data.db")
  cursor = conn.cursor()
  ```
* **Creating the Table and feeding input for the table**
```python
create_table_sql = """
CREATE TABLE IF NOT EXISTS sales (
    product TEXT,
    quantity INTEGER,
    price INTEGER
);
"""
cursor.execute(create_table_sql)
sales_data = [
    ('Laptop', 9, 1200),
    ('Mouse', 15, 25),
    ('Laptop', 3, 1200),
    ('Keyboard', 9, 75)
]
```
* **Query from the task**
```python
query = """
SELECT 
    product, 
    SUM(quantity) AS total_qty, 
    SUM(quantity * price) AS revenue 
FROM 
    sales 
GROUP BY 
    product
"""
df = pd.read_sql_query(query, conn)
```
* **Displays output using print and basic matplotlib bar chart**
```python
print("--- Aggregated Sales Summary ---")
print(df)
df.plot(kind='bar', x='product', y='revenue', 
        title='Total Revenue by Product', legend=False)
plt.ylabel('Revenue ($)')
plt.xticks(rotation=0) 
plt.show()
plt.savefig("sales_chart.png")
```
