# Rural Population: East Africa (2021–2024)

This repository presents a comprehensive analysis of rural population trends in East Africa, with a special focus on Rwanda from 2021 to 2024. The project combines data preprocessing in Python with data visualization using Power BI.

---

## Project Objectives

- Analyze the rural population index in Rwanda from 2021 to 2024.  
- Compare rural population distribution across East African countries.  
- Identify rural classification trends (*High Rural* and *Medium Rural*).  
- Use Power BI for visual storytelling and interactive filtering.

---

## Power BI Dashboard

You can download or view the Power BI dashboard (.pbix) via the link below:  
 [Download Power BI File](https://drive.google.com/file/d/1QMbc7fzHwkvOPH1VHQ87Asppb_5dOMJ6/view?usp=drive_link)

---

## Key Insights

- Rwanda’s rural population slightly decreased from **82.43% in 2021** to **81.92% in 2024**.  
- Most countries were classified as *High Rural* before the year 2000.  
- Somalia is the only country classified as *Medium Rural*.  
- Rural population is relatively evenly distributed across the region, with no country exceeding 16% dominance.

---

## Tools & Technologies

- **Power BI** – For interactive dashboards and visual analytics  
- **Python** – For data cleaning and computation  
- **Jupyter Notebook** – For exploratory data analysis and transformation logic

---


import pandas as pd

# Load dataset
df_raw = pd.read_csv('API_SP.RUR.TOTL.ZS_DS2_en_csv_v2_23429.csv', skiprows=4)

# Reshape and clean data
df_melted = df_raw.melt(
    id_vars=['Country Name', 'Country Code'], 
    var_name='Year', 
    value_name='Rural Population %'
)
df_melted.dropna(inplace=True)
df_melted['Year'] = df_melted['Year'].astype(int)
df_melted['Decade'] = (df_melted['Year'] // 10) * 10

# Categorize rural levels
def classify(pop):
    if pop < 30:
        return 'Low Rural'
    elif pop < 70:
        return 'Medium Rural'
    else:
        return 'High Rural'

df_melted['Category'] = df_melted['Rural Population %'].apply(classify)

# Period classification
df_melted['Period'] = df_melted['Year'].apply(lambda y: 'Before 2000' if y < 2000 else 'After 2000')

# Final cleaned dataset
df_cleaned = df_melted[
    ['Country Name', 'Country Code', 'Year', 'Rural Population %', 'Decade', 'Category', 'Period']
]
df_cleaned.to_csv('cleaned_rural_population_table.csv', index=False)

# Preview
print(df_cleaned.head(10))
