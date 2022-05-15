## Premier League Transfer Analysis

Transfers are the main component of professional soccer clubs building and improving their teams. A transfer is when a player is under contract for one club but moves to another. The leading way clubs transfer these players is by exchanging the player for a large sum of money that is agreed upon between the two clubs. This money comes from the club's investors, sponsors, and revenue. Because of the importance of money in soccer, fans believe that the more money spent the better the results for their team will be. Is this true? Who are the teams that are spending their money the best? Who are the teams that are spending their money the worst? Is the difference in transfer spending ruining the competitiveness of the league? These are the questions I will be answering in my analysis.

### Data

To gather the data on the results of each team in the premier league to see how they performed each year, I found and downloaded a dataset that contains all the results and final positions of all the teams in the league for that given year. This dataset contains results for every team from the 2010-2011 season all the way to the 2020-2021 season. The code to read the CSV file and to display the table is shown below.


```markdown
import os
import folium
import requests
import pandas
import numpy

epl_standings = pandas.read_csv("notebooks/Final/EPL_Standings.csv")

epl_standings


      Season  Pos                    Club  Pld   W   D   L  GF  GA  GD  Pts  \
0    2010-11    1       Manchester United   38  23  11   4  78  37  41   80   
1    2010-11    2                 Chelsea   38  21   8   9  69  33  36   71   
2    2010-11    3         Manchester City   38  21   8   9  60  33  27   71   
3    2010-11    4                 Arsenal   38  19  11   8  72  43  29   68   
4    2010-11    5       Tottenham Hotspur   38  16  14   8  55  46   9   62   
..       ...  ...                     ...  ...  ..  ..  ..  ..  ..  ..  ...   
215  2020-21   16  Brighton & Hove Albion   38   9  14  15  40  46  -6   41   
216  2020-21   17                 Burnley   38  10   9  19  33  55 -22   39   
217  2020-21   18                  Fulham   38   5  13  20  27  53 -26   28   
218  2020-21   19    West Bromwich Albion   38   5  11  22  35  76 -41   26   
219  2020-21   20        Sheffield United   38   7   2  29  20  63 -43   23   

                           Qualification or relegation  
0    Qualification for the Champions League group s...  
1    Qualification for the Champions League group s...  
2    Qualification for the Champions League group s...  
3    Qualification for the Champions League play-of...  
4    Qualification for the Europa League play-off r...  
..                                                 ...  
215                                     Not Applicable  
216                                     Not Applicable  
217                 Relegation to the EFL Championship  
218                 Relegation to the EFL Championship  
219                 Relegation to the EFL Championship  

[220 rows x 12 columns]

```

The other data we want to analyze and manipulate is the transfer data for all the clubs in the premier league. I downloaded data from transfermarkt.com which is a German-based website that has all the information available to the public in their database about soccer transfers. The data I downloaded was the net spending of each club in the past ten years. Net spend is the difference between the money a team made by selling players and the money a team spent by buying players. A positive net spend means a team made a profit by spending less than they sold and a negative net spend means a team spent more than they sold. The code to get the data and the original table are shown below. 

```markdown

transfer_table = pandas.read_csv("notebooks/Final/Premier_league_transfer.csv")

transfer_table

                      Club  2021-22  2020-21  2019-20  2018-19  2017-18  \
0                  Arsenal  -136.02   -66.85  -107.15   -71.05     9.15   
1         Newcastle United  -131.50   -38.73   -37.26    -8.70   -25.28   
2        Manchester United  -109.30   -64.30  -153.62   -52.15  -152.90   
3           Crystal Palace   -85.62    -2.40    47.78   -11.50   -45.95   
4          West Ham United   -70.27    -9.29   -64.32   -87.14    12.22   
..                     ...      ...      ...      ...      ...      ...   
15             Aston Villa    -2.82   -98.58  -156.50    -2.95    15.03   
16                 Chelsea     1.95  -189.80   112.27  -125.55   -65.90   
17  Brighton & Hove Albion     4.80    -7.90   -59.90   -73.50   -66.10   
18                 Everton     6.50   -68.95   -33.20   -71.15   -76.82   
19             Southampton    17.27   -11.00   -34.20   -36.15    37.10   

    2016-17  2015-16  2014-15  2013-14  2012-13    Total  
0   -102.69   -24.00   -91.18   -37.10     9.85  -617.04  
1     36.63  -102.28   -21.15    22.07   -17.17  -323.36  
2   -137.75   -55.33  -148.65   -75.33   -66.80 -1016.13  
3    -51.00   -23.40   -28.35   -33.00    14.67  -218.77  
4    -42.50   -34.19   -30.75   -23.47   -18.85  -368.54  
..      ...      ...      ...      ...      ...      ...  
15   -39.70    -1.85   -12.14   -11.74   -24.63  -335.88  
16   -23.90    -9.01     5.11   -52.42   -84.25  -431.50  
17    -8.75   -13.47     9.42     3.20    -0.67  -212.88  
18   -25.20   -37.90   -38.26    14.30    -2.90  -333.58  
19    16.15    -7.40    27.83   -35.40   -41.50   -67.30  

[20 rows x 12 columns]

```

## Cleaning and Manipuating the Data

Here I wanted to merge the two tables and display them, but there are some logistical things that must be taken care of first. I had to delete the 2021-2022 season as it is not finished yet and the transfer data on it was not fully up to date. Then I had to delete the club Brentford from the transfer table as the 2021-2022 season was the first season they were in the premier league, so there is no data for their results in the premier league for the last 10 seasons. I then deleted information that I don’t believe is relevant to the analysis I will be doing. I deleted games played (GP) as it doesn’t measure performance at all. I also deleted wins, losses, and draws because points encompass all 3 of these (W= 3 points, D= 1 point, L= 0 points). I also deleted the qualification column as it is just a description and has no numerical values.

Then I took the Net Spend for each club during each season from the transfer table and added it to the standings table which will allow me to provide a comprehensive performance review for each club during each season. The code for all of this and the resulting table is below.

```markdown

      Season  Pos                     Club  GF  GA  GD  Pts  Net_Spend
40   2012-13    1        Manchester United  86  43  43   89     -66.80
41   2012-13    2          Manchester City  66  34  32   78     -17.65
42   2012-13    3                  Chelsea  75  39  36   75      -0.67
43   2012-13    4                  Arsenal  72  37  35   73       9.85
44   2012-13    5        Tottenham Hotspur  66  46  20   72      -0.47
..       ...  ...                      ...  ..  ..  ..  ...        ...
212  2020-21   13  Wolverhampton Wanderers  36  52 -16   45       1.20
213  2020-21   14           Crystal Palace  41  66 -25   44      -2.40
214  2020-21   15              Southampton  47  68 -21   43     -11.00
215  2020-21   16   Brighton & Hove Albion  40  46  -6   41     -68.95
216  2020-21   17                  Burnley  33  55 -22   39     -98.58

[133 rows x 8 columns]

```

Then I took the average position over the last ten years and added it to the transfer table, so you can see each club's average position over the last ten years. This gives an overall idea of how a team performed in the last decade. 

I then took got data for the last five years, so I extracted the average position and total net spend over the last five years. This is an extremely important delineation in time because in 2016 the premier league signed a new television deal which saw revenue for the league and its clubs' skyrocket. So measuring the last five years will give a more accurate reading of how clubs are spending their money in the modern-day. 

```markdown
                      Club  2020-21  2019-20  2018-19  2017-18  ...  2012-13  \
0                  Arsenal   -66.85  -107.15   -71.05     9.15  ...     9.85   
1         Newcastle United   -38.73   -37.26    -8.70   -25.28  ...   -17.17   
2        Manchester United   -64.30  -153.62   -52.15  -152.90  ...   -66.80   
3           Crystal Palace    -2.40    47.78   -11.50   -45.95  ...    14.67   
4          West Ham United    -9.29   -64.32   -87.14    12.22  ...   -18.85   
..                     ...      ...      ...      ...      ...  ...      ...   
15             Aston Villa   -98.58  -156.50    -2.95    15.03  ...   -24.63   
16                 Chelsea  -189.80   112.27  -125.55   -65.90  ...   -84.25   
17  Brighton & Hove Albion    -7.90   -59.90   -73.50   -66.10  ...    -0.67   
18                 Everton   -68.95   -33.20   -71.15   -76.82  ...    -2.90   
19             Southampton   -11.00   -34.20   -36.15    37.10  ...   -41.50   

      Total  Total Last 5  Avg. Pos. Last 10  Avg. Pos. Last 5  
0   -617.04       -338.59           5.000000              6.40  
1   -323.36        -73.34          13.375000             12.00  
2  -1016.13       -560.72           4.000000              3.80  
3   -218.77        -63.07          12.625000             13.00  
4   -368.54       -191.03          10.888889             11.20  
..      ...           ...                ...               ...  
15  -335.88       -282.70          15.833333             14.00  
16  -431.50       -292.88           3.777778              3.40  
17  -212.88       -216.15          15.750000             15.75  
18  -333.58       -275.32           8.666667              9.00  
19   -67.30        -28.10          11.333333             13.40  

[19 rows x 14 columns]

```
