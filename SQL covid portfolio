 select * from portfolioproject..['covid death$']
 where continent is not null
 order by 2,3

select * from portfolioproject..['covid vaccination$']

select location from portfolioproject..['covid death$']

-- data we are going to use

select location, date, total_cases, new_cases, total_deaths, population
from portfolioproject..['covid death$']
order by 1,2


--looking at total cases vs total deaths
select location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as deathpercentage
from portfolioproject..['covid death$']
where location like '%Germany%'
order by 1,2


--looking at total cases vs population 
-- % of population got covid

Select location, date, total_cases, total_deaths,(total_cases/population)*100 as pecentageOfCases 
from portfolioproject..['covid death$']
where location like '%Germany%'
Order by 1,2


-- countries with highest infection rate compared to population

Select location, population, MAX(total_cases) as highestInfectionCount, max(total_cases/population)*100 
as percentMaxInfected
from portfolioproject..['covid death$']
--where location like '%Germany%'
group by population, location
Order by percentMaxInfected desc


--showing countries with highest death count(where continent is not null)

select location, MAX(cast(total_deaths as int)) as TotalDethcount
from portfolioproject..['covid death$']
--where location like '%germany%'
where continent  is not null
Group by location
order by TotalDethcount desc
 
 --(where continent is null) 

 select location, MAX(cast(total_deaths as int)) as TotalDethcount
from portfolioproject..['covid death$']
--where location like '%germany%'
where continent  is null
Group by location
order by TotalDethcount desc
 
-- breaking things by continent

select continent, MAX(cast(total_deaths as int)) as TotalDethcount
from portfolioproject..['covid death$']
--where location like '%state%'
where continent  is not null
Group by continent
order by TotalDethcount desc

--showing the continent with highest death count

select date, total_cases, total_deaths, (total_deaths/total_cases)*100 as deathPercentage
from portfolioproject..['covid death$']
--where location like '%state%'
where continent  is not null
order by 1,2

--global number

select  SUM(new_cases)as Totalcases, SUM(cast (new_deaths as int)) as Totaldeath, 
SUM(cast(new_deaths as int))/SUM(new_cases)*100 as dethpercentage
from portfolioproject..['covid death$']
where continent is not null
--Group by date
order by 1,2


--Select location, sum(cast(new_deaths as int)) as TotalDeathCount
--From portfolioproject..['covid death$']
----Where location like '%states%'
--Where continent is null
--and location not in ('World','European Union','International' )
--Group by location
--order by TotalDeathCount



--Select location, population, MAX(total_cases)as HighestInfectioncount, MAX((total_cases/population)) as percentpopultionInfected
--From portfolioproject..['covid death$']
----Where location like '%states%'
--Group by location, population
--order by percentpopultionInfected

--Select location, population,date,Max(total_cases) as HighestInfectionCount, MAX((total_cases/population))*100 as PercentPopulationInfected
--From portfolioproject..['covid death$']
----Where location like '%states%'
--Group by location, population, date
--order by PercentPopulationInfected



--joint the report of covid death vs covid vaccination

Select*
From portfolioproject..['covid death$'] dea
join portfolioproject..['covid vaccination$'] vac
	on dea.location =vac.location
	and dea.date =vac.date

Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations 
From portfolioproject..['covid death$'] dea
join portfolioproject..['covid vaccination$'] vac
	on dea.location =vac.location
	and dea.date =vac.date

	
--looking at the total population vs vaccination

With popvsvac (continent, Location, date, population, new_vaccinations, RollingPeopleVaccinated)
as
--popvsvac --> population vs vaccination 
(
Select dea.continent, dea.location,dea.date, dea.population, vac.new_vaccinations
, SUM(convert(int,vac.new_vaccinations)) over (partition by dea.location, dea.date) as RollingPeopleVaccinated
-- we can use(convert or cast)to change the type.
From portfolioproject..['covid death$'] dea
join portfolioproject..['covid vaccination$'] vac
	on dea.location= vac.location
	and dea. date=vac.date
where dea.continent is not null
--order by 2,3
)
select*,(RollingPeopleVaccinated/population)*100
From popvsvac

--TEMP TABLE

Drop table if exists #percentPopulationVaccinated
Create Table #percentPopulationVaccinated
(
Continent nvarchar(255),
Loction nvarchar(255),
Date datetime,
population numeric,
New_vaccination numeric,
RollingPeopleVaccinated numeric 
)

--Temp table
Drop table if exists #percentPopulationVaccinated
create Table #percentPopulationVaccinated
(continent nvarchar(255),
Location nvarchar(255),
date datetime,
population numeric,
New_vaccination numeric,
Rollingpeoplevaccinated numeric
)

Insert into #percentPopulationVaccinated
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(convert(int,vac.new_vaccinations)) over (partition by dea.location, dea.date) as RollingPeopleVaccinated
-- we can use(convert or cast)to change the type.
From portfolioproject..['covid death$'] dea
join portfolioproject..['covid vaccination$'] vac
	on dea.location= vac.location
	and dea. date=vac.date
where dea.continent is not null
--order by 2,3

select*,(RollingPeopleVaccinated/Population)*100
From #percentPopulationVaccinated


--Creating data to store later for visualiazations

Create View PercentPopulationVaccinated as
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(convert(int,vac.new_vaccinations)) over (partition by dea.location order by dea.location, dea.date) as RollingPeopleVaccinated
-- we can use(convert or cast)to change the type.
From portfolioproject..['covid death$'] dea
join portfolioproject..['covid vaccination$'] vac
	on dea.location= vac.location
	and dea. date=vac.date
where dea.continent is not null
--order by 2,3

select*
From percentPopulationVaccinated







