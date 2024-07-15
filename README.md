# Scraping Data from Wikipedia and Analyzing Largest US Companies by Revenue

## Table of Contents
1. [Project Overview](#project-overview)
2. [Data Sources](#data-sources)
3. [Tools](#tools)
4. [Data Cleaning/Preparation](#data-cleaningpreparation)
5. [Code](#code)
6. [Recommendations](#recommendations)
7. [Limitations](#limitations)
8. [References](#references)

## Project Overview
This project involves scraping data from a Wikipedia page listing the largest companies in the United States by revenue. The goal is to extract this data into a DataFrame for analysis to gain insights into the top-performing companies.

## Data Sources
- **Wikipedia Page**: [List of largest companies in the United States by revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue)

## Tools
- **Python**: Programming language used for scripting and data analysis.
- **BeautifulSoup**: Library for parsing HTML and extracting data from web pages.
- **Requests**: Library for making HTTP requests to fetch web page content.
- **Pandas**: Library for data manipulation and analysis.
- **GitHub**: Platform for version control and project hosting.

## Data Cleaning/Preparation
1. **Fetching Data**: Used the `requests` library to get the HTML content of the Wikipedia page.
2. **Parsing HTML**: Used `BeautifulSoup` to parse the HTML and locate the relevant table.
3. **Extracting Table Data**: Identified the correct table using indexing and extracted headers and rows.
4. **Cleaning Data**: Removed unnecessary whitespace and formatted the data properly.
5. **Storing Data**: Saved the cleaned data into a Pandas DataFrame and exported it to a CSV file.

## Code

```python
# Import necessary libraries
from bs4 import BeautifulSoup  # For parsing HTML content
import requests  # For making HTTP requests
import pandas as pd  # For data manipulation

# Define the URL of the Wikipedia page to scrape
url = 'https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue'

# Send an HTTP request to the URL and get the response
page = requests.get(url)

# Parse the HTML content of the page with BeautifulSoup
soup = BeautifulSoup(page.text, 'html.parser')

# Find all the tables on the page and select the correct one by index
table = soup.find_all('table')[1]

# Extract the table headers (column names)
world_titles = table.find_all('th')
world_table_titles = [title.text.strip() for title in world_titles]

# Create a pandas DataFrame with the extracted headers
df = pd.DataFrame(columns=world_table_titles)

# Find all rows in the table
column_data = table.find_all('tr')

# Extract data from each row and append to the DataFrame
for row in column_data[1:]:
    row_data = row.find_all('td')
    indv_row_data = [data.text.strip() for data in row_data]
    length = len(df)
    df.loc[length] = indv_row_data

# Export the DataFrame to a CSV file in the 'data' directory
df.to_csv('data/web_scraping_data.csv', index=False)
```

## Results/Findings
- Successfully extracted and cleaned the data from the Wikipedia page.
- The data is now stored in a CSV file for further analysis.

## Recommendations
- Future analysis can include visualizing the revenue distribution of these companies.
- Comparing the revenue data with other metrics like employee count, market capitalization, etc.

## Limitations
- The data is limited to what is available on the Wikipedia page and may not be up-to-date.
- The extraction process relies on the specific structure of the Wikipedia page, which may change over time.

## References
- Wikipedia: [List of largest companies in the United States by revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue)
- BeautifulSoup Documentation: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- Requests Documentation: https://docs.python-requests.org/en/master/
- Pandas Documentation: https://pandas.pydata.org/pandas-docs/stable/
