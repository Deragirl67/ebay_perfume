# Ebay_Unisex_Perfume
### Introduction
The Ebay Unisex Perfume dataset is obtained from the ebay e-commerce site showing the male and female categories of perfumes sold. The dataset comprises 65,000 rows and 5 columns
### Data Processing
- Changing of data type: changed available(column) data type to Nvarchar
- Filling the null: filled up the empty rows with appropriate values or context
- Removing invalid tests
### Exploratory Data Analysis
-TOTAL PRICE OF BOTH PERFUME (MALE AND FEMALE)
- COUNTED BRANDS THAT BELONG TO BOTH MALE AND FEMALE PERFUME
- HIGHLIGHTED BRAND WITH THE MOST EXPENSIVE MALE AND FEMALE PERFUME AND ALSO THEIR ITEM LOCATIONS
- SORTED THE TYPE OF MALE AND FEMALE PERFUME EBAY SHOULD SELL MORE
### Data Analysis
This includes SQL queries that were used to run the analysis

```SQL

select*
  FROM[dbo].[ebay_mens_perfume$]

  ---check for null values in brands
  SELECT brand, title, type
  FROM[dbo].[ebay_mens_perfume$]
 where brand is null

 ---replace null value for versace
update[dbo].[ebay_mens_perfume$]
 set brand= ISNULL(brand, 'Versace')

 ---cross check for column filled with characters '/'
 select brand,Type, title
 from[dbo].[ebay_mens_perfume$]
 where type= '/'

 ---replace '/' to a text
update[dbo].[ebay_mens_perfume$]
set type= REPLACE(type,'/', 'Eau de Parfum')  

select brand,type, title
from[dbo].[ebay_mens_perfume$]
where brand= 'unbranded'

---replace 'y' to brand type
update[dbo].[ebay_mens_perfume$]
set type= REPLACE(type, 'y', 'Eau de Perfume')

--- i checked for null values in title column which i found out that therem is no 'null' values there
select brand, type, title
from[dbo].[ebay_mens_perfume$]
where title is null

---i checked for 'null' in type column and i found just ''2'' 
select brand, type, available
from[dbo].[ebay_mens_perfume$]
where type is null

--- i checked for the brand which is 'Calvin Klein' which type has a 'null' value and i found just ''1''
select brand, type, available, title
from[dbo].[ebay_mens_perfume$]
where brand= 'Calvin Klein'

--- to replace 'null' values for ''Calvin Klein'' in type(column)
 update[dbo].[ebay_mens_perfume$]
 set type= 'Perfume'
 where type is null
 and brand= 'Calvin Klein'

 --- i checked for the brand which is 'As Show' which type has a 'null' value and i found just ''1''
 select brand, type, available, title
 from[dbo].[ebay_mens_perfume$]
 where brand= 'AS SHOW'
 update[dbo].['ebay_womens_perfume(1)$']
set lastUpdated= SUBSTRING(lastupdated,0,len(lastupdated)-3)
select brand, replace(itemlocation, 'USA', 'united states') as itemlocation
from[dbo].['ebay_womens_perfume(1)$']
---replace null value with the context(female perfume)
select available, availabletext, isnull(available, 1)available_text
from[dbo].['ebay_womens_perfume(1)$']
where availableText like 'last one%'

select available, availabletext, isnull(available, 0)available_text
from[dbo].['ebay_womens_perfume(1)$']
where availableText like 'out of stock%'

select available, availabletext, isnull(available, 4)available_text
from[dbo].['ebay_womens_perfume(1)$']
where availableText like 'Limited quantity available%'

select available, availabletext, isnull(available, 5)available_text
from[dbo].['ebay_womens_perfume(1)$']
where availabletext like '%disponible%'


select*
from[dbo].['ebay_womens_perfume(1)$']
where availabletext like '%disponible%'

select available, availableText, ISNULL(available, 4) avalable_text
from[dbo].['ebay_womens_perfume(1)$']
where availableText like '3 lots available%'

update[dbo].['ebay_womens_perfume(1)$']
set availableText= isnull (availableText, 'unavailable')

update[dbo].['ebay_womens_perfume(1)$']
set type= isnull (type,'bodyspray')
update[dbo].['ebay_womens_perfume(1)$']
set type= 'perfume'
where brand= 'unbranded'

update[dbo].['ebay_womens_perfume(1)$']
set lastUpdated= isnull(lastupdated, 'Un_updated')

update[dbo].['ebay_womens_perfume(1)$']
set lastUpdated= ''
where lastUpdated= 'Shiyaaka for Men EDP Spray 100ML (3.4 FL.OZ) By Khadlaj (Woody, Aromatic, Earth)'

update[dbo].['ebay_womens_perfume(1)$']
set lastUpdated= ''
where lastUpdated= 'un_upd'

---convert lastUpdated to date and time
select lastUpdated, convert (datetime, lastUpdated) as new_lastUpdated
from[dbo].['ebay_womens_perfume(1)$']

select*
from[dbo].['ebay_womens_perfume(1)$']

---cast(to change data type)
select available, CAST(available as nvarchar(30)) as new_available
from[dbo].['ebay_womens_perfume(1)$']

select available, isnull(CAST( available as nvarchar(30)),'not avalaible') as new_available
from[dbo].['ebay_womens_perfume(1)$']

---convert(to change data type)
select available, CONVERT(nvarchar(30),available) as new_available
from[dbo].['ebay_womens_perfume(1)$']

select available, isnull(convert (nvarchar(30), available), 'not available') as new_available
from[dbo].['ebay_womens_perfume(1)$']

update[dbo].['ebay_womens_perfume(1)$']
set lastUpdated= IIF(lastUpdated = '1900-01-01 00:00:00.000', null, lastupdated)

TOTAL PRICE OF BOTH PERFUME (MALE AND FEMALE)*/
SELECT SUM(PRICE) TOTAL_PRICE, brand
FROM ebay_mens_perfume$
GROUP BY brand
ORDER BY TOTAL_PRICE DESC

SELECT brand, SUM(PRICE) TOTAL_PRICE
FROM ['ebay_womens_perfume(1)$']
GROUP BY brand
ORDER BY TOTAL_PRICE DESC

---COUNT BRAND THAT BELONG TO BOTH MALE AND FEMALE PERFUME
SELECT BRAND, COUNT(BRAND) COUNT_PER_BRAND
FROM ebay_mens_perfume$
GROUP BY brand
ORDER BY COUNT_PER_BRAND DESC

SELECT BRAND, COUNT(BRAND) COUNT_PER_BRAND
FROM ['ebay_womens_perfume(1)$']
GROUP BY brand
ORDER BY COUNT_PER_BRAND DESC

---FILTER FOR NULL VALUES IN BRAND COLUMN
SELECT brand, title
FROM ['ebay_womens_perfume(1)$']
WHERE brand IS NULL

---REPLACE A NULL VALUE WITH A BRAND NAME
UPDATE ['ebay_womens_perfume(1)$']
SET BRAND = ISNULL (BRAND, 'VERSACE')

---WHAT BRAND IS THE MOST EXPENSIVE MALE PERFUME AND THEIR ITEM LOCATION
SELECT brand, itemLocation, MAX(price) MOST_EXPENSIVE_BRAND
FROM ebay_mens_perfume$
GROUP BY BRAND, itemLocation
ORDER BY MOST_EXPENSIVE_BRAND DESC

---WHAT BRAND IS THE MOST EXPENSIVE FEMALE PERFUME AND THEIR ITEM LOCATION  
SELECT BRAND, itemLocation, MAX(PRICE) MOST_EXPENSIVE_BRAND
FROM ['ebay_womens_perfume(1)$']
GROUP BY brand, itemLocation
ORDER BY MOST_EXPENSIVE_BRAND DESC

/*5. WHICH TYPE OF PERFUME SHOULD EBAY SELL MORE(MEN PERFUME)
EBAY COMPANY SHOULD SELL MORE OF 'GIORGIO ARMANI'  MEN PERFUME  BECAUSE IT HAS THE HIGHEST SALES AND GENERATED MORE FUNDS TO THE COMPANY*/
SELECT brand, COUNT(SOLD) AS TOTAL_SALES, SUM(PRICE) AS TOTAL_REVENUE
FROM ebay_mens_perfume$
GROUP BY BRAND
ORDER BY TOTAL_SALES DESC, TOTAL_REVENUE DESC

/*6. WHICH TYPE OF PERFUME SHOULD EBAY SELL MORE(WOMEN PERFUME)
EBAY COMPANY SHOULD SELL MORE OF 'Lancôme' WOMEN PERFUME BECAUSE IT HAS THE HIGHEST SALES, ALTHOUGH THE REVENUE GENERATED IS LESS COMPARE TO 'Jo Malone' BRAND WHICH GENERATED MORE REVENUE BUT LESSER SALES*/
SELECT brand, COUNT(SOLD) AS TOTAL_SALES, SUM(PRICE) AS TOTAL_REVENUE
FROM ['ebay_womens_perfume(1)$']
GROUP BY BRAND
ORDER BY TOTAL_SALES DESC, TOTAL_REVENUE DESC

### Findings and Results
According to my analyses in ebay company, i discovered that the most expensive male perfume brand in this company is 'CREED' which was sold at $259.09 each while the most expensive brand for the female perfume is 'MICHEAL KORS' and it was sold at $299.99 each.
after running my analyses, the results show that EBAY company should sell more of 'Giorgio Armani' male perfume because it has the highest of sales and also generated more funds to the company, While for the female perfume the brand or type of perfume the company should sell more is 'Jo Malone' reason is that this brand has the highest Revenue but lesser sales, and generating more revenue to the company with fewer sales, indicates higher profit margins, which means that each of the sales(jo malone) products contributes greatly to the company buttom line. compare to 'Lancôme' which makes highest sales but generated lesser revenue to the company.

