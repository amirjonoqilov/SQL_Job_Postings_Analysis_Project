# Introduction
Dive into the data job market. Focusing on data science roles, this project explores, top-paying jobs, in-demand skills, where high demand meets high salary.

Here you can find SQL queries. [project_sql folder](/project_sql/)
# Background
The questions I wanted to find answer through my queries.

1. What are the top-paying data science jobs?
2. What skills are requaired for this top-paying jobs?
3. What skills are the most demand for this role?
4. which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I Used
- SQL: The backbone of my analysis, allowing me to query the database and unearth critical insights.
- PostgreSQL: The chosen database management system, ideal for handling the job posting data.
- Visual Studio Code: My go-t for database management and executing SQL queries.
- Git & GitHub: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The Analysis

 ### Query 1: Top Paying Remote Data Scientist Jobs
This query lists the 10 highest-paying remote Data Scientist roles where salary data is available.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact AS job
LEFT JOIN company_dim AS company ON job.company_id = company.company_id
WHERE
    job_title_short = 'Data Scientist' AND
    job_location ='Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY 
    salary_year_avg DESC
LIMIT 10;  
```     

**Key Insights:**

- The highest average salary is $550,000, while the lowest in the top 10 is $300,000.

- Titles like “Staff”, “Director”, and “Head of Data Science” dominate, indicating that senior and leadership roles offer the highest pay.

- Companies like Selby Jennings and Demandbase appear multiple times, suggesting they are top recruiters for high-paying roles.

- Most postings are from mid to late 2023, indicating a relatively recent demand for high-level remote data science roles.

  ### Query 2: Top paying jobs associated with skills.

This query examines high-paying remote Data Science roles and the specific skills required for those jobs. The same job may appear multiple times if it's linked to more than one skill.
```sql
WITH top_paying_jobs AS(   
    SELECT
        job_id,
        job_title,
        job_location,
        job_schedule_type,
        salary_year_avg,
        job_posted_date,
        name AS company_name
    FROM
        job_postings_fact AS job
    LEFT JOIN company_dim AS company ON job.company_id = company.company_id
    WHERE
        job_title_short = 'Data Scientist' AND
        job_location ='Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY 
        salary_year_avg DESC
    LIMIT 10)

SELECT top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY 
salary_year_avg DESC; 
```         

Key Insights:

The most lucrative jobs demand skills like SQL, Python, Machine Learning, and Big Data tools.

Python and SQL are the most frequently associated skills with the top-paying roles, especially in senior-level positions.

Some roles that list AI/ML or data architecture tools show significantly higher average salaries, reaching up to $550,000.

All positions are remote and full-time, highlighting consistent flexibility and commitment expectations in top-tier jobs.

    ### Query 3: Top skills that are on high demand.
| Skill   | Demand Count |
| ------- | ------------ |
| Python  | 114,016      |
| SQL     | 79,174       |
| R       | 59,754       |
| SAS     | 29,642       |
| Tableau | 29,513       |
| AWS     | 26,311       |
| Spark   | 24,353       |
                

This table highlights the top 10 most in-demand skills for Data Science roles based on job posting frequency:

Python is by far the most in-demand skill, appearing in over 114,000 postings. It is essential for data manipulation, modeling, and machine learning.

SQL ranks second, indicating its importance for data querying and working with databases.

Tools like R, SAS, and Excel remain relevant, especially in statistical and enterprise analytics.

Together, these skills form the backbone of most Data Science job requirements, and mastering them significantly boosts employability.

   ### Query 4: Highest Paying Skills for Data Scientist Roles.

| Skill         | Average Salary (USD) |
| ------------- | -------------------- |
| Asana         | 215,477.38           |
| Airtable      | 201,142.86           |
| RedHat        | 189,500.00           |
| Watson        | 187,417.14           |
| Elixir        | 170,823.56           |
| Lua           | 170,500.00           |
| Slack         | 168,218.76           |
| Solidity      | 166,979.90           |
| Ruby on Rails | 166,500.00           |
| RShiny        | 166,436.21           |


```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 2) AS average_salary
FROM job_postings_fact AS job
INNER JOIN skills_job_dim ON job.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
    job_title_short = 'Data Scientist'
    AND salary_year_avg IS NOT NULL
GROUP BY 
    skills
ORDER BY average_salary DESC
LIMIT 20;
```
### Query 5: In-Demand & High-Paying Skills for Remote Data Scientist Jobs

| Skill        | Demand Count | Average Salary (USD) |
| ------------ | ------------ | -------------------- |
| Python       | 763          | 143,827.93           |
| SQL          | 591          | 142,832.59           |
| R            | 394          | 137,885.37           |
| Tableau      | 219          | 146,970.05           |
| AWS          | 217          | 149,629.96           |
| Spark        | 149          | 150,188.49           |
| TensorFlow   | 126          | 151,536.49           |
| Azure        | 122          | 142,305.83           |
| PyTorch      | 115          | 152,602.70           |
| Pandas       | 113          | 144,815.95           |
| SAS          | 110          | 129,919.88           |
| Hadoop       | 82           | 143,321.50           |
| Scikit-learn | 81           | 148,963.95           |
| Excel        | 77           | 129,224.44           |
| NumPy        | 73           | 149,089.24           |
| Snowflake    | 72           | 152,686.88           |
| Power BI     | 72           | 131,390.36           |
| Java         | 64           | 145,706.30           |
| Databricks   | 63           | 139,631.11           |
| GCP          | 59           | 155,810.57           |
| Git          | 58           | 132,598.80           |
| Go           | 57           | 164,691.09           |
| Looker       | 57           | 158,714.91           |
| Scala        | 56           | 156,701.92           |

This query combines demand and average salary for each skill in remote data science jobs with known salary data.

Key Takeaways:

Python and SQL dominate in demand, showing up in hundreds of job listings.

While R is also highly demanded, it offers slightly lower average pay compared to newer technologies.

High-paying but less common tools like Go, Looker, Scala, and GCP suggest that specialized or cloud-based skills are financially rewarding.

Libraries and frameworks such as TensorFlow, PyTorch, Scikit-learn, and Snowflake are associated with salaries above $150K, indicating their value in advanced analytics and machine learning.

Data engineering tools (e.g., Spark, Databricks, Snowflake) are in moderate demand but offer very competitive salaries.

This data helps pinpoint which skills offer the best balance between market demand and salary potential for remote roles.

```sql
WITH skills_demand AS(
    SELECT 
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact AS job
    INNER JOIN skills_job_dim ON job.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE 
        job_title_short = 'Data Scientist'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY 
        skills_dim.skill_id
), average_salary AS (
    SELECT
        skills_job_dim.skill_id, 
        ROUND(AVG(salary_year_avg), 2) AS average_salary
    FROM job_postings_fact AS job
    INNER JOIN skills_job_dim ON job.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE 
        job_title_short = 'Data Scientist'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY 
        skills_job_dim.skill_id
    )

SELECT 
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    average_salary
FROM 
    skills_demand       
INNER JOIN 
    average_salary ON skills_demand.skill_id = average_salary.skill_id
ORDER BY
    demand_count DESC,
    average_salary DESC
LIMIT 25;
```       



# What I Learned
## 🔍 Conclusions

- **Complex Query Crafting**  
  Mastered advanced SQL techniques, including multi-table joins and `WITH` clauses for clean, modular query design.

- **Data Aggregation Mastery**  
  Used `GROUP BY`, `COUNT()`, `AVG()`, and other aggregate functions to summarize and analyze datasets effectively.

- **Analytical Thinking**  
  Translated real-world questions into actionable SQL solutions, developing a sharper, insight-driven problem-solving mindset.

- **Confidence Boost**  
  Gained the skills and self-assurance to take on complex data challenges and support data-driven decision-making.

