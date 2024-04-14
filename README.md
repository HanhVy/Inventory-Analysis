

# Inventory Analysis - Dashboard


## Problem Statement
Company Problem: WarmeHands Logistics Inc. is having problems managing its warehouse
This dashboard helps fictitious Warm Hands Incorporated company figure out which are the best items for renewing or increasing inventory. Through different ratings, they get to know their improvement area, & thus they can optimize inventory control by identifying these areas. It also helps them reduce storage expenses, item costs, and unused goods and to increase profit , thus since using this dashboard they have identified this problem.


### Steps followed 

- Step 1 : Load data in Power Query Editor the file “WarmHands - data.xlsx” with all its tables.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on the entire dataset".
- Step 4 : Check errors and make sure all formats are correct. “Price” table has errors with some of its values. “Stock” table has a blank in “SKU-ID”. When merging the table “Price” into the table “Stock”, realize that just 103/104 match. In “SKU-ID” column of the “Stock” table contains an error, replacing “85135 C” value to “85135C”.

- Step 5:  Merging the addtional information of the table “Price” into the table “Stock”. 


![Screenshot 2024-04-11 134938](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/f8110db3-99f2-4dd7-afa1-c8062cbf7245)



- Step 6 :Import another file “categories.csv” and use the first row as a header.
![Screenshot 2024-04-11 134724](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/445c07d6-f8dd-4bbb-81f5-09891a023729)


- Step 7 :There are any mistakes in the “category” column of the “Categories” table. Using Replace Value to correct any mistake into one of five category types: “office & school”, “decoration”, “jewelry”,”home accessories”, “toys & edibles”.

Before

![Screenshot 2024-04-11 110953](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/9ccead2a-60f4-439b-90b6-c0d9f508c7ba)


After 

![Screenshot 2024-04-11 111030](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/49967829-c4b5-4b9a-8b84-212a877b4850)
![Screenshot 2024-04-11 110953](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/3453473e-b3fa-4f09-9c0c-73d7b4d034ae)


- Step 8 :Fix the column “ID” in the “categories” table so that it is similar to other tables.

![Screenshot 2024-04-11 111300](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/51a7b071-b022-46f7-a3e9-2d6849fed965)


- Step 9 :Cleaning the “Country” column in the “Orders” table. 

First: Split Column by Delimiter “-”

![Screenshot 2024-04-11 134724](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/c64219ad-7752-4562-8578-96f1f73b6b12)


Second: “Country” column have a error value, Replace value “Spain.” into “Spain”

![Screenshot 2024-04-11 134938](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/2596be09-bc6b-47cf-a346-14cb8ef81762)



- Step 10 :A 100% stacked column chart was also added to the report design area representing the total quantity of sale/country. While creating this visual, a field named "category" was also added to the Legends bucket, thus number of quantities are also segregated according to the category. 

![Screenshot 2024-04-11 135417](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/825f0ab0-7a79-40db-9e85-001988dd6bc6)





- Step 11 : Cleaning the “Cost” table.
First: Remove duplicate “SKU” column.
Second: Replace Value “..” into “.” in “factory_equipment_rent”.
Third: Change the format of all columns in the “Cost” table except the “SKU” column to “Fixed decimal number” in the data view.

- Step 12 : Calculated “COGS”column was created in which.


 ![Screenshot 2024-04-11 141138](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/ee950bcc-41aa-418d-bd53-80ca842cd435)



- Step 13 : A clustered column chart was also added to the report design area representing the COGS/ Category.

![Screenshot 2024-04-11 142344](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/51711fee-db28-417d-b38b-a7943bb51835)

=> There are 104 items within five categories, for which decoration and jewelry categories have the highest average costs of goods sold per item of around five dollars
- Step 15 : Calculated “Revenue_2020” column was created in the “Stock” table.
Following DAX expression was written for the same,

       Revenue_2020 = (Stock[2020_units_sold]*Stock[Retail_Price])


- Step 16 : Calculated “Profit_2020” column was created in the “Stock” table.

for creating new column following DAX expression was written,


     Profit_2020 = Stock[Revenue_2020]-(Stock[COGS]*Stock[2020_units_sold])


- Step 17:  A clustered column chart was also added to the report design area representing the Profit_2020/ Category.

![Screenshot 2024-04-11 145125](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/53079eda-01ba-4790-b47f-e43668a42875)



=> Profit dominated by “home accessories” in 2020 with $110K.
The item “Grow a flytrap or sunflower” is a top seller with more than 16,000 items sold.

- Step 18:  Create Drill Through to more detail revenue, profit, COGS about items of category group.


![Screenshot 2024-04-11 154151](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/d814038e-7b82-4e8c-b795-8662d9851bf2)


![Screenshot 2024-04-11 154111](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/3c168199-97c0-44ab-aafb-50ecc7c4dd2e)




- Step 19: Create a year column called “Year” from “InvoiceDate” in the table.

for creating new column following DAX expression was written;

          Year = YEAR(Orders[InvoiceDate])

- Step 20: Create a new column, “Total_orders”, that shows the total quantity of sales for each item in the “Stock” table.

for creating new column following DAX expression was written;


         Total_orders = CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,Orders[SKU]=Stock[SKU-ID]))


- Step 21: Create a new column called “Quantity_2021” in the “Stock” table that calculates the number of items sold in 2021.


for creating new column following DAX expression was written;

    Quantity_2021 =
    CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,  Orders[SKU]=Stock[SKU-ID] && Orders[Year]=2021)).

- Step 22: Create a new column called “Quantity_2022” in the “Stock” table that calculates the number of items sold in 2022.

for creating new column following DAX expression was written;


     Quantity_2022 = CALCULATE(SUM(Orders[Quantity]),FILTER(Orders,Orders[SKU]=Stock[SKU-ID] && Orders[Year]=2022)).


- Step 23: A Pie column chart was also added to the report design area representing the Quantity/Year.

![Screenshot 2024-04-11 170211](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/bb19035c-44fb-4ad6-b569-98b3e316a25c)

=> Total Quantity dominated with 92.69%.


- step 24: 	Adding two clustered column charts to the report design area representing the Quantity_2021/Description and  Quantity_2022/Description.

![Screenshot 2024-04-11 170730](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/694fdbbc-20b8-4f59-8e02-72e713e3eb17)
![Screenshot 2024-04-11 170902](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/deb1e968-1c94-4eca-8754-eb99198dd99e)


=> In 2021 and 2022, The item “Grow a flytrap or sunflower” is a top seller with 26,12% in 2022 and 10.85% in 2021.

 - Step 25 :Create a new table called “Turnover” from the “Stock” table that only retains the columns “SKU-ID”, “Description”,”2021_start_Stock”, “Quantity_2021” and “COGS”.

for creating new column following DAX expression was written;

     Turnover_2021 = SELECTCOLUMNS(Stock,"SKU-ID",Stock[SKU-ID],"Description",Stock[Description],"2021_start_stock",Stock[2021_start_stock],"Quantity_2021",Stock[Quantity_2021],"COGS",Stock[COGS]).


- Step 26:New measure was created to calculate total cost in 2021
. 

for creating new column following DAX expression was written;


    Total cost_2021 = Turnover_2021[Quantity_2021]*Turnover_2021[COGS].




- Step 27: Create a new table called “Avg_inventory”. 


for creating new column following DAX expression was written;

    Avg_inventory = (Turnover_2021[2021Endstock]+Turnover_2021[2021_start_stock])*Turnover_2021[COGS]/2



- Step 28: Create a new table called “Inventory_turnover”. 



for creating new column following DAX expression was written;



     Inventory turnover = Turnover_2021[Total cost_2021]/Turnover_2021[Avg_inventory]




- Step 29: Create a new table called “Revenue_2021”. 


for creating new column following DAX expression was written;


    Revenue_2021 = Stock[Quantity_2021]*Stock[Retail_Price]


- Step 30: Create a new table called “percentrevenue2020”.


for creating new column following DAX expression was written;


     Perccentrevenue2020 = Stock[Revenue_2020]/SUM(Stock[Revenue_2020])

- Step 31: Create a new table called “percentrevenue2021”.


for creating new column following DAX expression was written;


 
Perccentrevenue2021 = Stock[Revenue_2021]/SUM(Stock[Revenue_2021])




- Step 32: A cluttered column chart was also added to the report design area representing the Perccentrevenue2020 and Perccentrevenue2021  /Description.

![Screenshot 2024-04-11 182711](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/6a999477-301f-40ee-aa22-05bc9d7e856c)




Although the item “Grow a flytrap or sunflower” is a top seller with more than 16,000 items sold and contributes too much to total revenue. However, this item has a downward trend in revenue from 2020 to 2020.


The item “Set of 6 soldier skittles” has the highest grown percentage from 2020 to 2021 with 1.91% in 2020 and 5.6% in 2021.


- Step 33: Create a new table called “ABC” from the “Stock” table that only retains the columns “SKU-ID”, “Description”,”Revenue_2021”, “Perccentrevenue2021”.


for creating new column following DAX expression was written;


ABC = SELECTCOLUMNS(Stock,"SKU-ID",Stock[SKU-ID],"Description",Stock[Description],"Revenue_2021",Stock[Revenue_2021],"percent_revenueStock_2021",Stock[perccentrevenue2021]).




- Step 34: Sort “percent_revenueStock_2021”in descending.Create a new column called “CP_revenue” to calculate a cumulative increase over the sort.


for creating new column following DAX expression was written;


CP_revenue = CALCULATE(SUM(ABC[percent_revenueStock_2021]),FILTER(ABC,ABC[Revenue_2021]>=EARLIER(ABC[Revenue_2021])))



- Step 35: Create a new column “ABC_class” to classify a cumulative value.


for creating new column following DAX expression was written;


ABC_class = IF(ABC[CP_revenue]<0.7,"A[High Value]",IF(ABC[CP_revenue]>0.9,"C[Low Value]","B[Medium Value]"))
- Step 36: Create a new column called "Rank" based on the descending order of column.

for creating new column following DAX expression was written;
 
     Rank = RANK(DENSE,ORDERBY(ABC[percent_revenueStock_2021],DESC))
# Insights

## Overveiw page
![Screenshot 2024-04-12 164157](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/1a8f98cd-5367-4ae1-b386-0b15e7e47934)

- Diffetence in average and mean inventory turnover from A and B class items doesn't significantly, just 0.21 for average and 0.07 for mean Which means they sell almost equally fast.


## Preliminary results
![Screenshot 2024-04-12 165710](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/6f060307-8a29-4c71-9b77-8902123452cd)

 [1] Unit of sold: from 2020 to 2021

   Unit of sold in 2020 = 148000 (54.08%)
   
   Unit of sold in 2020 = 126000 (45.92%)
   
Thus, Unit of solid has a downward trend, specifically down 8.16.


[2]Percent of total revenue: 

-  “Set of 6 soldier skittles” has the highest increase from 2020 to 2021.
- The item with the most revenue percentage is “GROW A FLYTRAP OR SUNFLOWER IN TIN”. But it will tend to decrease from 2020 to 2021.
  
- The item with the most inventory is “DOUGHNUT LIP GLOSS” which means the item is ordered way more than average. It also has grown from 2020 to 2021 => The company should increase the inventory stock on DOUGHNUT LIP GLOSS to meet customer demand and increase its profit.

## “Smart Purchasing” 

  
![Screenshot 2024-04-12 174528](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/31f7c424-085a-4303-a4f8-3592d041a629)


Following inferences can be drawn from the dashboard;

  Based on two in the report, we realize that the item has the highest revenue (Set of 6 soldier skittles) and the item has lowest COGSRevenue (Woodland charlotte bag) not the same. This means the item that brings the highest revenue doesn't contribute to total profit much. The company should increase the inventory stock on Woodland charlotte bag to increase its profit.

##  “Inspecting Categories” 

![Screenshot 2024-04-12 174703](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/193221b6-c879-4d1c-97b8-2001fc16e5b7)

  
When filtering for medium value items, jewelry category disappears.
When filtering for high value items, office & school and jewelry  category disappears.
This mean that the items in office & school and jewelry  category don’t contribute to much for total revenue. However,office & school category has the highest Inventory turnover. The company should focus on to increase storage inventory in three of the other and reconsidering about quantity of office & school category to meet customer demand and optimal revenue.

## Revenue And Class

![Screenshot 2024-04-12 175257](https://github.com/HanhVy/Inventory-Analysis/assets/166614604/62c1126b-d2c7-4ba5-8fa3-feca0ea99f2e)

- United Kingdom is the biggest consumer of the item.
- DOUGHNUT LIP GLOSS has a high inventory turnover for having such a small revenue because the number of units ordered is relative low. This item could be has success with more inventory.
# Summary

-  DOUGHNUT LIP GLOSS has a the highest inventory turnover which means they sell this item fast.
- GROW A FLYTRAP OR SUNFLOWER IN TIN has lead in sales.
- SET OF 6 SOLDIER SKITTLES has increased in sales
=> All three belong to class A
- jewelry and office & school categories aren't in the A class which means they don't contribute too much to total revenue, however office & school category has the highest inventory turnover which means customers have high demand for this category.
- United Kingdom is the biggest consumer
- We should have approriate plan for  items have not only high revenue but also high COGSRevenue. This means the item that brings the highest revenue doesn't contribute to total profit much
