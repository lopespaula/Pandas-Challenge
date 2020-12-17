# Pandas-Challenge

#Dependencies and Setup
import pandas as pd
import csv

#File to Load
file_to_load = "Resources/purchase_data.csv"

#Read Purchasing File and store into Pandas data frame
pymoli_df = pd.read_csv(file_to_load)


# ## Player Count

# * Display the total number of players


total_players = len(pymoli_df["SN"].unique())

total_players_df = pd.DataFrame({"Total Players": [total_players]})

total_players_df


# ## Purchasing Analysis (Total)

# * Run basic calculations to obtain number of unique items, average price, etc.
# * Create a summary data frame to hold the results
# * Optional: give the displayed data cleaner formatting
# * Display the summary data frame


total_items = len(pymoli_df["Item ID"].unique())

av_price = pymoli_df["Price"].mean()

purchases = pymoli_df["Purchase ID"].count()

revenue = pymoli_df["Price"].sum()

purchasing_analysis_df = pd.DataFrame({"Number of Unique Items":[total_items], 
                                    "Average Price":[av_price],
                                   "Number of Purchases":[purchases],
                                   "Total Revenue":[revenue]})

#formatting
purchasing_analysis_df["Average Price"] = purchasing_analysis_df["Average Price"].map("${:,.2f}".format)
purchasing_analysis_df["Total Revenue"] = purchasing_analysis_df["Total Revenue"].map("${:,.2f}".format)


purchasing_analysis_df


# ## Gender Demographics

# * Percentage and Count of Male Players
# * Percentage and Count of Female Players
# * Percentage and Count of Other / Non-Disclosed


total_players = pymoli_df.loc[:, ["SN", "Gender"]]
total_players = total_players.drop_duplicates()

num_players = total_players.count() [0]

gender_demographics_total = total_players["Gender"].value_counts()
gender_demographics_percents = gender_demographics_total / num_players

gender_demographics_df = pd.DataFrame({"Total Count": gender_demographics_total, 
                                    "Percentage of Players": gender_demographics_percents})

#formatting
gender_demographics_df["Percentage of Players"] = gender_demographics_df["Percentage of Players"].map("{:,.2%}".format)


gender_demographics_df



# ## Purchasing Analysis (Gender)

# * Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
# * Create a summary data frame to hold the results 
# * Optional: give the displayed data cleaner formatting
# * Display the summary data frame



p_analysis_df = pymoli_df.loc[:, ["SN", "Gender", "Item ID", "Price"]]

purchase_count = p_analysis_df["Gender"].value_counts()

total_price = p_analysis_df.groupby(by=['Gender'])['Price'].sum()

average_price = total_price/purchase_count

#Calculating Total Players per Gender
total_players_df = pymoli_df.loc[:, ["SN", "Gender"]]
total_players_df = total_players_df.drop_duplicates()
total_players_per_gender = total_players_df["Gender"].value_counts()


average_price_per_person = total_price/total_players_per_gender



purchasing_analysis_df = pd.DataFrame({"Purchase Count": purchase_count, 
                                    "Average Purchase Price": average_price, 
                                    "Total Purchase Value": total_price,
                                   "Avg Total Purchase per Person": average_price_per_person,})

#formatting
purchasing_analysis_df["Average Purchase Price"] = purchasing_analysis_df["Average Purchase Price"].map("${:,.2f}".format)
purchasing_analysis_df["Total Purchase Value"] = purchasing_analysis_df["Total Purchase Value"].map("${:,.2f}".format)
purchasing_analysis_df["Avg Total Purchase per Person"] = purchasing_analysis_df["Avg Total Purchase per Person"].map("${:,.2f}".format)


purchasing_analysis_df



# ## Age Demographics

# * Establish bins for ages
# * Categorize the existing players using the age bins. Hint: use pd.cut()
# * Calculate the numbers and percentages by age group 
# * Create a summary data frame to hold the results
# * Optional: round the percentage column to two decimal points 
# * Display Age Demographics Table


total_players_df = pymoli_df.loc[:, ["SN", "Age"]]
total_players_df = total_players_df.drop_duplicates()
total_players = len(pymoli_df["SN"].unique())

#Create bins and labels
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]
group_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#Place the data series into a new column inside of the df
total_players_df["Age Ranges"] = pd.cut(total_players_df["Age"], bins, labels=group_labels)

#Create a GroupBy object based upon "Age Ranges"
age_groups = total_players_df.groupby("Age Ranges")


total_age_count = age_groups["Age"].count()

percentage_of_players = total_age_count/total_players


age_demographics_df = pd.DataFrame({"Total Count": total_age_count, 
                                    "Percentage of Players": percentage_of_players})


#formatting
age_demographics_df["Percentage of Players"] = age_demographics_df["Percentage of Players"].map("{:,.2%}".format)



age_demographics_df


# ## Purchasing Analysis (Age)

# * Bin the purchase_data data frame by age
# * Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below 
# * Create a summary data frame to hold the results
# * Optional: give the displayed data cleaner formatting
# * Display the summary data frame




#Create bins and labels
bins = [0, 9, 14, 19, 24, 29, 34, 39, 100]
group_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#Place the data series into a new column inside of the df
pymoli_df["Age Ranges"] = pd.cut(pymoli_df["Age"], bins, labels=group_labels)

#Create a GroupBy object based upon "Age Ranges"
age_groups = pymoli_df.groupby("Age Ranges")


purchase_count = age_groups["Age"].count()

total_purchase_value = pymoli_df.groupby(by=['Age Ranges'])['Price'].sum()

average_price = total_purchase_value/purchase_count

#Calculating total unique players
total_players_df = pymoli_df.loc[:, ["Age Ranges", "SN"]]
total_players_df = total_players_df.drop_duplicates()
total_players = total_players_df["Age Ranges"].value_counts()

av_purchase_price = total_purchase_value/total_players


purchasing_analysis_df = pd.DataFrame({"Purchase Count": purchase_count, 
                                    "Average Purchase Price": average_price,
                                    "Total Purchase Value": total_purchase_value,
                                    "Avg Total Purchase per Person": av_purchase_price})


#Formating
purchasing_analysis_df["Average Purchase Price"] = purchasing_analysis_df["Average Purchase Price"].map("${:,.2f}".format)
purchasing_analysis_df["Total Purchase Value"] = purchasing_analysis_df["Total Purchase Value"].map("${:,.2f}".format)
purchasing_analysis_df["Avg Total Purchase per Person"] = purchasing_analysis_df["Avg Total Purchase per Person"].map("${:,.2f}".format)


purchasing_analysis_df


# ## Top Spenders

# * Run basic calculations to obtain the results in the table below
# * Create a summary data frame to hold the results 
# * Sort the total purchase value column in descending order
# * Optional: give the displayed data cleaner formatting 
# * Display a preview of the summary data frame



#Purchase Count
purchase_count = pymoli_df.groupby(by=['SN'])['Item ID'].count()

#Total Purchase Value
total_purchase_value = pymoli_df.groupby(by=['SN'])['Price'].sum()


#Average Purchase Price
average_price = total_purchase_value/purchase_count


#DataFrame with the index = SN
top_spenders_df = pd.DataFrame({"Purchase Count": purchase_count, 
                             "Average Purchase Price": average_price,
                             "Total Purchase Value": total_purchase_value})


#Reorganize the dataframe in decreasing order of Total Purchase Value
sorted_df = top_spenders_df.sort_values(["Total Purchase Value"], ascending=False)

#Formating
sorted_df["Average Purchase Price"] = sorted_df["Average Purchase Price"].map("${:,.2f}".format)
sorted_df["Total Purchase Value"] = sorted_df["Total Purchase Value"].map("${:,.2f}".format)



sorted_df.head(5)


# ## Most Popular Items

# * Retrieve the Item ID, Item Name, and Item Price columns
# * Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value
# * Create a summary data frame to hold the results
# * Sort the purchase count column in descending order
# * Optional: give the displayed data cleaner formatting 
# * Display a preview of the summary data frame



#Purchase Count
purchase_count = pymoli_df.groupby(by=['Item ID', 'Item Name'])['Item ID'].count()

#Total Purchase Value
total_purchase_value = pymoli_df.groupby(by=['Item ID', 'Item Name'])['Price'].sum()

#Item Price
item_price = total_purchase_value/purchase_count


#DataFrame
top_spenders_df = pd.DataFrame({"Purchase Count": purchase_count, 
                             "Item Price": item_price,
                             "Total Purchase Value": total_purchase_value})


#Reorganize the dataframe in decreasing order of Purchase Count
sorted_df = top_spenders_df.sort_values(["Purchase Count"], ascending=False)


#Formating
sorted_df["Item Price"] = sorted_df["Item Price"].map("${:,.2f}".format)
sorted_df["Total Purchase Value"] = sorted_df["Total Purchase Value"].map("${:,.2f}".format)


sorted_df.head(5)


# ## Most Profitable Items

# * Sort the above table by total purchase value in descending order 
# * Optional: give the displayed data cleaner formatting 
# * Display a preview of the data frame



#Purchase Count
purchase_count = pymoli_df.groupby(by=['Item ID', 'Item Name'])['Item ID'].count()

#Total Purchase Value
total_purchase_value = pymoli_df.groupby(by=['Item ID', 'Item Name'])['Price'].sum()

#Item Price
item_price = total_purchase_value/purchase_count


#DataFrame
top_spenders_df = pd.DataFrame({"Purchase Count": purchase_count, 
                             "Item Price": item_price,
                             "Total Purchase Value": total_purchase_value})


#Reorganize the dataframe in decreasing order of Purchase Count
sorted_df = top_spenders_df.sort_values(["Total Purchase Value"], ascending=False)


#Formating
sorted_df["Item Price"] = sorted_df["Item Price"].map("${:,.2f}".format)
sorted_df["Total Purchase Value"] = sorted_df["Total Purchase Value"].map("${:,.2f}".format)


sorted_df.head(5)

