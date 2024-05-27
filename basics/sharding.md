# Sharding / Data Partitioning

## Partitioning Methods

### Horizontal Partitioning
Horizontal partitioning, also known as range-based sharding, involves dividing a table into multiple tables, where each table holds a range of rows. This method is effective when dealing with large datasets that can be split based on specific criteria.

- **Range-based Sharding**:
  - Rows are divided into different tables based on the values of a certain column (e.g., date ranges, ID ranges).
  - **Example**: Orders from 2020 go into one table, while orders from 2021 go into another.
  - **Con**:
    - If the range values are not chosen carefully, it can lead to unbalanced partitions where some servers may handle more data and load than others, causing performance bottlenecks.

### Vertical Partitioning
Vertical partitioning involves dividing a table into multiple tables based on columns. Each table contains a subset of columns.

- **Definition**:
  - Data related to specific features is stored on separate servers.
  - **Example**: A user table could be split into two tables: one with user personal information and another with user preferences.
- **Pro**:
  - It is straightforward to implement and typically has a low impact on the application.
- **Con**:
  - As the application grows, the database might need further partitioning to handle increased load and data volume.

### Directory-based Partitioning
Directory-based partitioning uses a central directory or lookup service that maintains information about the partitioning scheme. This directory abstracts the details of which data resides where, simplifying data access.

- **Definition**:
  - A lookup service manages and abstracts the partitioning scheme from the application.
  - **Example**: A central directory that directs queries to the appropriate server based on the data partitioning rules.
- **Advantages**:
  - Enables adding database servers or changing the partitioning scheme without modifying the application code.
- **Con**:
  - The directory service can become a single point of failure, potentially impacting the entire system if it goes down.

## Partitioning Criteria

### Key or Hash-based Partitioning
In key or hash-based partitioning, a hash function is applied to a key attribute (e.g., user ID) to determine the partition where the data should be stored.

- **Method**:
  - Hash the key to get a partition number.
  - **Example**: Hashing a user ID to determine the server that stores the user's data.
- **Problems**:
  - Adding new servers typically requires rehashing and redistributing data, leading to potential downtime.
- **Workaround**:
  - [Consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing) minimizes data movement when servers are added or removed, thereby reducing downtime.

### List Partitioning
List partitioning assigns specific values to each partition. Each partition handles a predefined set of values.

- **Method**:
  - Each partition is associated with a list of values.
  - **Example**: Partitioning a table where one partition stores data for specific regions (e.g., Europe, Asia).

### Round-robin Partitioning
Round-robin partitioning distributes data evenly across all partitions in a circular manner.

- **Method**:
  - With `n` partitions, the `i`-th row is assigned to partition `i % n`.
  - **Example**: Distributing incoming data requests sequentially to a set of servers.

### Composite Partitioning
Composite partitioning combines two or more partitioning schemes to form a new partitioning strategy.

- **Method**:
  - Combine different partitioning methods for a more tailored solution.
  - **Example**: Using consistent hashing (a combination of hash and list partitioning) to hash a key into a reduced key space, then using a list to map the reduced keys to partitions.

## Common Problems of Sharding

Sharding introduces complexity, particularly because operations involving multiple tables or rows may span multiple servers, creating several challenges.

### Joins and Denormalization
- **Issue**:
  - Performing joins across multiple servers can be inefficient and slow, as data must be gathered from different locations.
- **Workaround**:
  - **Denormalization**: Storing redundant data to minimize the need for joins, which can improve performance but may lead to data inconsistency if updates are not properly managed.

### Referential Integrity
- **Issue**:
  - Enforcing data integrity constraints, such as foreign keys, becomes difficult across multiple shards.
- **Workarounds**:
  - **Application-level Enforcement**: Implement referential integrity checks within the application logic.
  - **SQL Jobs**: Periodically run SQL jobs to clean up dangling references and maintain data consistency.

### Rebalancing
- **Necessity**:
  - Data distribution may become uneven over time, or certain shards may experience higher loads.
- **Solutions**:
  - Create additional database shards or rebalance existing ones. This involves redistributing data, which can change the partitioning scheme and require significant data movement.

Understanding these partitioning methods and criteria, along with addressing common sharding problems, is crucial for designing scalable and efficient database architectures.
