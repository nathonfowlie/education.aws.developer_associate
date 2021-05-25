# Databases

## RDS

- Relational databases, that are good for online transaction processing (OLTP).
- Provisioned database that scales vertically, not horizontally.
- PostgreSQL, MySQL, Oracle, Aurora and Aurora Serverless.

## DynamoDB

- NoSQL managed database.
- Key/value store to keep documents upto 400KB.
- Serverless.
- Can be provisioned with RCU/WCU and auto-scaling, or use On-Demand.

## ElastiCache

- In memory database.
- Redis or Memcached.
- Caches data coming from other databases, session data or caching queries.
- Provisioned the same as RDS databases.

## Redshift (OLAP)

- Online analytical processing.
- Data warehousing/data lakes.
- Analytics queries.
- Provisioned in advance.

## Neptune

- Graph database.

## DMS

- Database migration service to move data from one database to another.

## DocumentDB

- Managed MongoDB.
