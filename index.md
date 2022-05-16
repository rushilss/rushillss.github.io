

Transfers are the main component of how professional soccer clubs build and improve their teams. A transfer is when a player is under contract for one club but moves to another. The leading way clubs transfer these players is by exchanging the player for a large sum of money that is agreed upon between the two clubs. This money comes from the club's investors, sponsors, and revenue. Because of the importance of money in soccer, fans believe that the more money spent the better the results for their team will be. Is this true? Who are the teams that are spending their money the best? Who are the teams that are spending their money the worst? What aspect of the game does money affect more? Is the difference in transfer spending ruining the competitiveness of the league? These are the questions I want to explore in my analysis.

### Data

To gather the data on the results of each team in the premier league to see how they performed each year, I found and downloaded a dataset that contains all the results and final positions of all the teams in the league for that given year. This dataset contains results for every team from the 2010-2011 season all the way to the 2020-2021 season. The code to read the CSV file and to display the table is shown below.


```python
import os
import folium
import requests
import pandas
import numpy

epl_standings = pandas.read_csv("notebooks/Final/EPL_Standings.csv")

epl_standings
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
      <th>Season</th>
      <th>Pos</th>
      <th>Club</th>
      <th>Pld</th>
      <th>W</th>
      <th>D</th>
      <th>L</th>
      <th>GF</th>
      <th>GA</th>
      <th>GD</th>
      <th>Pts</th>
      <th>Qualification or relegation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010-11</td>
      <td>1</td>
      <td>Manchester United</td>
      <td>38</td>
      <td>23</td>
      <td>11</td>
      <td>4</td>
      <td>78</td>
      <td>37</td>
      <td>41</td>
      <td>80</td>
      <td>Qualification for the Champions League group s...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010-11</td>
      <td>2</td>
      <td>Chelsea</td>
      <td>38</td>
      <td>21</td>
      <td>8</td>
      <td>9</td>
      <td>69</td>
      <td>33</td>
      <td>36</td>
      <td>71</td>
      <td>Qualification for the Champions League group s...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010-11</td>
      <td>3</td>
      <td>Manchester City</td>
      <td>38</td>
      <td>21</td>
      <td>8</td>
      <td>9</td>
      <td>60</td>
      <td>33</td>
      <td>27</td>
      <td>71</td>
      <td>Qualification for the Champions League group s...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2010-11</td>
      <td>4</td>
      <td>Arsenal</td>
      <td>38</td>
      <td>19</td>
      <td>11</td>
      <td>8</td>
      <td>72</td>
      <td>43</td>
      <td>29</td>
      <td>68</td>
      <td>Qualification for the Champions League play-of...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2010-11</td>
      <td>5</td>
      <td>Tottenham Hotspur</td>
      <td>38</td>
      <td>16</td>
      <td>14</td>
      <td>8</td>
      <td>55</td>
      <td>46</td>
      <td>9</td>
      <td>62</td>
      <td>Qualification for the Europa League play-off r...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>215</th>
      <td>2020-21</td>
      <td>16</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>38</td>
      <td>9</td>
      <td>14</td>
      <td>15</td>
      <td>40</td>
      <td>46</td>
      <td>-6</td>
      <td>41</td>
      <td>Not Applicable</td>
    </tr>
    <tr>
      <th>216</th>
      <td>2020-21</td>
      <td>17</td>
      <td>Burnley</td>
      <td>38</td>
      <td>10</td>
      <td>9</td>
      <td>19</td>
      <td>33</td>
      <td>55</td>
      <td>-22</td>
      <td>39</td>
      <td>Not Applicable</td>
    </tr>
    <tr>
      <th>217</th>
      <td>2020-21</td>
      <td>18</td>
      <td>Fulham</td>
      <td>38</td>
      <td>5</td>
      <td>13</td>
      <td>20</td>
      <td>27</td>
      <td>53</td>
      <td>-26</td>
      <td>28</td>
      <td>Relegation to the EFL Championship</td>
    </tr>
    <tr>
      <th>218</th>
      <td>2020-21</td>
      <td>19</td>
      <td>West Bromwich Albion</td>
      <td>38</td>
      <td>5</td>
      <td>11</td>
      <td>22</td>
      <td>35</td>
      <td>76</td>
      <td>-41</td>
      <td>26</td>
      <td>Relegation to the EFL Championship</td>
    </tr>
    <tr>
      <th>219</th>
      <td>2020-21</td>
      <td>20</td>
      <td>Sheffield United</td>
      <td>38</td>
      <td>7</td>
      <td>2</td>
      <td>29</td>
      <td>20</td>
      <td>63</td>
      <td>-43</td>
      <td>23</td>
      <td>Relegation to the EFL Championship</td>
    </tr>
  </tbody>
</table>
<p>220 rows × 12 columns</p>
</div>



The other data we want to analyze and manipulate is the transfer data for all the clubs in the premier league. I downloaded data from https://www.transfermarkt.us/ which is a German-based website that has all the information available to the public in their database about soccer transfers. The data I downloaded was the net spending of each club in the past ten years. Net spend is the difference between the money a team made by selling players and the money a team spent by buying players. A positive net spend means a team made a profit by spending less than they sold and a negative net spend means a team spent more than they sold. Here is the code to get the data. 


```python
transfer_table = pandas.read_csv("notebooks/Final/Premier_league_transfer.csv")

transfer_table
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
      <th>Club</th>
      <th>2021-22</th>
      <th>2020-21</th>
      <th>2019-20</th>
      <th>2018-19</th>
      <th>2017-18</th>
      <th>2016-17</th>
      <th>2015-16</th>
      <th>2014-15</th>
      <th>2013-14</th>
      <th>2012-13</th>
      <th>Total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arsenal</td>
      <td>-136.02</td>
      <td>-66.85</td>
      <td>-107.15</td>
      <td>-71.05</td>
      <td>9.15</td>
      <td>-102.69</td>
      <td>-24.00</td>
      <td>-91.18</td>
      <td>-37.10</td>
      <td>9.85</td>
      <td>-617.04</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Newcastle United</td>
      <td>-131.50</td>
      <td>-38.73</td>
      <td>-37.26</td>
      <td>-8.70</td>
      <td>-25.28</td>
      <td>36.63</td>
      <td>-102.28</td>
      <td>-21.15</td>
      <td>22.07</td>
      <td>-17.17</td>
      <td>-323.36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Manchester United</td>
      <td>-109.30</td>
      <td>-64.30</td>
      <td>-153.62</td>
      <td>-52.15</td>
      <td>-152.90</td>
      <td>-137.75</td>
      <td>-55.33</td>
      <td>-148.65</td>
      <td>-75.33</td>
      <td>-66.80</td>
      <td>-1016.13</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Crystal Palace</td>
      <td>-85.62</td>
      <td>-2.40</td>
      <td>47.78</td>
      <td>-11.50</td>
      <td>-45.95</td>
      <td>-51.00</td>
      <td>-23.40</td>
      <td>-28.35</td>
      <td>-33.00</td>
      <td>14.67</td>
      <td>-218.77</td>
    </tr>
    <tr>
      <th>4</th>
      <td>West Ham United</td>
      <td>-70.27</td>
      <td>-9.29</td>
      <td>-64.32</td>
      <td>-87.14</td>
      <td>12.22</td>
      <td>-42.50</td>
      <td>-34.19</td>
      <td>-30.75</td>
      <td>-23.47</td>
      <td>-18.85</td>
      <td>-368.54</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Aston Villa</td>
      <td>-2.82</td>
      <td>-98.58</td>
      <td>-156.50</td>
      <td>-2.95</td>
      <td>15.03</td>
      <td>-39.70</td>
      <td>-1.85</td>
      <td>-12.14</td>
      <td>-11.74</td>
      <td>-24.63</td>
      <td>-335.88</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Chelsea</td>
      <td>1.95</td>
      <td>-189.80</td>
      <td>112.27</td>
      <td>-125.55</td>
      <td>-65.90</td>
      <td>-23.90</td>
      <td>-9.01</td>
      <td>5.11</td>
      <td>-52.42</td>
      <td>-84.25</td>
      <td>-431.50</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Brighton &amp; Hove Albion</td>
      <td>4.80</td>
      <td>-7.90</td>
      <td>-59.90</td>
      <td>-73.50</td>
      <td>-66.10</td>
      <td>-8.75</td>
      <td>-13.47</td>
      <td>9.42</td>
      <td>3.20</td>
      <td>-0.67</td>
      <td>-212.88</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Everton</td>
      <td>6.50</td>
      <td>-68.95</td>
      <td>-33.20</td>
      <td>-71.15</td>
      <td>-76.82</td>
      <td>-25.20</td>
      <td>-37.90</td>
      <td>-38.26</td>
      <td>14.30</td>
      <td>-2.90</td>
      <td>-333.58</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Southampton</td>
      <td>17.27</td>
      <td>-11.00</td>
      <td>-34.20</td>
      <td>-36.15</td>
      <td>37.10</td>
      <td>16.15</td>
      <td>-7.40</td>
      <td>27.83</td>
      <td>-35.40</td>
      <td>-41.50</td>
      <td>-67.30</td>
    </tr>
  </tbody>
</table>
<p>20 rows × 12 columns</p>
</div>



## Cleaning and Manipulating the Data

Here I wanted to merge the two tables and display them, but there are some logistical things that must be taken care of first. I had to delete the 2021-2022 season as it is not finished yet and the transfer data on it was not fully up to date. Then I had to delete the club Brentford from the transfer table as the 2021-2022 season was the first season they were in the premier league, so there is no data for their results in the premier league for the last 10 seasons.  I then deleted information that I don’t believe is relevant to the analysis I will be doing. I deleted games played (GP) as it doesn’t measure performance at all. I also deleted wins, losses, and draws because points encompass all 3 of these (W= 3 points, D= 1 point, L= 0 points). I also deleted the qualification column as it is just a description and has no numerical values.

Then I took the Net Spending for each club during each season from the transfer table and added it to the standings table which will allow me to provide a comprehensive performance review for each club during each season. The code for all of this is below.


```python
ntransfer_table = transfer_table.drop(columns="2021-22") #Delete 21-22 season
ntransfer_table= ntransfer_table.drop(10) #Delete Brentford
clublist= ntransfer_table['Club'].tolist()

nepl_standings = epl_standings.drop(range(0,40)) ##Delete the 2010-11 and 2011-12 seasons

nepl_standings = nepl_standings[nepl_standings['Club'].isin(clublist)] #Take out clubs that aren't in PL

nepl_standings = nepl_standings.drop(columns="W") #Delete Information not needed for analysis
nepl_standings = nepl_standings.drop(columns="D")
nepl_standings = nepl_standings.drop(columns="L")
nepl_standings = nepl_standings.drop(columns="Qualification or relegation")
nepl_standings = nepl_standings.drop(columns="Pld")

nepl_standings.loc[nepl_standings['Club'] == "Arsenal", 'Club_num'] = 0##Create this row to check seasons will delete later
nepl_standings.loc[nepl_standings['Club'] == "Newcastle United", 'Club_num'] = 1
nepl_standings.loc[nepl_standings['Club'] == "Manchester United", 'Club_num'] = 2
nepl_standings.loc[nepl_standings['Club'] == "Crystal Palace", 'Club_num'] = 3
nepl_standings.loc[nepl_standings['Club'] == "West Ham United", 'Club_num'] = 4
nepl_standings.loc[nepl_standings['Club'] == "Leicester City", 'Club_num'] = 5
nepl_standings.loc[nepl_standings['Club'] == "Tottenham Hotspur", 'Club_num'] = 6
nepl_standings.loc[nepl_standings['Club'] == "Leeds United", 'Club_num'] = 7
nepl_standings.loc[nepl_standings['Club'] == "Liverpool", 'Club_num'] = 8
nepl_standings.loc[nepl_standings['Club'] == "Manchester City", 'Club_num'] = 9
nepl_standings.loc[nepl_standings['Club'] == "Watford", 'Club_num'] = 11
nepl_standings.loc[nepl_standings['Club'] == "Norwich City", 'Club_num'] = 12
nepl_standings.loc[nepl_standings['Club'] == "Wolverhampton Wanderers", 'Club_num'] = 13
nepl_standings.loc[nepl_standings['Club'] == "Burnley", 'Club_num'] = 14
nepl_standings.loc[nepl_standings['Club'] == "Aston Villa", 'Club_num'] = 15
nepl_standings.loc[nepl_standings['Club'] == "Chelsea", 'Club_num'] = 16
nepl_standings.loc[nepl_standings['Club'] == "Brighton & Hove Albion", 'Club_num'] = 17
nepl_standings.loc[nepl_standings['Club'] == "Everton", 'Club_num'] = 18
nepl_standings.loc[nepl_standings['Club'] == "Southampton", 'Club_num'] = 18

nepl_standings['Club_num'] = nepl_standings['Club_num'].fillna(0).astype(int)
df= pandas.DataFrame()

for i, val in nepl_standings.iterrows():  ##adding the netspend to the dataframe
    seas= nepl_standings.at[i,'Season']
    cid = (nepl_standings.at[i,'Club_num'])
    df.at[i,'Net_Spend'] = ntransfer_table.iloc[cid][seas]

result = pandas.concat([nepl_standings, df], axis=1, join='inner')
nr = result.drop(columns= 'Club_num')
nr
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
      <th>Season</th>
      <th>Pos</th>
      <th>Club</th>
      <th>GF</th>
      <th>GA</th>
      <th>GD</th>
      <th>Pts</th>
      <th>Net_Spend</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>2012-13</td>
      <td>1</td>
      <td>Manchester United</td>
      <td>86</td>
      <td>43</td>
      <td>43</td>
      <td>89</td>
      <td>-66.80</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2012-13</td>
      <td>2</td>
      <td>Manchester City</td>
      <td>66</td>
      <td>34</td>
      <td>32</td>
      <td>78</td>
      <td>-17.65</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2012-13</td>
      <td>3</td>
      <td>Chelsea</td>
      <td>75</td>
      <td>39</td>
      <td>36</td>
      <td>75</td>
      <td>-0.67</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2012-13</td>
      <td>4</td>
      <td>Arsenal</td>
      <td>72</td>
      <td>37</td>
      <td>35</td>
      <td>73</td>
      <td>9.85</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2012-13</td>
      <td>5</td>
      <td>Tottenham Hotspur</td>
      <td>66</td>
      <td>46</td>
      <td>20</td>
      <td>72</td>
      <td>-0.47</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>212</th>
      <td>2020-21</td>
      <td>13</td>
      <td>Wolverhampton Wanderers</td>
      <td>36</td>
      <td>52</td>
      <td>-16</td>
      <td>45</td>
      <td>1.20</td>
    </tr>
    <tr>
      <th>213</th>
      <td>2020-21</td>
      <td>14</td>
      <td>Crystal Palace</td>
      <td>41</td>
      <td>66</td>
      <td>-25</td>
      <td>44</td>
      <td>-2.40</td>
    </tr>
    <tr>
      <th>214</th>
      <td>2020-21</td>
      <td>15</td>
      <td>Southampton</td>
      <td>47</td>
      <td>68</td>
      <td>-21</td>
      <td>43</td>
      <td>-11.00</td>
    </tr>
    <tr>
      <th>215</th>
      <td>2020-21</td>
      <td>16</td>
      <td>Brighton &amp; Hove Albion</td>
      <td>40</td>
      <td>46</td>
      <td>-6</td>
      <td>41</td>
      <td>-68.95</td>
    </tr>
    <tr>
      <th>216</th>
      <td>2020-21</td>
      <td>17</td>
      <td>Burnley</td>
      <td>33</td>
      <td>55</td>
      <td>-22</td>
      <td>39</td>
      <td>-98.58</td>
    </tr>
  </tbody>
</table>
<p>133 rows × 8 columns</p>
</div>



Then I took the average position over the last ten years and added it to the transfer table, so you can see each club's average position over the last ten years. I also got the average pts, goals for, and goals against in the last 10 years. This gives a very sound overall idea of how a team performed in the last decade and will allow me to do in depth analysis later. 

I then took got data for the last five years, so I extracted the average position, points, goals for, goals against, and total net spend over the last five years. This is an extremely important delineation in time because in 2016 the premier league signed a new television deal which saw revenue for the league and its clubs' skyrocket as you can see here https://www.statista.com/statistics/385002/premier-league-tv-rights-revenue/. So measuring the last five years will give a more accurate reading of how clubs are spending their money in the modern-day. 


```python
N = 4
# Drop first N columns of dataframe
ntr= ntransfer_table
for i, val in ntransfer_table.iterrows():
    ntr.at[i,'Total Last 5']= (ntransfer_table.at[i, '2016-17'] + ntransfer_table.at[i, '2017-18'] + ntransfer_table.at[i, '2018-19'] + ntransfer_table.at[i, '2019-20'] + ntransfer_table.at[i, '2020-21'])
    
for i, val in ntransfer_table.iterrows():
    club= ntransfer_table.at[i, 'Club']
    temp= nr[nr['Club']== club]
    ntr.at[i, 'Avg. Pos. Last 10'] = temp['Pos'].mean() ##Average position last 10 years
    ntr.at[i, 'Avg. Pts. Last 10'] = temp['Pts'].mean()
    ntr.at[i, 'Avg. GF Last 10'] = temp['GF'].mean()
    ntr.at[i, 'Avg. GA Last 10'] = temp['GA'].mean()
    temp = temp[temp.Season != '2012-13']
    temp = temp[temp.Season != '2013-14']
    temp = temp[temp.Season != '2014-15']
    temp = temp[temp.Season != '2015-16']    
    ntr.at[i, 'Avg. Pos. Last 5'] = temp['Pos'].mean() ##Average position last 5 years
    ntr.at[i, 'Avg. Pts. Last 5'] = temp['Pts'].mean()
    ntr.at[i, 'Avg. GF Last 5'] = temp['GF'].mean()
    ntr.at[i, 'Avg. GA Last 5'] = temp['GA'].mean()
    
pandas.set_option("display.max_rows", 10, "display.max_columns", None)
        
ntr['Total abs 10']= ntr['Total']*-1
ntr['Total abs 5']= ntr['Total Last 5']*-1
ntr
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
      <th>Club</th>
      <th>2020-21</th>
      <th>2019-20</th>
      <th>2018-19</th>
      <th>2017-18</th>
      <th>2016-17</th>
      <th>2015-16</th>
      <th>2014-15</th>
      <th>2013-14</th>
      <th>2012-13</th>
      <th>Total</th>
      <th>Total Last 5</th>
      <th>Avg. Pos. Last 10</th>
      <th>Avg. Pts. Last 10</th>
      <th>Avg. GF Last 10</th>
      <th>Avg. GA Last 10</th>
      <th>Avg. Pos. Last 5</th>
      <th>Avg. Pts. Last 5</th>
      <th>Avg. GF Last 5</th>
      <th>Avg. GA Last 5</th>
      <th>Total abs 10</th>
      <th>Total abs 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arsenal</td>
      <td>-66.85</td>
      <td>-107.15</td>
      <td>-71.05</td>
      <td>9.15</td>
      <td>-102.69</td>
      <td>-24.00</td>
      <td>-91.18</td>
      <td>-37.10</td>
      <td>9.85</td>
      <td>-617.04</td>
      <td>-338.59</td>
      <td>5.000000</td>
      <td>69.222222</td>
      <td>67.888889</td>
      <td>42.555556</td>
      <td>6.40</td>
      <td>65.0</td>
      <td>67.00</td>
      <td>46.60</td>
      <td>617.04</td>
      <td>338.59</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Newcastle United</td>
      <td>-38.73</td>
      <td>-37.26</td>
      <td>-8.70</td>
      <td>-25.28</td>
      <td>36.63</td>
      <td>-102.28</td>
      <td>-21.15</td>
      <td>22.07</td>
      <td>-17.17</td>
      <td>-323.36</td>
      <td>-73.34</td>
      <td>13.375000</td>
      <td>43.000000</td>
      <td>42.125000</td>
      <td>58.750000</td>
      <td>12.00</td>
      <td>44.5</td>
      <td>41.25</td>
      <td>53.75</td>
      <td>323.36</td>
      <td>73.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Manchester United</td>
      <td>-64.30</td>
      <td>-153.62</td>
      <td>-52.15</td>
      <td>-152.90</td>
      <td>-137.75</td>
      <td>-55.33</td>
      <td>-148.65</td>
      <td>-75.33</td>
      <td>-66.80</td>
      <td>-1016.13</td>
      <td>-560.72</td>
      <td>4.000000</td>
      <td>71.666667</td>
      <td>65.222222</td>
      <td>38.777778</td>
      <td>3.80</td>
      <td>71.2</td>
      <td>65.20</td>
      <td>38.20</td>
      <td>1016.13</td>
      <td>560.72</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Crystal Palace</td>
      <td>-2.40</td>
      <td>47.78</td>
      <td>-11.50</td>
      <td>-45.95</td>
      <td>-51.00</td>
      <td>-23.40</td>
      <td>-28.35</td>
      <td>-33.00</td>
      <td>14.67</td>
      <td>-218.77</td>
      <td>-63.07</td>
      <td>12.625000</td>
      <td>44.500000</td>
      <td>42.125000</td>
      <td>54.625000</td>
      <td>13.00</td>
      <td>44.2</td>
      <td>43.60</td>
      <td>57.40</td>
      <td>218.77</td>
      <td>63.07</td>
    </tr>
    <tr>
      <th>4</th>
      <td>West Ham United</td>
      <td>-9.29</td>
      <td>-64.32</td>
      <td>-87.14</td>
      <td>12.22</td>
      <td>-42.50</td>
      <td>-34.19</td>
      <td>-30.75</td>
      <td>-23.47</td>
      <td>-18.85</td>
      <td>-368.54</td>
      <td>-191.03</td>
      <td>10.888889</td>
      <td>48.666667</td>
      <td>50.222222</td>
      <td>55.333333</td>
      <td>11.20</td>
      <td>48.6</td>
      <td>51.60</td>
      <td>59.20</td>
      <td>368.54</td>
      <td>191.03</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Aston Villa</td>
      <td>-98.58</td>
      <td>-156.50</td>
      <td>-2.95</td>
      <td>15.03</td>
      <td>-39.70</td>
      <td>-1.85</td>
      <td>-12.14</td>
      <td>-11.74</td>
      <td>-24.63</td>
      <td>-335.88</td>
      <td>-282.70</td>
      <td>15.833333</td>
      <td>37.333333</td>
      <td>40.000000</td>
      <td>62.666667</td>
      <td>14.00</td>
      <td>45.0</td>
      <td>48.00</td>
      <td>56.50</td>
      <td>335.88</td>
      <td>282.70</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Chelsea</td>
      <td>-189.80</td>
      <td>112.27</td>
      <td>-125.55</td>
      <td>-65.90</td>
      <td>-23.90</td>
      <td>-9.01</td>
      <td>5.11</td>
      <td>-52.42</td>
      <td>-84.25</td>
      <td>-431.50</td>
      <td>-292.88</td>
      <td>3.777778</td>
      <td>73.555556</td>
      <td>68.333333</td>
      <td>39.000000</td>
      <td>3.40</td>
      <td>73.6</td>
      <td>67.40</td>
      <td>40.00</td>
      <td>431.50</td>
      <td>292.88</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Brighton &amp; Hove Albion</td>
      <td>-7.90</td>
      <td>-59.90</td>
      <td>-73.50</td>
      <td>-66.10</td>
      <td>-8.75</td>
      <td>-13.47</td>
      <td>9.42</td>
      <td>3.20</td>
      <td>-0.67</td>
      <td>-212.88</td>
      <td>-216.15</td>
      <td>15.750000</td>
      <td>39.500000</td>
      <td>37.000000</td>
      <td>53.500000</td>
      <td>15.75</td>
      <td>39.5</td>
      <td>37.00</td>
      <td>53.50</td>
      <td>212.88</td>
      <td>216.15</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Everton</td>
      <td>-68.95</td>
      <td>-33.20</td>
      <td>-71.15</td>
      <td>-76.82</td>
      <td>-25.20</td>
      <td>-37.90</td>
      <td>-38.26</td>
      <td>14.30</td>
      <td>-2.90</td>
      <td>-333.58</td>
      <td>-275.32</td>
      <td>8.666667</td>
      <td>55.666667</td>
      <td>52.666667</td>
      <td>48.444444</td>
      <td>9.00</td>
      <td>54.4</td>
      <td>50.20</td>
      <td>50.40</td>
      <td>333.58</td>
      <td>275.32</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Southampton</td>
      <td>-11.00</td>
      <td>-34.20</td>
      <td>-36.15</td>
      <td>37.10</td>
      <td>16.15</td>
      <td>-7.40</td>
      <td>27.83</td>
      <td>-35.40</td>
      <td>-41.50</td>
      <td>-67.30</td>
      <td>-28.10</td>
      <td>11.333333</td>
      <td>48.444444</td>
      <td>48.555556</td>
      <td>53.000000</td>
      <td>13.40</td>
      <td>43.2</td>
      <td>44.20</td>
      <td>59.40</td>
      <td>67.30</td>
      <td>28.10</td>
    </tr>
  </tbody>
</table>
<p>19 rows × 22 columns</p>
</div>



Next, I wanted to combine the data into different statistics that can properly answer the questions I want to discover. I made four new measures and calculated them in a new table for each club in the premier league. I did this for the last 10 years and the last 5 years to get a diverse measurement. Below I will briefly explain how I got each statistic and their significance.

PPMS stands for position per money spent and it takes the average position per season in the given time period and divides it by the total money spent during that same time period. Then I took the absolute value. A greater value means that the club has performed well compared to the money they have spent.

GPMS stands for goals per money spent and it takes the average goals scored per season in the given time period and divides it by the total money spent during that same time period. Then I took the absolute value. A greater value means that the club’s offense has performed well compared to the money they have spent.

GAPMS stands for goals per money spent and it takes the average goals allowed scored per season in the given time period and divides it by the total money spent during that same time period. Then I took the absolute value. A greater value means that the club’s defense has performed well compared to the money they have spent.

PtPMS stands for points per money spent and it takes the average points per season in the given time period and divides it by the total money spent during that same time period. Then I took the absolute value. A greater value means that the club has performed well compared to the money they have spent. This measures something different from the PPMS because some years teams will score more points however their position will be the same or lower because their position is dependent on how other teams perform in that given year.

The 5 and the 10 in each of the statistics represent the years that are being measured, the 5 is the last 5 years and the 10 is the last 10 years. 

Now here is the code I used to gather this information and organize it.


```python
analysis= pandas.DataFrame()
analysis['Club']= ntr['Club']
analysis['PPMS5']= (ntr['Avg. Pos. Last 5']/ntr['Total Last 5'])
analysis['PPMS5']=analysis['PPMS5'].abs()
analysis['PPMS10']= (ntr['Avg. Pos. Last 10']/ntr['Total'])
analysis['PPMS10']=analysis['PPMS10'].abs()
analysis['GPMS5'] =(ntr['Avg. GF Last 5']/ntr['Total Last 5'])
analysis['GPMS5']=analysis['GPMS5'].abs()
analysis['GPMS10'] =(ntr['Avg. GF Last 10']/ntr['Total'])
analysis['GPMS10']=analysis['GPMS10'].abs()
analysis['GAPMS5'] =(ntr['Avg. GA Last 5']/ntr['Total Last 5'])
analysis['GAPMS5']=analysis['GAPMS5'].abs()
analysis['GAPMS10'] =(ntr['Avg. GA Last 10']/ntr['Total'])
analysis['GAPMS10']=analysis['GAPMS10'].abs()
analysis['PtPMS5'] =(ntr['Avg. Pts. Last 5']/ntr['Total Last 5'])
analysis['PtPMS5']=analysis['PtPMS5'].abs()
analysis['PtPMS10'] =(ntr['Avg. Pts. Last 10']/ntr['Total'])
analysis['PtPMS10']=analysis['PtPMS10'].abs()
analysis = analysis[analysis.Club != 'Watford']
pandas.set_option("display.max_rows", None, "display.max_columns", None)

analysis
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
      <th>Club</th>
      <th>PPMS5</th>
      <th>PPMS10</th>
      <th>GPMS5</th>
      <th>GPMS10</th>
      <th>GAPMS5</th>
      <th>GAPMS10</th>
      <th>PtPMS5</th>
      <th>PtPMS10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Arsenal</td>
      <td>0.018902</td>
      <td>0.008103</td>
      <td>0.197879</td>
      <td>0.110023</td>
      <td>0.137630</td>
      <td>0.068967</td>
      <td>0.191973</td>
      <td>0.112184</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Newcastle United</td>
      <td>0.163621</td>
      <td>0.041363</td>
      <td>0.562449</td>
      <td>0.130273</td>
      <td>0.732888</td>
      <td>0.181686</td>
      <td>0.606763</td>
      <td>0.132979</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Manchester United</td>
      <td>0.006777</td>
      <td>0.003937</td>
      <td>0.116279</td>
      <td>0.064187</td>
      <td>0.068127</td>
      <td>0.038162</td>
      <td>0.126980</td>
      <td>0.070529</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Crystal Palace</td>
      <td>0.206120</td>
      <td>0.057709</td>
      <td>0.691295</td>
      <td>0.192554</td>
      <td>0.910100</td>
      <td>0.249691</td>
      <td>0.700809</td>
      <td>0.203410</td>
    </tr>
    <tr>
      <th>4</th>
      <td>West Ham United</td>
      <td>0.058630</td>
      <td>0.029546</td>
      <td>0.270115</td>
      <td>0.136273</td>
      <td>0.309899</td>
      <td>0.150142</td>
      <td>0.254410</td>
      <td>0.132053</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Leicester City</td>
      <td>0.079976</td>
      <td>0.034461</td>
      <td>0.579826</td>
      <td>0.253133</td>
      <td>0.523843</td>
      <td>0.221178</td>
      <td>0.541837</td>
      <td>0.246241</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Tottenham Hotspur</td>
      <td>0.019405</td>
      <td>0.017439</td>
      <td>0.314002</td>
      <td>0.256904</td>
      <td>0.170232</td>
      <td>0.160778</td>
      <td>0.313120</td>
      <td>0.267963</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leeds United</td>
      <td>0.097371</td>
      <td>0.060419</td>
      <td>0.670778</td>
      <td>0.416219</td>
      <td>0.584226</td>
      <td>0.362513</td>
      <td>0.638321</td>
      <td>0.396079</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Liverpool</td>
      <td>0.017934</td>
      <td>0.010610</td>
      <td>0.517517</td>
      <td>0.198141</td>
      <td>0.226734</td>
      <td>0.105522</td>
      <td>0.532889</td>
      <td>0.195847</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Manchester City</td>
      <td>0.002619</td>
      <td>0.001914</td>
      <td>0.152547</td>
      <td>0.088705</td>
      <td>0.051067</td>
      <td>0.034446</td>
      <td>0.145018</td>
      <td>0.084653</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Norwich City</td>
      <td>0.235239</td>
      <td>2.603369</td>
      <td>0.305810</td>
      <td>5.130168</td>
      <td>0.882145</td>
      <td>10.030628</td>
      <td>0.247001</td>
      <td>5.053599</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wolverhampton Wanderers</td>
      <td>0.037182</td>
      <td>0.039991</td>
      <td>0.184535</td>
      <td>0.198474</td>
      <td>0.190043</td>
      <td>0.204399</td>
      <td>0.221717</td>
      <td>0.238466</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Burnley</td>
      <td>0.202366</td>
      <td>0.182268</td>
      <td>0.610212</td>
      <td>0.486048</td>
      <td>0.831258</td>
      <td>0.694354</td>
      <td>0.706725</td>
      <td>0.564163</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Aston Villa</td>
      <td>0.049522</td>
      <td>0.047140</td>
      <td>0.169791</td>
      <td>0.119090</td>
      <td>0.199859</td>
      <td>0.186575</td>
      <td>0.159179</td>
      <td>0.111151</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Chelsea</td>
      <td>0.011609</td>
      <td>0.008755</td>
      <td>0.230128</td>
      <td>0.158362</td>
      <td>0.136575</td>
      <td>0.090382</td>
      <td>0.251297</td>
      <td>0.170465</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Brighton &amp; Hove Albion</td>
      <td>0.072866</td>
      <td>0.073985</td>
      <td>0.171177</td>
      <td>0.173807</td>
      <td>0.247513</td>
      <td>0.251315</td>
      <td>0.182743</td>
      <td>0.185551</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Everton</td>
      <td>0.032689</td>
      <td>0.025981</td>
      <td>0.182333</td>
      <td>0.157883</td>
      <td>0.183060</td>
      <td>0.145226</td>
      <td>0.197588</td>
      <td>0.166877</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Southampton</td>
      <td>0.476868</td>
      <td>0.168400</td>
      <td>1.572954</td>
      <td>0.721479</td>
      <td>2.113879</td>
      <td>0.787519</td>
      <td>1.537367</td>
      <td>0.719828</td>
    </tr>
  </tbody>
</table>
</div>



### Data Visualization and Analysis

The first question I wanted to explore was does spending more money makes premier league clubs perform better? To visualize this component I created four boxplots that can represent performance and compare it to the money being spent. The first two plots show the Average Position of Premier Clubs compared to the Money Spent by each of these clubs in the last 5 and 10 years. The second group of two plots shows the Average Points of Premier Clubs accrued compared to the Money Spent by each of these clubs in the last 5 and 10 years. In both of these scenarios performance is compared to money spent. The code and boxplots are shown below.


```python
import matplotlib.pyplot as plt
import matplotlib
import seaborn
matplotlib.style.use('ggplot')

ax = ntr.plot('Total abs 10', 'Avg. Pos. Last 10', kind='scatter', c='r', label='Premier League Clubs')
plt.title('Average Position vs Money Spent Last 10 Years')
plt.xlabel('Total Money Spent Last 10 Years (In Millions)')
plt.ylabel('Average Position Last 10 Years')
plt.show()
bx = ntr.plot('Total abs 5', 'Avg. Pos. Last 5', kind='scatter', c='r', label='Premier League Clubs')
plt.title('Average Position vs Money Spent Last 5 Years')
plt.xlabel('Total Money Spent Last 5 Years (In Millions)')
plt.ylabel('Average Position Last 5 Years')
plt.show()
```


    
![png](output_12_0.png)
    



    
![png](output_12_1.png)
    


The first two boxplots show that the more money teams spent the lower their position is. The lower the position the stronger the performance as the position is a ranking compared to other clubs (meaning 1 is first place and 2 is second place). This plot shows that the more money premier league clubs spend the lower their position which means the better their performance is. This is true in the last 10 years and the last 5 years. 


```python
cx = ntr.plot('Total abs 10', 'Avg. Pts. Last 10', kind='scatter', c='b')
plt.title('Average Points vs Money Spent Last 10 Years')
plt.xlabel('Total Money Spent Last 10 Years (In Millions)')
plt.ylabel('Average Points Last 10 Years')
plt.show()
dx = ntr.plot('Total abs 5', 'Avg. Pts. Last 5', kind='scatter', c='b')
plt.title('Average Points vs Money Spent Last 5 Years')
plt.xlabel('Total Money Spent Last 5 Years (In Millions)')
plt.ylabel('Average Points Last 5 Years')
plt.show()
```


    
![png](output_14_0.png)
    



    
![png](output_14_1.png)
    


The second group of two boxplots shows a similar analysis. These two plots show that the more money premier league teams spend the more points they accrue through the season which means their performance gets better as the team spends more money. This is also true throughout the last 5 and 10 years.

I created these next two boxplots to explore if spending more money affects improves the attack (or offense) or the defense of a team more. I decided to explore this because attackers are known to be more expensive than defenders and a lot of people believe a defense must be grown but an attack can be bought.


```python
ex = ntr.plot('Total abs 10', 'Avg. GF Last 10', kind='scatter', c='g')
plt.title('Goals Scored vs Money Spent Last 10 Years')
plt.xlabel('Total Money Spent Last 10 Years (In Millions)')
plt.ylabel('Average Goals Scored Last 10 Years')
plt.show()
fx = ntr.plot('Total abs 5', 'Avg. GA Last 10', kind='scatter', c='g')
plt.title('Goals Allowed vs Money Spent Last 10 Years')
plt.xlabel('Total Money Spent Last 10 Years (In Millions)')
plt.ylabel('Average Allowed Last 10 Years')
plt.show()
```


    
![png](output_17_0.png)
    



    
![png](output_17_1.png)
    


The two boxplots do support the conclusions made from the previous box plots as in both situations the more money teams spend the better their performances were, they let up fewer goals and score more goals. However, the assumption that offense is more greatly affected compared to defense by money is false as shown by the boxplots. The relationship between goals allowed and money spent is more linear than the relationship between goals scored and money spent. Money is actually a bigger factor in building a team’s defense than their attack which means other factors like coaching and tactics most likely have a bigger effect on a team's offense. 

Lastly, I wanted to analyze which teams were spending their money wisely. To do this I decided to plot bar graphs that will show how each team performed in a certain metric, the metrics I created. The first plot graphs the points per money spent for each team the higher the metric the better the team performed for the money they spent. The second plot graphs the position per money spent and the third plot graphs the goals per money spent. In each plot the higher the metric the better the team performed for the money they spent.


```python
analysis.plot(x='Club', y='PtPMS5', kind='bar') 
plt.title('Points Per Money Spent Last 5 Years')
plt.xlabel('Premier League Clubs')
plt.ylabel('PtPMS5')
plt.show()

analysis.plot(x='Club', y='PPMS5', kind='bar') 
plt.title('Position Per Money Spent Last 5 Years')
plt.xlabel('Premier League Clubs')
plt.ylabel('PPMS5')
plt.show()
```


    
![png](output_20_0.png)
    



    
![png](output_20_1.png)
    



```python
analysis.plot(x='Club', y='GPMS5', kind='bar') 
plt.title('Goals Scored Per Money Spent Last 5 Years')
plt.xlabel('Premier League Clubs')
plt.ylabel('GFPMS5')
plt.show()
```


    
![png](output_21_0.png)
    


These bar graphs show the teams that have spent their money wisely in the premier league. Teams like Southampton, Burnley, Crystal Palace, and Leeds United perform well in my calculations. All of these teams mentioned don’t frivolously spend money as they aren’t raking in money from revenue like other clubs. However, they continue to perform well in the premier league and these plots show that they perform well compared to the money they spend. 

The top six clubs, the six biggest clubs in the league, do not perform as well in these plots. The metrics I calculated do not favor these teams. Even though they are the teams with the overall best performances the calculations say that the astronomical amounts of money they spend are not proportional to how they are performing. The bar plots are showing that these teams could spend their money more wisely and still perform at the top of the tables, but as of now, they are overspending.

However, out of the top six, the team that spends their money the wisest is Liverpool. Liverpool is an analytically driven team as you can read more about here, https://www.nytimes.com/2019/05/22/magazine/soccer-data-liverpool.html. Using these analytics Liverpool spends less money on overpriced players and because of this, they perform well compared to their money spent. They spend their money efficiently and it has allowed them to keep more money and still perform as a top team in the league.


### Conclusion

After analyzing the premier league clubs' spending and performances, many things can be concluded. First of all spending more money does correlate to better performances and this makes perfect sense because these teams are able to buy more valuable players which tend to be the better players. However, just because these teams spend a lot of money does not mean these teams are spending their money efficiently. As seen in the calculations a lot of the teams that spend their money efficiently are not the richest clubs in the league. Throughout my analysis, I highlighted the teams that spent their money the wisest. 

As a lot of fans and soccer enthusiasts know, money has become a key factor in building a competitive team. Most people believe that the richest clubs have an unfair competitive advantage and even though they perform better on average, there is still hope for the rest of the clubs. Clubs like Liverpool who spend their money extremely efficiently can churn out results on par with the richest clubs in the world even though they don’t spend as much money. At the end of the day how you spend your money is more important than how much money you spend.
