# DAX Measures

Create these measures after loading the CSVs and renaming `order_items` to `Sales`.

```DAX
Total Revenue =
CALCULATE(
    SUMX(Sales, Sales[Quantity] * Sales[UnitPrice] * (1 - Sales[DiscountPct])),
    Sales[Status] = "Completed"
)

Total Orders =
CALCULATE(DISTINCTCOUNT(Sales[OrderID]), Sales[Status] = "Completed")

Total Units =
CALCULATE(SUM(Sales[Quantity]), Sales[Status] = "Completed")

Total Customers = DISTINCTCOUNT(Sales[CustomerID])

Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)

Total Cost =
CALCULATE(
    SUMX(Sales, Sales[Quantity] * RELATED(Products[CostPerUnit])),
    Sales[Status] = "Completed"
)

Gross Profit = [Total Revenue] - [Total Cost]

Profit Margin % = DIVIDE([Gross Profit], [Total Revenue], 0)

Cancelled Orders =
CALCULATE(DISTINCTCOUNT(Sales[OrderID]), Sales[Status] = "Cancelled")

Returned Orders =
CALCULATE(DISTINCTCOUNT(Sales[OrderID]), Sales[Status] = "Returned")

Cancellation Rate % =
DIVIDE([Cancelled Orders], DISTINCTCOUNT(Sales[OrderID]), 0)

Revenue LY =
CALCULATE([Total Revenue], DATEADD('Date'[Date], -1, YEAR))

Revenue Growth % =
DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY], 0)

Payment Variance =
SUMX(
    VALUES(Sales[OrderID]),
    VAR OrderValue = CALCULATE(
        SUMX(Sales, Sales[Quantity] * Sales[UnitPrice] * (1 - Sales[DiscountPct]))
    )
    VAR PaidValue = CALCULATE(SUM(Payments[PaymentAmount]))
    RETURN ABS(OrderValue - PaidValue)
)
```

## Date table

```DAX
Date =
ADDCOLUMNS(
    CALENDAR(DATE(2025,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Year Month", FORMAT([Date], "YYYY-MM"),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```
