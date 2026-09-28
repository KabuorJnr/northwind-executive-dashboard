# Northwind Executive Dashboard

A four page Power BI report built on the Northwinds database on your SQL Server (KABUORJNR). It uses a star schema with an import model.

## Open it

1. Unzip the folder anywhere on your Windows PC.
2. Open **Northwind Executive.pbip** in Power BI Desktop (January 2026 release or later).
3. The first time you open it, the visuals will be empty. Select **Home > Refresh** to load data from KABUORJNR. If Desktop asks for credentials, choose **Windows** (the same login you used for power_bi1.pbix).
4. If your server or database name changes, go to **Home > Transform data > Edit parameters** and update **Server** and **Database**.
5. To get a single file, use **File > Save as** and pick .pbix.

If Desktop says it can't open the project, turn on these two options, restart Desktop and try again: **File > Options and settings > Options > Preview features > Store reports using enhanced metadata format (PBIR)** and **Store semantic model using TMDL format**.

## Pages

1. **Executive Overview**: five headline KPIs (Revenue, Orders, Avg Order Value, Active Customers, On-Time Delivery), each compared with the same period last year. Below them are the monthly revenue trend with prior year and a 3 month average, revenue by category, the top 10 customers, revenue by country and revenue by sales rep.
2. **Products and Customers**: a category and product matrix (revenue, units, discount rate, share), the top 10 products and a customer Pareto (80/20) chart.
3. **Fulfilment and Logistics**: on-time rate, average days to ship, late and open orders, freight as a share of revenue, the on-time trend, the delivery status split, freight by shipper, a late orders list and shipping time by destination.
4. **Sales Team**: revenue by rep, category mix by rep and a rep scorecard.

Every page has Year, Customer country and Product category slicers. The Year slicer starts on **1998**, so the KPIs compare 1 Jan to 6 May 1998 with 1 Jan to 6 May 1997.

## Model

| Table | Grain | Source |
|---|---|---|
| FactSales | one row per order line | Order Details joined to Orders |
| FactOrders | one row per order | Orders |
| DimDate | one row per day, full calendar years covering the data | built in Power Query |
| DimCustomer | customer | Customers |
| DimProduct | product with category | Products and Categories |
| DimEmployee | sales rep | Employees |
| DimShipper | shipping company | Shippers |
| _Measures | measures only | none |

All relationships run one to many from each dimension to the facts, with single direction filtering. Keys are hidden, and implicit measures are turned off so every number in the report comes from a named, formatted measure.

Measures are grouped into display folders:

* **1 Sales**: Revenue, Gross Sales, Discounts, Discount %, Orders, Units Sold, Avg Order Value, Active Customers, Revenue per Customer
* **2 Time Intelligence**: prior year and YoY % for revenue, orders, order value and customers, plus Revenue YTD and Revenue 3M Avg. Prior year comparisons stop at the last date with sales, so a part year is compared with the same part of the previous year.
* **3 Fulfilment**: Order Count, Freight, Freight % of Revenue, Shipped, On-Time, Late and Open Orders, On-Time %, On-Time % change, Avg Days to Ship, Avg Days Late, Days Late
* **4 Analysis**: Revenue Share %, customer and product ranks, Top 10 revenue, Cumulative Revenue % (Pareto)
* **5 Labels**: the KPI change labels and colours, and the "Data through" and period captions

## Numbers to check after refresh

These figures come from the Orders table in your power_bi1.pbix. If your report matches them, the refresh and model are working.

| Check | Expected |
|---|---|
| Orders, all years | 830 |
| Orders, 1998 | 270 (129 in the same period of 1997) |
| Late orders, all years | 37 |
| Open (unshipped) orders | 21 |
| On-Time %, all years | 95.4% |
| Avg Days to Ship, all years | 8.5 |
| Freight, all years | $64,942.69 |

In the standard Northwind database, total Revenue across all years is about $1.27M.

## Design and accessibility

* The custom theme is in StaticResources/RegisteredResources. All text meets WCAG AA contrast (4.5:1 or better), and chart colours meet the 3:1 minimum for graphics.
* Growth is shown with an arrow and text, not colour alone.
* Every visual has alt text for screen readers, and the tab order runs left to right, top to bottom.
* Only the fields the report uses are loaded. Customer phone, fax, contact names and street addresses, and employee personal details, are left out of the model.

## Known limits

* Northwind has no product cost, so the report shows discounts rather than profit margin.
* Order level measures (freight, on-time, days to ship) respond to date, customer, rep and shipper filters. They don't respond to the Product category slicer, because an order can contain products from several categories.
* To publish to the Power BI service and share it, you need a Power BI Pro or Premium Per User licence, or a workspace on Fabric capacity. Your Fabric trial ends in about 12 days.
