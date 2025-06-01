# Sales-Report
Sales report of northwind dataset using visual analytics with powerbi as the analytic tool.
![sales report-images-0](https://github.com/user-attachments/assets/8617aa70-406f-4d2e-9b0a-58023cdb844d)


# This dataset was obtained as an sql dataset uploaded into an sql server.
# Dataset Upload
The dataset was imported into powerbi using the database connection by providing sql credentials (database-name, password, servername).

#Creating of New Measures
New measures were created to count and sum relevant columns.
Total Sales = SUMX('northwind orderdetails', 'northwind orderdetails'[Quantity] * LOOKUPVALUE('northwind products'[Price], 'northwind products'[ProductID], 'northwind orderdetails'[ProductID]))
No. of orders = COUNT('northwind orderdetails'[OrderID])
No. of customers = DISTINCTCOUNT('northwind customers'[CustomerName]).... amongst others.

#Creating of Calculated Tables
New tables were created to evaluate top performers and products.
Top 10 Suppliers = 
VAR topsuppliers = 
    SELECTCOLUMNS(
        'northwind suppliers',
        "Suppliers Name", 'northwind suppliers'[SupplierName],
        "Supplies", [No. of Quantity],
        "Suppliers Product", [No. of Products],
        "suppliers sales", [Total Sales]
    )
var top10 =
    TOPN(
        10, 
        topsuppliers,
        [suppliers sales],
        DESC
    )
RETURN top10

Top_10_products = 
VAR topProducts =
    SELECTCOLUMNS(
        'northwind products',
        "ProductID", 'northwind products'[ProductID],
        "Product Name", 'northwind products'[ProductName],
        "Product Price", 'northwind products'[Price],
        "Quantity", [No. of Quantity],
        "Sales amount", 'northwind products'[Price] * [No. of Quantity],
        "Category id", 'northwind products'[CategoryID]
    )
VAR addCategory = 
    ADDCOLUMNS(
        topProducts,
        "Product Category", LOOKUPVALUE('northwind categories'[CategoryName], 'northwind categories'[CategoryID], [Category id])
    )
VAR top10 =
    TOPN(10, addCategory, [Sales amount], DESC)
RETURN top10

#Analysis
![sales report-images-0](https://github.com/user-attachments/assets/b7c8c97a-cf02-46a3-b41c-6ae3b0cde464)
![sales report-images-1](https://github.com/user-attachments/assets/bd4a45af-8ef8-400e-b66d-6990a3462420)
![sales report-images-2](https://github.com/user-attachments/assets/7576e9fd-eb11-4180-8635-43ca51fcda63)
![sales report-images-3](https://github.com/user-attachments/assets/ef7e8205-93ff-4884-8b36-3d77aacd6b27)



