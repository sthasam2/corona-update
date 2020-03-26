# Corona-Update
Web scraping Corona updates table using Python (requests, bs4) from www.worldometers.info corona update and saving data in a spreadsheet.

## Worldometers site
![Worldometer](https://github.com/sthasam2/Corona-Update/blob/master/samples/Screenshot%20from%202020-03-26%2022-47-50.png)

### Code:
```python
"""WEB SCRAPING FOR CORONA UPDATES FROM THE WORLDOMETER WEBSITE USING REQUESTS, BEAUTIFUL SOUP AND PANDAS"""

from bs4 import BeautifulSoup
import pandas as pd
import requests


"""The Worldometer URL for corona updates"""
URL = "https://www.worldometers.info/coronavirus/"

print("Getting page info...")

# sending a GET request for the URL
page = requests.get(URL)

# parsing into the html source code using beautiful soup
soup = BeautifulSoup(page.content, 'html.parser')


print("Scraping page data...")
"""---------------------------------------------TABLE---------------------------------------------"""
# extracting the html of table of data using soup
table = soup.find(id='main_table_countries_today')


"""---------------------------------------------TABLE HEAD---------------------------------------------"""
# Getting the table head HTML
thead = table.find('thead')

# getting the html columns from the table head
thead_cols = thead.find_all('th')

# List for storing the table header titles
head = []

# iterating head columns to store seach value in list
for col in thead_cols:
    head.append(col.text)

"""---------------------------------------------TABLE BODY---------------------------------------------"""
# Getting the table body HTML from table
tbody = soup.find('tbody')

# Getting all the rows in the table body
rows = tbody.find_all('tr')

# list for storing data of each row
row_data = []

# iterating each row to extract data
for row in rows:
    # extracting each column from rows
    cols = row.find_all('td')

    # extracting the text of each column and saving into a list row_data
    row = [i.text.strip() for i in cols]
    row_data.append(row)

# Lists for storing each colums
Country = []
TotalCases = []
NewCases = []
TotalDeaths = []
NewDeaths = []
TotalRecovered = []
ActiveCases = []
SeriousCritical = []
TotCases1Mpop = []
TotDeaths1Mpop = []

# Iterating each row data and adding to the column lists
for row in row_data:
    Country.append(row[0])
    TotalCases.append(row[1])
    NewCases.append(row[2])
    TotalDeaths.append(row[3])
    NewDeaths.append(row[4])
    TotalRecovered.append(row[5])
    ActiveCases.append(row[6])
    SeriousCritical.append(row[7])
    TotCases1Mpop.append(row[8])
    TotDeaths1Mpop.append(row[9])


"""---------------------------------------------EXTRACTED DATA TABULATION---------------------------------------------"""
print("Exporting scraped data...")
# creating a dataframe with each columnlist
df = pd.DataFrame({'Country/Other': Country, 'Total Cases': TotalCases, 'New Cases': NewCases, 'Total Death': TotalDeaths, 'New Deaths': NewDeaths, 'Total Recovered': TotalRecovered,
                   'Active Cases': ActiveCases, 'Serious/Critical': SeriousCritical, 'Tot Cases/ 1M pop': TotCases1Mpop, 'Tot Deaths/ 1M pop': TotDeaths1Mpop})

# converting dataframe to csv file
df.to_csv('CoronaUpdate.csv', index=False, encoding='utf-8')

print("Data exported to: 'CoronaUpdate.csv' file")
```

### Output CSV file

![Output CSV](https://github.com/sthasam2/Corona-Update/blob/master/samples/Screenshot%20from%202020-03-26%2022-47-43.png)

## FUTURE ENHANCEMENTS
* Will try to create a webapp with charts and plots for data visualization
