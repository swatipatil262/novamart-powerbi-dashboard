# Data Dictionary

| File | Key columns | Purpose |
|---|---|---|
| customers.csv | CustomerID, City, Region, CustomerSegment | Customer dimension |
| products.csv | ProductID, ProductName, Category, UnitPrice, CostPerUnit | Product dimension |
| orders.csv | OrderID, OrderDate, CustomerID, Channel, Status | Order-level facts |
| order_items.csv | OrderID, ProductID, Quantity, UnitPrice, DiscountPct | Sales line items |
| payments.csv | PaymentID, OrderID, PaymentDate, PaymentMethod, PaymentAmount | Payment records |

All customer names, product names and transactions are synthetic.
