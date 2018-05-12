

```python
# Dependencies
import pandas as pd
```


```python
# Save path to data set in a variable
json_path = "purchase_data.json"
heroes_pymoli_df = pd.read_json(json_path)
heroes_pymoli_df.head(10)
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Players Count
players_count = len(heroes_pymoli_df["SN"].unique())
total_number_of_players = pd.DataFrame({"Total Players" : [players_count]})
total_number_of_players
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
#Purchasing Analysis (Total)
#Number of Unique Items
unique_items = len(heroes_pymoli_df["Item ID"].unique())

#Average Purchase Price
average_purchase_price = heroes_pymoli_df["Price"].mean()

#Total Number of Purchases
total_number_of_purchases = heroes_pymoli_df["Price"].count()

#Total Revenue
total_revenue = heroes_pymoli_df["Price"].sum()

#Create new dataframe using all calculations
purchasing_analysis = pd.DataFrame ({"Number of Unique Items" : [unique_items],
                                    "Average Price" : [average_purchase_price],
                                    "Number of Purchases" : [total_number_of_purchases],
                                    "Total Revenue": [total_revenue]})

# Use Map to format all the columns
purchasing_analysis["Average Price"] = purchasing_analysis["Average Price"].map("${:,.2f}".format)
purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].map("${:,.2f}".format)

#reorder columns
purchasing_summary = purchasing_analysis[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
purchasing_summary
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
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics

#Remove duplicate players
SN_df = heroes_pymoli_df[["SN","Gender","Age"]]
SN_gender_df = SN_df.drop_duplicates (["SN"])

#Percentage and Count of Male Players, Female Players,and Other / Non-Disclosed
gender_count = SN_gender_df["Gender"].value_counts()
percentage_of_gender = gender_count/players_count
percentage_of_gender

#Create new dataframe using both calculations
gender_demographic_df = pd.DataFrame({"Total Count":gender_count,
                                     "Percentage of Players":percentage_of_gender})
# Use Map to format all the columns
gender_demographic_df["Percentage of Players"] = gender_demographic_df["Percentage of Players"].map("{:,.2%}".format)

gender_demographic_df
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
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
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
#Purchasing Analysis (Gender)
#The below each broken by gender
#grouping by Gender
grouped_gender_df = heroes_pymoli_df.groupby (['Gender'])
grouped_gender_df.count().head()

#Purchase Count
purchase_count = grouped_gender_df["SN"].count()

#Average Purchase Price
average_purchase_price = grouped_gender_df["Price"].mean()

#Total Purchase Value
total_purchase_price = grouped_gender_df["Price"].sum()

#Normalized Totals
normalized_totals = total_purchase_price / gender_count

#Create new dataframe using both calculations
purchasing_analysis_gender = pd.DataFrame({"Purchase Count":purchase_count,
                                           "Average Purchase Price":average_purchase_price,
                                           "Total Purchase Value":total_purchase_price,
                                           "Normalized Totals":normalized_totals})

purchasing_analysis_gender_summary = purchasing_analysis_gender[["Purchase Count",
                                                                 "Average Purchase Price",
                                                                 "Total Purchase Value",
                                                                 "Normalized Totals"]]

# Use Map to format all the columns
purchasing_analysis_gender_summary["Average Purchase Price"] = purchasing_analysis_gender_summary["Average Purchase Price"].map("${:,.2f}".format)
purchasing_analysis_gender_summary["Total Purchase Value"] = purchasing_analysis_gender_summary["Total Purchase Value"].map("${:,.2f}".format)
purchasing_analysis_gender_summary["Normalized Totals"] = purchasing_analysis_gender_summary["Normalized Totals"].map("${:,.2f}".format)


purchasing_analysis_gender_summary
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
      <th>Normalized Totals</th>
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
#Purchasing Analysis - Age Demographics
# Create the bins
bins = [0,9,14,19,24,29,34,39,100]
age_group = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
SN_gender_df["Demographics"] = pd.cut(SN_gender_df["Age"],bins,labels=age_group)

#Percentage and Count of Demographics
age_group_count = SN_gender_df ["Demographics"].value_counts()
percentage_of_age_group = age_group_count/players_count

#Create new dataframe using both calculations
age_demographic_df = pd.DataFrame({"Total Count":age_group_count,
                                   "Percentage of Players":percentage_of_age_group})

# Use Map to format all the columns
age_demographic_df["Percentage of Players"] = age_demographic_df["Percentage of Players"].map("{:,.2%}".format)
age_demographic_df.sort_index(inplace=True)
age_demographic_df
```

    C:\Users\Mieae\Anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """
    




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
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.32%</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.01%</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.20%</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.18%</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.20%</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.71%</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1.92%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
#The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.) 
bins = [0,9,14,19,24,29,34,39,100]
age_group = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
heroes_pymoli_df["Demographics"] = pd.cut(heroes_pymoli_df["Age"],bins,labels=age_group)

#grouping by Demographic
grouped_demographic_df = heroes_pymoli_df.groupby (['Demographics'])

#Purchase Count
age_purchase_count = grouped_demographic_df["SN"].count()

#Average Purchase Price
age_average_purchase_price = grouped_demographic_df["Price"].mean()

#Total Purchase Value
age_total_purchase_price = grouped_demographic_df["Price"].sum()

#Normalized Totals
age_normalized_totals = age_total_purchase_price / age_group_count

#Create new dataframe using both calculations
purchasing_analysis_demographic_summary = pd.DataFrame({"Purchase Count":age_purchase_count,
                                                "Average Purchase Price":age_average_purchase_price,
                                                "Total Purchase Value":age_total_purchase_price,
                                                "Normalized Totals":age_normalized_totals})

purchasing_analysis_demographic_summary = purchasing_analysis_demographic[["Purchase Count",
                                                                           "Average Purchase Price",
                                                                           "Total Purchase Value",
                                                                           "Normalized Totals"]]
# Use Map to format all the columns
purchasing_analysis_demographic_summary["Average Purchase Price"] = purchasing_analysis_demographic_summary["Average Purchase Price"].map("${:,.2f}".format)
purchasing_analysis_demographic_summary["Total Purchase Value"] = purchasing_analysis_demographic_summary["Total Purchase Value"].map("${:,.2f}".format)
purchasing_analysis_demographic_summary["Normalized Totals"] = purchasing_analysis_demographic_summary["Normalized Totals"].map("${:,.2f}".format)

purchasing_analysis_demographic_summary
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
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$4.22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$3.78</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$4.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$4.20</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$4.42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$4.89</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$4.39</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spender
#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
grouped_SN_df = heroes_pymoli_df.groupby (['SN'])

#Total Purchase Value
SN_total_purchase_price= grouped_SN_df["Price"].sum()
#Purchase Count
SN_purchase_count = grouped_SN_df["SN"].count()
#Average Purchase Price
SN_average_purchase = grouped_SN_df["Price"].mean()

#Create new dataframe 
SN_table = pd.DataFrame({"Total Purchase Value":SN_total_purchase_price,
                           "Average Purchase Price":SN_average_purchase,
                           "Purchase Count":SN_purchase_count})

SN_summary = pd.DataFrame(SN_table.nlargest(5,'Total Purchase Value'))
SN_top5 = SN_summary[["Purchase Count","Average Purchase Price","Total Purchase Value"]]

# Use Map to format all the columns
SN_top5 ["Average Purchase Price"] = SN_top5 ["Average Purchase Price"].map("${:,.2f}".format)
SN_top5 ["Total Purchase Value"] = SN_top5 ["Total Purchase Value"].map("${:,.2f}".format)

SN_top5 
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
#Most Popular Items
#Identify the 5 most popular items by purchase count, then list (in a table):
#Item ID, Item Name, Item Price
grouped_item_df = heroes_pymoli_df.groupby (['Item ID','Item Name'])
#['Price']

item_price = grouped_item_df["Price"].sum()/grouped_item_df["Item ID"].count()
#Purchase Count
item_purchase_count = grouped_item_df["Item ID"].count()
#Total Purchase Value
price_sum = grouped_item_df["Price"].sum()

item_table=pd.DataFrame({"Purchase Count":item_purchase_count,
                        "Total Purchase Value":price_sum,
                        "Item Price":item_price})

item_summary=pd.DataFrame(item_table.nlargest(5,'Purchase Count'))
item_top5 = item_summary[["Purchase Count","Item Price","Total Purchase Value"]]

# Use Map to format all the columns
item_top5 ["Total Purchase Value"] = item_top5 ["Total Purchase Value"].map("${:,.2f}".format)
item_top5 ["Item Price"] = item_top5 ["Item Price"].map("${:,.2f}".format)

item_top5
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Identify the 5 most profitable items by total purchase value, then list (in a table):
#Item ID
#Item Name
#Purchase Count
#Item Price
#Total Purchase Value

profitable_summary=pd.DataFrame(item_table.nlargest(5,'Total Purchase Value'))
profitable_top5 = profitable_summary[["Purchase Count","Item Price","Total Purchase Value"]]

# Use Map to format all the columns
profitable_top5 ["Total Purchase Value"] = profitable_top5 ["Total Purchase Value"].map("${:,.2f}".format)
profitable_top5 ["Item Price"] = profitable_top5 ["Item Price"].map("${:,.2f}".format)

profitable_top5
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


