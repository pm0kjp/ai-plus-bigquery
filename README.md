# AI+ BigQuery First Steps

This repository holds links to the materials used in the AI+ BigQuery First Steps course.

First, a few notes for success in the course.

* Please use a computer and network that will allow you to use Google products (gmail, Google Drive, etc.).  Some workplaces do not allow this on their devices!
* If you don't already have a personal (not work) Google Cloud Platform account, you will set one up in the first section of today's course.  You will need a mobile phone number and a credit card, but will not be charged.  If you're an established GCP user, you might be past your "free tier" usage this month and may accrue a small charge for today's work, but it's unlikely.

* [Slideshow in Google Drive](https://docs.google.com/presentation/d/1PD4Gkp7Sl9aqs3WygG8iTlAceTiCE1rq21YGTcAEzQQ/edit?usp=sharing) or https://bit.ly/ai-plus-bq

## Links in Hour 1 Slides:

* https://accounts.google.com to make a new identity in Google
* https://console.cloud.google.com: Google Cloud Platform console
* https://cloud.google.com/products/databases: GCP Data solutions


## Code and Links in Hour 2 Slides:

Exercise 1 Solution:

```
SELECT *
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE
   complaint_type = "Facades"
```

OR

```
SELECT count(*) as num_facades_complaints
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE
   complaint_type = "Facades"
```

Exercise 3 Solution:

```
SELECT city
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE
   complaint_type = "Facades" AND
   status = "Closed"
```

OR  

```
SELECT DISTINCT city
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE
   complaint_type = "Facades" AND
   status = "Closed"
```

"Practical Example" for Saving Views:

```
SELECT
   agency,
   complaint_type,
   count(*) AS num_complaints
FROM `bigquery-public-data.new_york.311_service_requests`
GROUP BY agency, complaint_type
ORDER BY num_complaints DESC
```

Aggregation example:

```
SELECT
   MIN(created_date)
FROM `bigquery-public-data.new_york.311_service_requests`;
```

Aggregation links:

https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions
https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_analytic_functions

More Aggregation Examples:

```
SELECT
     STRING_AGG(DISTINCT landmark, ", ") AS landmark_list
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE borough = "BRONX";
```

```
SELECT
 AVG(latitude) AS lat,
 AVG(longitude) AS long,
FROM `bigquery-public-data.new_york.311_service_requests`;
```

```
SELECT
 MAX(created_date) AS latest_complaint,
 COUNTIF(borough = "BRONX") AS bronx_n,
 COUNTIF(borough = "MANHATTAN") AS manhattan_n,
 COUNT(*) AS total_n
FROM `bigquery-public-data.new_york.311_service_requests`;
```

Grouping Example:

```
SELECT
 city,
 count(*) as n
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE
   complaint_type = "Facades" AND
   status != "Closed"
GROUP BY city;
```

Grouping and Aggregation Examples

```
SELECT
   agency,
   borough,
   MIN(created_date) as first,
   MAX(created_date) as latest
FROM `bigquery-public-data.new_york.311_service_requests`
GROUP BY
   agency,
   borough;
```

```
SELECT
   complaint_type,
   EXTRACT(year FROM created_date) as year,
   COUNTIF(status = "Closed") AS closed_tickets,
   COUNTIF(status != "Closed") AS non_closed_tickets
FROM `bigquery-public-data.new_york.311_service_requests`
GROUP BY
   complaint_type,
   year;
```

Exercise 1 Solution:

```
SELECT
 COUNTIF(incident_zip IS NULL) as missing_zips,
 COUNTIF(incident_zip IS NULL)/COUNT(*) as proportion_missing
FROM `bigquery-public-data.new_york.311_service_requests`
```

Exercise 2 Solution:

```
SELECT
 open_data_channel_type,
 count(*) as num_service_requests
FROM `bigquery-public-data.new_york.311_service_requests`
WHERE complaint_type = "Graffiti"
GROUP BY open_data_channel_type
```

What Constitutes a Join? queries

```
SELECT DISTINCT species_scientific_name
FROM `bigquery-public-data.new_york.tree_species`
ORDER BY species_scientific_name
```

```
SELECT DISTINCT spc_latin
FROM `bigquery-public-data.new_york.tree_census_2015`  
ORDER BY spc_latin
```

```
SELECT DISTINCT species_common_name
FROM `bigquery-public-data.new_york.tree_species`
ORDER BY species_common_name
```

```
SELECT DISTINCT spc_common
FROM `bigquery-public-data.new_york.tree_census_2015`  
ORDER BY spc_common
```

Let's Try an Inner Join

```
SELECT
 tree_id,
 species_scientific_name,
 species_common_name,
 status,
 health,
 growth_rate,
 fall_color,
 tree_size,
 zipcode,
 boroname
FROM
 `bigquery-public-data.new_york.tree_species` JOIN `bigquery-public-data.new_york.tree_census_2015`
ON LOWER(species_scientific_name) = LOWER (spc_latin) OR
 LOWER(species_common_name) = LOWER(spc_common);
```

What About a Left Join?

```
SELECT
 tree_id,
 species_scientific_name,
 species_common_name,
 status,
 health,
 growth_rate,
 fall_color,
 tree_size,
 zipcode,
 boroname
FROM
 `bigquery-public-data.new_york.tree_census_2015` LEFT JOIN `bigquery-public-data.new_york.tree_species`
ON LOWER(species_scientific_name) = LOWER (spc_latin) OR
 LOWER(species_common_name) = LOWER(spc_common);
 ```

 Optional Exercise (may or may not be given in class):

 https://data.cityofnewyork.us/Environment/2018-Central-Park-Squirrel-Census-Squirrel-Data/vfnx-vebw
 https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95

 ## Code and Links in Hour 3 Slides:

 Exploring With Data RStudio queries:

 ```
 SELECT
 tree_id,
 species_scientific_name,
 species_common_name,
 status,
 health,
 growth_rate,
 fall_color,
 tree_size,
 zipcode,
 boroname
FROM
 `bigquery-public-data.new_york.tree_census_2015` JOIN `bigquery-public-data.new_york.tree_species`
ON LOWER(species_scientific_name) = LOWER (spc_latin) OR
 LOWER(species_common_name) = LOWER(spc_common);
```

OR

```
SELECT * FROM `your-project-id.my_new_york.tree_inner_join`
```

Colab, in Google's Words

https://colab.research.google.com/

Start With A Colab Example

https://colab.research.google.com/notebooks/bigquery.ipynb

Connecting to BigQuery

https://colab.research.google.com/drive/1TlVsd7hcvA2QVhgII_a_g1kmDbEp8r-a?usp=sharing

Exercise, Part 3, Pandas

https://pandas.pydata.org/pandas-docs/version/0.23.4/generated/pandas.DataFrame.html

What Have We Done? Pandas data frames

https://pandas.pydata.org/docs/reference/frame.html

Strings (optional, may or may not appear in class)

https://pandas.pydata.org/docs/reference/frame.html

```
SELECT
  COUNT(*) as num_oaks
FROM
  `bigquery-public-data.new_york.tree_census_2015`
WHERE
  ENDS_WITH(spc_common, "oak");
```

```
SELECT
  CONCAT(species_scientific_name, " / ", species_common_name)
FROM
  `bigquery-public-data.new_york.tree_species`;
```

```
SELECT
  MAX(CHAR_LENGTH(species_scientific_name)) AS longest_name_length,
  MIN(CHAR_LENGTH(species_scientific_name)) AS shortest_name_length
FROM
  `bigquery-public-data.new_york.tree_species`;
```

Regular Expressions (optional, may or may not appear in class)

https://github.com/google/re2/wiki/Syntax
https://regex101.com/

```
SELECT
  address,
  REGEXP_CONTAINS(address, r"[0-9]+\s+[a-zA-Z0-9\s]+\s+[STREET|AVENUE|DRIVE]") AS is_valid
FROM
  `bigquery-public-data.new_york.tree_census_2015`
ORDER BY is_valid;
```

## Code and Links in Hour 4 Slides:

Pandas Data Viz, hands on!

https://colab.research.google.com/drive/1NLBDcqfYUmw1Xrx4Ub6CJ2H5QcRBQWCL

to add:

```
borough_colors = nyc_trees.groupby(["boroname", "fall_color"]).size()
borough_colors
```

Plotly and Bokeh

https://plotly.com/python/

https://docs.bokeh.org/en/latest/docs/gallery.html#notebook-examples

A Sample Notebook

https://colab.research.google.com/drive/1GU9cVD_ReIlIBDiuTRKNo7jn9Nd8lvYB?usp=sharing

Folium

https://python-visualization.github.io/folium/index.html

Let's Map!

https://colab.research.google.com/drive/1al8XaGfvWYY0Rs4Z9kPTInPnBB1KjrSo?usp=sharing
