# 📊 Data Jobs Market Analysis
An in-depth exploration of the data job market focusing on **Data Analyst roles**—analyzing top-paying skills, in-demand competencies, and optimal career strategies.

# 🎯 Project Overview
This project was created out of a desire to navigate and understand the data job market more effectively. It delves into the top-paying and in-demand skills to help identify **optimal job opportunities for data analysts**.

The data is sourced from **[Luke Barousse's Python Course](https://github.com/lukebarousse/Python_Data_Analytics_Course)**, providing a foundation for analysis with detailed information on job titles, salaries, locations, and essential skills.

# 📋 Key Questions Answered
1. 	What are the most demanded skills for the top 3 data roles?
2. 	How are in-demand skills trending for Data Analysts?
3. 	How well do jobs and skills pay for Data Analysts?
4. 	What are the optimal skills for data analysts to learn (High Demand + High Paying)?

# 🛠️ Tools & Libraries Used
1. **Python:**      Core analysis language
2. **Pandas:** 	    Data manipulation & analysis
3. **Matplotlib:**	Data visualization
4. **Seaborn:**    	Advanced statistical visuals
5. **Jupyter:**     Notebook	Interactive development environment
6. **GitHub:**	    Version control & project sharing

# 📥 Data Preparation & Cleanup
```
# Importing Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import ast

# Loading Dataset
df = pd.read_csv(r'C:\Users\ARJUN\Python_projects\Python_Course\data_jobs.csv')

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda skill_list: ast.literal_eval(skill_list) if pd.notna(skill_list) else skill_list)
```

# 📈 Analysis & Results
## 1️⃣ Most Demanded Skills for Top 3 Data Roles
**Approach:** Filtered the 3 most popular data roles and identified their top 5 skills.

## Visualize Data
```
fig, ax = plt.subplots(len(job_roles), 1)

sns.set_theme(style = 'ticks')

for i, job in enumerate(job_roles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job].head()
    sns.barplot(data = df_plot, x = 'skill_percent', y = 'job_skills', ax = ax[i], hue = 'skill_percent', palette = 'dark:b_r')
    sns.despine()
    ax[i].set_title(job)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].legend().remove()
    ax[i].set_xlim(0,75)
    ax[i].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'{int(x)}%'))

    if i != len(job_roles) - 1:
        ax[i].set_xticks([])
        
    for index, value in enumerate(df_plot['skill_percent']):
        ax[i].text(value + 1, index, f'{value: .0f}%', va = 'center')
    
fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize = 20)
fig.tight_layout(h_pad = 1.0)
plt.show()
```

## Results
![Skill_Demand](images/1_Skill_Demand.png)

**Key Insights:**
1. SQL is the most requested skill for Data Analysts (51%) and Data Engineers (72%)
2. Python dominates for Data Scientists (72%)
3. Data Engineers need specialized technical skills (AWS, Azure, Spark)
4. Python is versatile across all three roles (72% for Data Scientists, 65% for Data Engineers)


## 2️⃣ Trending Skills for Data Analysts (2023)
**Approach:** Grouped skills by month to track demand changes throughout the year.

## Visualize Data
```
df_plot = df_DA_US_perc.iloc[:,:5]

sns.set_theme(style = 'ticks')

sns.lineplot(data = df_plot, palette = 'tab10', dashes = False)
sns.despine()
plt.title('Trending Monthly Skills for Data Analysts in the US')
plt.xlabel('2023')
plt.ylabel('Likelihood in Job Postings')
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, _: f'{int(y)}%'))
plt.legend().set_visible(False)

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i]) # fetching the last value of each column

plt.tight_layout()
plt.show()
```

## Results
![Skills_Trend](images/2_Skills_Trend.png)

**Key Insights:**
1. SQL remains most consistently demanded, though gradually decreasing
2. Excel surged in demand from September, surpassing Python/Tableau by year-end
3. Python & Tableau show stable demand with minor fluctuations
4. Power BI shows slight upward trend toward year's end


## 3️⃣ Salary Analysis for Data Jobs & Skills
**Approach:** Analyzed salary distributions across job titles, then compared highest-paid vs. most in-demand skills.

## Visualize Data
```
# df_plot = [df_US['salary_year_avg'][df_US['job_title_short'] == job_title] for job_title in job_titles]
# plt.boxplot(df_plot, vert = False, labels = job_titles)

sns.set_theme(style = 'ticks')

(
    sns.boxplot(
    data = df_US[df_US['job_title_short'].isin(job_titles)], x = 'salary_year_avg', y = 'job_title_short', 
    
    order = df_US[df_US['job_title_short'].isin(job_titles)].groupby('job_title_short')['salary_year_avg'].agg('median')
    .sort_values(ascending = False).index)
)
sns.despine()
plt.title('Salary Distributions for Data Jobs in the US')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
plt.xlim(0,600000)
plt.show()
```
