# Day-3 **Sync, Async Architecture**

**Recap of Day 2 with Demo**

1. *Open Google Chrome: Launch the Google Chrome browser on your computer.*
2. *Access Developer Tools: Right-click anywhere on a webpage and select "Inspect" from the context menu. Alternatively, you can use the keyboard shortcut **`Ctrl + Shift + I`** (Windows/Linux) or **`Cmd + Option + I`** (Mac) to open the DevTools.*
3. *Switch to the Network tab: In the DevTools panel that opens, you should see various tabs at the top. Click on the "Network" tab.*
4. *Enable recording: Ensure that the "Record" button (a small circular button at the top left of the Network tab) is red. If it's greyed out, click on it to start recording network activity.*
5. *Perform actions on the webpage: Interact with the webpage or application you want to inspect. This can include clicking on links, submitting forms, or triggering any actions that involve network requests.*
6. *Observe network requests: As you interact with the webpage, you will see the network requests appearing in the Network tab. Each row represents a single request made by the webpage, displaying information such as the request URL, request method (GET, POST, etc.), status code, and size.*
7. *Inspect request details: Clicking on a specific network request will reveal more information about it in the right pane. You can explore various tabs like Headers, Preview, Response, and Timing to analyze different aspects of the request and response.*
8. *Filter and search requests: If there are numerous requests in the Network tab, you can use the search bar or the filtering options to narrow down the requests based on specific criteria like file type, domain, or text search.*
9. *WebSocket inspection: To inspect WebSocket (ws) connections, look for requests with the "Upgrade: websocket" header. Clicking on such a request will provide you with the WebSocket-specific details, including frames, payloads, and connection status.*

*By following these steps, you can leverage the Chrome DevTools' Network tab to gain insights into the HTTP and WebSocket calls made by a webpage or application, helping you understand the underlying network activity and debug any issues.*

### Day- 3

We will consider a Stock Market  System

1. **Sign up**: Ensuring successful user registration.
2. **Buy/Sell transaction**: Monitoring successful stock transactions.
3. **Market Data**: Observing real-time data transmission for stock market fluctuations.
4. **Profit Loss Reports**: Checking data calls for report generation.
5. **Portfolio**: Validating calls for portfolio updates.
6. **Payments**: Watching over secure transactions with payment gateways.
7. **Notification**: Monitoring requests to push notification services.
8. **Logging & Monitoring**: Inspecting the status and details of logging data transmission.

## Monolith vs Microservices

|  | Monolith | Microservices |
| --- | --- | --- |
| Advantages | Single Codebase: All components of the app exist in one codebase. Easy Development: Simpler to add new features.
Easy Testing: Easier to simulate and test scenarios. 
Easy Deployment: Easy to deploy the entire platform. 
Easy Debugging: Easier to trace bugs.
Easy Performance Monitoring: Easier to monitor performance of all features. | Agile Development: Faster development, can update services independently.
Scalability: Can scale only necessary services. 
Highly Testable & Maintainable: Each microservice can be tested and maintained separately.
Flexibility: Different services can use different technology stacks.
Independent Deployment: Each microservice can be deployed independently. |
| Disadvantages | Slower Development Speed: As the system grows, adding new features can slow down.
 Scalability Issues: Scaling issues can arise when user base grows.
Reliability: A bug in one part can bring down the entire system.
Flexibility Issues: Can't add a feature if it requires a different tech stack.
Deployment Complexity: Small changes require complete deployment. | Management Maintenance: Managing multiple services can be complex.
Infrastructure Costs: Can increase due to separate databases and servers for each service. Organizational Issues: Communication challenges can arise among teams working on different services.
Debugging Issues: Requires advanced tools for efficient debugging.
Lack of Standardization: Different standards among services can create integration issues.
Lack of Code Ownership: Potential issues in shared code areas due to divided responsibilities. |

# **Monolith to microservices**

There are different ways of migration.

## Strangler Fig Pattern

The Strangler Fig Pattern can be an effective method for migrating a monolithic system to microservices. Here's how it can be applied to our stock market application:

1. **Identify Part to Migrate**: Identify a part of the existing system that needs migration. For instance, we may choose the 'Buy/Sell transaction' functionality in the monolith application that we wish to replace.
2. **Implement New Microservice**: Implement this functionality into a new microservice. We could create a new 'Transaction Service' that handles all the buy/sell operations independently.
3. **Redirect Requests**: Start gradually redirecting the requests from the old monolithic system to the new 'Transaction Service'. This can be done using a routing mechanism, which routes a specific portion of requests to the new service. This allows the new microservice to start handling real-world requests while also giving a chance to monitor its performance and correct any issues before it fully takes over the functionality from the monolith.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79ed4e91-0079-46e5-bd2a-f4164db66a7f/Untitled.png)

### **Parallel Run**

Parallel run refers to the simultaneous operation of the old and new systems for a given period, allowing comparison of results to ensure correctness of the new system. Here's how it can be done:

1. **Initiate Parallel Run**: For the 'Buy/Sell transaction' function, initiate a parallel run where the transaction is processed both by the monolith system and the new microservice.
2. **Compare Results**: The results from the monolith system and the new microservice are compared to ensure the microservice is providing the expected output.

This process is beneficial, particularly when the legacy code isn't well-understood by the current team (for instance, the original developers have left the company). It helps to validate that the new microservice functions correctly and produces the same results as the legacy system before completely phasing out the old functionality.

## **Branch By Abstraction Pattern**

### **What is Modular Code?**

Modular code refers to the practice of dividing a software system into separate, independent modules where each contains everything necessary to execute only one aspect of the desired functionality. A module can be considered a separate file, a collection of files, or even a piece of a single file, as long as it represents a distinct functionality of the software.

### **Why is Modular Code Important?**

1. **Maintainability**: Modular code is easier to understand, update, and debug because you can focus on one piece of the system at a time. Each module can be maintained separately by its own team, which promotes code ownership and responsibility.
2. **Reusability**: With modular code, you can reuse modules across different parts of an application, or even across different applications, which can save significant development time.
3. **Scalability**: Modular code makes it easier to scale your application because you can identify which modules need to be scaled based on their individual load or demand.
4. **Testability**: Each module can be tested independently, which promotes robust testing and can help in identifying issues quickly.

### **How to Make Code Modular?**

1. **Divide by Functionality**: Divide your application based on functionality. Each functionality should represent a module. For instance, in a stock market application, you might have separate modules for user management, stock transactions, and reporting.
2. **Encapsulation**: Encapsulate the code, data, and processes related to each module. Encapsulation ensures that the module exposes only what's necessary and keeps other details private.
3. **Interfaces**: Define clear interfaces for how modules will interact with each other. This will help in reducing dependencies between modules.
4. **Consistency**: Ensure consistency in design and coding standards across modules.

### Branch By Abstraction Pattern

1. **Abstraction**: Identify the 'Buy/Sell transaction' part of the monolithic system. Create an interface called 'TransactionService' that defines the operations like 'buy' and 'sell'. The existing monolith codebase would implement this interface.
2. **New Implementation**: Now, start developing the new microservice which will also implement the 'TransactionService' interface. This new microservice is designed to handle the 'Buy/Sell transaction' operations independently.
3. **Switch**: Once the microservice is ready and thoroughly tested, gradually start redirecting the 'Buy/Sell transaction' requests from the monolithic system to the new microservice. This could be accomplished through feature toggles or a routing mechanism, which allows you to control which requests are processed by the new microservice.
4. **Remove Legacy Code**: When the new microservice has fully taken over the 'Buy/Sell transaction' operations and is working as expected, the legacy 'Buy/Sell transaction' code in the monolith system can be safely removed.

Branch by Abstraction allows this transition to happen smoothly, without disrupting the functioning of the system. The old and new systems can coexist and operate in parallel during the transition, reducing risks and enabling continuous delivery.

### Function Call vs RPC vs HTTP

**1. Function Call:**

A function call is one of the most basic operations in programming. It is a process of using a function's code somewhere else in the program.

Let's say we have a function in a program that adds two numbers:

```python
pythonCopy code
def add(a, b):
    return a + b
```

We can call this function elsewhere in the program to perform an addition operation:

```python
pythonCopy code
result = add(5, 3)
print(result)  # Outputs: 8
```

**2. Remote Procedure Call (RPC):**

A Remote Procedure Call (RPC) is similar to a function call, but it's used in the context of networked applications. It allows a program running on one machine to call a function on a different machine (a remote server) as if it were a local function.

For example, consider a client-server application where the server provides a function to add two numbers, similar to our local example. But instead of calling this function locally, a client on a different machine can use RPC to call this function on the server.

```python

# On the server
def add(a, b):
    return a + b

# On the client
result = rpc_call("add", 5, 3)  # Using a hypothetical 'rpc_call' function
print(result)  # Outputs: 8
```

Here, **`rpc_call`** is a hypothetical function illustrating the concept of RPC. In reality, how you implement an RPC may depend on the language and the libraries you're using.

**3. HTTP (Hypertext Transfer Protocol):**

HTTP is a protocol used for transmitting hypermedia documents, such as HTML, over the internet. It defines how messages are formatted and transmitted, and what actions servers and browsers should take in response to various commands.

For example, when you type a URL in your browser, it sends an HTTP request to the server hosting the webpage. The server then responds with the content of the webpage, which your browser renders for you to view.

In the context of our example, imagine the add function is provided by a web server as an API endpoint. A client could use an HTTP request to call this function:

```
httpCopy code
POST /add HTTP/1.1
Host: www.example.com
Content-Type: application/json

{
    "a": 5,
    "b": 3
}
```

The server would respond with the result of the addition:

```
httpCopy code
HTTP/1.1 200 OK
Content-Type: application/json

{
    "result": 8
}
```

This example uses the HTTP protocol's POST method to send data (the numbers to add) to the server. The server adds the numbers and returns the result in the response.

In monolith we do function call (Procedure Calls) and in microservices we can use RPC or HTTP

### **RPC**

Remote Procedure Call (RPC) is a protocol that one program can use to request a service from a program located in another computer on a network without having to understand network details.

### **gRPC**

gRPC is a modern, open-source, high-performance RPC framework developed by Google. It uses Protocol Buffers (protobuf) as its interface definition language, which describes the service interface and the structure of the payload messages. This is an efficient binary format that provides a simpler and faster data exchange compared to JSON and XML.

**Protocol Buffers**

Protocol Buffers (Protobuf) is a binary encoding format that allows you to specify a schema for your data using a specification language. This schema is used to generate code in various languages and provides a wide range of data structures that result in serialized data being small and quick to encode/decode.

### **gRPC in Action**

Here's a simplified version of how gRPC works in a client-server architecture:

1. **gRPC Client**: The process starts from the gRPC client. The client makes a call through a client stub, which has the same methods as the server. The data for the call is serialized using Protobuf into a binary format.
2. **Transport**: The serialized data is then sent over the network via the underlying transport layer.
3. **HTTP/2**: gRPC utilizes HTTP/2 as its transport protocol. One significant benefit of HTTP/2 is that it allows multiplexing, which is the ability to send multiple streams of messages over a single, long-lived TCP connection. This reduces latency and increases the performance of network communication.
4. **gRPC Server**: The server receives the serialized data, deserializes it back into the method inputs, and executes the method. The result is then sent back in the reverse direction: serialized and sent back to the client via HTTP/2 and the transport layer, then deserialized by the client stub.

The adoption of gRPC in web client and server communication has been slower due to a few significant factors:

1. **Browser Compatibility**: Not all browsers fully support HTTP/2, the protocol underlying gRPC. Even where HTTP/2 is supported, the necessary HTTP/2 features such as trailers might not be available.
2. **gRPC-Web**: While gRPC-Web, a JavaScript implementation of gRPC for browsers, does exist, it doesn't support all the features of gRPC, such as bidirectional streaming, and is less mature than other gRPC libraries.
3. **Text-Based Formats**: In the context of web development, formats like JSON and XML are very common and convenient for data interchange. They're directly compatible with JavaScript and are human-readable. gRPC, on the other hand, defaults to Protocol Buffers, a binary format that's more efficient but not as straightforward to use on the web.
4. **Firewalls and Proxies**: Some internet infrastructure might not support HTTP/2 or might block gRPC traffic, causing potential network issues.
5. **REST Familiarity**: REST over HTTP is a well-understood model with broad support in many programming languages, frameworks, and tools. It's simpler to use and understand, which can speed up development and debugging.
6. **Increased Complexity**: While gRPC has performance benefits, it also adds complexity to the system. The performance gain might not always be worth the added complexity, particularly for applications that don't require high-performance inter-service communication.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e9bb434-043b-4671-b29f-6d43c4ddc033/Untitled.png)

There are also other examples for RPC other than gRPC.

1. **XML-RPC**: A protocol that uses XML to encode its calls and HTTP as a transport mechanism.
2. **JSON-RPC**: A remote procedure encoded in JSON. Like XML-RPC, it's language agnostic and can be used with HTTP.
3. **SOAP**: A messaging protocol that allows programs running on disparate operating systems to communicate using HTTP and XML.
4. **CORBA**: Allows communication between systems developed in different programming languages on different platforms, using the object-oriented model.

| Protocol | Pros | Cons |
| --- | --- | --- |
| XML-RPC | Simple, Human readable, Language agnostic | Verbose, slower compared to binary protocols |
| JSON-RPC | Simple, Human readable, Language agnostic | Not as rich in features |
| SOAP | Language, platform, and transport agnostic, Highly extensible | Complex, verbose, requires more bandwidth and resources |
| CORBA | Language and platform agnostic, Supports complex operations | Difficult to set up and maintain, Considered outdated |

### **Webhooks and Event-Driven Architecture**

Webhooks are a method of augmenting or altering the behavior of a web page or application with custom callbacks. These callbacks can be maintained, modified, and managed by third-party users and developers who may not necessarily be affiliated with the originating website or application.

In the context of a stock market application like Zerodha, this translates to the following:

Zerodha, a brokerage platform, wants to stay updated with price changes from the stock exchange (SEBI). To achieve this, Zerodha would provide a webhook, essentially a callback URL, to SEBI. This URL is designed to be hit whenever the specific event of interest, such as a particular stock reaching a certain price, occurs.

This is an example of an Event-Driven Architecture where communication happens based on events, rather than constant polling or maintaining a persistent connection.

Here's the sequence of steps in more detail:

1. **Register**: Zerodha first registers a webhook with SEBI. This is a callback URL that Zerodha exposes and asks SEBI to call when a certain event happens. In this case, when a particular stock price reaches a specified value.
2. **Trigger Event**: When the stock price reaches the specified value, the event is triggered on the SEBI side.
3. **Invoke Webhook**: SEBI then sends an HTTP request (usually a POST request) to the registered webhook URL provided by Zerodha. The request would contain information about the event in its body, typically formatted in JSON or XML.
4. **Receive and Process**: Zerodha receives the HTTP request and processes the data contained in the body of the request. Based on the information received, it can take necessary action, such as notifying the user about the price change.

This event-driven method allows efficient communication and helps Zerodha stay updated with real-time changes in stock prices. It avoids the need for long polling and persistent connections, which could be expensive and not scalable when dealing with millions of clients.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a0f98aae-b3e9-4c37-886c-12874041e8ee/Untitled.png)

other examples:

1. CI/CD Deployment Actions
2. MailChimp
3. Zapier
4. Stripe
