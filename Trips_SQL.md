# Trips_SQL
The Trips table stores information about trips taken by various modes of travel and their respective fares. Each trip, owing to its mode has some relevant information specific to the mode. For instance, a car trip has the rental company and the mileage, where as train trip is characterized by the trainspeed, type, coach, and number of stops. The TravelCompany is interested in answering the following queries. 

## Q1. List car rental companies which have a mileage of at least 27 miles/gallon. 
```
SELECT distinct RentalCompany FROM BYCAR
WHERE Mileage>=27;
```

## Q2. List trip IDs taken on train costing strictly more than $150.
```
SELECT TRIPS.TID FROM TRIPS INNER JOIN BYTRAIN
ON TRIPS.TID=BYTRAIN.TID
WHERE FARE>150;
```

## Q3.Find trip IDs and their fare , that are not taken in the US i.e., 'Non-US' trips. 
```
SELECT TID, FARE FROM TRIPS
WHERE TRIPSTATE ='Non-US';
```

## Q4.Find the business class plane trip IDs that are greater than $1000.
```
SELECT BYPLANE.TID FROM BYPLANE
INNER JOIN TRIPS
ON TRIPS.TID=BYPLANE.TID
WHERE BYPLANE.CLASS='Business'
AND TRIPS.FARE > 1000;
```

### Q5 List pairs of distinct trips that have exactly the same value of mileage.Note a pair of distinct trips is of the format: (TID1, TID2).This distinct pair is not the same as the pair (TID2, TID1)
```
SELECT distinct a.TID AS TID1, b.TID AS TID2
FROM BYCAR a, BYCAR b
WHERE a.MILEAGE=b.MILEAGE AND a.TID != b.TID;
```

### Q6.List pairs of distinct train trips that do not have the same speed.
```
SELECT  distinct a.TID AS TID1,b.TID AS TID2
FROM BYTRAIN A, BYTRAIN B
WHERE a.TRAINSPEED != b.TRAINSPEED;
```

### Q7.Find those pair of trips in the same state with the same mode of travel. List such pairs only once. In other words, given a pair (TID1,TID2) do NOT list (TID2,TID1).
```
SELECT a.TID AS TID1,b.TID AS TID2
FROM TRIPS a, TRIPS b
WHERE a.TRIPSTATE = b.TRIPSTATE
AND a.TRAVELMODE = b.TRAVELMODE
AND a.TID <>b.TID
AND a.TID >b.TID;
```

### Q8.Find a state in which trips have been taken by all three modes of transportation: train, plane, and car.
```
SELECT distinct a.TRIPSTATE
FROM TRIPS a, TRIPS b, TRIPS c
WHERE a.TRIPSTATE = b.TRIPSTATE AND b.TRIPSTATE = c.TRIPSTATE
AND a.TRAVELMODE != b.TRAVELMODE AND b.TRAVELMODE != c.TRAVELMODE AND a.TRAVELMODE!=c.TRAVELMODE;
```
### Q9.Find the details of a) the most costly trip,b) the cheapest trip, regardless of the travel mode. Write two separate queries for (a) and (b). Write both (a) and (b) as a self-join with basic SQL operators(Filter, Project, Rename, Join (cross-join, natural join), Union, Intersect, and Difference). Do not use ALL, ANY, DISTINCT, GROUP BY, HAVING, MAX, MIN, ORDER BY.
```
--a) the most costly trip
SELECT a.TID, a.TRAVELMODE,a.TRIPSTATE,a.FARE
FROM TRIPS a, TRIPS b
Where a.FARE > b.FARE
and rownum = 1

--b) the cheapest trip, regardless of the travel mode
SELECT a.TID, a.TRAVELMODE,a.TRIPSTATE,a.FARE
FROM TRIPS a, TRIPS b
Where a.FARE < b.FARE
and rownum = 1
```



