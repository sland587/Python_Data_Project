# The Analysis

## What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles.  This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here:
[2_Skills_Demand.ipynb](3_Project/2_Skills_Demand.ipynb)

### Visualize Data

``` python
import matplotlib.ticker as mtick

fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
    
    # use seaborn instead of matplotlib
    # df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=job_title)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,78)
    ax[i].xaxis.set_major_formatter(mtick.PercentFormatter())

    # Inner loop to display percentage text for each bar
    for n, v in enumerate(df_plot['skill_percent']):
        # On the current axis, for each value(v) in index(n) print the value as a percentage
        # text(x_value, y_value, text to print, va=vertical alignment)
        # v + 1 to push label to right so it's not right on end of bar
        # 
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')
    
    # Turn off x scale for everything but the last subplot
    if(i != len(job_titles) - 1):
         ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()
```
### Results
![Visualization of Top Skills for Data Nerds](3_Project/images/skill_demand_all_data_roles.png)

### Insights
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).
- SQL is the most requested skill for Data Analysts and Data Scientist, with it in over half the job postings for both roles.  For Data Engineers, SQL is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data
``` python
# Plot the Top 5 Skills
num_skills = len(df_plot.columns)
num_months = len(df_plot)
lbl_buffer = .2

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove the top and right border

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

# show percentage on Y axis labels
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

# Label each line with the skill name
for i in range(num_skills):
    plt.text((num_months - 1) + lbl_buffer, df_plot.iloc[-1, i], df_plot.columns[i])

plt.show()
```
### Results
![Trending Top Skills for Data Analysts in the US](3_Project/images/skill_trend_DA.png)
*Bar graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights:
- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.