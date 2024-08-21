---
jupyter:
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.12.4
  nbformat: 4
  nbformat_minor: 5
---

::: {#49d254ba-e3ec-4065-b696-c46ee1a687b4 .cell .markdown}
### Inside EPL 20/21: A Data Analysis Perspective

*The goal of this analysis is to delve into the performance trends and
key insights from the English Premier League 20/21 season. By examining
various statistical metrics, including goals scored, assists,
possession, and defensive capabilities, the analysis aims to uncover
patterns that defined the season. The focus is on identifying standout
performers, team strategies, and factors that influenced match outcomes.
Through this analysis, a comprehensive overview of the season\'s
dynamics will be provided, offering valuable insights for football
enthusiasts, analysts, and clubs alike*
:::

::: {#564f9d5c-ade7-4094-9443-6c538bc2dee2 .cell .markdown}
#### Importing Necessary Libraries
:::

::: {#9987281a-7b87-43d9-be45-2f17ea72f094 .cell .code execution_count="1"}
``` python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from IPython.display import display, Markdown
```
:::

::: {#141b8125-ebd9-4fd4-b366-a5b5f3e11740 .cell .code execution_count="52"}
``` python
def displaytext(title, list):
    display(Markdown(f"##### {title}"))
    display(list)
```
:::

::: {#0c4c5491-7ed8-418b-b809-181df491b2c8 .cell .markdown}
#### Reading the data
:::

::: {#429403fe-f9db-40fa-9524-692cc587f78a .cell .code execution_count="3"}
``` python
df = pd.read_csv("EPL_20_21.csv")
```
:::

::: {#aa637ca0-36a8-42ca-a5c6-24e7f1aae5dd .cell .code execution_count="4"}
``` python
df.head(2)
```

::: {.output .execute_result execution_count="4"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#d1d47f1e-9165-4d35-9bbe-47a70fec692a .cell .markdown}
#### \"Minutes Per Match\" & \"Goals Per Match\" {#minutes-per-match--goals-per-match}
:::

::: {#b648e15c-f8b6-4ab0-9ee4-914acb9280ec .cell .markdown}
*Create two columns: \"Minutes Per Match\" & \"Goals Per Match*
:::

::: {#fbe61b31-0033-40c5-8459-3af3663f5b7f .cell .code execution_count="5"}
``` python
df["MinutesPerMatch"] = (df.Mins / df.Matches).astype(int)
df["GoalsPerMatch"] = (df.Goals / df.Matches).astype(float) 
```
:::

::: {#9cea2ee5-cf34-4b57-9a83-ba845942ed4d .cell .code execution_count="6"}
``` python
df.head(2)
```

::: {.output .execute_result execution_count="6"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#12d92bb9-c4e7-4d03-aa38-176ba02982cd .cell .markdown}
#### Total Goals

*Total Goals Scored During the Campaign*
:::

::: {#29ffb2cd-c6ea-4e42-8fb6-a63edf7e5f00 .cell .code execution_count="7"}
``` python
total_goals = df.Goals.sum()
total_goals
```

::: {.output .execute_result execution_count="7"}
    986
:::
:::

::: {#fb57aabd-e8d0-43fe-be70-b857421229ae .cell .markdown}
#### Penalty Attempts

*Total Penalty Attempts*
:::

::: {#a2aa3076-1370-458b-b9b0-66e309beca96 .cell .code execution_count="8"}
``` python
penalty_attempted = df.Penalty_Attempted.sum()
penalty_attempted
```

::: {.output .execute_result execution_count="8"}
    125
:::
:::

::: {#2a4f338f-d34e-4d85-a464-44e250a3fd01 .cell .markdown}
#### Penalty Goals

*Total Penalty Goals Scored*
:::

::: {#bf77e9b9-e279-48dd-be45-496ecbcc6ac4 .cell .code execution_count="9"}
``` python
penalty_goals = df.Penalty_Goals.sum()
penalty_goals
```

::: {.output .execute_result execution_count="9"}
    102
:::
:::

::: {#ff6873f8-bf43-4c49-af72-0d5b56fe4f3f .cell .markdown}
#### Pie Chart of Penalty Missed vs Scored
:::

::: {#113d0d9e-4f7a-4ae9-8e92-be4b8c30efa6 .cell .code execution_count="10"}
``` python
plt.figure(figsize=(14, 7))
pen_missed = penalty_attempted - penalty_goals
data = [pen_missed, penalty_goals]
labels = ["Penalty Missed", "Penalty Scored"]
color = sns.color_palette("Set2")
plt.pie(data, labels = labels, colors = color, autopct = "%.0f%%")
plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/47b157482b85c7e032ef003e56ca10fb1c82fd74.png)
:::
:::

::: {#7cd2501c-d727-45b7-8d64-6c197f286655 .cell .markdown}
#### Chart of Goals Scored with Assists vs Without Assists
:::

::: {#aa61ec03-757e-49d4-bba7-54fbd4298441 .cell .code execution_count="554"}
``` python
plt.figure(figsize=(14,7))
assists = df.Assists.sum()
data = [total_goals - assists, assists]
label = ["Goals without Assist", "Goals with Assist"]
color = sns.color_palette("Set2")
plt.pie(data, labels = label, colors = color, autopct = "%.0f%%")
plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/ca35529ee1829012e41eafcbf619fe6a24bdba0e.png)
:::
:::

::: {#716c665d-a292-4f9b-87ec-7373cd1164eb .cell .code execution_count="11"}
``` python
df.head(2)
```

::: {.output .execute_result execution_count="11"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#a833f29f-ec21-4e0e-9b3f-802dfd5aac35 .cell .markdown}
#### Number of Unique Countries Represented in the Campaign
:::

::: {#aace1565-4f42-444d-a6ec-afdaee1b8a75 .cell .code execution_count="12"}
``` python
np.size((df.Nationality).unique())
```

::: {.output .execute_result execution_count="12"}
    59
:::
:::

::: {#de81bada-9440-4422-919a-cfb8cdc53429 .cell .markdown}
#### How Various Countries were Represented During the Campaign
:::

::: {#a24259cb-2d75-49a0-96ae-523059707322 .cell .code execution_count="319"}
``` python
nationality = df.groupby("Nationality").size().to_frame()
nationality
```

::: {.output .execute_result execution_count="319"}
```{=html}
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
      <th>0</th>
    </tr>
    <tr>
      <th>Nationality</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>ALG</th>
      <td>3</td>
    </tr>
    <tr>
      <th>ARG</th>
      <td>8</td>
    </tr>
    <tr>
      <th>AUS</th>
      <td>4</td>
    </tr>
    <tr>
      <th>AUT</th>
      <td>1</td>
    </tr>
    <tr>
      <th>BEL</th>
      <td>11</td>
    </tr>
    <tr>
      <th>BFA</th>
      <td>1</td>
    </tr>
    <tr>
      <th>BIH</th>
      <td>1</td>
    </tr>
    <tr>
      <th>BRA</th>
      <td>27</td>
    </tr>
    <tr>
      <th>CAN</th>
      <td>1</td>
    </tr>
    <tr>
      <th>CIV</th>
      <td>8</td>
    </tr>
    <tr>
      <th>CMR</th>
      <td>2</td>
    </tr>
    <tr>
      <th>COD</th>
      <td>2</td>
    </tr>
    <tr>
      <th>COL</th>
      <td>5</td>
    </tr>
    <tr>
      <th>CRO</th>
      <td>2</td>
    </tr>
    <tr>
      <th>CZE</th>
      <td>3</td>
    </tr>
    <tr>
      <th>DEN</th>
      <td>6</td>
    </tr>
    <tr>
      <th>EGY</th>
      <td>5</td>
    </tr>
    <tr>
      <th>ENG</th>
      <td>192</td>
    </tr>
    <tr>
      <th>ESP</th>
      <td>26</td>
    </tr>
    <tr>
      <th>FRA</th>
      <td>31</td>
    </tr>
    <tr>
      <th>GAB</th>
      <td>2</td>
    </tr>
    <tr>
      <th>GER</th>
      <td>9</td>
    </tr>
    <tr>
      <th>GHA</th>
      <td>5</td>
    </tr>
    <tr>
      <th>GRE</th>
      <td>1</td>
    </tr>
    <tr>
      <th>GUI</th>
      <td>1</td>
    </tr>
    <tr>
      <th>IRL</th>
      <td>21</td>
    </tr>
    <tr>
      <th>IRN</th>
      <td>1</td>
    </tr>
    <tr>
      <th>ISL</th>
      <td>3</td>
    </tr>
    <tr>
      <th>ITA</th>
      <td>5</td>
    </tr>
    <tr>
      <th>JAM</th>
      <td>3</td>
    </tr>
    <tr>
      <th>JPN</th>
      <td>2</td>
    </tr>
    <tr>
      <th>KOR</th>
      <td>1</td>
    </tr>
    <tr>
      <th>MAR</th>
      <td>2</td>
    </tr>
    <tr>
      <th>MEX</th>
      <td>1</td>
    </tr>
    <tr>
      <th>MKD</th>
      <td>1</td>
    </tr>
    <tr>
      <th>MLI</th>
      <td>2</td>
    </tr>
    <tr>
      <th>MTN</th>
      <td>1</td>
    </tr>
    <tr>
      <th>NED</th>
      <td>16</td>
    </tr>
    <tr>
      <th>NGA</th>
      <td>7</td>
    </tr>
    <tr>
      <th>NIR</th>
      <td>5</td>
    </tr>
    <tr>
      <th>NOR</th>
      <td>3</td>
    </tr>
    <tr>
      <th>NZL</th>
      <td>1</td>
    </tr>
    <tr>
      <th>PAR</th>
      <td>2</td>
    </tr>
    <tr>
      <th>POL</th>
      <td>5</td>
    </tr>
    <tr>
      <th>POR</th>
      <td>21</td>
    </tr>
    <tr>
      <th>RSA</th>
      <td>2</td>
    </tr>
    <tr>
      <th>SCO</th>
      <td>20</td>
    </tr>
    <tr>
      <th>SEN</th>
      <td>5</td>
    </tr>
    <tr>
      <th>SKN</th>
      <td>1</td>
    </tr>
    <tr>
      <th>SRB</th>
      <td>4</td>
    </tr>
    <tr>
      <th>SUI</th>
      <td>6</td>
    </tr>
    <tr>
      <th>SVK</th>
      <td>2</td>
    </tr>
    <tr>
      <th>SWE</th>
      <td>5</td>
    </tr>
    <tr>
      <th>TUR</th>
      <td>5</td>
    </tr>
    <tr>
      <th>UKR</th>
      <td>2</td>
    </tr>
    <tr>
      <th>URU</th>
      <td>1</td>
    </tr>
    <tr>
      <th>USA</th>
      <td>6</td>
    </tr>
    <tr>
      <th>WAL</th>
      <td>12</td>
    </tr>
    <tr>
      <th>ZIM</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#7af1e9fc-aaae-42ba-b430-fb4935b548be .cell .markdown}
#### Countries with the Most Representation
:::

::: {#6f30ba0f-c299-4c5c-a936-ea41cbe1e4d2 .cell .code execution_count="14"}
``` python
nationality = df.groupby("Nationality").size().sort_values(ascending = False)
nationality.head(10).plot(kind = 'bar', figsize=(12,6), color = sns.color_palette("magma"))
```

::: {.output .execute_result execution_count="14"}
    <Axes: xlabel='Nationality'>
:::

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/93fb498ef4a24618c9d0814ef61e43453e5d4131.png)
:::
:::

::: {#f858d364-8b3d-41db-a643-50bf1c2f36c9 .cell .code execution_count="15"}
``` python
# Observing the data
df.head(2)
```

::: {.output .execute_result execution_count="15"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#a968f6f6-24db-465a-9fd4-29939ef59f97 .cell .markdown}
#### Ranking of Clubs by Squad Numbers
:::

::: {#66a5845a-a8b7-481f-9d28-a52cd31c5077 .cell .code execution_count="327"}
``` python
# df.groupby("Club").Name.sum()
df.Club.value_counts().plot(kind = "bar", color = sns.color_palette("magma"))
sns.set_style("whitegrid")
plt.ylim(0, 35)
```

::: {.output .execute_result execution_count="327"}
    (0.0, 35.0)
:::

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/af1ad00a3e68b6f8a036674dc1bac2d6a8d01f69.png)
:::
:::

::: {#c242fb89-2485-457e-a47f-b407dd1f260e .cell .markdown}
#### Ranking Clubs by Squad Numbers - Bottom 5
:::

::: {#27b9bac6-c10e-4e90-9d45-d624e300b0eb .cell .code execution_count="536"}
``` python
df.Club.value_counts().nsmallest(5).plot(kind = "bar", color = sns.color_palette("magma"))
sns.set_style("whitegrid")
plt.ylim(0, 25)
plt.xticks(rotation=45)

plt.title("\nRanking Clubs by Squad Size - Bottom 5\n", fontsize=17)
plt.xlabel("Clubs", fontsize=15)
plt.ylabel("Squad Size", fontsize=15)

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/b1adb69e20338f2ae7af6610c0462ba93fdae3ac.png)
:::
:::

::: {#7ce1b07e-c028-493e-b300-d419ac0d21e2 .cell .code execution_count="18"}
``` python
# Observing the data
df.head(2)
```

::: {.output .execute_result execution_count="18"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#0135e9bb-8932-4ef9-85cf-c706092b0750 .cell .markdown}
#### Age Binning (U20, U25, U30 & \>30) {#age-binning-u20-u25-u30--30}
:::

::: {#1611a1da-f126-4e09-99ad-0fb3619b69ce .cell .markdown}
##### Under 20
:::

::: {#d6454f92-c3ae-45de-af40-e97bbcd430d2 .cell .code execution_count="537"}
``` python
under20 = df[df.Age <= 20]
```
:::

::: {#11dc3dcc-a3d0-45f4-9bda-4d3d4fb1e80c .cell .markdown}
##### Under 25
:::

::: {#73362a6b-e6ca-4ed7-bb74-4b45f25a1b3b .cell .code execution_count="538"}
``` python
under25 = df[(df.Age > 20) & (df.Age <=25)] 
```
:::

::: {#2d0b3268-45cb-46b0-b11a-a3cba65a9ee1 .cell .markdown}
##### Under 30
:::

::: {#13b29c77-b315-4d40-9c49-256a0ed5f260 .cell .code execution_count="539"}
``` python
under30 = df[(df.Age > 25) & (df.Age <=30)] 
```
:::

::: {#55ea8d9b-b39c-432d-95f5-191db1a00746 .cell .markdown}
##### Players Above 30
:::

::: {#0ed60e97-db78-4b6e-98df-20315a7bce07 .cell .code execution_count="540"}
``` python
above30 = df[df.Age > 30]
```
:::

::: {#60384228-d2cf-433c-842e-191988b334a0 .cell .markdown}
#### Pie Chart Showing Players Size by Age Group
:::

::: {#b59ffafd-b440-47fd-aeb4-c67bb0b8289b .cell .code execution_count="542"}
``` python
fig, ax = plt.subplots(figsize=(10, 10))

x = np.array([under20["Name"].count(), under25["Name"].count(), under30["Name"].count(), above30["Name"].count()])
mylabels = ["U-20", "U-25", "U-30", ">30"]

plt.title("Total Players with Age Groups", fontsize = 15)
plt.pie(x, labels = mylabels, autopct = "%.1f%%")

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/78c2dd059c2cac03ecb4d21f8527f229d801764b.png)
:::
:::

::: {#f1bb111d-2e3e-4bdf-b3cc-92abe46c3e28 .cell .markdown}
#### Under-20 Players Stats
:::

::: {#a80ac1e5-d2fb-4953-8a90-f8447c233a85 .cell .code execution_count="567"}
``` python
under20.loc[:, "Goals Contribution"] = under20.Goals + under20.Assists

under20stats_top10 = under20.groupby("Name").agg({"Goals Contribution": "first", "Goals": "sum", "Assists": "sum"}).nlargest(10, "Goals Contribution").reset_index()
title = "Ranking U20 Players by Goals Contribution"

displaytext(title, under20stats_top10)
```

::: {.output .display_data}
##### Ranking U20 Players by Goals Contribution
:::

::: {.output .display_data}
```{=html}
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
      <th>Name</th>
      <th>Goals Contribution</th>
      <th>Goals</th>
      <th>Assists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Phil Foden</td>
      <td>14</td>
      <td>9</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pedro Neto</td>
      <td>11</td>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ferrán Torres</td>
      <td>9</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mason Greenwood</td>
      <td>9</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bukayo Saka</td>
      <td>8</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Dwight McNeil</td>
      <td>7</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Fábio Silva</td>
      <td>7</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Emile Smith-Rowe</td>
      <td>6</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Callum Hudson-Odoi</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Conor Gallagher</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#774a7b76-46ee-434b-8ab6-77d2f3245fb8 .cell .markdown}
#### Under-25 Players Stats
:::

::: {#7a51c03e-5d5e-46f4-beee-da672ddf61ca .cell .code execution_count="398"}
``` python
under25.loc[:, "Goals Contribution"] = under25.Goals + under25.Assists

under25stats_top10 = under25.groupby("Name").agg({"Goals Contribution": "first", "Goals": "sum", "Assists": "sum"}).nlargest(10, "Goals Contribution").reset_index()
title = "Ranking U25 Players by Goals Contribution"
displaytext(title, under25stats_top10)
```

::: {.output .display_data}
##### Ranking U25 Players by Goals Contribution
:::

::: {.output .display_data}
```{=html}
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
      <th>Name</th>
      <th>Goals Contribution</th>
      <th>Goals</th>
      <th>Assists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bruno Fernandes</td>
      <td>30</td>
      <td>18</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcus Rashford</td>
      <td>20</td>
      <td>11</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ollie Watkins</td>
      <td>19</td>
      <td>14</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Matheus Pereira</td>
      <td>17</td>
      <td>11</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Raheem Sterling</td>
      <td>17</td>
      <td>10</td>
      <td>7</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Dominic Calvert-Lewin</td>
      <td>16</td>
      <td>16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Jack Grealish</td>
      <td>16</td>
      <td>6</td>
      <td>10</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jack Harrison</td>
      <td>16</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>8</th>
      <td>James Ward-Prowse</td>
      <td>15</td>
      <td>8</td>
      <td>7</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Raphael Dias Belloli</td>
      <td>15</td>
      <td>6</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#2960af33-f346-4684-a44c-5b19343757cf .cell .markdown}
#### Under-30 Players Stats
:::

::: {#6f6eea06-436b-430f-8a80-7dc2fbf16059 .cell .code execution_count="400"}
``` python
under30.loc[:, "Goals Contribution"] = under30.Goals + under25.Assists

under30stats_top10 = under30.groupby("Name").agg({"Goals Contribution": "first", "Goals": "sum", "Assists": "sum"}).nlargest(10, "Goals Contribution").reset_index()
title = "Ranking U30 Players by Goals Contribution"
displaytext(title, under30stats_top10)
```

::: {.output .display_data}
##### Ranking U30 Players by Goals Contribution
:::

::: {.output .display_data}
```{=html}
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
      <th>Name</th>
      <th>Goals Contribution</th>
      <th>Goals</th>
      <th>Assists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aaron Cresswell</td>
      <td>NaN</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abdoulaye Doucouré</td>
      <td>NaN</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ahmed Hegazi</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alex McCarthy</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alex Oxlade-Chamberlain</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Alex Telles</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Alexandre Lacazette</td>
      <td>NaN</td>
      <td>13</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Alireza Jahanbakhsh</td>
      <td>NaN</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Alisson</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Allan</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#4eb22c69-96b6-49f5-92bf-e44085b5abf3 .cell .markdown}
#### Above-30 Players Stats
:::

::: {#43c7d222-35cf-43e5-8d69-3f1ef2430ece .cell .code execution_count="402"}
``` python
above30.loc[:, "Goals Contribution"] = above30.Goals + above30.Assists

above30stats_top10 = above30.groupby("Name").agg({"Goals Contribution": "first", "Goals": "sum", "Assists": "sum"}).nlargest(10, "Goals Contribution").reset_index()
title = "Ranking Above-30 Players by Goals Contribution"
displaytext(title, above30stats_top10)
```

::: {.output .display_data}
##### Ranking Above-30 Players by Goals Contribution
:::

::: {.output .display_data}
```{=html}
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
      <th>Name</th>
      <th>Goals Contribution</th>
      <th>Goals</th>
      <th>Assists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jamie Vardy</td>
      <td>24</td>
      <td>15</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edinson Cavani</td>
      <td>13</td>
      <td>10</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gareth Bale</td>
      <td>13</td>
      <td>11</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pierre-Emerick Aubameyang</td>
      <td>13</td>
      <td>10</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>David McGoldrick</td>
      <td>9</td>
      <td>8</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Willian</td>
      <td>6</td>
      <td>1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Sergio Agüero</td>
      <td>5</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Jonny Evans</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Olivier Giroud</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Angelo Ogbonna</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#2d50bf6d-019f-437f-bbdc-54157ec31b28 .cell .markdown}
#### Pie Chart Showing Distribution of Goals Scored by Age Groups
:::

::: {#b57ab902-5b61-4c88-9d7d-79b8ffa37189 .cell .code execution_count="558"}
``` python
fig, ax = plt.subplots(figsize=(10,10))

x = np.array([under20["Goals"].sum(), under25["Goals"].sum(), under30["Goals"].sum(), above30["Goals"].sum()])

mylabels = ["U-20", "U-25", "U-30", "Above 30"]
plt.title("Pie Chart of Goals Scored by Age Groups", fontsize = 15)
plt.pie(x, labels = mylabels, autopct = "%.1f%%")

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/5fb6194b1958a3dad431013448bbfd6a5e981837.png)
:::
:::

::: {#97bf95d4-faaa-46eb-ba58-acc0dbf96d98 .cell .markdown}
#### Distribution of U-20 Players by Club
:::

::: {#52619546-885a-4295-9cb7-cdf57a3e0a96 .cell .code execution_count="24"}
``` python
df[df.Age <= 20].Club.value_counts().plot(kind = 'bar', color = sns.color_palette("magma"))
```

::: {.output .execute_result execution_count="24"}
    <Axes: xlabel='Club'>
:::

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/20d880fe450ba45adb7dc5f474d0502f7ae14c15.png)
:::
:::

::: {#5eab9c25-7f99-45b7-961b-dd1d64898723 .cell .markdown}
#### Average Age of Players in Each Club
:::

::: {#630aecb3-92e5-451d-abbe-821ad68c78d8 .cell .code execution_count="561"}
``` python
avg_age_plyr = df.groupby("Club").Age.sum()/df.groupby("Club").size()
avg_age_plyr = avg_age_plyr.round(1)
mean_age = avg_age_plyr.sort_values(ascending = False ).reset_index()
mean_age.columns = ["Club", "Mean Age"]
```
:::

::: {#d0ddeb2f-2f26-473b-b944-7332c791d6df .cell .code execution_count="562"}
``` python
plt.figure(figsize=(14, 8))
sns.pointplot(x=mean_age.Club, y=mean_age["Mean Age"], data=df, linestyles="none", color='blue', markers='o')

plt.xlabel('Mean Age')
plt.xticks(rotation=75)
plt.yticks(range(23,30))
plt.title('Mean Age of Teams')

mean_age_mean = mean_age['Mean Age'].mean()

plt.text(0.95, 0.05, f'Overall Mean Age: {mean_age_mean}', 
         horizontalalignment='right', 
         verticalalignment='bottom', 
         transform=plt.gca().transAxes)

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/d942c4f1336bbf4d540bfe315ec837d4b492ab82.png)
:::
:::

::: {#650b4c56-4899-4928-9552-fa229beed63c .cell .markdown}
#### Squad Age Comparison: Youngest Teams
:::

::: {#8be0a07c-e57d-425f-b342-2f075e9c8d8e .cell .markdown}
##### Under 20 players in Man United
:::

::: {#9a58b079-0b1b-40d7-8dae-d02a955804cb .cell .code execution_count="565"}
``` python
under20[under20.Club == "Manchester United"]["Name"].to_frame()
```

::: {.output .execute_result execution_count="565"}
```{=html}
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
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>61</th>
      <td>Mason Greenwood</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Brandon Williams</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Amad Diallo</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Anthony Elanga</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Shola Shoretire</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Hannibal Mejbri</td>
    </tr>
    <tr>
      <th>79</th>
      <td>William Thomas Fish</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#2627853d-531d-4257-bacf-7c08a138b1a9 .cell .markdown}
##### Under 20 players in Southampton
:::

::: {#f780006b-6107-410c-a4ab-87bd643e46d6 .cell .code execution_count="348"}
``` python
under20[under20.Club == "Southampton"]["Name"].to_frame()
```

::: {.output .execute_result execution_count="348"}
```{=html}
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
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>385</th>
      <td>William Smallbone</td>
    </tr>
    <tr>
      <th>388</th>
      <td>Kayne Ramsey</td>
    </tr>
    <tr>
      <th>389</th>
      <td>Jake Vokins</td>
    </tr>
    <tr>
      <th>390</th>
      <td>Alexandre Jankewitz</td>
    </tr>
    <tr>
      <th>392</th>
      <td>Michael Obafemi</td>
    </tr>
    <tr>
      <th>393</th>
      <td>Caleb Watts</td>
    </tr>
    <tr>
      <th>394</th>
      <td>Allan Tchaptchet</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#ce526020-f825-488b-b80c-215e18f7f5e0 .cell .markdown}
#### Box Plot Distribution of Ages
:::

::: {#614e68d4-bd62-4469-89db-f5503c4ecdf2 .cell .code execution_count="549"}
``` python
# Boxplot

plt.figure(figsize=(12, 6))
sns.boxplot(x = "Club", y = "Age", data = df)
plt.xticks(rotation = 85)
plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/e5689e4d681f32bb23908ef1e0231e9dc210b11b.png)
:::
:::

::: {#d2509615-ee5c-4d36-bc3c-62aca05bf12a .cell .markdown}
#### Goals and Assists Ranking
:::

::: {#47f018ce-1933-4d92-8188-35e765997d6f .cell .code execution_count="31"}
``` python
df.head(2)
```

::: {.output .execute_result execution_count="31"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#331eec2e-873b-416f-b837-438dcdb23bda .cell .code execution_count="157" scrolled="true"}
``` python
team_ass = df.groupby("Club").Assists.sum().sort_values(ascending = False).to_frame()
title = "Club Ranking by Assists"
displaytext(title, team_ass)
```

::: {.output .display_data}
##### Club Ranking by Assists
:::

::: {.output .display_data}
```{=html}
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
      <th>Assists</th>
    </tr>
    <tr>
      <th>Club</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Manchester City</th>
      <td>55</td>
    </tr>
    <tr>
      <th>Manchester United</th>
      <td>51</td>
    </tr>
    <tr>
      <th>Tottenham Hotspur</th>
      <td>50</td>
    </tr>
    <tr>
      <th>West Ham United</th>
      <td>46</td>
    </tr>
    <tr>
      <th>Leeds United</th>
      <td>45</td>
    </tr>
    <tr>
      <th>Leicester City</th>
      <td>45</td>
    </tr>
    <tr>
      <th>Liverpool FC</th>
      <td>43</td>
    </tr>
    <tr>
      <th>Aston Villa</th>
      <td>38</td>
    </tr>
    <tr>
      <th>Arsenal</th>
      <td>38</td>
    </tr>
    <tr>
      <th>Chelsea</th>
      <td>38</td>
    </tr>
    <tr>
      <th>Southampton</th>
      <td>33</td>
    </tr>
    <tr>
      <th>Everton</th>
      <td>32</td>
    </tr>
    <tr>
      <th>Crystal Palace</th>
      <td>29</td>
    </tr>
    <tr>
      <th>Newcastle United</th>
      <td>26</td>
    </tr>
    <tr>
      <th>Brighton</th>
      <td>24</td>
    </tr>
    <tr>
      <th>Wolverhampton Wanderers</th>
      <td>21</td>
    </tr>
    <tr>
      <th>Burnley</th>
      <td>20</td>
    </tr>
    <tr>
      <th>West Bromwich Albion</th>
      <td>20</td>
    </tr>
    <tr>
      <th>Fulham</th>
      <td>18</td>
    </tr>
    <tr>
      <th>Sheffield United</th>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#e1de3fa0-11fe-44b3-b543-df3e6d409181 .cell .code execution_count="297"}
``` python
clubs_stat = df.groupby("Club").agg({"Goals": "sum", "Assists": "sum"}).reset_index()
```
:::

::: {#f50987ad-ae22-4d85-8525-efdf0794cb03 .cell .markdown}
#### Club Ranking by Assists
:::

::: {#0b48364c-d7f3-40fb-abd6-43e9e1e00b55 .cell .code execution_count="435"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

bubble_sizes = [ assists * 20 for assists in clubs_stat["Assists"]]
scatter = ax.scatter(clubs_stat["Club"], clubs_stat["Assists"], s=bubble_sizes, alpha=0.5, c=bubble_sizes, cmap='viridis', edgecolors='w', linewidth=0.5)

# labels and title
plt.ylabel('Assists')
plt.title('Club Ranking by Assists\n', fontsize=12)
plt.xticks(rotation=90)

for i, team in enumerate(clubs_stat["Club"]):
    y_offset = 15 if i % 2 == 0 else 25
    ax.annotate(team, (clubs_stat["Club"][i], clubs_stat["Assists"][i]), textcoords="offset points", xytext=(0, y_offset), ha='center', fontsize=8)

ax.set_xticks(range(len(clubs_stat["Club"])))
ax.set_xticklabels([])

plt.yticks([i * 10 for i in range(0, 7)])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/a5f325e2e1de1c57dc0daa844f789c9f48faaa96.png)
:::
:::

::: {#5a174bd5-18ae-4ee0-81c8-c2d1353f62c9 .cell .markdown}
#### Club Ranking by Goals
:::

::: {#ad82ce88-21bd-4ca9-9a5f-9c90946d3114 .cell .code execution_count="350"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

bubble_sizes = [goals * 20 for goals in clubs_stat["Goals"]]  
scatter = ax.scatter(clubs_stat["Club"], clubs_stat["Goals"], s=bubble_sizes, alpha=0.5, c=bubble_sizes, cmap='viridis', edgecolors='w', linewidth=0.5)

# labels and title
plt.ylabel('Goals')
plt.title('Club Ranking by Goals\n', fontsize=12)
plt.xticks(rotation=90)

for i, team in enumerate(clubs_stat["Club"]):
    y_offset = 15 if i % 2 == 0 else 25
    ax.annotate(team, (clubs_stat["Club"][i], clubs_stat["Goals"][i]), textcoords="offset points", xytext=(0, y_offset), ha='center', fontsize=8)

ax.set_xticks(range(len(clubs_stat["Club"])))
ax.set_xticklabels([])

plt.xlim(-1, len(clubs_stat["Club"]))

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/2d7462f5f6bd3c7fd99dbfeb12afa3fe1242ef23.png)
:::
:::

::: {#f9004aab-5bb2-432a-92ec-813c416837af .cell .markdown}
#### Players Ranking by Goals
:::

::: {#5497f354-51b5-4438-b80d-51e7c8734e5c .cell .code execution_count="298"}
``` python
players_stat = df.groupby("Name").agg({"Goals": "sum", "Assists": "sum"}).reset_index()
```
:::

::: {#02f0e1a4-b755-44b6-a9f8-7435564f965b .cell .code execution_count="606"}
``` python
top20goals_plyr = players_stat.nlargest(20, "Goals").reset_index()

fig, ax = plt.subplots(figsize=(16, 10))

bubble_sizes = [age * 20 for age in top20goals_plyr["Goals"]] 
scatter = ax.scatter(top20goals_plyr["Name"], top20goals_plyr["Goals"], s=bubble_sizes, alpha=0.5, c=bubble_sizes, cmap='viridis', edgecolors='w', linewidth=0.5)

# labels and title
plt.ylabel('Goals')
plt.title('Players Ranking by Goals - Top 20\n', fontsize=12)
plt.xticks(rotation=90)
plt

for i, team in enumerate(top20goals_plyr["Name"]):
    y_offset = 15 if i % 2 == 0 else 25
    ax.annotate(team, (top20goals_plyr["Name"][i], top20goals_plyr["Goals"][i]), textcoords="offset points", xytext=(0, y_offset), ha='center', fontsize=8)

ax.set_xticklabels([])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/92ac5e4887c360be69679f53b8fb91ba8bab329e.png)
:::
:::

::: {#f2af3448-e0e7-4ef4-894e-4376cc3ba591 .cell .markdown}
#### Players Ranking by Assists
:::

::: {#9b965a59-0fe4-41d2-9ad9-29f4d05e93dd .cell .code execution_count="608"}
``` python
top20assists_plyr = players_stat.nlargest(20, "Assists").reset_index()

fig, ax = plt.subplots(figsize=(16, 10))

bubble_sizes = [assist * 20 for assist in top20assists_plyr["Assists"]] 
scatter = ax.scatter(top20assists_plyr["Name"], top20assists_plyr["Assists"], s=bubble_sizes, alpha=0.5, c=bubble_sizes, cmap='viridis', edgecolors='w', linewidth=0.5)

# labels and title
plt.ylabel('Assists')
plt.title('Players Ranking by Assists - Top 20\n', fontsize=12)
plt.xticks(rotation=90)

for i, team in enumerate(top20assists_plyr["Name"]):
    y_offset = 15 if i % 2 == 0 else 25
    ax.annotate(team, (top20assists_plyr["Name"][i], top20assists_plyr["Assists"][i]), textcoords="offset points", xytext=(0, y_offset), ha='center', fontsize=8)

ax.set_xticklabels([])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/8517ea354a112a4436349772bf6587f32fe5a0db.png)
:::
:::

::: {#ac55f4a7-0c3f-4835-a9dd-5abc889ff118 .cell .markdown}
#### Scatter Plot of Goal Contribution (Goals + Assists) - Top 20 {#scatter-plot-of-goal-contribution-goals--assists---top-20}
:::

::: {#9753680b-5c7f-4615-a88e-c9c8bd19e09d .cell .code execution_count="315"}
``` python
goal_contribution = df.groupby("Name").agg({"Goals": "sum", "Assists": "sum"}).reset_index()
goal_contribution["Goals Contribution"] = goal_contribution.Goals + goal_contribution.Assists
goal_contribution_20 = goal_contribution.nlargest(20, "Goals Contribution").reset_index()
```
:::

::: {#506b0773-61a5-439a-8b06-e5c7778e9e9a .cell .code execution_count="357"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

sns.scatterplot(x='Goals', y='Assists', data=goal_contribution_20, ax=ax, s=100, color='b', edgecolor='w', linewidth=0.5)

for i, player in enumerate(goal_contribution_20['Name']):
    y_offset = 5 if i % 2 == 0 else 10
    ax.annotate(player, (goal_contribution_20['Goals'][i], goal_contribution_20['Assists'][i]), textcoords="offset points", xytext=(5,y_offset), ha='center', fontsize=8)

# labels and title
plt.xlabel('Goals')
plt.ylabel('Assists')
plt.title('Goals Contribution - Top 20')
plt.xticks(range(5, 26))
plt.yticks(range(0, 16))

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/05f9d32e3ee7c7ca94d486b1994c9113db2946df.png)
:::
:::

::: {#a1e2fad1-c9bf-42ac-81ad-9198b994ed87 .cell .markdown}
#### Scatter Plot of Goal Contribution U20 - Top 10
:::

::: {#b8a1d84d-86b1-4615-93df-a985bed27c4a .cell .code execution_count="388"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

sns.scatterplot(x="Goals", y="Assists", data=under20stats_top10, color="b", ax=ax, s=100, edgecolor='w', linewidth=0.5)

for i, player in enumerate(under20stats_top10["Name"]):
    y_offset = 5 if i % 2 == 0 else 20
    ax.annotate(player, (under20stats_top10["Goals"][i], under20stats_top10["Assists"][i]), textcoords="offset points", xytext=(5,y_offset), ha="center")

plt.xlabel("Goals", fontsize=15)
plt.ylabel("Assists", fontsize=15)
plt.title("U20 Goal Contribution - Top 10", fontsize=15)
plt.xticks([i * 1 for i in range(0,12)])
plt.yticks([i * 1 for i in range(0,10)])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/0f1c1e91038e80d66c3fa48d4884546fe3d64298.png)
:::
:::

::: {#43ae1b83-e267-47f9-9d33-de970ef7ff09 .cell .markdown}
#### Goals per Match for Top 10 Goal Scorers
:::

::: {#1c57b1c5-a6c5-44fc-bf6a-df3155b406f4 .cell .code execution_count="525"}
``` python
top_20_goalspermatch = df[["Name", "GoalsPerMatch", "Goals", "Matches"]].nlargest(n=20, columns="Goals").reset_index()
```
:::

::: {#a7137511 .cell .code execution_count="617"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

sns.scatterplot(x="Goals", y="Matches", data=top_20_goalspermatch, color="b", ax=ax, s=100, edgecolor='w', linewidth=0.5)

for i, player in enumerate(top_20_goalspermatch["Name"]):
    y_offset = 2 if i % 2 == 0 else 8
    ax.annotate(player, (top_20_goalspermatch["Goals"][i], top_20_goalspermatch["Matches"][i]), textcoords="offset points", xytext=(5,y_offset), ha="center", fontsize=9)

plt.xlabel("Goals", fontsize=15)
plt.ylabel("Matches Played", fontsize=15)
plt.title("Goals per Match - Top 10 Goal Scorers", fontsize=15)
plt.xticks([i * 1 for i in range(8,26)])
plt.yticks([i * 2 for i in range(8,22)])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/af88b24266e2c92c47a9e7e241e649b18565a5a6.png)
:::
:::

::: {#6c1873e9-e74c-4bb2-93f5-d22f9addefa0 .cell .code execution_count="590"}
``` python
df.head(2)
```

::: {.output .execute_result execution_count="590"}
```{=html}
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
      <th>Name</th>
      <th>Club</th>
      <th>Nationality</th>
      <th>Position</th>
      <th>Age</th>
      <th>Matches</th>
      <th>Starts</th>
      <th>Mins</th>
      <th>Goals</th>
      <th>Assists</th>
      <th>Passes_Attempted</th>
      <th>Perc_Passes_Completed</th>
      <th>Penalty_Goals</th>
      <th>Penalty_Attempted</th>
      <th>xG</th>
      <th>xA</th>
      <th>Yellow_Cards</th>
      <th>Red_Cards</th>
      <th>MinutesPerMatch</th>
      <th>GoalsPerMatch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mason Mount</td>
      <td>Chelsea</td>
      <td>ENG</td>
      <td>MF,FW</td>
      <td>21</td>
      <td>36</td>
      <td>32</td>
      <td>2890</td>
      <td>6</td>
      <td>5</td>
      <td>1881</td>
      <td>82.3</td>
      <td>1</td>
      <td>1</td>
      <td>0.21</td>
      <td>0.24</td>
      <td>2</td>
      <td>0</td>
      <td>80</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Edouard Mendy</td>
      <td>Chelsea</td>
      <td>SEN</td>
      <td>GK</td>
      <td>28</td>
      <td>31</td>
      <td>31</td>
      <td>2745</td>
      <td>0</td>
      <td>0</td>
      <td>1007</td>
      <td>84.6</td>
      <td>0</td>
      <td>0</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2</td>
      <td>0</td>
      <td>88</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {#f1bf1367-f1a6-4c28-afa5-ac9b199c31e7 .cell .markdown}
#### xG of Top 20 Goal Scorers
:::

::: {#00aa2cd7-b658-44ae-a1ff-a5427991ec26 .cell .code execution_count="598"}
``` python
top_20_xGoals = df[["Name", "xG", "Goals"]].nlargest(n=20, columns="Goals").reset_index()
```
:::

::: {#f29adc40-6d91-4750-8e21-060303812b35 .cell .code execution_count="599"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

sns.scatterplot(x="Goals", y="xG", data=top_20_xGoals, color="b", ax=ax, s=100, edgecolor='w', linewidth=0.5)

for i, player in enumerate(top_20_xGoals["Name"]):
    y_offset = 2 if i % 2 == 0 else 8
    ax.annotate(player, (top_20_xGoals["Goals"][i], top_20_xGoals["xG"][i]), textcoords="offset points", xytext=(5,y_offset), ha="center", fontsize=9)

plt.xlabel("Goals", fontsize=15)
plt.ylabel("xG", fontsize=15)
plt.title("xG of Top 20 Goal Scorers", fontsize=15)
plt.xticks([i * 1 for i in range(8,26)])
# plt.yticks([i * 1 for i in range(8,22)])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/27468bbfd5d870efecedd2b5470b0f617120c6d2.png)
:::
:::

::: {#428ac75d-c274-4554-b9b0-a915e62b7cb2 .cell .markdown}
#### xA of Top 20 Goal Creators
:::

::: {#4cdf133f-8cf1-4f14-9620-18d35e36ce7f .cell .code execution_count="604"}
``` python
top_20_xAssists = df[["Name", "xA", "Assists"]].nlargest(n=20, columns="Assists").reset_index()
```
:::

::: {#9322d786-cce7-4447-ba88-e2490a68dd5d .cell .code execution_count="636"}
``` python
fig, ax = plt.subplots(figsize=(16, 10))

sns.scatterplot(x="Assists", y="xA", data=top_20_xAssists, color="b", ax=ax, s=100, edgecolor='w', linewidth=0.5)

for i, player in enumerate(top_20_xAssists["Name"]):
    y_offset = 1 if i % 2 == 0 else 8
    ax.annotate(player, (top_20_xAssists["Assists"][i], top_20_xAssists["xA"][i]), textcoords="offset points", xytext=(5,y_offset), ha="center", fontsize=9)


plt.xlabel("Assists", fontsize=15)
plt.ylabel("xA", fontsize=15)
plt.title("xA of Top 20 Goal Creators", fontsize=15)
plt.xticks([i * 1 for i in range(5,16)])
plt.yticks([i * 0.02 for i in range(3, 25)])

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/69bbd1af7520c384efea78981b04e36af2e840a7.png)
:::
:::

::: {#fa455c33-f358-472c-8d17-456799dcc861 .cell .markdown}
#### Pie Chart of Yellow Cards vs Red Cards
:::

::: {#9b575ff1-868c-45e9-8613-ef8469f33cc3 .cell .code execution_count="611"}
``` python
plt.figure(figsize=(14,7))

colors = ['yellow', 'red']
sizes = [yellowcards, redcards]

yellowcards = df.Yellow_Cards.sum()
redcards = df.Red_Cards.sum()

data = [yellowcards, redcards]
label = ["Yellow Cards", "Red Cards"]
color = sns.color_palette("Set1")

def autopct_format(values):
    def my_format(pct):
        total = sum(values)
        val = int(round(pct*total/100.0))
        return f'{val} ({pct:.1f}%)'
    return my_format

plt.pie(data, labels = label, colors = colors, autopct = autopct_format(sizes))
plt.title("Pie Chart of Yellow Cards vs Red Cards")
plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/079b07679b0d2826f4ede77459cd230c0db761d2.png)
:::
:::

::: {#1a3d4cbb-24c2-4a1f-bfc6-b2be417139be .cell .markdown}
#### Red and Yellow Cards by Clubs
:::

::: {#597c4e57-6638-43e0-b9bf-02b274478a83 .cell .code execution_count="526"}
``` python
import matplotlib.pyplot as plt
import numpy as np

cards = df.groupby("Club").agg({"Yellow_Cards": "sum", "Red_Cards": "sum"}).reset_index()
# Define the bar width and positions
bar_width = 0.35
index = np.arange(len(cards.Club))

# Create the clustered bar chart
fig, ax = plt.subplots()

bar1 = ax.bar(index, cards.Yellow_Cards, bar_width, label='Yellow Cards', color='yellow')
bar2 = ax.bar(index + bar_width, cards.Red_Cards, bar_width, label='Red Cards', color='red')

# labels, title, and legend
ax.set_xlabel('Club', fontsize=15)
ax.set_ylabel('Number of Cards', fontsize=15)
ax.set_title('Total Red and Yellow Cards by Clubs', fontsize=15)
ax.set_xticks(index + bar_width / 2)
ax.set_xticklabels(cards.Club)
ax.legend()

plt.xticks(rotation=45)

plt.show()
```

::: {.output .display_data}
![](vertopal_fad71fda17a84a1093d104d87eb1d2bc/b88c44524a6983939a7e535d5742e1ccaa2a665c.png)
:::
:::

::: {#04fb7a60-e6b6-448a-824c-c2924a204492 .cell .markdown}
### Conclusion

*This analysis of the English Premier League 20/21 season provides a
comprehensive understanding of the key performance metrics and trends
that shaped the competition. By examining the goals scored, how it was
scored, the Expected Goals(xG), assists and Expected Assists, we have
identified the standout players and teams that made significant impacts.
This season was characterized by its intense competition and the
remarkable consistency of top-performing teams.*
:::
