
In RDS, the maximum value backup retention period is 35 days


## Aurora
Amazon Aurora is belong to RDS
Amzon Aurora is 5 times faster than MySQL
By default, RDS has 16TB volume
Two key features
-   Multi-AZ-For Disaster Recovery
-   Read Replicas-For Performance
-   6 copies by default

### OLTP vs OLAP
OLTP是写操作比较多， OLAP是读操作比较多
**OLTP** Online Transaction Processing
-   Manages transaction-oriented applications on the internet in a 3-tier architecture.
-   manages day to day transaction of an organization
-   Used by the end users such as DBA, DB professional.
-   Use simple and standard queries
-   Use Insert, Delete and Update statement
-   Used for Data processing
-   Backups are rarely needed

**OLAP** online Anyalytics Processing
-   Consist software tools that are used for data analysis for business decisions.
-   Used for offline storage of data
-   Used for business analyst, manager, executive for analysis and reporting purposes.
-   Use large and complex queires like aggregation
-   use Select query for fetching the data
-   Allows database information extraction from multiple databases.
-   data is backed up regularly

criteria affecting your billing for RDS
-   Clock hours of server time
-   additional storage
-   number of requests