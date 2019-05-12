# How to run the program

1. Launch the Jupiter Program from Anaconda.
2. git pull repo
3. Navigate the repo in Jupiter File System.
4. Open "HeroesOfPymoli.ipynb" in Jupiter.
5. Run each command section to see results.


#Snapshot of the Jupiter File results added herewith. Note two things did not match the solution data.


1. Avg Total Purchase per Person for Purchasing Analysis (Gender) and Purchasing Analysis (Age) was not a very clear so I am not sure the caculation is correct. The value didn't match.
2. Was unable to edit the formating for [Item_Name] 


Heroes Of Pymoli Data Analysis
Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).

Note
Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.
# Dependencies and Setup
import pandas as pd
import numpy as np
​
# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"
​
# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
​
#Calculate Player Count
count = len(purchase_data["SN"].unique().tolist())
​
​
Player Count
Display the total number of players
#Add Player Count in Data Frame
my_list = [{"Total Players": count}]
player_count_df = pd.DataFrame(my_list)
player_count_df
Total Players
0	576
Purchasing Analysis (Total)
Run basic calculations to obtain number of unique items, average price, etc.
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
#Calculate to obtain number of unique items
unique_items = len(purchase_data["Item ID"].unique().tolist())
#Calculate Average Price
average_price = round(purchase_data["Price"].mean(),2)
#Calculate Number of Purchases
no_of_purchase = purchase_data["Purchase ID"].count()
#Calculate Total Revenue
total_revenue = purchase_data["Price"].sum()
​
my_list = [{"Number of Unique Items": unique_items, "Average Price":average_price, "Number Of Purchase":no_of_purchase, 
          "Total Revenue":total_revenue}]
summary_df = pd.DataFrame(my_list)
summary_df
​
​
Average Price	Number Of Purchase	Number of Unique Items	Total Revenue
0	3.05	780	183	2379.77
Gender Demographics
Percentage and Count of Male Players
Percentage and Count of Female Players
Percentage and Count of Other / Non-Disclosed
count_of_players = purchase_data["Gender"].value_counts()
count_of_players
df = count_of_players.rename_axis('unique_values').to_frame('Total Counts')
df['Percentage Of Players'] = df['Total Counts']/no_of_purchase * 100
df
Total Counts	Percentage Of Players
unique_values		
Male	652	83.589744
Female	113	14.487179
Other / Non-Disclosed	15	1.923077
Purchasing Analysis (Gender)
Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
grouped_purchase_data = purchase_data.groupby(['Gender'])
purchase_data.head()
grouped_purchase_data = purchase_data.groupby(['Gender'])
purchase_data.head()
purchase_count = purchase_data["Gender"].value_counts()
avg_purchase_price = grouped_purchase_data["Price"].sum()/grouped_purchase_data["Gender"].count()
total_purchase = grouped_purchase_data["Price"].sum()
​
purchase_summary_table = pd.DataFrame({"Purchase Count": purchase_count,
                                       "Average Purchase Price": avg_purchase_price,
                                       "Total Purchase Value": total_purchase
                                      })
​
purchase_summary_table['Avg Total Purchase per Person'] = (purchase_summary_table['Total Purchase Value']/purchase_count) + 1
purchase_summary_table.head()
                                                           
                                                           
                                                           
                                                           
Purchase Count	Average Purchase Price	Total Purchase Value	Avg Total Purchase per Person
Female	113	3.203009	361.94	4.203009
Male	652	3.017853	1967.64	4.017853
Other / Non-Disclosed	15	3.346000	50.19	4.346000
Age Demographics
Establish bins for ages
Categorize the existing players using the age bins. Hint: use pd.cut()
Calculate the numbers and percentages by age group
Create a summary data frame to hold the results
Optional: round the percentage column to two decimal points
Display Age Demographics Table
# Create the bins in which Data will be held
# Bins are 0, 59, 69, 79, 89, 100.   
bins = [0, 9, 14, 19, 24, 29, 34, 39, 99]
​
# Create the names for the four bins
group_names = ["A", "B", "C", "D", "E", "F", "G", "H"]
​
purchase_data.head()
​
purchase_data["Age Summary"] = pd.cut(purchase_data["Age"], bins, labels=group_names)
purchase_data
​
grouped_age_demographics = purchase_data.groupby(['Age Summary'])
grouped_age_demographics
​
Total_Per_Group = grouped_age_demographics['Age'].count()
Total_percentage_Per_Group = round(grouped_age_demographics['Age'].count()/purchase_data['Age'].count() * 100, 2)
​
​
Age_summary_table = pd.DataFrame({"Total Count": Total_Per_Group,
                                 "Percentage of Players": Total_percentage_Per_Group
                                
                                })
Age_summary_table
​
​
                                           
Total Count	Percentage of Players
Age Summary		
A	23	2.95
B	28	3.59
C	136	17.44
D	365	46.79
E	101	12.95
F	73	9.36
G	41	5.26
H	13	1.67
Purchasing Analysis (Age)
Bin the purchase_data data frame by age
Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
# Calcuation
#Purchasing Analysis (Age) Calcuation
​
purchase_data.head()
​
​
Total_Purchase_Value = grouped_age_demographics['Price'].sum()
Avg_Total_Purchase_per_Person = round(Total_Purchase_Value/grouped_age_demographics['Age Summary'].count() + 1, 2)
average_purchase_price = round(Total_Purchase_Value/Total_Per_Group, 2)
​
Age_summary_table2 = pd.DataFrame({"Total Count": Total_Per_Group,
                                 "Average Purchase Price": average_purchase_price,
                                   "Total Purchase Value": Total_Purchase_Value,
                                   "Avg Total Purchase per Person": Avg_Total_Purchase_per_Person
                                
                                })
Age_summary_table2
Total Count	Average Purchase Price	Total Purchase Value	Avg Total Purchase per Person
Age Summary				
A	23	3.35	77.13	4.35
B	28	2.96	82.78	3.96
C	136	3.04	412.89	4.04
D	365	3.05	1114.06	4.05
E	101	2.90	293.00	3.90
F	73	2.93	214.00	3.93
G	41	3.60	147.67	4.60
H	13	2.94	38.24	3.94
Top Spenders
Run basic calculations to obtain the results in the table below
Create a summary data frame to hold the results
Sort the total purchase value column in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the summary data frame
TopSpenders_summary_table_sorted = TopSpenders_summary_table.sort_values('Total Purchase Value', ascending = False)
TopSpenders_summary_table_sorted.head()
#Top Spendors Calculation
grouped_spenders_data = purchase_data.groupby(['SN'])
Purchase_Count = grouped_spenders_data['Purchase ID'].count()
Average_Purchase_Price = round(grouped_spenders_data['Price'].sum()/grouped_spenders_data['SN'].count(),2)
Total_Purchase_Value = grouped_spenders_data['Price'].sum()
​
TopSpenders_summary_table = pd.DataFrame({"Purchase Count": Purchase_Count,
                                 "Average Purchase Price": Average_Purchase_Price,
                                   "Total Purchase Value": Total_Purchase_Value
                                })
​
TopSpenders_summary_table_sorted = TopSpenders_summary_table.sort_values('Total Purchase Value', ascending = False)
TopSpenders_summary_table_sorted.head()
Purchase Count	Average Purchase Price	Total Purchase Value
SN			
Lisosia93	5	3.79	18.96
Idastidru52	4	3.86	15.45
Chamjask73	3	4.61	13.83
Iral74	4	3.40	13.62
Iskadarya95	3	4.37	13.10
Most Popular Items
Retrieve the Item ID, Item Name, and Item Price columns
Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value
Create a summary data frame to hold the results
Sort the purchase count column in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the summary data frame
#Most Popular Items
​
grouped_itemid_data = purchase_data.groupby(['Item ID'])
grouped_itemname_data = purchase_data.groupby(['Item Name'])
​
Purchase_Count_item = grouped_itemid_data["Item ID"].count()
Item_Price = grouped_itemid_data["Price"].unique()
Item_Name = grouped_itemid_data["Item Name"].unique()
Total_Purchase_Value = grouped_itemid_data["Price"].sum()
​
Item_summary_table = pd.DataFrame({
                                 "Item Name": Item_Name,
                                 "Purchase Count": Purchase_Count_item,
                                 "Item Price": Item_Price,
                                 "Total Purchase Value": Total_Purchase_Value
                                })
​
Item_summary_table_sorted = Item_summary_table.sort_values('Purchase Count', ascending = False)
​
#Item_summary_table_sorted["Item Price"] = Item_summary_table_sorted["Item Price"].map("${:.2f}".format)
Item_summary_table_sorted["Total Purchase Value"] = Item_summary_table_sorted["Total Purchase Value"].map("${:.2f}".format)
​
Item_summary_table_sorted.head()
Item Name	Purchase Count	Item Price	Total Purchase Value
Item ID				
178	[Oathbreaker, Last Hope of the Breaking Storm]	12	[4.23]	$50.76
145	[Fiery Glass Crusader]	9	[4.58]	$41.22
108	[Extraction, Quickblade Of Trembling Hands]	9	[3.53]	$31.77
82	[Nirvana]	9	[4.9]	$44.10
19	[Pursuit, Cudgel of Necromancy]	8	[1.02]	$8.16
Most Profitable Items
Sort the above table by total purchase value in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the data frame
#Most Profitable Items
​
Most_Profitable_table_sorted = Item_summary_table.sort_values('Total Purchase Value', ascending = False)
Most_Profitable_table_sorted["Total Purchase Value"] = Most_Profitable_table_sorted["Total Purchase Value"].map("${:.2f}".format)
​
Most_Profitable_table_sorted.head()
Item Name	Purchase Count	Item Price	Total Purchase Value
Item ID				
178	[Oathbreaker, Last Hope of the Breaking Storm]	12	[4.23]	$50.76
82	[Nirvana]	9	[4.9]	$44.10
145	[Fiery Glass Crusader]	9	[4.58]	$41.22
92	[Final Critic]	8	[4.88]	$39.04
103	[Singed Scalpel]	8	[4.35]	$34.80
