# Data-Analytics-Report
print("UNICEF Indicator Assignment Report")

UNICEF Indicator Assignment Report

Introduction

This report analyzes a UNICEF indicator dataset related to the percentage of young people with correct knowledge of HIV/AIDS. The analysis examines how this knowledge varies across countries, genders and time, and how it correlates with socioeconomic factors such as GDP per capita, life expectancy and birth rates.

import pandas as pd
import plotly.express as px
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="whitegrid")

df = pd.read_excel("/content/unicef_indicator_1.xlsx", sheet_name="unicef_indicator_1")

!pip install plotly
Requirement already satisfied: plotly in /usr/local/lib/python3.11/dist-packages (5.24.1)
Requirement already satisfied: tenacity>=6.2.0 in /usr/local/lib/python3.11/dist-packages (from plotly) (9.1.2)
Requirement already satisfied: packaging in /usr/local/lib/python3.11/dist-packages (from plotly) (24.2)

df.drop_duplicates(inplace=True)
df.dropna(subset=['obs_value', 'country', 'sex', 'time_period'], inplace=True)

df['time_period'] = pd.to_numeric(df['time_period'], errors='coerce')

print("Data Overview:")
print(df.head())
print("\nSummary Statistics:")
print(df.describe())

Data Overview:
       country alpha_2_code alpha_3_code  numeric_code  \
0  Afghanistan           AF          AFG             4   
1  Afghanistan           AF          AFG             4   
2      Albania           AL          ALB             8   
3      Albania           AL          ALB             8   
4      Albania           AL          ALB             8   

                                           indicator  time_period  obs_value  \
0  Per cent of young people (aged 15-24 years) wh...       2015.0          9   
1  Per cent of young people (aged 15-24 years) wh...       2015.0         27   
2  Per cent of young people (aged 15-24 years) wh...       2018.0         27   
3  Per cent of young people (aged 15-24 years) wh...       2009.0         37   
4  Per cent of young people (aged 15-24 years) wh...       2018.0         23   

      sex unit_multiplier unit_of_measure observation_status  \
0  Female           Units               %           Reported   
1    Male           Units               %           Reported   
2  Female           Units               %           Reported   
3    Male           Units               %           Reported   
4    Male           Units               %           Reported   

  observation_confidentaility  \
0                        Free   
1                        Free   
2                        Free   
3                        Free   
4                        Free   

  time_period_activity_related_to_when_the_data_are_collected  \
0                                   End of fieldwork            
1                                   End of fieldwork            
2                                   End of fieldwork            
3                                   End of fieldwork            
4                                   End of fieldwork            

          current_age  population, total  GDP per capita (constant 2015 US$)  \
0  15 to 24 years old         33831764.0                          565.569730   
1  15 to 24 years old         33831764.0                          565.569730   
2  15 to 24 years old          2866376.0                         4431.555595   
3  15 to 24 years old          2927519.0                         3432.170872   
4  15 to 24 years old          2866376.0                         4431.555595   

   GDP growth (annual %)  Life expectancy at birth, total (years)  \
0               1.451315                                   62.659   
1               1.451315                                   62.659   
2               4.019346                                   79.184   
3               3.354289                                   77.781   
4               4.019346                                   79.184   

   Birth rate, crude (per 1,000 people)  
0                                38.803  
1                                38.803  
2                                10.517  
3                                11.841  
4                                10.517  

Summary Statistics:
       numeric_code  time_period   obs_value  population, total  \
count    318.000000   316.000000  318.000000       3.100000e+02   
mean     449.581761  2010.224684   56.789308       5.433261e+07   
std      242.003750     5.528296   24.869304       1.830249e+08   
min        4.000000  2000.000000    2.000000       1.778530e+05   
25%      231.000000  2006.000000   37.000000       8.213931e+06   
50%      454.000000  2010.000000   60.000000       1.407112e+07   
75%      646.000000  2015.000000   78.000000       2.927383e+07   
max      894.000000  2021.000000   99.000000       1.414204e+09   

       GDP per capita (constant 2015 US$)  GDP growth (annual %)  \
count                          310.000000             310.000000   
mean                          1880.878250               5.454395   
std                           1838.871809               5.394718   
min                            257.412699             -21.399900   
25%                            730.291525               2.997273   
50%                           1225.605419               5.254040   
75%                           2205.585240               7.407486   
max                          10280.391540              34.500000   

       Life expectancy at birth, total (years)  \
count                               310.000000   
mean                                 62.292008   
std                                   8.066441   
min                                  43.641000   
25%                                  57.081000   
50%                                  62.291000   
75%                                  67.712000   
max                                  79.943000   

       Birth rate, crude (per 1,000 people)  
count                            310.000000  
mean                              32.116800  
std                                9.678294  
min                               10.200000  
25%                               23.756250  
50%                               34.092000  
75%                               39.662250  
max                               50.290000  

country_counts = df['country'].value_counts().reset_index()
country_counts.columns = ['Country', 'Count']
fig = px.bar(country_counts.head(10), x='Count', y='Country', orientation='h', title='Top 10 Countries by Number of Records')
fig.show()

This visualization highlights the uneven distribution of data across countries. Some countries have significantly more records, suggesting either a higher frequency of data collection or more accessible reporting structures compared to others. The disparity also points to potential biases in data availability, which could influence the conclusions drawn from the analysis. Understanding where data is concentrated helps identify regions that are either more proactive in reporting or have more established data collection infrastructures, while also revealing potential gaps where further data gathering efforts are needed.

top_countries = df['country'].value_counts().head(5).index
gender_data = df[df['country'].isin(top_countries)]
fig = px.bar(gender_data, x='country', y='obs_value', color='sex', barmode='group', title='HIV Knowledge by Gender in Top 5 Countries')
fig.update_layout(xaxis_title='Country', yaxis_title='% with Correct HIV Knowledge')
fig.show()

Across most countries, males tend to report slightly higher levels of HIV knowledge. However, the gender gap is country-specific, suggesting the influence of localized education and awareness campaigns. The visualization also underscores how cultural, societal and educational differences can shape knowledge levels differently for men and women. It raises important questions about the design and reach of public health interventions, highlighting the need for gender-sensitive education strategies to ensure that both males and females recieve equitable access to information.

selected_country = 'Albania'
time_data = df[df['country'] == selected_country]
fig = px.line(time_data, x='time_period', y='obs_value', color='sex', markers=True, title=f"HIV Knowledge Over Time in {selected_country}")
fig.update_layout(xaxis_title='Year', yaxis_title='% with Correct HIV Knowledge')
fig.show()

Albania shows a steady increase in awareness levels over time for both genders, with occasional dips that may reflect data inconsistencies or social changes. This trend analysis highlights the effectiveness of long-term awareness efforts and the benefits of sustained public health campaigns. The visualization also suggestd that while progress has been made, continuous monitoring is crucial to maintain and build upon these gains, especially considering how sociopolitical shifts or funding changes might impact educational outreach.

fig = px.scatter(df, x='GDP per capita (constant 2015 US$)', y='obs_value', color='sex', title='HIV Knowledge vs GDP per Capita', labels={'obs_value': '% with Correct HIV Knowledge'})
fig.show()

fig = px.scatter(df, x='Life expectancy at birth, total (years)', y='obs_value', color='sex', title='HIV Knowledge vs Life Expectancy', labels={'obs_value': '% with Correct HIV Knowledge'})
fig.show()

Countries with higher life expectancies often show higher HIV awareness levels. This may be due to better healthcare systems, more robust education infrastructure and improved living standards that prioritize both health and education. However, the relationship is not absolute, suggesting that while development indicators generally support higher knowledge levels, targeted interventions are still necessary. The scatter plot highlights the interconnectedness of health outcomes and education, reinforcing the argument that holistic development policies are critical for improving HIV awareness.

fig = px.scatter(df, x='Birth rate, crude (per 1,000 people)', y='obs_value', color='sex', title='HIV Knowledge vs Birth Rate', labels={'obs_value': '% with Correct HIV Knowledge'})
fig.show()

plt.figure(figsize=(12, 8))
correlation_data = df[['obs_value', 'GDP per capita (constant 2015 US$)', 'GDP growth (annual %)', 'Life expectancy at birth, total (years)', 'Birth rate, crude (per 1,000 people)']]
sns.heatmap(correlation_data.corr(), annot=True, cmap='coolwarm', fmt='.2f')
plt.title("Correlation Heatmap of Indicators")
plt.tight_layout()
plt.show()

The heatmap indicates moderate positive correlation between HIV knowledge and GDP per capita and life expectancy while showing a negative correlation with birth rate. These relationships emphasize the higher national income and better overall health standards are often associated with greater awareness levels, whereas high birth rates - which can sometimes correlate with lower access to education and healthcare - are associated with lower HIV knowledge. The heatmap provides a comprehensive view of how multiple social and economic factors interplay with public health education, underscoring the multifaceted nature of development challenges.

print("""
Insight:
- HIV knowledge levels vary significantly between genders and countries.
- Socioeconomic conditions likely influence the levels of HIV awareness.
- Time trends suggest progress in some countries but stagnation in others.
- Policy efforts might be more needed in countries with lower GDP and knowledge levels.
""")

Insight:
- HIV knowledge levels vary significantly between genders and countries.
- Socioeconomic conditions likely influence the levels of HIV awareness.
- Time trends suggest progress in some countries but stagnation in others.
- Policy efforts might be more needed in countries with lower GDP and knowledge levels.

Recommendations

- Introduce targeted awareness campaigns in countries with poor performance.
- Use gender-specific approaches to bridge knowledge gaps.
- Further research could investigate urban / rural divides or educational attainment.
