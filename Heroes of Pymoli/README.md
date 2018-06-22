

```python
## Import dependencies
import pandas as pd

## Read File
filename = "Resources/purchase_data.json"
dataframe = pd.read_json(filename) 
```


```python
## Player Count
# Total Number of Players
unique_players = len(dataframe["SN"].unique())
pd.DataFrame({"Total Players": [unique_players]})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Purchasing Analysis (Total) 
# Number of Unique Items 
unique_items = len(dataframe["Item ID"].unique())

# Average Purchase Price 
item_group = dataframe.groupby("Item ID")["Price"]
unique_item_avg = item_group.mean().mean()

# Total Number of Purchases
total_purchases = len(dataframe["Price"])

# Total Revenue 
total_price = dataframe["Price"].sum()


# CREATE DATA FRAME 
purchase_analysis_df = pd.DataFrame({"Number of Unique Items": [unique_items], "Average Price": [unique_item_avg], 
              "Total Number of Purchases": [total_purchases], "Total Revenue":[total_price]})
# Formatting
purchase_analysis_df["Average Price"] = purchase_analysis_df["Average Price"].map("${:.2f}".format)
purchase_analysis_df["Total Revenue"] = purchase_analysis_df["Total Revenue"].map("${:.2f}".format)
purchase_analysis_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.95</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Gender Demographics and Purchasing Analysis by Gender 
# The unique genders listed... in case they want to change the gender listing to include more genders
genders = dataframe["Gender"].unique()

# Create groupby object grouping by gender
gendergroup = dataframe.groupby("Gender")

# Unique SN under each gender (Total calculate number of players per gender)
gender_unique = gendergroup["SN"].unique()
# Count SN under each gender (Total purchases per gender)
gender_count = gendergroup["SN"].count()
# The average price per gender
gender_mean = gendergroup["Price"].mean()
# Total price per gender
gender_total = gendergroup["Price"].sum()

# Creating dictionaries
gender_uniquer = {}
gender_percent = {}
gender_counter = {}
gender_meaner = {}
gender_totaler = {}

gender_norm = {}

# For all the genders... 
for x in genders:
    # Calculating number of players and percentage 
    gender_uniquer[x] = len(gender_unique[x])
    gender_percent[x] = (gender_uniquer[x]/unique_players)
    # Number of purchases, average price, and total price
    gender_counter[x] = gender_count[x]
    gender_meaner[x] = gender_mean[x]
    gender_totaler[x] = gender_total[x]
```


```python
# Creating Dataframe for "Gender Demographics"
gender_df = pd.DataFrame({"Percentage of Players": gender_percent, "Total Count": gender_uniquer})

# Format
gender_df["Percentage of Players"] = gender_df["Percentage of Players"].map("{:.2%}".format)
# Label index name
gender_df.index.name = "Gender"

gender_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Normalizing totals
for x in genders: 
    gender_norm[x] = (gender_total[x] / gender_df["Total Count"][x])

# Creating Dataframe for "Purchasing Analysis (Gender)"
gender2_df = pd.DataFrame({"Purchase Count": gender_counter, "Average Purchase Price": gender_meaner,
                          "Total Purchase Value": gender_totaler, "Normalized Total": gender_norm})

# Formatting
gender2_df["Average Purchase Price"] = gender2_df["Average Purchase Price"].map("${:,.2f}".format)
gender2_df["Total Purchase Value"] = gender2_df["Total Purchase Value"].map("${:,.2f}".format)
gender2_df["Normalized Total"] = gender2_df["Normalized Total"].map("${:,.2f}".format)
# Label index name
gender2_df.index.name = "Gender"

gender2_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Age Demographics ##

bins = [0]
labelname = ["<10"]

# Creating the bins
x = 10
while x <= 40:
    bins.append(x)
    x = x + 5
bins.append(200)

# Creating the labels
for x in bins[1:-2]:
    labelname.append(str(x) + "-" + str(x+4))
labelname.append("40+")
    
# Binning "Age Group"
dataframe["Age Group"] = pd.cut(dataframe["Age"], bins, labels = labelname)
```


```python
# Grouping by age
agegroup = dataframe.groupby('Age Group')

# Total number of purchases per age group
age_count = agegroup["SN"].count()
# Unique SN per Age Group to calculate total number of players
age_unique = agegroup["SN"].unique()
# Average purchase price 
age_mean = agegroup["Price"].mean()
# Total purchase price
age_total = agegroup["Price"].sum()

# Creating dictionaries
age_counter = {}
age_percent = {}
age_meaner = {}
age_totaler = {}
age_uniquer = {}

age_normalized = {}

# For each age group...
for x in labelname:
    # Number of unique players
    age_uniquer[x] = len(age_unique[x])
    # Percentage of players of specific age groups out of total paying players
    age_percent[x] = (age_uniquer[x]/unique_players)
    # The total count of purchases per age group
    age_counter[x] = age_count[x]
    # The mean of the purchase prices per age gorup
    age_meaner[x] = age_mean[x]
    # The total purchase price per age group
    age_totaler[x] = age_total[x]
    
```


```python
# Create dataframe
age_df = pd.DataFrame({"Percentage of Players": age_percent, "Purchase Count": age_counter})
# Formating
age_df["Percentage of Players"] = age_df["Percentage of Players"].map("{:.2%}".format)
# Set label for index
age_df.index.name = "Age Group"

age_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Purchase Count</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>9.42%</td>
      <td>78</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26%</td>
      <td>184</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84%</td>
      <td>305</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08%</td>
      <td>76</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68%</td>
      <td>58</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.36%</td>
      <td>44</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52%</td>
      <td>3</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>3.84%</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Normalizing totals:
for x in labelname:
    age_normalized[x]= age_total[x]/ age_uniquer[x]
    

# Creating dataframe
age2_df = pd.DataFrame({"Purchase Count": age_counter, "Average Purchase Price": age_meaner, 
                        "Total Purchase Value": age_totaler, "Normalized Total":age_normalized})

# Formatting
age2_df["Average Purchase Price"] = age2_df["Average Purchase Price"].map("${:.2f}".format)
age2_df["Total Purchase Value"] = age2_df["Total Purchase Value"].map("${:.2f}".format)
age2_df["Normalized Total"] = age2_df["Normalized Total"].map("${:.2f}".format)

# Set index
age2_df.index.name = "Age Group"

age2_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
    <tr>
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>78</td>
      <td>$2.87</td>
      <td>$224.15</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>184</td>
      <td>$2.87</td>
      <td>$528.74</td>
      <td>$3.80</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>305</td>
      <td>$2.96</td>
      <td>$902.61</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>76</td>
      <td>$2.89</td>
      <td>$219.82</td>
      <td>$4.23</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>58</td>
      <td>$3.07</td>
      <td>$178.26</td>
      <td>$4.05</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>44</td>
      <td>$2.90</td>
      <td>$127.49</td>
      <td>$5.10</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
      <td>$2.88</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Top Spenders ##
# Groupby SN
user_data = dataframe.groupby("SN")

# Find the total purchase price of all unique screennames
user_sum = user_data['Price'].sum()
# Find the purchase count of each SN
user_count = user_data["Price"].count()
# Find the purchase price average of each SN
user_avg = user_data["Price"].mean()

# Sort the total purchase price
user_sort = user_sum.sort_values(ascending = False)[0:5]

# Create a dataframe from the sorted data
new_df = pd.DataFrame(user_sort)

# Reset the original sorted data's index
user_sort = user_sort.reset_index()

user_count_ls = []
user_avg_ls = []

# For every row in the sorted data, locate the average and count value corresponding to the SN
for x in range(5):
    user_avg_ls.append(user_avg.loc[user_sort["SN"][x]])
    user_count_ls.append(user_count.loc[user_sort["SN"][x]])
    
# Adding new columns
new_df["Purchase Count"] = user_count_ls
new_df["Average Purchase Price"] = user_avg_ls
# Rename Column
new_df = new_df.rename(columns={"Price":"Total Purchase Value"})
# Rearrange columns
new_df = new_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

# Formatting
new_df["Average Purchase Price"] = new_df["Average Purchase Price"].map("${:.2f}".format)
new_df["Total Purchase Value"] = new_df["Total Purchase Value"].map("${:.2f}".format)

new_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Most Popular Items ##
# Group by Item ID
item_data = dataframe.groupby("Item ID")

# The total purchase price by Item ID
item_sum = item_data['Price'].sum()
# Total purchase count by Item ID
item_count = item_data["Price"].count()
# Average purchase count for each Item ID
item_avg = item_data["Price"].mean()

# Sort based off of purchase count
item_sort = item_count.sort_values(ascending = False)[0:5]

# Create dataframe
item_df = pd.DataFrame(item_sort)

# Reset index for original sorted series
item_sort = item_sort.reset_index()

item_count_ls = []
item_avg_ls = []
item_sum_ls = []
item_name_ls = []

# For every row in the sorted data, locate the average, count, total value and item name of the item
for x in range(5):
    item_avg_ls.append(item_avg.loc[item_sort["Item ID"][x]])
    item_count_ls.append(item_count.loc[item_sort["Item ID"][x]])
    item_sum_ls.append(item_sum.loc[item_sort["Item ID"][x]])
    item_name_ls.append(dataframe.loc[item_sort["Item ID"][x],"Item Name"])

    
# Create new columns
item_df["Item Name"] = item_name_ls
item_df["Item Price"] = item_avg_ls
item_df["Total Purchase Value"] = item_sum_ls

# Rename column
item_df = item_df.rename(columns={"Price":"Purchase Count"})
# Reorganize columns
item_df = item_df[["Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

# Formatting
item_df["Item Price"] = item_df["Item Price"].map("${:.2f}".format)
item_df["Total Purchase Value"] = item_df["Total Purchase Value"].map("${:.2f}".format)

item_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>Thorn, Satchel of Dark Souls</td>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Stormfury Mace</td>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Shadow Strike, Glory of Ending Hope</td>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Alpha, Reach of Ending Hope</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
item_sort2 = item_sum.sort_values(ascending = False)[0:5]

item_df2 = pd.DataFrame(item_sort2)
item_sort2 = item_sort2.reset_index()

item_count_ls2 = []
item_avg_ls2 = []
item_sum_ls2 = []
item_name_ls2 = []

for x in range(5):
    item_avg_ls2.append(item_avg.loc[item_sort2["Item ID"][x]])
    item_count_ls2.append(item_count.loc[item_sort2["Item ID"][x]])
    item_sum_ls2.append(item_sum.loc[item_sort2["Item ID"][x]])
    item_name_ls2.append(dataframe.loc[item_sort2["Item ID"][x],"Item Name"])
    
item_df2["Item Name"] = item_name_ls2
item_df2["Item Price"] = item_avg_ls2
item_df2["Purchase Count"] = item_count_ls2

item_df2 = item_df2.rename(columns={"Price":"Total Purchase Value"})
item_df2 = item_df2[["Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

item_df2["Item Price"] = item_df2["Item Price"].map("${:.2f}".format)
item_df2["Total Purchase Value"] = item_df2["Total Purchase Value"].map("${:.2f}".format)

item_df2

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>Alpha, Reach of Ending Hope</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Thorn, Conqueror of the Corrupted</td>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Rage, Legacy of the Lone Victor</td>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Mercy, Katana of Dismay</td>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Spectral Diamond Doomblade</td>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


