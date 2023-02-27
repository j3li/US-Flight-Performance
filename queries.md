# Queries I Performed For Analysis

I've listed the columns in the database I performed queries on. I've split some columns from the dataset for better analysis.
| year  | month | carrier | carrier_name | airport | location | city | state | airport_name | arr_flights | arr_del15 | carrier_ct | weather_ct | nas_ct | security_ct | late_aircraft_ct | arr_cancelled | arr_diverted | arr_delay | carrier_delay | weather_delay | nas_delay | security_delay | late_aircraft_delay
| ----- | ------| ------- | ------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| year | month | carrier | airline | airport code  | city, state/territory | city  | state/territory  | airport name | # of flights | # of flights delayed | # of carrier delay | # of weather delays | # of nas delays | # of security delays | # of late aircraft delays | # of flights cancelled | # of flights diverted | time delayed (minutes) | carrier delayed (minutes)| weather delayed (minutes) | nas delayed (minutes) | security delayed (minutes) | aircraft delayed (minutes) |



***

## Which states had the most flight traffic?
```sql
SELECT state, SUM(arr_flights) AS flights
FROM delays
GROUP BY state
ORDER BY flights DESC;
```
![image](https://user-images.githubusercontent.com/50200083/221445777-e9700fcf-db55-4ad6-83ef-147e5ec49d21.png)

## Which airports get the most traffic?
```sql
SELECT airport_name AS airport, SUM(arr_flights) AS flights
FROM delays
GROUP BY airport_name
ORDER BY flights DESC;
```
![image](https://user-images.githubusercontent.com/50200083/221445916-e8f01231-59db-4b3f-90b7-95d8e32e7689.png)

## Which airlines have the most flights?
```sql
SELECT carrier_name AS airline, SUM(arr_flights) AS flights
FROM delays
GROUP BY airline
ORDER BY flights DESC;
```
![image](https://user-images.githubusercontent.com/50200083/221446134-df41893a-1fb2-4d77-aaf2-a52e59922c41.png)


## What are the average flights per month?
```sql
SELECT month, SUM(arr_flights)/5 AS average_flights
FROM delays
GROUP BY month
ORDER BY month ASC;
```
![image](https://user-images.githubusercontent.com/50200083/221445687-8ccd8ae1-bb1c-40dc-923d-c3a0b056d8ed.png)

## What percentage of flights are delayed month?
```sql
SELECT month, ROUND(SUM(arr_del15)/SUM(arr_flights), 3) AS percent_of_flights_delayed
FROM delays
GROUP BY month
ORDER BY month ASC;
```
![image](https://user-images.githubusercontent.com/50200083/221448100-6dcfc842-ebee-42f9-9223-d52a85b8e6ad.png)


## Which airports have the highest percentage of flights delayed?
```sql
SELECT airport_name AS airport, ROUND(SUM(arr_del15)/SUM(arr_flights), 3) AS percent_of_flights_delayed
FROM delays
GROUP BY airport_name
ORDER BY percent_of_flights_delayed DESC;
```
![image](https://user-images.githubusercontent.com/50200083/221448201-490722ca-776d-4cb1-a4ee-22c5591d8020.png)




