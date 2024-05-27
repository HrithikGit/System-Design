# Caching Concepts Explained

**Caching** is a technique used in computing to store copies of frequently accessed data in temporary storage (cache) to reduce access time for future requests. This mechanism improves performance by reducing the need to retrieve data from slower primary storage systems.

## Locality of Reference Principle

This principle suggests that programs tend to access a relatively small portion of their address space at any given time. It includes:
- **Temporal Locality**: Recently accessed data is likely to be accessed again soon.
- **Spatial Locality**: Data near recently accessed data is likely to be accessed soon.

## Levels of Cache in Architecture

Caches exist at multiple levels in a computing architecture:
1. **Processor Cache (L1, L2, L3)**: Closest to the CPU, fastest but smallest.
2. **Memory Cache**: Between RAM and disk storage, larger but slower than CPU cache.
3. **Application Server Cache**: Between the application and the database, used to store query results or computed data.

## Application Server Cache

An **Application Server Cache** is placed on a request layer node, often to store the results of database queries or other computationally expensive operations. When a request layer node is scaled out (expanded to many nodes), issues can arise:

- **Load Balancer**: Distributes incoming requests across multiple nodes, which can lead to the same request being routed to different nodes and increase cache misses.
- **Solutions**:
  - **Global Caches**: A shared cache accessible by all nodes.
  - **Distributed Caches**: Cache data is partitioned across nodes using consistent hashing.

## Distributed Cache

**Distributed Cache** involves splitting the cache across multiple nodes using a consistent hashing function. Each node is responsible for a subset of the cache data.

- **Pro**: Easy to scale by adding more nodes.
- **Con**: If a node goes down, its portion of the cache is lost.

## Global Cache

A **Global Cache** is a centralized cache accessible by all request layer nodes, often faster than the original data store. There are two common forms:

1. **Cache Server Handles Cache Miss**:
   - Most common setup.
   - A dedicated cache server processes cache misses and updates the cache.

2. **Request Nodes Handle Cache Miss**:
   - Request nodes are responsible for loading data into the cache on a miss.
   - Effective when the data is static and shouldn't be evicted.

## Content Distribution Network (CDN)

A **CDN** is used for serving static content (e.g., images, videos) efficiently across geographical locations.

- **Process**:
  - A request asks the CDN for content.
  - If the CDN has the content, it serves it.
  - If not, the CDN fetches it from the back-end servers, caches it, and then serves it.

- **Building CDN for Small Systems**:
  - Serve static media off a subdomain using a lightweight HTTP server like Nginx.
  - Later switch the DNS of this subdomain to a CDN for better scalability.

## Cache Invalidation

**Cache Invalidation** ensures the cache remains consistent with the source of truth (e.g., the database). When the source data changes, the cache must be invalidated or updated.

## Types of Cache Write Policies

1. **Write-through Cache**:
   - Data is written to both cache and permanent storage simultaneously.
   - **Pro**: Ensures data consistency, robust against system failures.
   - **Con**: Higher latency for write operations.

2. **Write-around Cache**:
   - Data is written directly to permanent storage, not cache.
   - **Pro**: Reduces cache pollution with infrequently accessed data.
   - **Con**: Cache misses and higher latency for recently written data.

3. **Write-back Cache**:
   - Data is written to cache only, with periodic writes to permanent storage.
   - **Pro**: Low latency and high throughput for write-intensive applications.
   - **Con**: Risk of data loss if the system fails before data is written to permanent storage.

## Cache Eviction Policies

When the cache is full, older data needs to be evicted to make space for new data. Common policies include:

- **FIFO (First In, First Out)**: Evicts the oldest data.
- **LIFO (Last In, First Out)**: Evicts the most recently added data.
- **LRU (Least Recently Used)**: Evicts the least recently accessed data.
- **MRU (Most Recently Used)**: Evicts the most recently accessed data.
- **LFU (Least Frequently Used)**: Evicts data that is accessed least often.
- **RR (Random Replacement)**: Evicts data randomly.

Understanding these caching concepts and strategies helps design efficient and scalable systems by improving data retrieval times and overall application performance.
