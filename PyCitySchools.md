

```python
# dependencies
import pandas as pd
import numpy as np
import os
```


```python
#load school csv
cityschools = os.path.join("schools_complete.csv")
```


```python
#read cityschools with pandas
cityschools_df = pd.read_csv(cityschools)

```


```python
total_budget = cityschools_df['budget'].sum()

```


```python
#load student csv
citystudents = os.path.join("students_complete.csv")
```


```python
#read citystudents with pandas
citystudents_df = pd.read_csv(citystudents)

```


```python
#change the school column header in cityschools
cityschools_df = cityschools_df.rename(columns={"name": "school"})
#cityschools_df.head()
```


```python
#merget tables
schoolsdata_df = pd.merge(citystudents_df, cityschools_df, on="school")
#schoolsdata_df.head()
```


```python
#**District Summary**

# * Create a high level snapshot (in table form) of the district's key metrics, including:
#   * Total Schools
total_schools = schoolsdata_df['school'].nunique()
#print(total_schools)
```


```python
#   * Total Students
total_students=len(schoolsdata_df.axes[0])
#print(total_students)
```


```python
#   * Average Math Score
district_average_math = schoolsdata_df['math_score'].mean()
#print(district_average_math)
```


```python
#   * Average Reading Score
district_average_reading = schoolsdata_df['reading_score'].mean()
#print(district_average_reading)
```


```python
#  * % Passing Math
bins = [1, 70, 100]
score = [False, True]
```


```python
pd.cut(schoolsdata_df['math_score'], bins, labels=score)
schoolsdata_df['passing_math'] = pd.cut(schoolsdata_df['math_score'], bins, labels=score)
#schoolsdata_df.head()
```


```python
schoolsdata_df['passing_math'] = schoolsdata_df['passing_math'] == True
mpass_district = schoolsdata_df['passing_math'].sum()
#print(mpass_district)                 
```


```python
pmpass_district = mpass_district / total_students * 100
#print(pmpass_district)
```


```python
#   * % Passing Reading
Bins = [1, 70, 100]
Score = [False, True]
```


```python
pd.cut(schoolsdata_df['reading_score'], bins, labels=score)
schoolsdata_df['passing_reading'] = pd.cut(schoolsdata_df['reading_score'], bins, labels=score)
#schoolsdata_df.head()
```


```python
schoolsdata_df['passing_reading'] = schoolsdata_df['passing_reading'] == True
rpass_district = schoolsdata_df['passing_reading'].sum()
#print(rpass_district)
```


```python
prpass_district = rpass_district / total_students * 100
#print(prpass_district)
```


```python
#   * Overall Passing Rate (Average of the above two)
district_passing = pmpass_district / prpass_district * 100
#print(district_passing)
```


```python
district_summary = pd.DataFrame({'Total Schools': [total_schools],
                                 'Total Students': [total_students], 
                                 'Total Budget': [total_budget], 
                                 'Average Math Score': [district_average_math],
                                 'Average Reading Score': [district_average_reading],
                                 '% Passing Math': [pmpass_district],
                                 '% Passing Reading': [prpass_district],
                                 '% Overall Passing Rate': [district_passing]
})
district_summary = district_summary[['Total Schools',
                                    'Total Students',
                                    'Total Budget',
                                    'Average Math Score',
                                    'Average Reading Score',
                                    '% Passing Math',
                                    '% Passing Reading',
                                    '% Overall Passing Rate']]
district_summary = district_summary.round(2)
```


```python
schooltype_df = (schoolsdata_df['type']. groupby(schoolsdata_df['school'])).describe()
```


```python
schooltype_df.reset_index(inplace=True)
```


```python
schooltype_df = schooltype_df.rename(columns={"count": "Total Students", "top": "School Type"})
```


```python
#   * Total School Budget
schoolbudget_df = (schoolsdata_df['budget']. groupby(schoolsdata_df['school'])).describe()
```


```python
schoolbudget_df.reset_index(inplace=True)
```


```python
df1_df = pd.merge(schooltype_df, schoolbudget_df, on='school') 
```


```python
df1_df.reset_index(inplace=False)
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
      <th>index</th>
      <th>school</th>
      <th>Total Students</th>
      <th>unique</th>
      <th>School Type</th>
      <th>freq</th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Bailey High School</td>
      <td>4976</td>
      <td>1</td>
      <td>District</td>
      <td>4976</td>
      <td>4976.0</td>
      <td>3124928.0</td>
      <td>0.0</td>
      <td>3124928.0</td>
      <td>3124928.0</td>
      <td>3124928.0</td>
      <td>3124928.0</td>
      <td>3124928.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Cabrera High School</td>
      <td>1858</td>
      <td>1</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1858.0</td>
      <td>1081356.0</td>
      <td>0.0</td>
      <td>1081356.0</td>
      <td>1081356.0</td>
      <td>1081356.0</td>
      <td>1081356.0</td>
      <td>1081356.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Figueroa High School</td>
      <td>2949</td>
      <td>1</td>
      <td>District</td>
      <td>2949</td>
      <td>2949.0</td>
      <td>1884411.0</td>
      <td>0.0</td>
      <td>1884411.0</td>
      <td>1884411.0</td>
      <td>1884411.0</td>
      <td>1884411.0</td>
      <td>1884411.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Ford High School</td>
      <td>2739</td>
      <td>1</td>
      <td>District</td>
      <td>2739</td>
      <td>2739.0</td>
      <td>1763916.0</td>
      <td>0.0</td>
      <td>1763916.0</td>
      <td>1763916.0</td>
      <td>1763916.0</td>
      <td>1763916.0</td>
      <td>1763916.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>1468</td>
      <td>1</td>
      <td>Charter</td>
      <td>1468</td>
      <td>1468.0</td>
      <td>917500.0</td>
      <td>0.0</td>
      <td>917500.0</td>
      <td>917500.0</td>
      <td>917500.0</td>
      <td>917500.0</td>
      <td>917500.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Hernandez High School</td>
      <td>4635</td>
      <td>1</td>
      <td>District</td>
      <td>4635</td>
      <td>4635.0</td>
      <td>3022020.0</td>
      <td>0.0</td>
      <td>3022020.0</td>
      <td>3022020.0</td>
      <td>3022020.0</td>
      <td>3022020.0</td>
      <td>3022020.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Holden High School</td>
      <td>427</td>
      <td>1</td>
      <td>Charter</td>
      <td>427</td>
      <td>427.0</td>
      <td>248087.0</td>
      <td>0.0</td>
      <td>248087.0</td>
      <td>248087.0</td>
      <td>248087.0</td>
      <td>248087.0</td>
      <td>248087.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Huang High School</td>
      <td>2917</td>
      <td>1</td>
      <td>District</td>
      <td>2917</td>
      <td>2917.0</td>
      <td>1910635.0</td>
      <td>0.0</td>
      <td>1910635.0</td>
      <td>1910635.0</td>
      <td>1910635.0</td>
      <td>1910635.0</td>
      <td>1910635.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Johnson High School</td>
      <td>4761</td>
      <td>1</td>
      <td>District</td>
      <td>4761</td>
      <td>4761.0</td>
      <td>3094650.0</td>
      <td>0.0</td>
      <td>3094650.0</td>
      <td>3094650.0</td>
      <td>3094650.0</td>
      <td>3094650.0</td>
      <td>3094650.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>962</td>
      <td>1</td>
      <td>Charter</td>
      <td>962</td>
      <td>962.0</td>
      <td>585858.0</td>
      <td>0.0</td>
      <td>585858.0</td>
      <td>585858.0</td>
      <td>585858.0</td>
      <td>585858.0</td>
      <td>585858.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Rodriguez High School</td>
      <td>3999</td>
      <td>1</td>
      <td>District</td>
      <td>3999</td>
      <td>3999.0</td>
      <td>2547363.0</td>
      <td>0.0</td>
      <td>2547363.0</td>
      <td>2547363.0</td>
      <td>2547363.0</td>
      <td>2547363.0</td>
      <td>2547363.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Shelton High School</td>
      <td>1761</td>
      <td>1</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1761.0</td>
      <td>1056600.0</td>
      <td>0.0</td>
      <td>1056600.0</td>
      <td>1056600.0</td>
      <td>1056600.0</td>
      <td>1056600.0</td>
      <td>1056600.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Thomas High School</td>
      <td>1635</td>
      <td>1</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1635.0</td>
      <td>1043130.0</td>
      <td>0.0</td>
      <td>1043130.0</td>
      <td>1043130.0</td>
      <td>1043130.0</td>
      <td>1043130.0</td>
      <td>1043130.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Wilson High School</td>
      <td>2283</td>
      <td>1</td>
      <td>Charter</td>
      <td>2283</td>
      <td>2283.0</td>
      <td>1319574.0</td>
      <td>0.0</td>
      <td>1319574.0</td>
      <td>1319574.0</td>
      <td>1319574.0</td>
      <td>1319574.0</td>
      <td>1319574.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Wright High School</td>
      <td>1800</td>
      <td>1</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1800.0</td>
      <td>1049400.0</td>
      <td>0.0</td>
      <td>1049400.0</td>
      <td>1049400.0</td>
      <td>1049400.0</td>
      <td>1049400.0</td>
      <td>1049400.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#   * % Passing Math
schoolmathpass_df = (schoolsdata_df['passing_math']. groupby(schoolsdata_df['school'])).mean().to_frame()
schoolmathpass_df.reset_index(inplace=True)
#schoolmathpass_df.head()
```


```python
df2_df = pd.merge(df1_df, schoolmathpass_df, on="school")
```


```python
#   * % Passing Reading
schoolreadpass_df = (schoolsdata_df['passing_reading']. groupby(schoolsdata_df['school'])).mean().to_frame()
schoolreadpass_df.reset_index(inplace=True)
#schoolreadpass_df.head() 
```


```python
schoolsum_df = pd.merge(df2_df, schoolreadpass_df, on='school')
#schoolsum_df.head()
```


```python
schoolsummary_df = schoolsum_df[["school", "School Type", "Total Students", "mean", "passing_math", "passing_reading"]]
```


```python
#   * Average Math Score
schoolmathavg = (schoolsdata_df['math_score']. groupby(schoolsdata_df['school'])).mean().to_frame()
schoolmathavg.reset_index(inplace=True)
```


```python
#   * Average Reading Score
schoolreadingavg = (schoolsdata_df['reading_score']. groupby(schoolsdata_df['school'])).mean().to_frame()
schoolreadingavg.reset_index(inplace=True)
```


```python
averagescores_df = pd.merge(schoolmathavg, schoolreadingavg, on="school")
```


```python
school_summary_df = pd.merge(schoolsummary_df, averagescores_df, on="school")

```


```python
#   * Per student Budget
school_summary_df["per_student_budget"] = school_summary_df["mean"]/school_summary_df["Total Students"]

```


```python
#   * Overall Passing Rate (Average of the above two)
school_summary_df["% Overall Passing Rate"] = (school_summary_df["passing_math"] + school_summary_df["passing_reading"])/2

```


```python
school_summary_df["per_student_budget"] = school_summary_df.per_student_budget.astype(float)
```


```python
school_summary_df = school_summary_df.rename(columns={"mean": "Total School Budget", "passing_math": "% Passing Math", "passing_reading": "% Passing Reading", "math_score": "Average Math Score", "reading_score": "Average Reading Score", "per_student_budget": "Per Student Budget"})
school_summary_df = school_summary_df[["school", "School Type", "Total Students", "Total School Budget", "Per Student Budget", "Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
```


```python
# **Top Performing Schools (By Passing Rate)**

# * Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
#   * School Name
#   * School Type
#   * Total Students
#   * Total School Budget
#   * Per School Budget
#   * Average Math Score
#   * Average Reading Score
#   * % Passing Math
#   * % Passing Reading
#   * Overall Passing Rate (Average of the above two)

Top_performing = school_summary_df.sort_values(by='% Overall Passing Rate', ascending=False)

```


```python
Bottom_performing = school_summary_df.sort_values(by='% Overall Passing Rate', ascending=True)
```


```python
# **Math Scores by Grade**

# * Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.
schoolmathscores = schoolsdata_df[["school", "grade", "math_score"]]

```


```python
# **Reading Scores by Grade**

# * Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.
schoolreadscores = schoolsdata_df[["school", "grade", "reading_score"]]

```


```python
# **Scores by School Spending**

# * Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
#   * Average Math Score
#   * Average Reading Score
#   * % Passing Math
#   * % Passing Reading
#   * Overall Passing Rate (Average of the above two)
#print(school_summary_df["Per Student Budget"].max())
#print(school_summary_df["Per Student Budget"].min())
bins = [0,580,615,635,660]
group_labels = ["<$580","$580-$615","$615-$635","$635-$660"]
```


```python
school_summary_df["Spending Range (Per Student)"] = pd.cut(school_summary_df["Per Student Budget"], bins, labels=group_labels) 

```


```python
spending_range = school_summary_df.groupby("Spending Range (Per Student)")
spending_range = spending_range[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
```


```python
size_bins = [0,1000,2000,5000]
size_labels = ["Small", "Medium", "Large"]
pd.cut(school_summary_df["Total Students"], size_bins, labels=size_labels).head()
```




    0     Large
    1    Medium
    2     Large
    3     Large
    4    Medium
    Name: Total Students, dtype: category
    Categories (3, object): [Small < Medium < Large]




```python
school_summary_df["School Size"] = pd.cut(school_summary_df["Total Students"], size_bins, labels=size_labels) 

```


```python
by_school = school_summary_df.groupby("School Size")
by_school = by_school[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
```


```python
# **Scores by School Type**

# * Repeat the above breakdown, but this time group schools based on school type (Charter vs. District).
by_type = school_summary_df.groupby("School Type")
by_type = by_type[["Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
```


```python
school_summary = school_summary_df[["school", "School Type", "Total Students", "Total School Budget", "Per Student Budget", "Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]
```


```python
school_summary['Total School Budget'] = school_summary_df['Total School Budget'].map("${:,.0f}".format)
school_summary['Per Student Budget'] = school_summary_df['Per Student Budget'].map("${:,.0f}".format)
pd.options.mode.chained_assignment = None
```


```python
district_summary
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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>72.39</td>
      <td>82.97</td>
      <td>87.25</td>
    </tr>
  </tbody>
</table>
</div>




```python
school_summary
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
      <th>school</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>$3,124,928</td>
      <td>$628</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>0.646302</td>
      <td>0.793006</td>
      <td>0.719654</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356</td>
      <td>$582</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>0.895587</td>
      <td>0.938644</td>
      <td>0.917115</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411</td>
      <td>$639</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>0.637504</td>
      <td>0.784334</td>
      <td>0.710919</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>$1,763,916</td>
      <td>$644</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>0.657539</td>
      <td>0.775100</td>
      <td>0.716320</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>$917,500</td>
      <td>$625</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>0.897139</td>
      <td>0.933924</td>
      <td>0.915531</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>$3,022,020</td>
      <td>$652</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>0.647465</td>
      <td>0.781877</td>
      <td>0.714671</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087</td>
      <td>$581</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>0.906323</td>
      <td>0.927400</td>
      <td>0.916862</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635</td>
      <td>$655</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>0.633185</td>
      <td>0.788138</td>
      <td>0.710662</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650</td>
      <td>$650</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>0.638521</td>
      <td>0.782819</td>
      <td>0.710670</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858</td>
      <td>$609</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>0.916840</td>
      <td>0.922037</td>
      <td>0.919439</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363</td>
      <td>$637</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>0.640660</td>
      <td>0.777444</td>
      <td>0.709052</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>$1,056,600</td>
      <td>$600</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>0.898921</td>
      <td>0.926178</td>
      <td>0.912550</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,043,130</td>
      <td>$638</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>0.902141</td>
      <td>0.929052</td>
      <td>0.915596</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574</td>
      <td>$578</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>0.909330</td>
      <td>0.932545</td>
      <td>0.920937</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,049,400</td>
      <td>$583</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>0.902778</td>
      <td>0.934444</td>
      <td>0.918611</td>
    </tr>
  </tbody>
</table>
</div>




```python
Top_performing.head()
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
      <th>school</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574.0</td>
      <td>578.0</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>0.909330</td>
      <td>0.932545</td>
      <td>0.920937</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858.0</td>
      <td>609.0</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>0.916840</td>
      <td>0.922037</td>
      <td>0.919439</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400.0</td>
      <td>583.0</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>0.902778</td>
      <td>0.934444</td>
      <td>0.918611</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356.0</td>
      <td>582.0</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>0.895587</td>
      <td>0.938644</td>
      <td>0.917115</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087.0</td>
      <td>581.0</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>0.906323</td>
      <td>0.927400</td>
      <td>0.916862</td>
    </tr>
  </tbody>
</table>
</div>




```python
Bottom_performing.head()
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
      <th>school</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363.0</td>
      <td>637.0</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>0.640660</td>
      <td>0.777444</td>
      <td>0.709052</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635.0</td>
      <td>655.0</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>0.633185</td>
      <td>0.788138</td>
      <td>0.710662</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650.0</td>
      <td>650.0</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>0.638521</td>
      <td>0.782819</td>
      <td>0.710670</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411.0</td>
      <td>639.0</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>0.637504</td>
      <td>0.784334</td>
      <td>0.710919</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020.0</td>
      <td>652.0</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>0.647465</td>
      <td>0.781877</td>
      <td>0.714671</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.pivot_table(schoolmathscores, index= "school", columns= "grade", aggfunc="mean")
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
    <tr>
      <th></th>
      <th colspan="4" halign="left">math_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.pivot_table(schoolreadscores, index= "school", columns= "grade", aggfunc="mean")
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
    <tr>
      <th></th>
      <th colspan="4" halign="left">reading_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>




```python
spending_range.mean()
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Range (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$580</th>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>0.909330</td>
      <td>0.932545</td>
      <td>0.920937</td>
    </tr>
    <tr>
      <th>$580-$615</th>
      <td>83.549353</td>
      <td>83.903238</td>
      <td>0.904090</td>
      <td>0.929741</td>
      <td>0.916915</td>
    </tr>
    <tr>
      <th>$615-$635</th>
      <td>80.199966</td>
      <td>82.425360</td>
      <td>0.771721</td>
      <td>0.863465</td>
      <td>0.817593</td>
    </tr>
    <tr>
      <th>$635-$660</th>
      <td>77.866721</td>
      <td>81.368774</td>
      <td>0.679574</td>
      <td>0.802681</td>
      <td>0.741127</td>
    </tr>
  </tbody>
</table>
</div>




```python
by_school.mean()
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small</th>
      <td>83.821598</td>
      <td>83.929843</td>
      <td>0.911582</td>
      <td>0.924719</td>
      <td>0.918150</td>
    </tr>
    <tr>
      <th>Medium</th>
      <td>83.374684</td>
      <td>83.864438</td>
      <td>0.899313</td>
      <td>0.932448</td>
      <td>0.915881</td>
    </tr>
    <tr>
      <th>Large</th>
      <td>77.746417</td>
      <td>81.344493</td>
      <td>0.676313</td>
      <td>0.801908</td>
      <td>0.739111</td>
    </tr>
  </tbody>
</table>
</div>




```python
by_type.mean()
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>0.903632</td>
      <td>0.930528</td>
      <td>0.917080</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>0.643025</td>
      <td>0.783246</td>
      <td>0.713135</td>
    </tr>
  </tbody>
</table>
</div>




```python
# As final considerations:

# * Your script must work for both data-sets given.
# * You must use the Pandas Library and the Jupyter Notebook.
# * You must submit a link to your Jupyter Notebook with the viewable Data Frames. 
# * You must include an exported markdown version of your Notebook called  `README.md` in your GitHub repository.  
# * You must include a written description of three observable trends based on the data. 
# * See [Example Solution](PyCitySchools/PyCitySchools_Example.pdf) for a reference on the expected format. 
```


```python
# ## Hints and Considerations

# * These are challenging activities for a number of reasons. For one, these activities will require you to analyze thousands of records. Hacking through the data to look for obvious trends in Excel is just not a feasible option. The size of the data may seem daunting, but Python Pandas will allow you to efficiently parse through it. 

# * Second, these activities will also challenge you by requiring you to learn on your feet. Don't fool yourself into thinking: "I need to study Pandas more closely before diving in." Get the basic gist of the library and then _immediately_ get to work. When facing a daunting task, it's easy to think: "I'm just not ready to tackle it yet." But that's the surest way to never succeed. Learning to program requires one to constantly tinker, experiment, and learn on the fly. You are doing exactly the _right_ thing, if you find yourself constantly practicing Google-Fu and diving into documentation. There is just no way (or reason) to try and memorize it all. Online references are available for you to use when you need them. So use them!

# * Take each of these tasks one at a time. Begin your work, answering the basic questions: "How do I import the data?" "How do I convert the data into a DataFrame?" "How do I build the first table?" Don't get intimidated by the number of asks. Many of them are repetitive in nature with just a few tweaks. Be persistent and creative!

# * Expect these exercises to take time! Don't get discouraged if you find yourself spending  hours initially with little progress. Force yourself to deal with the discomfort of not knowing and forge ahead. This exercise is likely to take between 15-30 hours of your time. Consider these hours an investment in your future!

# * As always, feel encouraged to work in groups and get help from your TAs and Instructor. Just remember, true success comes from mastery and _not_ a completed homework assignment. So challenge yourself to truly succeed!
```
