

```python
import pandas as pd
import numpy as np
```


```python
DataFileName = 'purchase_data.json'
```


```python
Item_Purchase_Data_df = pd.read_json(DataFileName)
```


```python
Item_Purchase_Data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
  </tbody>
</table>
</div>




```python
NumofPlayers = Item_Purchase_Data_df['SN'].drop_duplicates().count()
NumofPlayers_df = pd.DataFrame({'Number of Players': NumofPlayers}, index =[0])
NumofPlayers_df
#NumofPlayers_df = pd.DataFrame(data=NumofPlayers_dict)

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Players</th>
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
#Number of Unique Items
Unique_Items_df = Item_Purchase_Data_df.groupby('Item Name').nunique()
Unique_Items_Num = len(Unique_Items_df['Item Name'])

#Average Purchase Price
Avg_PurchPrice = Item_Purchase_Data_df['Price'].mean()

#Total Number of Purchases
Total_Purch = len(Item_Purchase_Data_df.index)

#Total Revenue
Total_Revenue = Item_Purchase_Data_df['Price'].sum()


#Print results to a dataframe
Purch_Analysis_df = pd.DataFrame({'Average Price': Avg_PurchPrice,'Number of Unique Items': Unique_Items_Num
                                 ,'Total Revenue':Total_Revenue,'Number of Purchases':Total_Purch},index =[0])

Purch_Analysis_df['Average Price'] = Purch_Analysis_df['Average Price'].map("${0:,.2f}".format)
Purch_Analysis_df['Total Revenue'] = Purch_Analysis_df['Total Revenue'].map("${0:,.2f}".format)
Purch_Analysis_df = Purch_Analysis_df[['Number of Unique Items','Average Price','Number of Purchases','Total Revenue']]
Purch_Analysis_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Make a DF of unique SNs
Unique_SN_df = Item_Purchase_Data_df.drop_duplicates('SN', keep = 'last')

#Make a DF of just males
Males_df = Unique_SN_df[Unique_SN_df['Gender'] == 'Male']

#Make a DF of just Females
Females_df = Unique_SN_df[Unique_SN_df['Gender'] == 'Female']

#Make a DF of just people who identify as Other
Other_df = Unique_SN_df[Unique_SN_df['Gender'] == 'Other / Non-Disclosed']

#Find the number of each gender
Num_Males = len(Males_df.index)
Num_Females = len(Females_df.index)
Num_Other = len(Other_df.index)

#Find the Percentage of each gender
Males_Perc = Num_Males/NumofPlayers*100
Females_Perc = Num_Females/NumofPlayers*100
Other_Perc = Num_Other/NumofPlayers*100

#Create a DF of the results
Gender_Analysis_df = pd.DataFrame({'Percentage of Players': [Males_Perc,Females_Perc,Other_Perc],'Total Count':[Num_Males,Num_Females,Num_Other]},index =['Male','Female','Other'])

#Formatting
Gender_Analysis_df['Percentage of Players'] = Gender_Analysis_df['Percentage of Players'].map("{0:,.2f}%".format)

Gender_Analysis_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Other</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Counts 

#Male Purchase Counts and average
Male_Purchases_df =Item_Purchase_Data_df[Item_Purchase_Data_df['Gender'] == 'Male']
Male_Num = len(Male_Purchases_df.index)
Percent_Male = Male_Num/NumofPlayers
Avg_Male_Purch = Male_Purchases_df['Price'].mean()
#Sum_Male_Purch = Male_Purchases_df['Price'].sum()
Male_Unique_df = Unique_SN_df[Unique_SN_df['Gender']=='Male']
Male_Unique_Num = len(Male_Unique_df.index)
Sum_Male_Purch = Male_Unique_df['Price'].sum()
Norm_Male_Purch = Sum_Male_Purch/Male_Unique_Num

#Female Purchase Counts and average
Female_Purchases_df =Item_Purchase_Data_df[Item_Purchase_Data_df['Gender'] == 'Female']
Female_Num = len(Female_Purchases_df.index)
Percent_Female = Female_Num/NumofPlayers
Avg_Female_Purch = Female_Purchases_df['Price'].mean()
#Sum_Female_Purch = Female_Purchases_df['Price'].sum()
Female_Unique_df = Unique_SN_df[Unique_SN_df['Gender']=='Female']
Female_Unique_Num = len(Female_Unique_df.index)
Sum_Female_Purch = Female_Unique_df['Price'].sum()
Norm_Female_Purch = Sum_Female_Purch/Female_Unique_Num

#Other Purchase Counts and average
Other_Purchases_df = Item_Purchase_Data_df[Item_Purchase_Data_df['Gender'] == 'Other / Non-Disclosed']
Other_Num = len(Other_Purchases_df.index)
Percent_Other = Other_Num/NumofPlayers
Avg_Other_Purch = Other_Purchases_df['Price'].mean()
#Sum_Other_Purch = Other_Purchases_df['Price'].sum()
Other_Unique_df = Unique_SN_df[Unique_SN_df['Gender']=='Other / Non-Disclosed']
Other_Unique_Num = len(Other_Unique_df.index)
Sum_Other_Purch = Other_Unique_df['Price'].sum()
Norm_Other_Purch = Sum_Other_Purch/Other_Unique_Num

Gender_Purch_df = pd.DataFrame({'Purchase Count':[Female_Num,Male_Num,Other_Num],'Average Purchase Price':[Avg_Female_Purch, Avg_Male_Purch,Avg_Other_Purch],'Total Purchase Value':[Sum_Female_Purch,Sum_Male_Purch,Sum_Other_Purch],'Normalized Totals':[Norm_Female_Purch,Norm_Male_Purch,Norm_Other_Purch]}, index = ['Female','Male','Other'])

Gender_Purch_df['Average Purchase Price'] = Gender_Purch_df['Average Purchase Price'].map("${0:,.2f}".format)
Gender_Purch_df['Normalized Totals'] = Gender_Purch_df['Normalized Totals'].map("${0:,.2f}".format)
Gender_Purch_df['Total Purchase Value'] = Gender_Purch_df['Total Purchase Value'].map("${0:,.2f}".format)

Gender_Purch_df = Gender_Purch_df[['Purchase Count','Average Purchase Price','Total Purchase Value','Normalized Totals']]
Gender_Purch_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$273.36</td>
      <td>$2.73</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,351.73</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$25.83</td>
      <td>$3.23</td>
    </tr>
  </tbody>
</table>
</div>




```python
Age_Max = Item_Purchase_Data_df['Age'].max()
Age_Bin = [0,10,14,19,24,29,34,39,Age_Max]
Bin_Title = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']

Item_Purchase_Data_df['Age Range'] = pd.cut(Item_Purchase_Data_df['Age'], Age_Bin,labels=Bin_Title)

#Group by age range to get basid df
Age_df = Item_Purchase_Data_df.groupby("Age Range")
#This gets the age range count
Age_Count_df = pd.DataFrame(Age_df['Age Range'].count())
#We now need to add a column to the df that has the percentage of the total for each age range
Age_Count_df['Percentage of Players'] = Age_Count_df['Age Range']/NumofPlayers*100

#Format the Percentage of Players column to two decimals and a percentage
Age_Count_df['Percentage of Players'] = Age_Count_df['Percentage of Players'].map("{0:,.2f}%".format)

Age_Count_df = Age_Count_df[['Percentage of Players','Age Range']]
Age_Count_df = Age_Count_df.rename(columns={"Age Range":"Total Count"})
Age_Count_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5.58%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>5.41%</td>
      <td>31</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.21%</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>58.64%</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>21.82%</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>11.17%</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>7.33%</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.97%</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Analysis By Age
Purch_Count_df = pd.DataFrame(Age_df['Item ID'].count())
Purch_AvgPrice_df = pd.DataFrame(Age_df['Price'].mean())
Purch_TotalPrice_df = pd.DataFrame(Age_df['Price'].sum())

#Unique Bins
Unique_Age_Max = Unique_SN_df['Age'].max()

Unique_SN_df['Age Range'] = pd.cut(Unique_SN_df['Age'], Age_Bin,labels=Bin_Title)
Unique_Age_df = Unique_SN_df.groupby('Age Range')
Unique_Age_Count_df = pd.DataFrame(Unique_Age_df['Age Range'].count())
Norm_Purch_total_df = Purch_TotalPrice_df/Unique_Age_Count_df.values[0,:]

Purch_Count_df['Average Purchase Price'] = Purch_AvgPrice_df
Purch_Count_df['Total Purchase Value'] = Purch_TotalPrice_df
Purch_Count_df['Normalized Totals'] = Norm_Purch_total_df

Purch_Count_df = Purch_Count_df.rename(columns={'Item ID':'Purchase Count'})
Purch_Count_df['Average Purchase Price'] = Purch_Count_df['Average Purchase Price'].map("${0:,.2f}".format)
Purch_Count_df['Total Purchase Value'] = Purch_Count_df['Total Purchase Value'].map("${0:,.2f}".format)
Purch_Count_df['Normalized Totals'] = Purch_Count_df['Normalized Totals'].map("${0:,.2f}".format)
Purch_Count_df
```

    C:\Users\micha\Anaconda3\lib\site-packages\ipykernel_launcher.py:9: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$4.39</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$17.56</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$44.49</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$16.83</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$8.97</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$5.43</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$2.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spender

#Calculate the Top buyer, Total Spent, and Average Spent
Top_Buys_df = pd.DataFrame(Item_Purchase_Data_df.groupby('SN')['Price'].count())
Total_Price_df = pd.DataFrame(Item_Purchase_Data_df.groupby('SN')['Price'].sum())
Avg_Buys_df = pd.DataFrame(Item_Purchase_Data_df.groupby('SN')['Price'].mean())

#Reset the index to merge the df
Top_Buys_df = Top_Buys_df.reset_index()
Total_Price_df = Total_Price_df.reset_index()
Avg_Buys_df = Avg_Buys_df.reset_index()

#Merge the Top Buys df with the Average Price df
Top_Spender_df = pd.merge(Top_Buys_df,Avg_Buys_df, how='outer', on='SN')
Top_Spender_df= Top_Spender_df.rename(columns={'Price_x':'Purchase Count','Price_y':'Average Purchase Price'})

#Merge the previous df with the total price df
Top_Spender_df = pd.merge(Top_Spender_df,Total_Price_df, how='outer', on='SN')
Top_Spender_df= Top_Spender_df.rename(columns={'Price':'Total Purchase Value'})

Top_Spender_df = Top_Spender_df.sort_values('Total Purchase Value', ascending = False)
Top_Spender_df= Top_Spender_df.set_index('SN')
Top_Spender_df['Average Purchase Price'] = Top_Spender_df['Average Purchase Price'].map("${0:,.2f}".format)
Top_Spender_df['Total Purchase Value'] = Top_Spender_df['Total Purchase Value'].map("${0:,.2f}".format)
Top_Spender_df.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
Item_ID_df = Item_Purchase_Data_df[['Item ID','Item Name','Price']]
Item_Sum_df = pd.DataFrame(Item_Purchase_Data_df.groupby('Item ID')['Price'].sum())
Item_Count_df = pd.DataFrame(Item_Purchase_Data_df.groupby('Item ID')['Item Name'].count())

#Prepare dfs to be merged
Item_ID_df = pd.DataFrame(Item_ID_df.drop_duplicates())
Item_Sum_df = Item_Sum_df.reset_index()
Item_Count_df = Item_Count_df.reset_index()

#Merge each independed df
Pop_Item_df = pd.merge(Item_ID_df, Item_Count_df, how='inner',on = 'Item ID')
Pop_Item_df = Pop_Item_df.rename(columns={'Item Name_x':'Item Name','Item Name_y':'Purchase Count'})
Pop_Item_df = pd.merge(Pop_Item_df,Item_Sum_df,how='inner',on='Item ID')
Pop_Item_df = Pop_Item_df.rename(columns={'Price_x':'Item Price','Price_y':'Total Purchase Value'})
Pop_Item_df = Pop_Item_df.set_index('Item ID')

#Format df 
Most_Pop_Item_df = Pop_Item_df[['Item Name','Purchase Count','Item Price','Total Purchase Value']]
Most_Pop_Item_df = Most_Pop_Item_df.sort_values('Purchase Count',ascending=False)
Most_Pop_Item_df['Item Price'] = Most_Pop_Item_df['Item Price'].map("${0:,.2f}".format)
Most_Pop_Item_df['Total Purchase Value'] = Most_Pop_Item_df['Total Purchase Value'].map("${0:,.2f}".format)

Most_Pop_Item_df.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <th>39</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
  </tbody>
</table>
</div>




```python
Most_Profitable_Item_df = Pop_Item_df[['Item Name','Purchase Count','Item Price','Total Purchase Value']]
Most_Profitable_Item_df = Most_Profitable_Item_df.sort_values('Total Purchase Value',ascending=False)

Most_Profitable_Item_df['Item Price'] = Most_Profitable_Item_df['Item Price'].map("${0:,.2f}".format)
Most_Profitable_Item_df['Total Purchase Value'] = Most_Profitable_Item_df['Total Purchase Value'].map("${0:,.2f}".format)
Most_Profitable_Item_df.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>Retribution Axe</td>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Orenmir</td>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Singed Scalpel</td>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


