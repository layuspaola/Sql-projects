Data Obtained from https://ourworldindata.org/covid-deaths

Research and data: Edouard Mathieu, Hannah Ritchie, Lucas Rodés-Guirao, Cameron Appel, Daniel Gavrilov, Charlie Giattino, Joe Hasell, Bobbie Macdonald, 
Saloni Dattani, Diana Beltekian, Esteban Ortiz-Ospina, and Max Roser.

as it is a large dataset with multiples rows i decided to divide it by deaths and vaccination
I used Google Bigquery
As i am from Argentina the focus will be on my country, continent

link to bigquery consults https://console.cloud.google.com/bigquery?sq=684960940844:d674d18e8ad64ccabaec017a586b8c4d

#Covid 19 Data Exploration

### Table ordered by continent and location

``` sql
SELECT * FROM `mi-1er-proyecto-382317.covid.covid_deaths` 
order by continent, location
```

### Table ordered by continent and location

```sql
SELECT * FROM `mi-1er-proyecto-382317.covid.covid_deaths` 
order by continent, location
```


### show covid death table ordered by location and date

```sql
SELECT * FROM `mi-1er-proyecto-382317.covid.covid_deaths` 
where continent is not null
order by location, date
```

### After Exploration 
I decide to start with selecting the data to work with

```sql
SELECT Location, date, total_cases, new_cases, total_deaths, population
FROM `mi-1er-proyecto-382317.covid.covid_deaths` 
where continent is not null
order by location, date
```


## Total Cases vs Total Deaths
#### Shows likelihood of dying if you contract covid in your country where the cases are more that 0

```sql
Select Location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
FROM `mi-1er-proyecto-382317.covid.covid_deaths`
Where location like '%Arg%'
and total_cases is not null 
order by location, date
```

### Countries with the highest infection compared with their population

```sql
Select Location, Population, MAX(total_cases) as infection_highest, Max((total_cases/population))*100 as pop_infected
FROM `mi-1er-proyecto-382317.covid.covid_deaths`
Group by Location, Population
order by pop_infected desc
```

### Countries that had the more deaths compared with their population

```sql
Select Location, MAX(cast(Total_deaths as int)) as Total_Death
FROM `mi-1er-proyecto-382317.covid.covid_deaths`
Where continent is not null 
Group by Location
order by Total_Death desc
```


### Ordered Death_count by continent

```sql
Select continent, MAX(cast(Total_deaths as int)) as Total_Death
FROM `mi-1er-proyecto-382317.covid.covid_deaths`
Where continent is not null 
Group by continent
order by Total_Death desc
```


### Now i start the vaccines exploration by joining the tables
once joined the tables i proceed to select the data of when the countries started to vaccinate, the total vaccines orderd by continent, location and date

```select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations 
FROM `mi-1er-proyecto-382317.covid.covid_deaths`  dea
join `mi-1er-proyecto-382317.covid.covid_vacs` vac
on dea.location = vac.location 
and dea.date = vac.date
where dea.continent is not null
and vac.new_vaccinations is not null
order by 1,2,3
```
