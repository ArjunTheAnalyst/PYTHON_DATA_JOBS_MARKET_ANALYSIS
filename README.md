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

Results

**Key Insights:**
1. SQL is the most requested skill for Data Analysts (51%) and Data Engineers (72%)
2. Python dominates for Data Scientists (72%)
3. Data Engineers need specialized technical skills (AWS, Azure, Spark)
4. Python is versatile across all three roles (72% for Data Scientists, 65% for Data Engineers)
