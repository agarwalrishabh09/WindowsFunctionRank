When we write aggregate queries, we get aggregate values based on the fields mentioned in GROUP BY clause. Window Expression in ABAP SQL is similar in this sense. However, it gives you individual records as well.
Windowing is a way to create partitions of the query result and perform various operations on those partitions . 
The Window Expression is the expression that helps you do this partition. Once the partition is done, you can apply window functions on the partitions.

What is Window Expression?
A window expression starts with a keywords OVER to define a subset of the result set of a query and applies a window function to it.

SELECT FROM /dmo/flight
  FIELDS
    carrier_id,
    connection_id,
    flight_date,
    price,
    AVG( price ) OVER( PARTITION BY carrier_id, connection_id ) 
                 AS average_price
    ORDER BY carrier_id, connection_id, flight_date
    INTO TABLE @DATA(lt_window).

OVER keyword starts the Window Expression and PARTITION BY is similar to GROUP BY where partitioning fields will be mentioned.

This query can then be enhanced further with

Different partitions e.g. partition on only Carrier ID
More aggregate functions like Sum, Max, Min, Count
Ranking Functions like row number, rank within partition
Value Functions like lead, lag within partition

Window Functions?
Aggregate Functions, Ranking Functions and Value Functions together are called as Window Function in the context of Window Expressions. 

SELECT FROM /dmo/flight
  FIELDS
    carrier_id as carrier,
    connection_id as connection,
    flight_date as date,
    price,
    COUNT(*)
        OVER( PARTITION BY carrier_id )
        AS cnt_car,
    COUNT(*)
        OVER( PARTITION BY carrier_id, connection_id )
        AS cnt_conn,
    AVG( price )
        OVER( PARTITION BY carrier_id, connection_id )
        AS avg_price,
    MIN( price )
        OVER( PARTITION BY carrier_id, connection_id )
        AS min_price,
    MAX( price )
        OVER( PARTITION BY carrier_id, connection_id )
        AS max_price,
    SUM( price )
        OVER( PARTITION BY carrier_id, connection_id )
        AS total_price,
    division( 100 * price,
              SUM( price )
                OVER( PARTITION BY carrier_id, connection_id ),
              2 )
        AS percentage
    ORDER BY carrier_id, connection_id, flight_date
    INTO TABLE @DATA(lt_window).


    
