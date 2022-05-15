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
transfer_table = pandas.read_csv("notebooks/Final/Premier_league_transfer.csv")

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

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/rushilss/rushillss.github.io/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
