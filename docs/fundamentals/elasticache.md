# ElastiCache

## What is Elasticache?

- Managed cache (Redis/Memcached)
- In-memory database with high performance, low latency.
- Reduces database load for read instensive workloads.
- Helps make applications stateless.
- Write scaling using sharding.
- Read scaling using read replicas.
- Mult AZ failover.
- Data needs to be well structured.

## Use Cases

### DB Cache

- Application queries elasticache, if no hit, then get the data from the RDS.
- Requires an invalidation strategy to ensure current/relevant data is cached.

### User Session Store

- User logs into the application, then writes session date into elasticache.
- The user hits another instance of the application, it gets the state from elasticache to avoid re-authenticating.

## Redis

- Multi AZ with auto failover.
- Read replicas to scale reads and have HA.
- Data durability using AOF persistence.
- Has backup and restore.
- Can be used as a database, cache or message broker.
- Need to create a Redis token  for applications to authenticate.
- Support encryption at rest and during transit.
- Can specify a backup window.

### Cluster Modes

#### Disabled

- Each shard has 1 primary node used for read/write, and up to 5 replicas.
- Asynchronous replication.
- Additional nodes are used for read only (read replicas).
- All nodes have all of the data if there's only 1 shard. This guards against data loss on  node failure.
- Multi-AZ fail over enabled by default.
- Good for scaling read performance.

#### Enabled

- Data partitioned across many shards (scales writes).
- Each shard has 1 primary node, and upto 5 replica nodes.
- Multi-AZ fail over enabled by default.
- Up to 500 nodes per cluster,
    - 500 shards with 1 master.
    - 250 shards with 1 master and 1 replica.
    - 83 shards with 1 master and 5 replicas.

## Memcached

- Multi-node data partitioning (sharding).
- Cache is non persistent.
- No backup and restore.
- Multi-threaded architecture.

## Caching Strategies

### Lazy Loading/Cache Aside/Lazy Population

Best for read optimistation.

1. Check elasticcache first for data.
2. If cache miss, query the RDS.
3. Write data to cache.

#### Pros
- Only requested data is cached

- Node failures are not fatal.

#### Cons

- Cache miss penalty that results in 3 round trips.
- Data can be stale.

### Write Through

- Add or update cache when the database is updated.
- Best for write optimisation.
- Can be combined with lazy loading.

1. On read, retrieve from cache.
2. On write, write to RDS, then write to elasticache.

#### Pros

- Data is never stale.
- Reads a quick.
- Write penalty instead of a read penalty (2 calls required).

#### Cons

- Data is missing until the RDS is updated.
- High cache churn. Some data may never be read.

### Cache Evictions/TTL

Three main ways that cache eviction can happen:

- Cache item is explicitly deleted.
- Memory is full and data is not recently used (LRU)
- Set a time to live (TTL).

If there's too many evictions, then need to scale elasticache up/out. Avoid for write optimised cache.
