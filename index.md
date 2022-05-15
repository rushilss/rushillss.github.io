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


Season	Pos	Club	Pld	W	D	L	GF	GA	GD	Pts	Qualification or relegation
0	2010-11	1	Manchester United	38	23	11	4	78	37	41	80	Qualification for the Champions League group s...
1	2010-11	2	Chelsea	38	21	8	9	69	33	36	71	Qualification for the Champions League group s...
2	2010-11	3	Manchester City	38	21	8	9	60	33	27	71	Qualification for the Champions League group s...
3	2010-11	4	Arsenal	38	19	11	8	72	43	29	68	Qualification for the Champions League play-of...
4	2010-11	5	Tottenham Hotspur	38	16	14	8	55	46	9	62	Qualification for the Europa League play-off r...
...	...	...	...	...	...	...	...	...	...	...	...	...
215	2020-21	16	Brighton & Hove Albion	38	9	14	15	40	46	-6	41	Not Applicable
216	2020-21	17	Burnley	38	10	9	19	33	55	-22	39	Not Applicable
217	2020-21	18	Fulham	38	5	13	20	27	53	-26	28	Relegation to the EFL Championship
218	2020-21	19	West Bromwich Albion	38	5	11	22	35	76	-41	26	Relegation to the EFL Championship
219	2020-21	20	Sheffield United	38	7	2	29	20	63	-43	23	Relegation to the EFL Championship
220 rows × 12 columns
```

The other data we want to analyze and manipulate is the transfer data for all the clubs in the premier league. I downloaded data from transfermarkt.com which is a German-based website that has all the information available to the public in their database about soccer transfers. The data I downloaded was the net spending of each club in the past ten years. Net spend is the difference between the money a team made by selling players and the money a team spent by buying players. A positive net spend means a team made a profit by spending less than they sold and a negative net spend means a team spent more than they sold. The code to get the data and the original table are shown below. 

```markdown

transfer_table = pandas.read_csv("notebooks/Final/Premier_league_transfer.csv")

transfer_table


Club	2021-22	2020-21	2019-20	2018-19	2017-18	2016-17	2015-16	2014-15	2013-14	2012-13	Total
0	Arsenal	-136.02	-66.85	-107.15	-71.05	9.15	-102.69	-24.00	-91.18	-37.10	9.85	-617.04
1	Newcastle United	-131.50	-38.73	-37.26	-8.70	-25.28	36.63	-102.28	-21.15	22.07	-17.17	-323.36
2	Manchester United	-109.30	-64.30	-153.62	-52.15	-152.90	-137.75	-55.33	-148.65	-75.33	-66.80	-1016.13
3	Crystal Palace	-85.62	-2.40	47.78	-11.50	-45.95	-51.00	-23.40	-28.35	-33.00	14.67	-218.77
4	West Ham United	-70.27	-9.29	-64.32	-87.14	12.22	-42.50	-34.19	-30.75	-23.47	-18.85	-368.54
5	Leicester City	-63.60	-5.63	-15.80	-18.80	-33.75	-26.05	-40.45	-22.86	0.65	-1.72	-228.00
6	Tottenham Hotspur	-61.78	-97.20	-84.00	5.35	-19.70	-31.20	16.25	-4.33	15.85	-0.47	-261.23
7	Leeds United	-58.90	-106.80	30.40	-4.10	-10.93	-1.00	-1.46	3.72	-2.27	2.38	-148.96
8	Liverpool	-57.50	-65.45	34.10	-140.88	10.62	5.48	-35.95	-52.16	-25.60	-60.15	-387.49
9	Manchester City	-40.70	-95.65	-88.52	-20.99	-226.15	-179.65	-141.03	-72.50	-104.20	-17.65	-987.04
10	Brentford	-35.70	54.70	6.27	28.40	3.85	9.25	15.12	-1.80	-0.31	NaN	79.78
11	Watford	-30.65	67.50	-23.10	21.74	-54.66	-12.35	-73.05	-8.45	2.08	3.99	-106.96
12	Norwich City	-25.65	30.72	-6.62	32.15	18.52	10.25	-29.01	-0.97	-25.22	-10.70	-6.53
13	Wolverhampton Wanderers	-5.80	-8.60	-92.60	-87.20	-20.54	-33.11	5.73	-2.02	4.26	14.84	-225.05
14	Burnley	-5.40	1.20	-10.30	-25.00	14.26	-44.40	-5.01	-12.62	4.11	6.35	-76.81
15	Aston Villa	-2.82	-98.58	-156.50	-2.95	15.03	-39.70	-1.85	-12.14	-11.74	-24.63	-335.88
16	Chelsea	1.95	-189.80	112.27	-125.55	-65.90	-23.90	-9.01	5.11	-52.42	-84.25	-431.50
17	Brighton & Hove Albion	4.80	-7.90	-59.90	-73.50	-66.10	-8.75	-13.47	9.42	3.20	-0.67	-212.88
18	Everton	6.50	-68.95	-33.20	-71.15	-76.82	-25.20	-37.90	-38.26	14.30	-2.90	-333.58
19	Southampton	17.27	-11.00	-34.20	-36.15	37.10	16.15	-7.40	27.83	-35.40	-41.50	-67.30

```

## Cleaning and Manipuating the Data

Here I wanted to merge the two tables and display them, but there are some logistical things that must be taken care of first. I had to delete the 2021-2022 season as it is not finished yet and the transfer data on it was not fully up to date. Then I had to delete the club Brentford from the transfer table as the 2021-2022 season was the first season they were in the premier league, so there is no data for their results in the premier league for the last 10 seasons. I then deleted information that I don’t believe is relevant to the analysis I will be doing. I deleted games played (GP) as it doesn’t measure performance at all. I also deleted wins, losses, and draws because points encompass all 3 of these (W= 3 points, D= 1 point, L= 0 points). I also deleted the qualification column as it is just a description and has no numerical values.

Then I took the Net Spend for each club during each season from the transfer table and added it to the standings table which will allow me to provide a comprehensive performance review for each club during each season. The code for all of this and the resulting table is below.

markdown```

	Season	Pos	Club	GF	GA	GD	Pts	Net_Spend
40	2012-13	1	Manchester United	86	43	43	89	-66.80
41	2012-13	2	Manchester City	66	34	32	78	-17.65
42	2012-13	3	Chelsea	75	39	36	75	-0.67
43	2012-13	4	Arsenal	72	37	35	73	9.85
44	2012-13	5	Tottenham Hotspur	66	46	20	72	-0.47
...	...	...	...	...	...	...	...	...
212	2020-21	13	Wolverhampton Wanderers	36	52	-16	45	1.20
213	2020-21	14	Crystal Palace	41	66	-25	44	-2.40
214	2020-21	15	Southampton	47	68	-21	43	-11.00
215	2020-21	16	Brighton & Hove Albion	40	46	-6	41	-68.95
216	2020-21	17	Burnley	33	55	-22	39	-98.58

```


