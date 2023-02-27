# Queries I Performed For Analysis

I've listed the columns in the database I performed queries on. I've split some columns from the dataset for better analysis. Most screenshots are cropped images of the entire query result because of size.
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

## Which airlines have the highest percentage of flights delayed?
```sql
SELECT carrier_name AS airline, ROUND(SUM(arr_del15)/SUM(arr_flights), 3) AS percent_of_flights_delayed
FROM delays
GROUP BY carrier_name
ORDER BY percent_of_flights_delayed DESC;
```
![image](https://user-images.githubusercontent.com/50200083/221448361-ea4e19c7-d22d-4238-b115-f4c430ecc8f6.png)

## What is the distribution of causes of delays?
```sql
WITH totals as 
(
 SELECT SUM(carrier_ct) AS carrier_tot, SUM(weather_ct) AS weather_tot,
        SUM(nas_ct) AS nas_tot, SUM(security_ct) AS security_tot,
        SUM(late_aircraft_ct) AS late_aircraft_tot, SUM(arr_del15) AS arr_del15_tot
 FROM delays
)
SELECT ROUND(carrier_tot/arr_del15_tot,3) AS carrier_percent,
       ROUND(weather_tot/arr_del15_tot,3) AS weather_percent,
       ROUND(nas_tot/arr_del15_tot,3) AS nas_percent,
       ROUND(security_tot/arr_del15_tot,3) AS security_percent,
       ROUND(late_aircraft_tot/arr_del15_tot,3) AS later_aircraft_percent
FROM totals;
```
![image](https://user-images.githubusercontent.com/50200083/221454373-446f1354-3a82-4704-93d6-a7c7498244d4.png)



