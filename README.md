# Data-Cleaning-MySQL-
This project focuses on data cleaning and preparing of real-world dataset using MySQL.

The project includes:
- Remove Duplicates
- Standardize the data
- Null, value or blank values
- Remove any columns or Rows

## 🛠️ Tools & Technologies

- MySQL
- MySQL Workbench

## 📂 Dataset
The dataset contains company layoffs information as:
- Company name
- Industry
- Location
- Total layoffs
- Percentage laid off
- Funding raised
- Stage
- Country
- Data

## 1. Creating a Staging Table
A staging table was created to preserve the original raw dataset during the cleaning process.

```sql
CREATE TABLE layoffs_staging
LIKE layoffs;
```

Data copy into the staging table:
```sql
INSERT layoffs_staging
SELECT *
FROM layoffs;
```

## 2. Removing Duplicate Records

Duplicate rows were identified using the `ROW_NUMBER()` window function.

```sql
ROW_NUMBER() OVER(
PARTITION BY company, location, industry,
total_laid_off, percentage_laid_off,
date, stage, country, funds_raised_millions
)
```

## 3. Standardizing Data

Several data-cleaning operations were performed:

### ✅ Trimming Extra Spaces

```sql
UPDATE layoffs_staging2
SET company = TRIM(company);
```

## 4. Formatting Date Columns

The `date` column was converted from text format into MySQL `DATE` format.

```sql
STR_TO_DATE(date, '%m/%d/%Y')
```

The column type was then modified:

```sql
ALTER TABLE layoffs_staging2
MODIFY COLUMN date DATE;
```

---

## 5. Handling Null & Blank Values

### ✅ Identifying Missing Values

Missing values in the `industry` column were identified and updated using self joins based on matching company names.

### ✅ Removing Invalid Rows

Rows where both:
- `total_laid_off`
- `percentage_laid_off`

were NULL were removed.

```sql
DELETE
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;
```

---

## 6. Removing Unnecessary Columns

The helper column `row_num` used during duplicate removal was dropped after cleaning.

```sql
ALTER TABLE layoffs_staging2
DROP COLUMN row_num;
```

---

# 📊 Key SQL Concepts Used

- Common Table Expressions (CTEs)
- Window Functions (`ROW_NUMBER`)
- Joins
- Data Standardization
- NULL Handling
- Date Formatting
- Table Alteration
- Data Cleaning Best Practices

---
