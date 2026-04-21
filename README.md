# Uncovering the World's Oldest Businesses

## Overview

This project explores one fascinating question: **What characteristics help a business survive for centuries?**

Using a cleaned dataset originally researched by BusinessFinancing.co.uk, this analysis investigates the **oldest companies still operating in almost every country**. Some businesses have endured wars, regime changes, industrial revolutions, economic crises, and technological disruption—yet remain active today.

By combining multiple datasets through SQL joins and aggregation, this project uncovers patterns about business longevity across continents, countries, and industries.

---

## Objectives

The project focuses on three key questions:

* Which are the **oldest surviving businesses by continent**?
* Which countries currently have **no identified oldest business records** in the dataset?
* Which **business categories** are most likely to survive over centuries?

---

## Dataset Description

The project uses four relational tables:

### `businesses`

| Column        | Description                   |
| ------------- | ----------------------------- |
| business      | Name of the business          |
| year_founded  | Year the business was founded |
| category_code | Business category code        |
| country_code  | ISO country code              |

### `new_businesses`

Contains newly identified businesses using the same structure as `businesses`.

### `countries`

| Column       | Description      |
| ------------ | ---------------- |
| country_code | ISO country code |
| country      | Country name     |
| continent    | Continent name   |

### `categories`

| Column        | Description            |
| ------------- | ---------------------- |
| category_code | Category code          |
| category      | Business category name |

---

## Tools & Skills Used

* **SQL**
* Joins (`INNER JOIN`, `LEFT JOIN`)
* Subqueries
* Aggregations (`MIN`, `COUNT`)
* Data exploration
* Historical business trend analysis

---

## Key Analysis & Findings

---

## 1. Oldest Business in Each Continent

| Continent     | Country   | Business                    | Founded |
| ------------- | --------- | --------------------------- | ------- |
| Africa        | Mauritius | Mauritius Post              | 1772    |
| Asia          | Japan     | Kongō Gumi                  | 578     |
| Europe        | Austria   | St. Peter Stifts Kulinarium | 803     |
| North America | Mexico    | La Casa de Moneda de México | 1534    |
| Oceania       | Australia | Australia Post              | 1809    |
| South America | Peru      | Casa Nacional de Moneda     | 1565    |

### Insights

* **Asia leads globally**, with Japan’s **Kongō Gumi** founded in **578**.
* Europe follows closely with institutions over 1,200 years old.
* Postal systems appear among the oldest organizations in multiple continents.
* Africa and Oceania have comparatively younger surviving businesses in the dataset.

---

## 2. Countries Without Recorded Oldest Businesses

| Continent     | Countries Without Businesses |
| ------------- | ---------------------------- |
| Africa        | 3                            |
| Asia          | 7                            |
| Europe        | 2                            |
| North America | 5                            |
| Oceania       | 10                           |
| South America | 3                            |

### Insights

* **Oceania** has the highest number of countries without entries.
* **Europe** has the most complete historical coverage.
* Missing entries may reflect unavailable records rather than absence of long-standing businesses.

---

## 3. Business Categories Most Suited for Longevity

### Notable Ancient Industries by Region

| Continent     | Oldest Category                  | Founded |
| ------------- | -------------------------------- | ------- |
| Africa        | Postal Service                   | 1772    |
| Asia          | Construction                     | 578     |
| Europe        | Distillers, Vintners & Breweries | 862     |
| North America | Manufacturing & Production       | 1534    |
| Oceania       | Postal Service                   | 1809    |
| South America | Banking & Finance                | 1565    |

### Industry Trends

Businesses that tend to survive longest include:

* **Postal Services**
* **Construction**
* **Manufacturing**
* **Banking & Finance**
* **Food & Beverage**
* **Breweries / Wineries**
* **Hospitality (Hotels / Restaurants)**

### Why These Industries Last

These sectors often survive because they provide:

* Essential public services
* Stable recurring demand
* Strong institutional trust
* Ability to adapt over generations
* Cultural or historical significance

---

## Example SQL Queries Used

### Oldest Business by Continent

```sql
SELECT ct.continent, ct.country, b.business, b.year_founded
FROM businesses AS b
JOIN countries AS ct
ON b.country_code = ct.country_code
JOIN (
    SELECT continent, MIN(year_founded) AS min_year
    FROM businesses
    JOIN countries USING (country_code)
    GROUP BY continent
) AS c
ON ct.continent = c.continent
AND b.year_founded = c.min_year;
```

### Countries Without Businesses

```sql
SELECT ct.continent,
COUNT(*) AS countries_without_businesses
FROM countries ct
LEFT JOIN (
    SELECT country_code FROM businesses
    UNION
    SELECT country_code FROM new_businesses
) ab
ON ct.country_code = ab.country_code
WHERE ab.country_code IS NULL
GROUP BY ct.continent;
```

## Business Categories Best Suited for Longevity

```sql id="4m8s21"
SELECT c.continent,
       cat.category,
       MIN(b.year_founded) AS year_founded
FROM businesses b
INNER JOIN categories cat
    ON b.category_code = cat.category_code
INNER JOIN countries c
    ON b.country_code = c.country_code
GROUP BY c.continent, cat.category
ORDER BY c.continent, cat.category;
```

---

## Final Conclusion

The world’s oldest businesses reveal that **longevity is rarely accidental**. Organizations that survive for centuries usually operate in sectors tied to fundamental human needs—communication, shelter, food, finance, and trade.

While technology changes and empires rise and fall, businesses that continuously adapt while preserving trust and relevance can endure across generations.

In many ways, the oldest businesses are not just companies—they are living institutions.

---

## Author

**Jeremiah Kehinde**
Data Analytics Project | SQL Portfolio

---

## How to Use This Project

1. Clone the repository
2. Load the datasets into your SQL environment
3. Run the queries
4. Explore historical business insights

---

## Repository Tags

`SQL` `Data Analysis` `Business Intelligence` `Historical Data` `Portfolio Project`
