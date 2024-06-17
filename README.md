# Demographic-Data-Analyzer

### Project Description:

The Demographic Data Analyzer project involves analyzing demographic data from a dataset of census information. You will create a function to read the dataset and perform various calculations to derive meaningful insights about the data. The function will return a dictionary containing these insights.

### Objectives

1. **Read the dataset**: Load the dataset from a CSV file into a pandas DataFrame.
2. **Calculate various demographic statistics**: Compute specific metrics based on the dataset.
3. **Return results in a dictionary**: Output the results in a structured format.

### Dataset

The dataset contains demographic information with columns such as age, workclass, education, education-num, marital-status, occupation, relationship, race, sex, capital-gain, capital-loss, hours-per-week, and native-country.

### Function Specifications

1. **Input**: Path to the CSV file.
2. **Output**: A dictionary containing the following keys and their corresponding calculated values:
   - `'race_count'`: Count of each race (a pandas Series sorted by index).
   - `'average_age_men'`: Average age of men.
   - `'percentage_bachelors'`: Percentage of people with a Bachelor's degree.
   - `'higher_education_rich'`: Percentage of people with higher education (Bachelors, Masters, or Doctorate) earning >50K.
   - `'lower_education_rich'`: Percentage of people without higher education earning >50K.
   - `'min_work_hours'`: Minimum number of hours a person works per week.
   - `'rich_percentage'`: Percentage of people who work the minimum number of hours per week earning >50K.
   - `'highest_earning_country'`: Country with the highest percentage of people earning >50K.
   - `'highest_earning_country_percentage'`: Highest percentage of people earning >50K in the highest earning country.
   - `'top_IN_occupation'`: Most popular occupation for those who earn >50K in India.

### Steps to Follow

1. **Load the Data**:
   - Use pandas to read the CSV file into a DataFrame.

2. **Calculate Demographic Statistics**:
   - **Race Count**: Use the `value_counts()` method to count the number of occurrences of each race in the dataset.
   - **Average Age of Men**: Filter the dataset for males and calculate the mean age.
   - **Percentage of People with a Bachelor's Degree**: Calculate the percentage of people who have a Bachelor's degree.
   - **Higher Education Rich**: Filter the dataset for people with higher education and calculate the percentage earning >50K.
   - **Lower Education Rich**: Filter the dataset for people without higher education and calculate the percentage earning >50K.
   - **Minimum Work Hours**: Find the minimum number of hours a person works per week.
   - **Rich Percentage Among Min Work Hours**: Calculate the percentage of people who work the minimum number of hours per week and earn >50K.
   - **Highest Earning Country**: Determine the country with the highest percentage of people earning >50K.
   - **Highest Earning Country Percentage**: Calculate the highest percentage of people earning >50K in the highest earning country.
   - **Top Occupation in India**: Determine the most popular occupation for those who earn >50K in India.

3. **Return the Results**:
   - Store each calculated value in a dictionary and return it.

### Example Implementation

```python
import pandas as pd

def calculate_demographic_data(print_data=True):
    # Read data from file
    df = pd.read_csv('adult.data.csv')
    
    # Race count
    race_count = df['race'].value_counts()

    # Average age of men
    average_age_men = df[df['sex'] == 'Male']['age'].mean()

    # Percentage with Bachelors degrees
    percentage_bachelors = (df['education'] == 'Bachelors').mean() * 100

    # Higher education (Bachelors, Masters, or Doctorate)
    higher_education = df['education'].isin(['Bachelors', 'Masters', 'Doctorate'])
    lower_education = ~higher_education

    # Percentage of higher education earning >50K
    higher_education_rich = ((df[higher_education]['salary'] == '>50K').mean()) * 100

    # Percentage of lower education earning >50K
    lower_education_rich = ((df[lower_education]['salary'] == '>50K').mean()) * 100

    # Minimum work hours
    min_work_hours = df['hours-per-week'].min()

    # Percentage of rich among those who work minimum hours
    min_workers = df[df['hours-per-week'] == min_work_hours]
    rich_percentage = (min_workers['salary'] == '>50K').mean() * 100

    # Country with highest percentage of rich
    country_salary = df.groupby('native-country')['salary']
    highest_earning_country = (country_salary.apply(lambda x: (x == '>50K').mean()).idxmax())
    highest_earning_country_percentage = (country_salary.apply(lambda x: (x == '>50K').mean()).max()) * 100

    # Most popular occupation for those who earn >50K in India
    top_IN_occupation = (df[(df['native-country'] == 'India') & (df['salary'] == '>50K')]
                         .groupby('occupation').size().idxmax())

    if print_data:
        print("Number of each race:\n", race_count) 
        print("Average age of men:", average_age_men)
        print(f"Percentage with Bachelors degrees: {percentage_bachelors}%")
        print(f"Percentage with higher education that earn >50K: {higher_education_rich}%")
        print(f"Percentage without higher education that earn >50K: {lower_education_rich}%")
        print(f"Min work time: {min_work_hours} hours/week")
        print(f"Percentage of rich among those who work fewest hours: {rich_percentage}%")
        print("Country with highest percentage of rich:", highest_earning_country)
        print(f"Highest percentage of rich people in country: {highest_earning_country_percentage}%")
        print("Top occupations in India:", top_IN_occupation)

    return {
        'race_count': race_count,
        'average_age_men': average_age_men,
        'percentage_bachelors': percentage_bachelors,
        'higher_education_rich': higher_education_rich,
        'lower_education_rich': lower_education_rich,
        'min_work_hours': min_work_hours,
        'rich_percentage': rich_percentage,
        'highest_earning_country': highest_earning_country,
        'highest_earning_country_percentage': highest_earning_country_percentage,
        'top_IN_occupation': top_IN_occupation
    }

# Example usage:
result = calculate_demographic_data(print_data=True)
```

### Expected Output

The expected output will be a dictionary with the calculated values, and if `print_data` is set to `True`, the calculated metrics will be printed to the console.

This project provides practice with data analysis using pandas, including filtering, grouping, and calculating various statistics based on conditions. It also helps in understanding and manipulating data to derive meaningful insights.
