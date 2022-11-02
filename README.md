# World-Population-Analysis-Using-Python
# World Population Analysis


This is World Population Dataset which covers 
the number of population in each country in the world,Area of each country and the Growth Rate of population.
The World Population Data are collected from 1970 to 2022.

------------------------------------------------------------------------------

# Highlights of The Notebook


-in this notebook we will focus on the Growth Rate of population and which county has Population increase
and more.


**************************************************************************************************************************

!pip install missingno

import numpy as np
import pandas as pd 
import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno

## Importing Data

world_population = pd.read_csv('world_population.csv')

world_population

**At first let's check f there is any missing Values.**

msno.matrix(world_population);
plt.title('Any missing values?', size=40, fontweight='bold' ,color="green");

this figuer show there isn't missing values

# ***Data Preparation and Cleaning***

world_population.info()

world_population.describe()

world_population.columns

world_population.shape

**From the previous cells we can see that our data contains 234 columns and 17 rows 
and It is also clear to us that this data does not contain any non values, but let's make sure**

world_population.isnull()

**The Dataframe doesn't has any Non Values.**

population_distribution = world_population['Density (per km²)'] / world_population['Area (km²)']
population_distribution

**After Calculating The Distribution of Population in Each Country, Let's add this column to the DataFrame.***

world_population['population_distribution'] = population_distribution

world_population

# ***Exploratory Analysis and Visualization***

world_population.head()

data_heatmap = world_population.corr()
data_heatmap

plt.title("World Population from 1970 to 2022")
sns.heatmap(world_population.corr(), fmt=".01g", annot=True, cmap='Greens');

# ***Ask & answer questions about the data***
***Q1: Which Countries have the largest population?***

world_population.set_index("Country", inplace=True)

plt.title('Countries with Largest Population')
plt.xlabel("Country")
plt.ylabel("No.Population")
world_population['2020 Population'].nlargest().plot(kind = "bar" ,color="green")

in the previos plot that represents Counries with the largest population, we can see that China and India 
have the most number of people and Pakistan has the smallest, X-Axis represents names of Countries and Y-Axis represent
how many people each country has.

**From The previos Bars we can see that China and India have the Largest Population**

***Q2: Which country with the biggest Density? India or China?***

india = (world_population.loc["India"])["Density (per km²)"]
china = (world_population.loc["China"])["Density (per km²)"]
print("The Density in India = " ,india)
print("The Density in China = " ,china)

**From The Previous equations and Calculations we can see that The biggest density is India**

area1 = (world_population.loc["India"])["Area (km²)"]
area2 = (world_population.loc["China"])["Area (km²)"]

countries = [india,china]
area = [area1,area2]
plt.xlabel("Density")
plt.ylabel("Area")
plt.plot(countries,area, "-g" , linewidth=5 ,linestyle="solid")
plt.show()

from previous plot we can observe when the density is increase area is decrease and it is very logical.

***Q3: What do you predict for the Density in the next four years?***

population_2030 = world_population["2020 Population"] * (world_population["Growth Rate"] ** 10)
population_2040 = world_population["2020 Population"] * (world_population["Growth Rate"] ** 20)
population_2050 = world_population["2020 Population"] * (world_population["Growth Rate"] ** 30)
population_2060 = world_population["2020 Population"] * (world_population["Growth Rate"] ** 40)
print("The Density in 2030 is " , population_2030)
print("The Density in 2040 is " , population_2040)
print("The Density in 2050 is " , population_2050)
print("The Density in 2060 is " , population_2060)

**Let's add this prediction as columns in our dataframe**

world_population["population_2030"] = population_2030
world_population["population_2040"] = population_2040
world_population["population_2050"] = population_2050
world_population["population_2060"] = population_2060

world_population

**let's see what happen to Population in India and China**

india_population_2030 = (world_population.loc["India"])["population_2030"]
China_population_2030 = (world_population.loc["China"])["population_2030"]
india_population_2040 = (world_population.loc["India"])["population_2040"]
China_population_2040 = (world_population.loc["China"])["population_2040"]
india_population_2050 = (world_population.loc["India"])["population_2050"]
China_population_2050 = (world_population.loc["China"])["population_2050"]
india_population_2060 = (world_population.loc["India"])["population_2060"]
China_population_2060 = (world_population.loc["China"])["population_2060"]

indiapopulation = [india_population_2030,india_population_2040,india_population_2050,india_population_2060]
years = [2030,2040,2050,2060]
chinapopulation = [China_population_2030,China_population_2040,China_population_2050,China_population_2060]

plt.style.use("dark_background")
plt.title("What Happen for Population in India at the next 4 years")
plt.plot(years,indiapopulation ,color="g" ,linewidth=5 ,linestyle="solid" ,label="India_population")
plt.plot(years,chinapopulation ,color="orange" ,linewidth=5 ,linestyle="--" ,label="China_population")
plt.xlabel("years")
plt.ylabel("population")
plt.legend()
plt.show()

**from the previous plots We can see that at the last 4 years India population will increase over China with a big Difference
and China's Population will be fixed as shown in the figure. **

***Q4: What is total population in each Country?*** 

total_2060 = world_population['population_2060'].sum()
total_2050 = world_population['population_2050'].sum()
total_2040 = world_population['population_2040'].sum()
total_2030 = world_population['population_2030'].sum()
total_2020 = world_population['2020 Population'].sum()
total_2010 = world_population['2010 Population'].sum()
total_2000 = world_population['2000 Population'].sum()
total_1990 = world_population['1990 Population'].sum()
total_1980 = world_population['1980 Population'].sum()
total_1970 = world_population['1970 Population'].sum()

world = [total_1970,total_1980,total_1990,total_2000,total_2010,total_2020,total_2030,total_2040,
         total_2050,total_2060]
year = [1970,1980,1990,2000,2010,2020,2030,2040,2050,2060]

plt.style.use("dark_background")
plt.title("Expected World Population(1970-2060)")
plt.plot(year,world ,color="g" ,linewidth=5 ,linestyle="solid")
plt.xlabel("year")
plt.ylabel("world")
plt.show()

From Previous plot The expectation is that the world population keeps on growing.

***Q5: What is The total number of Countries in a Continent?***

plt.figure(figsize= (12,5))
plt.title("Number of Countries in a Continent")
sns.countplot(x = 'Continent', data = world_population.sort_values(by = 'Continent', ascending = True), palette= 'crest')
plt.tight_layout()
plt.xlabel('CONTINENT')
plt.ylabel('No. of Countries')

this plot describe the number of countres in each Continent and this show that Africa has the lagest amount of countries 
then Asiaand Europe, Asia and Europe have the same amount of Countries and South America has the smallest amount of countries.

continent_rank=world_population.groupby(by="Continent").sum().sort_values(by="2022 Population",ascending=False)
fig, ax = plt.subplots(figsize =(15, 10))
x=continent_rank.index
y=continent_rank["2022 Population"]
plt.pie(y,labels=x,autopct='%1.1f%%')
plt.show()

The previous chart shows that Asia is the most Population Continent and Ociena is the smallest.

***Q6: What is The World Population Percentage?***

*at first let's find the population percentage of each continent*

continent_data_percentage = world_population.groupby('Continent')['World Population Percentage'].sum().sort_values(ascending= False).reset_index()
continent_data_percentage

***Explanation of The Functions I uesed.***


**GroupBy():is used to split the data into groups based on some criteria.**

    
**Sum(): is usef fo calculating total.**


**sort():is used to sort elements in ascending or decending order based on your condition.***
    
**reset_index(): is used if the DataFrame has a MultiIndex.**

Continent_percentage= continent_data_percentage['Continent']
Continent_percentage

population_percentage = world_population["World Population Percentage"]
population_percentage

# Inferences and Conclusion

**From The previous Visualization and Analysing we Can Concluse that:**
* Number of Population in the world will be Increase.
* Based on this increasing, Area of Agricultural land will be decrease and will be used in building.
* Number of Bectera and viruses will be increase and it will be lead to spread of disease.
* Based on Population increasing, Resources will be decrease and to avoid that each country should be work hard to produce more
and more.
* Increase in Wars and Famines.

# Future Work

* More pretty seaborn graph.
* Use Maps.

# References

* https://www.kaggle.com/datasets/iamsouravbanerjee/world-population-dataset


