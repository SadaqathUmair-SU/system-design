## Types of Database </br>
**Relational Database** </br>
**Non Relational Database** 


## Relational Database

**Schema** </br>
**ACID**

**Schema** in relational DB refer's to, how the data is going to be structured.
One of the reasons to use Schema is, if the requirement has some constraints.


**ACID Properties:**

**Atomicity** - It is a transaction in database which ensures the transaction happens successfully or doesn't happens at all.</br>
**Consistency** - It ensures to maintain consistency while reading the data so that while fetching the data doesn't gets changed at any point of time.</br>
**Isolation** - It ensures the occurrence of multiple transactions concurrently without a database state leading to a state of inconsistency.</br>
                For ex: Let's say there are two transactions happening read and write respectively.</br>
                Isolation ensures to do the transactions in a sequence i.e, to read first and to write it at second.</br>
**Durability** - It ensures that changes made to the database (transactions) that are successfully committed will survive permanently, even in the case of system failures.</br>

**Few below points where Relational DB is difficult to use:**</br>
1. Let's say if you not aware of any constraints and seems like the requirement is going to be changed frequently in the future at that point it is difficult to keep on updating scehema.</br>
2. Using Schema, Vertical Scaling can happen easily whereas Horizontal Scaling is bit difficult


## Non Relational Database

Non relational databases are also known as NO SQL.

**Types of Non Relational databases:**</br>
**Key-Value Stores**</br>
1. K-V stores are used similar to HashMap for ex, if you want to enable certain feature in a certain city for your application etc.
2. K-V are used to cache at the server side using Reddis, Dynamo DB etc
3. K-V stores are quite fast and they provide quick access because most of the data stores are in-memory.
4. Examples - Amazon Dynamo DB, Couchbase, Redis etc..

**Documents Based Database**
1. There is no fixed schema
2. They can support heavy read and writes
3. Documents DB has Collections and Documents. You can think of like Collections and Documents are similar to Tables and Rows in Relational Database respectively



## How do we handle millions of requests in single database server?

Handling millions of requests on a single database server can be challenging, but there are several techniques and best practices that can help improve performance and scalability:

Query Optimization: Optimize your database queries to ensure they are efficient and performant. This includes indexing frequently accessed columns, avoiding unnecessary joins or subqueries, and optimizing complex queries. Analyze query execution plans and use appropriate database query tuning tools to identify and resolve bottlenecks.

Connection Pooling: Implement a connection pooling mechanism to manage and reuse database connections efficiently. Connection pooling helps reduce the overhead of establishing new database connections for each request, improving overall performance.

Caching: Utilize caching techniques to store frequently accessed data in memory. This can include implementing application-level caching or using dedicated caching solutions like Redis or Memcached. Caching can significantly reduce the load on the database server and improve response times.

Scaling Vertically: Upgrade the hardware resources of the database server itself. This can involve increasing CPU power, memory capacity, and storage performance to handle higher loads. Vertical scaling has limitations, but it can provide some immediate relief for handling increased requests.

Database Indexing: Properly index your database tables to enhance query performance. Identify frequently queried columns and create indexes to speed up the retrieval process. However, be mindful of the trade-off between read performance and the overhead of maintaining indexes during write operations.

Sharding: If the data can be partitioned, consider implementing sharding, where data is distributed across multiple database servers based on a predefined strategy (e.g., by customer, geographical location, or specific data ranges). Sharding allows for parallel processing and can help distribute the load across multiple servers.

Horizontal Scaling: Introduce multiple database servers and distribute the load among them using techniques like load balancing. This approach involves setting up a cluster of database servers that work together to handle the requests. Horizontal scaling improves scalability and can handle higher request volumes.

Asynchronous Processing: Offload time-consuming tasks from the main request flow by implementing asynchronous processing. This involves leveraging message queues or task queues to handle background jobs or long-running processes separately, freeing up resources to handle incoming requests.

Database Replication: Implement database replication to create replicas of the primary database server. Replicas can handle read-only queries, reducing the load on the primary server and improving scalability for read-heavy workloads.

Performance Monitoring and Optimization: Continuously monitor the performance of your database server and make necessary optimizations based on real-time data. Use performance monitoring tools to identify bottlenecks, optimize configurations, and fine-tune the database server settings.

It's important to note that the effectiveness of these approaches may vary based on the specific requirements of your application, the nature of the workload, and the underlying database technology being used. A combination of these techniques might be necessary to effectively handle millions of requests on a single database server.
