# laid off dataset cleaning

## Table of content 
- [Project overview](Project-overview)
- [Data Cleaning ](Data-Cleaning)
- [Codes](Codes) 
### Project overview 
Employing SQL, this data cleaning project focuses on refining a dataset concerning total laid-off employees spanning the years 2019 to 2023. The process involves meticulous examination and manipulation of the data to ensure accuracy and consistency.

### Data source 
Laid off data : the main dataset used for this project is the "layoffs.csv" containing detailed informations about the total laid off number made by various companies .

### Tools 
- SQL Server

### Data Cleaning 
- Identifying and rectifying inconsistencies
- Handling missing values / null values
- Standardizing formats and removing duplicates

### Codes

---SQL
SELECT * ,
ROW_NUMBER() OVER (
PARTITION BY  company, industry,total_laid_off ,percentage_laid_off ,`date`) as row_num
FROM layoffs_staging
---
WITH duplicate_cte AS
(
SELECT * ,
ROW_NUMBER() OVER (
PARTITION BY  company,location,stage,country,funds_raised_millions,industry,
total_laid_off,percentage_laid_off,`date`) AS row_num
FROM layoffs_staging
)
SELECT *
FROM duplicate_cte 
WHERE row_num > 1 ;
---
SELECT *
FROM layoffs_staging
WHERE company = '100 Thieves';
---
CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `row_num` int ,
  `funds_raised_millions` int DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
---
INSERT INTO layoffs_staging2
SELECT * ,
ROW_NUMBER()OVER (
 PARTITION BY stage,country,funds_raised_millions) AS row_num
FROM layoffs_staging;
---
DELETE
FROM layoffs_staging2
WHERE row_num > 1;
---

  
