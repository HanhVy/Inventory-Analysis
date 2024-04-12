# Inventory Analysis-Dashboard


## Problem Statement
Company Problem: WarmeHands Logistics Inc. spotted a risk about lose providers.
This dashboard helps fictitious Warm Hands Incorporated company figure out which are the best items for renewing or increasing inventory. Through different ratings, they get to know their improvement area, & thus they can optimize inventory control by identifying these areas. It also helps them reduce storage expenses, item costs, and unused goods and to increase profit , thus since using this dashboard they have identified this problem.






### Steps followed 

- Step 1 : Load data in Power Query Editor the file “WarmHands - data.xlsx” with all its tables.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on the entire dataset".
- Step 4 : Check errors and make sure all formats are correct. “Price” table has errors with some of its values. “Stock” table has a blank in “SKU-ID”. When merging the table “Price” into the table “Stock”, realize that just 103/104 match. In “SKU-ID” column of the “Stock” table contains an error, replacing “85135 C” value to “85135C”.


- Step 5: Merging the addtional information of the table “Price” into the table “Stock”. 


- Step 6: Import another file “categories.csv” and use the first row as a header.

- Step 7: There are any mistakes in the “category” column of the “Categories” table. Using Replace Value to correct any mistake into one of five category types: “office & school”, “decoration”, “jewelry”,”home accessories”, “toys & edibles”.

Before



After 



- Step 8: Fix the column “ID” in the “categories” table so that it is similar to other tables.



- Step 9: Cleaning the “Country” column in the “Orders” table. 
First: Split Column by Delimiter “-”


Second: “Country” column have a error value, Replace value “Spain.” into “Spain”



- Step 10: A 100% stacked column chart was also added to the report design area representing the total quantity of sale/country. While creating this visual, a field named "category" was also added to the Legends bucket, thus number of quantities are also segregated according to the category. 







-Step 10: Cleaning the “Cost” table.
   First: Remove duplicate “SKU” column.
   Second: Replace Value “..” into “.” in “factory_equipment_rent”.
   Third: Change the format of all columns in the “Cost” table except the “SKU” column to “Fixed decimal number” in the data view.
- Step 11 : Calculated “COGS”column was created in which.


 


-Step 12:  A clustered column chart was also added to the report design area representing the COGS/ Category.


There are 104 items within five categories, for which decoration and jewelry categories have the highest average costs of goods sold per item of around five dollars
- Step 13 : Calculated “Revenue_2020” column was created in the “Stock” table.

for creating new column following DAX expression was written;

   Revenue_2020 = Stock[2020_units_sold]*Stock[Retail_Price]

- Step 14 : Calculated “Profit_2020” column was created in the “Stock” table.
  

Profit_2020 = Stock[Revenue_2020]-(Stock[COGS]*Stock[2020_units_sold])


-Step 15:  A clustered column chart was also added to the report design area representing the Profit_2020/ Category.




Profit dominated by “home accessories” in 2020 with $110K.
The item “Grow a flytrap or sunflower” is a top seller with more than 16,000 items sold.
-Step 16:  Create Drill Through to more detail revenue, profit, COGS about items of category group.








-Step 17: Create a year column called “Year” from “InvoiceDate” in the table.

for creating new column following DAX expression was written;

          Year = YEAR(Orders[InvoiceDate])

-Step 17: Create a new column, “Total_orders”, that shows the total quantity of sales for each item in the “Stock” table.

for creating new column following DAX expression was written;

Total_orders = CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,Orders[SKU]=Stock[SKU-ID]))


-Step 18: Create a new column called “Quantity_2021” in the “Stock” table that calculates the number of items sold in 2021.

for creating new column following DAX expression was written;

Quantity_2021 = CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,Orders[SKU]=Stock[SKU-ID] && Orders[Year]=2021)).

-Step 19: Create a new column called “Quantity_2022” in the “Stock” table that calculates the number of items sold in 2022.

for creating new column following DAX expression was written;

Quantity_2022 = CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,Orders[SKU]=Stock[SKU-ID] && Orders[Year]=2022)).

-Step 20: A Pie column chart was also added to the report design area representing the Quantity/Year.



Total Quantity dominated with 92.69%.


-Step 21: 	Adding two clustered column charts to the report design area representing the Quantity_2021/Description and  Quantity_2022/Description.



In 2021 and 2022, The item “Grow a flytrap or sunflower” is a top seller with 26,12% in 2022 and 10.85% in 2021.

-Step 22: Create a new table called “Turnover” from the “Stock” table that only retains the columns “SKU-ID”, “Description”,”2021_start_Stock”, “Quantity_2021” and “COGS”.

for creating new column following DAX expression was written;
Turnover_2021 = SELECTCOLUMNS(Stock,"SKU-ID",Stock[SKU-ID],"Description",Stock[Description],"2021_start_stock",Stock[2021_start_stock],"Quantity_2021",Stock[Quantity_2021],"COGS",Stock[COGS]).

-Step 23:New measure was created to calculate total cost in 2021. 

for creating new column following DAX expression was written;

Total cost_2021 = Turnover_2021[Quantity_2021]*Turnover_2021[COGS].


-Step 24: Create a new table called “Avg_inventory”. 

for creating new column following DAX expression was written;

Avg_inventory = (Turnover_2021[2021Endstock]+Turnover_2021[2021_start_stock])*Turnover_2021[COGS]/2

-Step 25: Create a new table called “Inventory_turnover”. 


for creating new column following DAX expression was written;


Inventory turnover = Turnover_2021[Total cost_2021]/Turnover_2021[Avg_inventory]


-Step 26: Create a new table called “Revenue_2021”. 

for creating new column following DAX expression was written;

Revenue_2021 = Stock[Quantity_2021]*Stock[Retail_Price]


-Step 27: Create a new table called “percentrevenue2020”.

for creating new column following DAX expression was written;

Perccentrevenue2020 = Stock[Revenue_2020]/SUM(Stock[Revenue_2020])
-Step 28: Create a new table called “percentrevenue2021”.

for creating new column following DAX expression was written;

 
Perccentrevenue2021 = Stock[Revenue_2021]/SUM(Stock[Revenue_2021])


-Step 29: A cluttered column chart was also added to the report design area representing the Perccentrevenue2020 and Perccentrevenue2021  /Description.





Although the item “Grow a flytrap or sunflower” is a top seller with more than 16,000 items sold and contributes too much to total revenue. However, this item has a downward trend in revenue from 2020 to 2020.
The item “Set of 6 soldier skittles” has the highest grown percentage from 2020 to 2021 with 1.91% in 2020 and 5.6% in 2021.
-Step 30: Create a new table called “ABC” from the “Stock” table that only retains the columns “SKU-ID”, “Description”,”Revenue_2021”, “Perccentrevenue2021”.

for creating new column following DAX expression was written;

ABC = SELECTCOLUMNS(Stock,"SKU-ID",Stock[SKU-ID],"Description",Stock[Description],"Revenue_2021",Stock[Revenue_2021],"percent_revenueStock_2021",Stock[perccentrevenue2021]).




-Step 31: Sort “percent_revenueStock_2021”in descending.Create a new column called “CP_revenue” to calculate a cumulative increase over the sort.

for creating new column following DAX expression was written;

CP_revenue = CALCULATE(SUM(ABC[percent_revenueStock_2021]),FILTER(ABC,ABC[Revenue_2021]>=EARLIER(ABC[Revenue_2021])))


-Step 32: Create a new column “ABC_class” to classify a cumulative value.

for creating new column following DAX expression was written;

ABC_class = IF(ABC[CP_revenue]<0.7,"A[High Value]",IF(ABC[CP_revenue]>0.9,"C[Low Value]","B[Medium Value]"))

# Insights

A “Preliminary results” page report was created on Power BI Desktop 




Following inferences can be drawn from the dashboard;

### [1] Unit of sold: from 2020 to 2021

   Unit of sold in 2020 = 148000 (54.08%)
   
   Unit of sold in 2020 = 126000 (45.92%)
   
Thus, Unit of solid has a downward trend, specifically down 8.16.

### [2]Percent of total revenue: 

    “Set of 6 soldier skittles” has the highest increase from 2020 to 2021.
     The item with the most revenue percentage is “GROW A FLYTRAP OR SUNFLOWER IN TIN”. But it will tend to decrease from 2020 to 2021.
  
The item with the most inventory is “DOUGHNUT LIP GLOSS” which means the item is ordered way more than average. It also has grown from 2020 to 2021 => The company should increase the inventory stock on DOUGHNUT LIP GLOSS to meet customer demand and increase its profit.

A “Smart Purchasing” page report was created on Power BI Desktop 

  


Following inferences can be drawn from the dashboard;
  Based on two in the report, we realize that the item has the highest revenue (Set of 6 soldier skittles) and the item has lowest COGSRevenue (Woodland charlotte bag) not the same. This means the item that brings the highest revenue doesn't contribute to total profit much. The company should increase the inventory stock on Woodland charlotte bag to increase its profit.

A “Inspecting Categories” page report was created on Power BI Desktop 


  
When filtering for medium value items, jewelry category disappears.
When filtering for high value items, office & school and jewelry  category disappears.
This mean that the items in office & school and jewelry  category don’t contribute to much for total revenue. However,office & school category has the highest Inventory turnover. The company should focus on to increase storage inventory in three of the other and reconsidering about quantity of office & school category to meet customer demand and optimal revenue.




