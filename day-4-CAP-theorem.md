# Day-4 - CAP Theorem

Recap of last week
→ Microservices (Multiple Services / Multiple DBs and Load balancer to handle the requests)

How to avoid single point of failure
Lets consider multiple services like order service / auth service / report service.

We can have multiple instance of order service ( by having backup server)
if one goes down and we can have other service to take request.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f54369b-a0ce-47a0-8e66-240c198df5a4/Untitled.png)

### Distributed Systems

Apart from Services. There are distributed Databases (DynamoDB, MongoDB, Cassandra), Distributed Filesystem( Hadoop , EFS)

You have three databases: Database 1, Database 2, and Database 3. Database 1 is your primary database where you perform both read and write operations. Databases 2 and 3 are read-only replicas.

### Consistency

**Eventual Consistency**: As the name suggests, eventual consistency means that changes to the value of a data item will eventually propagate to all replicas, but there is a lag, and during this lag, the replicas might return stale data. Your scenario where changes in Database 1 take a minute to replicate to Databases 2 and 3 is an example of eventual consistency. Suppose you have a blog post counter. If you increment the counter in Database 1, Databases 2 and 3 might still show the old count until they sync up after that 1-minute lag.
(RYW - Consistency) (Read your write consistency)
RYW (Read-Your-Writes) consistency is achieved when the system guarantees that any attempt to read a record after it has been updated will return the updated value. RDBMS typically provides read-write consistency

![When we read immediately we get old value as there is delayed sync
](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0eef6b64-7453-423c-b92a-67199af93e91/Untitled.png)

When we read immediately we get old value as there is delayed sync

**Strong Consistency**: In strong consistency, all replicas agree on the value of a data item before any of them responds to a read or a write. If a write operation occurs, it's not considered successful until the update has been received by all replicas. For example, consider a banking transaction. If you withdraw money from an ATM (Database 1), that new balance is immediately propagated to Databases 2 and 3 before the transaction is considered complete. This ensures that any subsequent transactions, perhaps from another ATM (representing Databases 2 or 3), will have the correct balance and you won't be able to withdraw more money than you have. Synchronous Replication

![Even when we read immediately we get new value as there is Immediate sync
](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/22d31870-f049-42c1-ae6c-867716c47cfc/Untitled.png)

Even when we read immediately we get new value as there is Immediate sync

### **Functional Requirements vs Non-Functional Requirements**

**Functional Requirements** are the basic things a system must do. They describe the tasks or processes the system needs to perform. For example, an e-commerce site must be able to process payments and track orders.

**Non-Functional Requirements** are qualities a system must have. They describe characteristics or attributes of the system. For example, the e-commerce site must be secure (to protect user data), fast (for good user experience), Availability (system shouldn’t be down for very long) and scalable (to support growth in users and orders).

**Availability**

Availability in terms of information technology refers to the ability of a system or a service to be operational and accessible when users need it. It's usually expressed as a percentage of the total system downtime over a predefined period.

Let's illustrate it with an example:

Consider an e-commerce website like Amazon. Availability refers to the system being operational and accessible for users to browse products, add items to the cart, and make purchases. If Amazon's website is down and users can't access it to shop, then the website is experiencing downtime and its availability is affected.

In the world of distributed systems, we often aim for high availability. The term "Five Nines" (99.999%) availability is often mentioned as the gold standard, meaning the service is guaranteed to be operational 99.999% of the time, which translates to about 5.26 minutes of downtime per year

*SLA stands for Service Level Agreement. It's a contract or agreement between a service provider and a customer that specifies, usually in measurable terms, what services the provider will furnish.*

| Availability | Downtime per year |
| --- | --- |
| 90% (one nine) | More than 36 days |
| 95% | About 18 days |
| 98% | About 7 days |
| 99% (two nines) | About 3.65 days |
| 99.9% (three nines) | About 8.76 hours |
| 99.99% (four nines) | About 52.6 minutes |
| 99.999% (five nines) | About 5.26 minutes |
| 99.9999% (six nines) | About 31.5 seconds |
| 99.99999% (seven nines) | About 3.15 seconds |

### how to Increase the availability of your system ?

| Strategy | Explanation | Example |
| --- | --- | --- |
| Replication | Creating duplicate instances of data or services | Keeping multiple copies of a database, so if one crashes, others can handle requests |
| Redundancy | Having backup components that can take over if the primary one fails | Using multiple servers to host a website, so if one server goes down, others can continue serving |
| Scaling | Adding more resources to a system to handle increased load | Adding more servers during peak traffic times to maintain system performance |
| Geographical Distribution (CDN) | Distributing resources in different physical locations | Using a Content Delivery Network (CDN) to serve web content to users from the closest server |
| Load-Balancing | Distributing workload across multiple systems to prevent any single system from getting overwhelmed | Using a load balancer to distribute incoming network traffic across several servers |
| Failover Mechanisms | Automatically switching to a redundant system upon the failure of a primary system | If the primary server fails, an automatic failover process redirects traffic to backup servers |
| Monitoring | Keeping track of system performance and operation | Using monitoring software to identify when system performance degrades or a component fails |
| Cloud Services | Using cloud resources that can be scaled as needed | Using cloud-based storage that can be increased or decreased based on demand |
| Scheduled Maintenances | Performing regular system maintenance during off-peak times | Scheduling system updates and maintenance during times when user traffic is typically low |
| Testing & Simulation | Regularly testing system performance and failover procedures | Conducting stress tests to simulate high load conditions and ensure the system can handle it |

### CAP THEOREM

we try to make high available and high consistency.
We have two db 1 and 2 , we do read and write on both

but suddenly the communication between both my system is lost. so, both the db is not synchronized . because of a partition happening

### H**ighly Available and Highly Consistent System**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/343a4bb6-9f9e-45d7-a4f5-7d5ce7600674/Untitled.png)

When a network Partition occurs, there are two possibilities

### P**ossibilitiy no - 1 (AP)**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0ca8fb8-88d1-4e58-bdad-c849d381ea5f/Untitled.png)

- **Availability**: Despite the network partition (communication loss between Database 1 and Database 2), your system continues to handle read/write requests on both databases. This means the system is prioritizing availability - it remains accessible and responsive to the client's requests even during the network partition.
- **Partition Tolerance**: Your system continues to function despite the network partition. Even though Database 1 and Database 2 can't communicate with each other, they still operate independently. This is what Partition Tolerance means in the context of CAP theorem.

However, the system is not Consistent in this scenario. Once the network partition happens, the data in Database 1 and Database 2 may become inconsistent because updates made on one database are not immediately reflected on the other. Therefore, a client could potentially read different data from the two databases, reflecting a lack of consistency.

Remember, the CAP theorem states that in a distributed system, you can only guarantee two out of the three properties: Consistency, Availability, and Partition Tolerance, especially during a network partition (as in this scenario).

### Possibility no-2 (CP)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bbe37334-4459-4452-b310-dd242de4e4c4/Untitled.png)

Write operations fail during a network partition because the system cannot maintain consistency across databases, the system follows Consistency and Partition Tolerance from the CAP theorem. Here's why:

- **Consistency**: The system prioritizes data consistency. It means if a write operation occurs on one database, it won't complete unless the data can be synchronized with the other database. During a network partition, because the system can't maintain data consistency (as the databases can't communicate), it rejects the write operation. This ensures that any successful write operation is seen by all nodes in the system (in this case, both databases), maintaining strict consistency.
- **Partition Tolerance**: Your system is still tolerant of network partitions. Despite the network issue preventing communication between Database 1 and Database 2, your system continues to operate, even if that operation is rejecting write requests.

In this scenario, the system is not Available in the sense defined by the CAP theorem. During a network partition, clients might not be able to perform write operations, thus the system isn't always responding to all requests. The system sacrifices availability to ensure consistency during a network partition.

### CAP THEOREM

The CAP theorem is a fundamental principle that specifies that it's impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:

- **Consistency (C)**: Every read from the system receives the latest write or an error.
- **Availability (A)**: Every request to the system receives a non-error response, without guarantee that it contains the most recent write.
- **Partition Tolerance (P)**: The system continues to operate despite an arbitrary number of network failures.

Let's illustrate this with an example:

Think of a popular social media platform where users post updates (like Twitter). This platform uses a distributed system to store all the tweets. The system is designed in such a way that it spreads its data across many servers for better performance, scalability, and resilience.

- **Consistency**: When a user posts a new tweet, the tweet becomes instantly available to everyone. When this happens, it means the system has a high level of consistency.
- **Availability**: Every time a user tries to fetch a tweet, the system guarantees to return a tweet (although it might not be the most recent one). This is a high level of availability.
- **Partition Tolerance**: If a network problem happens and servers can't communicate with each other, the system continues to operate and serve tweets. It might show outdated tweets, but it's still operational.

According to the CAP theorem, only two of these guarantees can be met at any given time. So, if the network fails (Partition), the system must choose between Consistency and Availability. It might stop showing new tweets until the network problem is resolved (Consistency over Availability), or it might show outdated tweets (Availability over Consistency). It can't guarantee to show new tweets (Consistency) and never fail to deliver a tweet (Availability) at the same time when there is a network problem

### CA in a distributed system

Correct, in a single-node system (a system that is not distributed), we can indeed have Consistency and Availability (CA) since the issue of network partitions doesn't arise. Every read receives the latest write (Consistency), and every request receives a non-error response (Availability). There's no need for Partition Tolerance since there are no network partitions within a single-node system.

However, once you move to a distributed system where data is spread across multiple nodes (computers, servers, regions), you need to handle the possibility of network partitions. Network partitions are inevitable in a distributed system due to various reasons such as network failures, hardware failures, etc. The CAP theorem stipulates that during a network partition, you can only have either Consistency or Availability.

That is why it's said you can't achieve CA in a distributed system. You have to choose between Consistency and Availability when a Partition happens. This choice will largely depend on the nature and requirements of your specific application. For example, a banking system might prefer Consistency over Availability, while a social media platform might prefer Availability over Consistency.

Examples : 

| Application/Service | Consistency | Availability |
| --- | --- | --- |
| Youtube Comments | ❌ | ✔️ |
| Instagram Post/Feed | ❌ | ✔️ |
| Amazon Cart | ✔️ | ❌ |
| Uber Payment | ✔️ | ❌ |
| Uber Search Cab | ❌ | ✔️ |
| Whatsapp Messaging | ✔️ | ❌ |
| ATM | ✔️ | ❌ |
| Whatsapp Active Status | ❌ | ✔️ |
| File Sharing (Google Docs) | ✔️ | ❌ |

*Note: Examples can vary depending on our requirements and there is no one correct answer*

| Term | Definition | Example |
| --- | --- | --- |
| Latency | The time it takes for data to move from one place to another. | Time it takes for a server to respond to a click |
| Throughput | The actual amount of data that can be transferred through the system in a given amount of time. | 200 transactions processed per second |
| Bandwidth | The maximum amount of data that can be transferred through the system in a given amount of time. | 100 Mbps internet connection |
| Fault | A defect or malfunction in a system component. | A server's hard drive crashes |
| Failure | When a system or system component is unable to perform its intended function. | The server stops working due to a hard drive crash |
| Resiliency | The ability of a system to recover from failures and continue to function. | The server switches over to a backup hard drive after the primary one crashes |
| Redundancy | The duplication of critical system components to ensure that a backup is available in case of a failure. | The backup hard drive in the server |

### Stateful Systems vs Stateless systems

|  | Stateful Systems | Stateless Systems |
| --- | --- | --- |
| Definition | Systems that maintain or remember state of the interactions. | Systems that don't maintain any state information from previous interactions. |
| Example | E-commerce website remembering items in your shopping cart. | HTTP protocol treating each request independently. |
| Advantages | Can provide a more personalized experience based on past interactions. | Easier to scale and manage since no state information is stored. |
| Disadvantages | More difficult to scale due to dependency on previous interactions. May need more resources (e.g. memory) for maintaining state. | Can't provide personalized experiences based on past interactions. Requires all needed data to be sent with each request. |

###
