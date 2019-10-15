from bs4 import BeautifulSoup
import requests
import pandas as pd

URL = 'https://en.wikipedia.org/wiki/List_of_largest_technology_companies_by_revenue'
response = requests.get(URL)
soup = BeautifulSoup(response.text, 'html.parser')

#print(soup)

table = soup.find('table', {'class':'wikitable sortable plainrowheads'}).tbody
print(table)

index = []
company = []
year = []
revenue = []
employee=[]
country=[]


for row in table.findAll("tr"):
    cells = row.findAll("td")
#     print(len(cells))

 #For each "tr", assign each "td") to a variable.
    if len(cells) == 7:
        index.append(cells[1].find(text=True))
        company.append(cells[2].findAll(text=True))
        year.append(cells[3].find(text=True))
        revenue.append(cells[4].find(text=True))
        employee.append(cells[5].find(text=True))
        country.append(cells[6].find(text=True))

company2=[]
for i in company:
    for j in i:
        print(j.replace('\n',""))
        company2.append(j)

company3=[]
for i in range(len(company2)):
    if i%2==1:
        pass
    else:
        company3.append(company2[i])
print(company3)

data2=pd.DataFrame()
data2['Company']=company3
data2['year']=year
data2['revenue']=revenue
data2['Employee']=employee
data2['Country']=country
print(data2)

data2.to_csv('Project.csv')