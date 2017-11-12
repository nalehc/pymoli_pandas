

```python
#Trend 1: Players age 20-24 spend the most Pymoli Cents. They don't buy significantly more expensive items, but rather
#the quantity of items purchases by this group is much greater than other age categories. Likewise, men spend more. Men
#also make up a large percentage of the player base, so this is hardly surprising. Again, the amount per item spent by 
#is similar to the amount per item spent by women. 

#Trend 2: Top spending players indicate more revenue can be captured outside the 20-24 male demographic. Though 2 out 
#of 5 of the top spending players fall into this demographic, 3 do not. Mindimnya67 is female player, age 39 who has 
#has spent 12.74 Pymoli Cents. Sondim43 and Saedue76 are both male but they are 20 and 25 respectively. Together, they 
#have spent 26.58 Pymoli Cents.

#Trend 3: Cheaper items sell better, increasing purchase of Pymoli Cents. Though some of our higher priced items sell 
#well, most of the profitable items are low-to medium priced, and in demand among the player base. The highest priced
#best seller is the Spectral Diamond Doomblade, at a mere 4 and a quarter Pymoli cent cost. 
```


```python
# Initial Setup. Note: I joined both datasets into a single one, but from what I can tell the
#script should work on each individually as well.
import pandas as pd
import numpy as np
heroesPymoli_df1 = pd.read_json("purchase_data.json")
heroesPymoli_df2 = pd.read_json("purchase_data2.json")
frames = [heroesPymoli_df1,heroesPymoli_df2]
heroesPymoli_df = pd.concat(frames)
heroesPymoli_df = heroesPymoli_df.rename(columns = {"Price":"Price in Pymoli Cents"})

#Retrieve number of players
screen_names = heroesPymoli_df["SN"].unique()
screen_names.size
heroesPymoli_df.head()
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
      <th>Price in Pymoli Cents</th>
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
#Purchasing Analysis, I jokingly created an in-game currency as the value (I assume players can buy with
#real currency in the country they live in)
game_items = heroesPymoli_df["Item Name"].unique()
game_items.size
heroesPymoli_df = heroesPymoli_df[["SN", "Age", "Gender", "Item ID", "Item Name", "Price in Pymoli Cents"]]
average_purchase = (heroesPymoli_df["Price in Pymoli Cents"].sum())/(screen_names.size)
average_purchase
purchases = len(heroesPymoli_df.index)
purchases
total_revenue = heroesPymoli_df["Price in Pymoli Cents"].sum()
total_revenue

d = {"Unique Game Items": (game_items.size), 
     "Average Pymoli Cents Spent" : (average_purchase), 
     "Number Purchases" : (purchases),
    "Total Revenue" : (total_revenue)}
purchasing_df = pd.DataFrame(data = d, index=[0])
purchasing_df
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
      <th>Average Pymoli Cents Spent</th>
      <th>Number Purchases</th>
      <th>Total Revenue</th>
      <th>Unique Game Items</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.108546</td>
      <td>858</td>
      <td>2514.43</td>
      <td>180</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
heroesPymoli_players = heroesPymoli_df.drop_duplicates(["SN"])
females = heroesPymoli_players.loc[heroesPymoli_players.Gender == "Female", "Gender"].count()
males = heroesPymoli_players.loc[heroesPymoli_players.Gender == "Male", "Gender"].count()
other = screen_names.size - (females + males)
genders = ["Female", "Male", "Other/Not Disclosed"]
gender_data = {"Total Count" : [females, males, other]}
gender_demo = pd.DataFrame(data = gender_data, index = genders)
percent_players = lambda x: (x/screen_names.size)*100
gender_demo["Percentage of Players"] = (gender_demo["Total Count"].map(percent_players)).round(2)
gender_demo                                     
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>108</td>
      <td>17.65</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>495</td>
      <td>80.88</td>
    </tr>
    <tr>
      <th>Other/Not Disclosed</th>
      <td>9</td>
      <td>1.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Payment Analysis
#First few columns...
gender_group = heroesPymoli_df.groupby( ["Gender"] )
gender_purchases = gender_group.size().to_frame(name = "Purchases").reset_index()
total_purchase = gender_group.aggregate(np.sum)
total_purchase = total_purchase.set_index(gender_purchases.index)
gender_purchases["Total Purchase Value"] = (total_purchase["Price in Pymoli Cents"])
gender_demo = gender_demo.set_index(gender_purchases.index)
gender_purchase_avg = (total_purchase["Price in Pymoli Cents"].divide(gender_demo["Total Count"]))
gender_purchases["Average Purchase Value"] = gender_purchase_avg


#Additional Calculation for "Normalized" values. 
#Used this method: https://en.wikipedia.org/wiki/Feature_scaling#Rescaling
min_price = heroesPymoli_df["Price in Pymoli Cents"].min()
max_price = heroesPymoli_df["Price in Pymoli Cents"].max()
normal_prices = (heroesPymoli_df["Price in Pymoli Cents"]).map(lambda x: (x - min_price) / (max_price - min_price))
heroesPymoli_df["Normalized Prices"] = normal_prices
gender_group_norm = heroesPymoli_df.groupby( ["Gender"] )
total_purchase_norm = gender_group_norm.aggregate(np.sum)
total_purchase_norm = total_purchase_norm.set_index(gender_purchases.index)
gender_purchases["Normalized Total"] = total_purchase_norm["Normalized Prices"]
gender_purchases
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
      <th>Gender</th>
      <th>Purchases</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>149</td>
      <td>424.29</td>
      <td>3.928611</td>
      <td>69.290076</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>697</td>
      <td>2052.28</td>
      <td>4.146020</td>
      <td>341.307888</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>12</td>
      <td>37.86</td>
      <td>4.206667</td>
      <td>6.519084</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
bins = [0, 10, 15, 20, 25, 30, 35, 40,
        (heroesPymoli_df["Age"].max())]
age_labels = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
heroesPymoli_df["Age Group"] = pd.cut(heroesPymoli_df["Age"], bins, labels=age_labels)
age_group = heroesPymoli_df.groupby("Age Group")

age_purchases = age_group.size().to_frame(name = "Purchases").reset_index()
total_purchase = age_group.aggregate(np.sum)
total_purchase = total_purchase.set_index(age_purchases.index)
age_purchases["Total Purchase Value"] = (total_purchase["Price in Pymoli Cents"])
age_purchases["Average Purchase Value"] = (age_purchases["Total Purchase Value"].divide(age_purchases["Purchases"]))

age_group_norm = heroesPymoli_df.groupby( ["Age"] )
total_purchase_norm = age_group_norm.aggregate(np.sum)
age_purchases["Normalized Total"] = (total_purchase["Normalized Prices"])
age_purchases

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
      <th>Age Group</th>
      <th>Purchases</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Value</th>
      <th>Normalized Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;10</td>
      <td>37</td>
      <td>110.44</td>
      <td>2.984865</td>
      <td>18.498728</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>82</td>
      <td>236.36</td>
      <td>2.882439</td>
      <td>38.860051</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>204</td>
      <td>583.43</td>
      <td>2.859951</td>
      <td>95.508906</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>338</td>
      <td>1003.03</td>
      <td>2.967544</td>
      <td>167.498728</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>80</td>
      <td>230.59</td>
      <td>2.882375</td>
      <td>37.910941</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>65</td>
      <td>194.73</td>
      <td>2.995846</td>
      <td>32.679389</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>49</td>
      <td>147.21</td>
      <td>3.004286</td>
      <td>24.740458</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40+</td>
      <td>3</td>
      <td>8.64</td>
      <td>2.880000</td>
      <td>1.419847</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spending Players
player_group = heroesPymoli_df.groupby(["SN"])
player_purchases = player_group.size().to_frame(name = "Purchases").reset_index()
total_purchase = player_group.aggregate(np.sum)
total_purchase = total_purchase.set_index(player_purchases.index)
player_purchases["Total Purchase Value"] = (total_purchase["Price in Pymoli Cents"])
player_purchases = player_purchases.sort_values("Total Purchase Value", ascending = False)
player_purchases = player_purchases.reset_index(drop=True)
player_purchases["Average Purchase Value"] = (player_purchases["Total Purchase Value"].divide(player_purchases["Purchases"]))
player_purchases.head()
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
      <th>SN</th>
      <th>Purchases</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>17.06</td>
      <td>3.412</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aerithllora36</td>
      <td>4</td>
      <td>15.10</td>
      <td>3.775</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>13.56</td>
      <td>3.390</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sondim43</td>
      <td>4</td>
      <td>13.02</td>
      <td>3.255</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Mindimnya67</td>
      <td>4</td>
      <td>12.74</td>
      <td>3.185</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Popular Items
id_lookup = heroesPymoli_df.set_index("Item Name").to_dict()["Item ID"]
item_group = heroesPymoli_df.groupby(["Item Name"])
item_purchases = item_group.size().to_frame(name = "Purchases").reset_index()
total_purchase = item_group.aggregate(np.sum)
total_purchase = total_purchase.set_index(item_purchases.index)
item_purchases["Total Purchase Value"] = (total_purchase["Price in Pymoli Cents"])
item_purchases = item_purchases.sort_values("Purchases", ascending = False)
item_purchases["Item ID"] = item_purchases["Item Name"].map(id_lookup)
item_purchases.head()
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
      <th>Purchases</th>
      <th>Total Purchase Value</th>
      <th>Item ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>Final Critic</td>
      <td>14</td>
      <td>38.60</td>
      <td>101</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Arcane Gem</td>
      <td>12</td>
      <td>29.34</td>
      <td>84</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Stormcaller</td>
      <td>12</td>
      <td>40.19</td>
      <td>180</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>25.85</td>
      <td>39</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Trickster</td>
      <td>10</td>
      <td>23.22</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Profitable Items
item_purchases.sort_values("Total Purchase Value", ascending = False).head()
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
      <th>Purchases</th>
      <th>Total Purchase Value</th>
      <th>Item ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>138</th>
      <td>Stormcaller</td>
      <td>12</td>
      <td>40.19</td>
      <td>180</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Final Critic</td>
      <td>14</td>
      <td>38.60</td>
      <td>101</td>
    </tr>
    <tr>
      <th>113</th>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>37.26</td>
      <td>34</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Splitter, Foe Of Subtlety</td>
      <td>9</td>
      <td>33.03</td>
      <td>107</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>29.75</td>
      <td>115</td>
    </tr>
  </tbody>
</table>
</div>


