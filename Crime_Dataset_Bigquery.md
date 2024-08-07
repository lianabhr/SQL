## SQL Timestamp
- Categorizing timestamp in 'date' column.

```
SELECT
  date
  ,EXTRACT(MONTH FROM date) AS crime_month
  ,EXTRACT(HOUR FROM date) AS crime_hour
  ,FORMAT_DATE('%b, %Y',date) AS crime_month_str
  ,CASE 
    WHEN EXTRACT(HOUR FROM date) BETWEEN 6 AND 12 THEN "Morning"
    WHEN EXTRACT(HOUR FROM date) BETWEEN 13 AND 16 THEN "Afternoon"
    WHEN EXTRACT(HOUR FROM date) BETWEEN 17 AND 20 THEN "Evening"
    WHEN EXTRACT(HOUR FROM date) BETWEEN 21 AND 23 THEN "Night"
    WHEN EXTRACT(HOUR FROM date) BETWEEN 0 AND 5 THEN "Midnight"
  ELSE NULL END AS crime_time_category
FROM
  `source.crime_dataset_20240515`
```

## SQL daily
- Separating timestamp in 'date' column date into daily

```
SELECT id,
  CURRENT_DATE() AS day,
  FORMAT_DATE('%A', CURRENT_DATE()) AS weekday_name_full
  FROM `source.crime_dataset_20240515`;
```

## Fix location data in 'location' column
- Looker Studio's Google Map chart only accept coordinates with comma in between latitude and longitude

```
SELECT DISTINCT
location
,REPLACE(REPLACE(location,"(",""), ")", "") AS new_location
FROM
`source.crime_dataset_20240515`
```

# Inner Join
- combining two table

```
SELECT *
FROM `ds360-jul2024-liana.source.crime_month` as UN_SDG
INNER JOIN `ds360-jul2024-liana.source.crime_gps` as WB_WDI on WB_WDI.id = UN_SDG.id
```
