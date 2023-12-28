# SQL-Projects-Misc

Dataset:

Tools:

STEP 1:
--Create database and upload my dataset (Obtained from Office of National Statistics - https://www.ons.gov.uk/).  
The dataset included separate tables for each month.  Due to the size of each .csv file, I decided to only look at the past 6 months (Mar-Aug)
to analyse as part of this project.

    SQL query used - CREATE DATABASE West_Midland_Crime

STEP 2:
--Utilised 'Union All' to combine all the tables into one table to store the combined data

    USE West_Midlands_Crime
    
    SELECT * INTO CombinedCrimeData
    FROM dbo.[2023-03-west-midlands-street]
    UNION ALL
    SELECT * FROM dbo.[2023-04-west-midlands-street]
    UNION ALL
    SELECT * FROM dbo.[2023-05-west-midlands-street]
    UNION ALL
    SELECT * FROM dbo.[2023-06-west-midlands-street]
    UNION ALL
    SELECT * FROM dbo.[2023-07-west-midlands-street]
    UNION ALL
    SELECT * FROM dbo.[2023-08-west-midlands-street];


STEP 3:
Questions I will be answering using SQL:
  1. Most popular locations where crime is committed across the region
  2. Most Common Type of Crime
  3. Geographical Hotspots for Specific Crimes
  4. Outcomes of reported crimes
  5. Correlation Between Crime Type and Outcome
  6. Analysis of how specific crimes trend over time


  QUESTION 1
  
  --Distribution of Crimes by Location:
    SELECT location, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    GROUP BY location
    ORDER BY crime_count DESC;

    Location                                           Crime_count
    -------------------------------------------------- -----------
    On or near unspecified                             9906
    On or near Parking Area                            6036
    On or near Supermarket                             5365
    On or near Shopping Area                           3324
    On or near Petrol Station                          3015
    On or near High Street                             981
    On or near Hospital                                947
    On or near Nightclub                               934
    On or near Sports/Recreation Area                  891
    On or near New Street                              631


QUESTION 2

--Most Common Type of Crime
    SELECT crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    GROUP BY crime_type
    ORDER BY crime_count DESC;

    Crime_type                                         Crime_count
    -------------------------------------------------- -----------
    Violence and sexual offences                       75867
    Vehicle crime                                      17701
    Criminal damage and arson                          14237
    Anti-social behaviour                              13497
    Public order                                       13448
    Other theft                                        12829
    Shoplifting                                        12051
    Burglary                                           9581
    Robbery                                            4412
    Drugs                                              3912
    Possession of weapons                              3522
    Other crime                                        3191
    Theft from the person                              2019
    Bicycle theft                                      1143
    

QUESTION 3:

--Geographical Hotspots for Specific Crimes:
    USE		West_Midlands_Crime
    
    SELECT top 10 location, crime_type, COUNT(*) AS crime_count 
    FROM CombinedCrimeData
    GROUP BY location, crime_type 
    HAVING COUNT(*) > 100 
    ORDER BY crime_count DESC;

  
    Location                                           Crime_type                                         Crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    On or near Unspecified                             Violence and sexual offences                       2516
    On or near Supermarket                             Shoplifting                                        2187
    On or near Parking Area                            Violence and sexual offences                       2021
    On or near Unspecified                             Shoplifting                                        1823
    On or near Unspecified                             Other theft                                        1231
    On or near Shopping Area                           Shoplifting                                        987
    On or near Supermarket                             Violence and sexual offences                       985
    On or near Unspecified                             Anti-social behaviour                              913
    On or near Unspecified                             Public order                                       882
    On or near Shopping Area                           Violence and sexual offences                       791
    

QUESTION 4:   

--Outcomes of Reported Crimes:
    use West_Midlands_Crime
    
    SELECT last_outcome_category, COUNT(*) AS outcome_count
    FROM CombinedCrimeData
    GROUP BY last_outcome_category;

    last_outcome_category                                                                                                                                                                                                                                            outcome_count
    ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------
    Under investigation                                                                                                                                                                                                                                              36479
    Action to be taken by another organisation                                                                                                                                                                                                                       2315
    Awaiting court outcome                                                                                                                                                                                                                                           5487
    NULL                                                                                                                                                                                                                                                             13497
    Local resolution                                                                                                                                                                                                                                                 1658
    Suspect charged as part of another case                                                                                                                                                                                                                          3
    Unable to prosecute suspect                                                                                                                                                                                                                                      72397
    Status update unavailable                                                                                                                                                                                                                                        1412
    Offender given a caution                                                                                                                                                                                                                                         596
    Further action is not in the public interest                                                                                                                                                                                                                     20
    Further investigation is not in the public interest                                                                                                                                                                                                              58
    Formal action is not in the public interest                                                                                                                                                                                                                      13
    Investigation complete; no suspect identified                                                                                                                                                                                                                    53475
    
 
QUESTION 5:

--Correlation Between Crime Type and Outcome:
    USE West_Midlands_Crime
    
    SELECT TOP 10 crime_type, Last_outcome_category, COUNT(*) AS outcome_count
    FROM CombinedCrimeData
    GROUP BY crime_type, Last_outcome_category
    ORDER BY outcome_count DESC

    Crime_type                                         Last_outcome_category                              outcome_count
    -------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------------
    Violence and sexual offences                       Unable to prosecute suspect                        45962
    Violence and sexual offences                       Under investigation                                13863
    Anti-social behaviour                              NULL                                               13497
    Vehicle crime                                      Investigation complete; no suspect identified      11552
    Violence and sexual offences                       Investigation complete; no suspect identified      10786
    Other theft                                        Investigation complete; no suspect identified      6302
    Criminal damage and arson                          Investigation complete; no suspect identified      6299
    Public order                                       Unable to prosecute suspect                        6112
    Burglary                                           Investigation complete; no suspect identified      5032 
    Shoplifting                                        Investigation complete; no suspect identified      4550

QUESTION 6:
--Identifing Time Trends in Specific Crime Types: 
    SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Criminal damage and arson'
    GROUP BY month, crime_type
    ORDER BY month;

    Month                                              Crime_type                                         Crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Criminal damage and arson                          2421
    2023-04                                            Criminal damage and arson                          2379
    2023-05                                            Criminal damage and arson                          2377
    2023-06                                            Criminal damage and arson                          2367
    2023-07                                            Criminal damage and arson                          2423
    2023-08                                            Criminal damage and arson                          2270
    

SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Anti-Social behaviour'
    GROUP BY month, crime_type
    ORDER BY month;
    month                                              crime_type                                         crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Anti-social behaviour                              1455
    2023-04                                            Anti-social behaviour                              1953
    2023-05                                            Anti-social behaviour                              2040
    2023-06                                            Anti-social behaviour                              2679
    2023-07                                            Anti-social behaviour                              2479
    2023-08                                            Anti-social behaviour                              2891
    
SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Burglary'
    GROUP BY month, crime_type
    ORDER BY month;

    month                                              crime_type                                         crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Burglary                                           1810
    2023-04                                            Burglary                                           1551
    2023-05                                            Burglary                                           1509
    2023-06                                            Burglary                                           1456
    2023-07                                            Burglary                                           1528
    2023-08                                            Burglary                                           1727
    
    

SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Drugs'
    GROUP BY month, crime_type
    ORDER BY month;

    month                                              crime_type                                         crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Drugs                                              698
    2023-04                                            Drugs                                              585
    2023-05                                            Drugs                                              665
    2023-06                                            Drugs                                              635
    2023-07                                            Drugs                                              689
    2023-08                                            Drugs                                              640
    

SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Violence and sexual offences'
    GROUP BY month, crime_type
    ORDER BY month;

    month                                              crime_type                                         crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Violence and sexual offences                       13189
    2023-04                                            Violence and sexual offences                       12700
    2023-05                                            Violence and sexual offences                       12723
    2023-06                                            Violence and sexual offences                       12977
    2023-07                                            Violence and sexual offences                       12781
    2023-08                                            Violence and sexual offences                       11497
    
SELECT month, crime_type, COUNT(*) AS crime_count
    FROM CombinedCrimeData
    WHERE crime_type = 'Vehicle crime'
    GROUP BY month, crime_type
    ORDER BY month;

    month                                              crime_type                                         crime_count
    -------------------------------------------------- -------------------------------------------------- -----------
    2023-03                                            Vehicle crime                                      3312
    2023-04                                            Vehicle crime                                      3291
    2023-05                                            Vehicle crime                                      2866
    2023-06                                            Vehicle crime                                      2692
    2023-07                                            Vehicle crime                                      2843
    2023-08                                            Vehicle crime                                      2697





