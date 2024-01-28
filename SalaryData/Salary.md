
```{python}
#| include: false
import pandas as pd
import altair as alt
import numpy as np

from IPython.display import Markdown
from IPython.display import display
from tabulate import tabulate


```


```{python}
#| include: false
#Importing Data From Github
#Data Download Link: https://www.kaggle.com/datasets/mohithsairamreddy/salary-data?resource=download

url = "https://raw.githubusercontent.com/loganray48/Personal-Portfolio/main/SalaryData/salarydata.csv"

full_df = pd.read_csv(url)
```

```{python}
#| include: false
#Dropping Null for education level
full_df = full_df.dropna(subset=['Education Level'])

#Fixing Data Issues
#Fixing Masters Degree
full_df['Education Level'] = full_df['Education Level'].apply(lambda x: "Master's Degree" if x.lower() == "master's" else x)
#Fixing Bachelor's degree data
full_df['Education Level'] = full_df['Education Level'].apply(lambda x: "Bachelor's Degree" if x.lower() == "bachelor's" else x)
#fixing PHD
full_df['Education Level'] = full_df['Education Level'].apply(lambda x: "PhD" if x.lower() == "phd" else x)

#To much data so used sample of 800 
df = full_df.sample(n=800)

```

## US Salary Data

I explored a dataset sourced from Kaggle named Salary_Data by the author Sai Reddy, I aimed to uncover valuable insights by examining relationships between various variables. Specifically, I delved into the connections between age and salary. Furthermore, I conducted an analysis of the impact of education levels and what the averages education level is. Additionally, I investigated the dynamics between years of experience and salary, carefully considering how these relationships manifested across distinct education levels. Through this exploration, I sought to unveil meaningful trends and patterns within the data, contributing to a richer understanding of the intricate connections between these essential variables.


### Age vs Salary

```{python}

#Scatter chart showing Age and Salary
chart = alt.Chart(df).mark_circle(size=30).encode(
    x='Age:Q',
    y='Salary:Q',
    tooltip=['Age', 'Salary']
).properties(
    title='Age vs Salary',
    width=600,
    height=400
)

chart
```

This illuminating scatter plot serves as a visual testament to the compelling relationship between age and salary. A discernable positive correlation is evident, suggesting that, in most scenarios, as individuals advance in age, their salaries tend to show an upward trajectory. A noteworthy observation surfaces, indicating that individuals aged 40 and beyond often command significantly higher earnings compared to those in the earlier stages of their careers.

### Education Level Distribution

```{python}
edu_df = full_df.sample(n=4000)

# Define the custom sort order for the 'Education Level' column
custom_sort_order = ['High School', "Bachelor's Degree", "Master's Degree", 'PhD']

# Bar chart for Education Level distribution
education_chart = alt.Chart(edu_df).mark_bar().encode(
    x='count()',
     y=alt.Y('Education Level:N', title='Education Level', sort=custom_sort_order),
    color='Education Level:N',
).properties(
    width=600,
    height=200
)

education_chart
```

This informative bar chart presents a randomly sampled subset of 4000 individuals from the dataset, showcasing their educational attainment. The predominant trend reveals that a substantial majority hold a bachelor's degree, with master's and Ph.D. degrees following closely. Notably, individuals with high school-level education represent the smallest proportion in this diverse sample.

### Years of Experience vs Salary

```{python}

# Calculate mean salary for each combination of Education Level and Years of Experience
mean_line = alt.Chart(edu_df).transform_aggregate(
    mean_salary='mean(Salary)',
    groupby=['Education Level', 'Years of Experience']
).mark_line().encode(
    x=alt.X('Years of Experience', title='Years of Experience'),  # Hide x-axis labels
    y=alt.Y('mean_salary:Q', title='Average Salary'),  # Hide y-axis labels
    color='Education Level:N',
    size=alt.value(3)  # Adjust line thickness as needed
)


# Combine scatter plot and mean lines
mean_line
```

This insightful chart illustrates the average salary variation based on individuals' educational levels. Across the four distinct educational tiers, a consistent upward trend emerges as years of experience increase. On average, those with a high school education tend to earn the least, while the highest average salaries alternate among individuals with different educational backgrounds.
