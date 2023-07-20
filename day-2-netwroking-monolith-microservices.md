# Day-2  **Networking, Monolith and Microservices**

**Recap of Day-1**

IP (Internet Protocol) and Port are essential components of internet communications.

1. **IP (Internet Protocol) Address**: This is the unique identifier that devices use to connect to each other on a network. It can be likened to a postal address for the internet, telling data packets where to go. There are two types of IP addresses in wide use: IPv4, which is a series of four numbers separated by dots (e.g., 192.168.1.1), and IPv6, which is a longer series of numbers and letters separated by colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334). *(This is like a house address, but for devices on the internet. Just like a letter needs an address to know where to go, data packets use IP addresses to find their destination)*
2. **Port**: A port is a virtual data connection in which the data from a device can be split into multiple channels. The port number identifies what type of port it is. For instance, web traffic often uses port 80 for HTTP and port 443 for HTTPS. Ports allow a single IP address to manage multiple connections at the same time, with each connection using a unique port number. They can range from 0 to 65535, and are divided into three main ranges: Well-Known Ports (0-1023), Registered Ports (1024-49151), and Dynamic or Private Ports (49152-65535). (*These are like different doors or windows of that house (IP address). Each type of service (like web browsing, email, etc.) has its own door (port number) to come and go).*

Together, an IP address and a port number form a complete destination for network traffic. This combination allows the network to deliver data to the right device (IP address) and to the right program within that device (port number).

Transport layer takes care of connection between Client and Server

For example, *when you open a website (say, **[www.example.com](http://www.example.com/)**), your device (the client) uses DNS to get the IP address for that website (the server), then sends a SYN packet to that IP address on port 80 (for HTTP) or 443 (for HTTPS). The server responds with a SYN-ACK, and the client acknowledges with an ACK, thus establishing the TCP connection. Then, the client can request the website data from the server, which the server sends back. When all data has been transferred, they close the connection.*

Transport Layer handles the technical aspects of sending and receiving data, while the Application Layer interacts with the software (browser, in this case) to prepare the data and display the received data to the user.

________________________________________________________________________________________________________
**Day-2 Class**

**Realtime communication / data exchange between server to clients.**

There are different ways like
**WebSockets, Server-Sent Events (SSE), Polling, WebRTC, Push Notifications, MQTT, Socket.IO**

**Polling** 

**Polling**: In the context of client-server communication, polling is like continually asking "do you have any updates?" from the client side. For example, imagine you're waiting for a friend to finish a task. You keep asking "Are you done yet?" - that's like polling.

### **1) Short Polling**

In short polling, the client sends a request to the server asking if there's any new information. The server immediately responds with the data if it's available or says "no data" if it's not. The client waits for a short period before sending another request. It's like asking your friend "Are you done yet?" every 5 minutes.

**Short Polling**

Advantages:

1. **Simple to Implement:** Short polling is simple and requires little work to set up. It doesn't require any special type of server-side technology.
2. **Instantaneous Error Detection:** If the server is down, the client will know almost immediately when it tries to poll.

Disadvantages:

1. **High Network Overhead:** Short polling can cause a lot of network traffic as the client keeps polling the server at regular intervals.
2. **Wasted Resources:** Many of the requests might return empty responses (especially if data updates are infrequent), wasting computational and network resources.
3. **Not Real-Time:** There is a delay between when the new data arrives at the server and when the client receives it. This delay could be up to the polling interval.

### 2**. Long Polling**

 In long polling, the client asks the server if there's any new information, but this time the server does not immediately respond with "no data". Instead, it waits until it has some data or until a timeout occurs. Once the client receives a response, it immediately sends another request. In our friend example, it's like asking "Let me know when you're done" and waiting until your friend signals they've finished before asking again.

**Long Polling**

Advantages:

1. **Reduced Network Overhead:** Compared to short polling, long polling reduces network traffic as it waits for an update before responding.
2. **Near Real-Time Updates:** The client receives updates almost instantly after they arrive on the server, because the server holds the request until it has new data to send.

Disadvantages:

1. **Complexity:** Long polling is more complex to implement than short polling, requiring better handling of timeouts and more server resources to keep connections open.
2. **Resource Intensive:** Keeping connections open can be resource-intensive for the server if there are many clients.
3. **Delayed Error Detection:** If the server is down, the client might not know until a timeout occurs.

Assignment : Write a code for long polling and short polling, by creating client and server locally in your own language.

**Pseudo-Code for Short polling**

```jsx
function fetchData() {
    response = makeHttpRequest(GET, URL)

    if (response.status == 200) {
        processData(response.body)
    } else {
        handleRequestError(response.status)
    }
}

function initiateShortPolling() {
    while (true) {
        fetchData()
        wait(5 seconds)  // wait for 5 seconds before the next request
    }
}

initiateShortPolling() 
```

**Pseudo-Code for Long polling**

```jsx
function initiateLongPolling() {
    response = makeHttpRequest(GET, URL)

    if (response.status == 200) {
        processData(response.body)
        initiateLongPolling()  
    } else if (response.status == 504) {  // HTTP status code for timeout
        initiateLongPolling()  
    } else {
        handleError(response.status)
    }
}

initiateLongPolling()  // Start the long polling
```

### **3. Websockets**

WebSockets is a communication protocol that provides full-duplex communication between a client and a server over a long-lived connection. It's commonly used in applications that require real-time data exchange, such as chat applications, real-time gaming, and live updates.

how WebSocket work:

1. **Opening Handshake**: The process begins with the client sending a standard HTTP request to the server, with an "Upgrade: WebSocket" header. This header indicates that the client wishes to establish a WebSocket connection.
2. **Server Response**: If the server supports the WebSocket protocol, it agrees to the upgrade and responds with an "HTTP/1.1 101 Switching Protocols" status code, along with a "Upgrade: WebSocket" header. This completes the opening handshake, and the initial HTTP connection is upgraded to a WebSocket connection.
3. **Data Transfer**: Once the connection is established, data can be sent back and forth between the client and the server. This is different from the typical HTTP request/response paradigm; with WebSocket, both the client and the server can send data at any time. The data is sent in the form of WebSocket frames.
4. **Pings and Pongs**: The WebSocket protocol includes built-in "ping" and "pong" messages for keeping the connection alive. The server can periodically send a "ping" to the client, who should respond with a "pong". This helps to ensure that the connection is still active and that the client is still responsive.
5. **Closing the Connection**: Either the client or the server can choose to close the WebSocket connection at any time. This is done by sending a "close" frame, which can include a status code and a reason for closing. The other party can then respond with its own "close" frame, at which point the connection is officially closed.
6. **Error Handling**: If an error occurs at any point, such as a network failure or a protocol violation, the WebSocket connection is closed immediately.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/611d681c-a98b-45d2-93fd-190cb303a44b/Untitled.png)

Assignment : Debug WhatsApp Web to see the requests for WebSocket.

**Key Differences (Long-Polling vs WebSocket)**:

1. **Bidirectional vs Unidirectional**: WebSockets provide a bidirectional communication channel between client and server, meaning data can be sent in both directions independently. Long polling is essentially unidirectional, with the client initiating all requests.
2. **Persistent Connection**: WebSockets establish a persistent connection between client and server that stays open for as long as needed. In contrast, long polling uses a series of requests and responses, which are essentially separate HTTP connections.
3. **Efficiency**: WebSockets are generally more efficient for real-time updates, especially when updates are frequent, because they avoid the overhead of establishing a new HTTP connection for each update. Long polling can be less efficient because it involves more network overhead and can tie up server resources keeping connections open.
4. **Complexity**: WebSockets can be more complex to set up and may require specific server-side technology. Long polling is easier to implement and uses traditional HTTP connections.

### **4.SSE**

Server-Sent Events (SSE) is a standard that allows a web server to push updates to the client whenever new information is available. This is particularly useful for applications that require real-time data updates, such as live news updates, sports scores, or stock prices.

Here's a detailed explanation of how SSE works:

1. **Client Request**: The client (usually a web browser) makes an HTTP request to the server, asking to subscribe to an event stream. This is done by setting the "Accept" header to "text/event-stream".
2. **Server Response**: The server responds with an HTTP status code of 200 and a "Content-Type" header set to "text/event-stream". From this point on, the server can send events to the client at any time.
3. **Data Transfer**: The server sends updates in the form of events. Each event is a block of text that is sent over the connection. An event can include an "id", an "event" type, and "data". The "data" field contains the actual message content.
4. **Event Handling**: On the client side, an EventSource JavaScript object is used to handle the incoming events. The EventSource object has several event handlers that can be used to handle different types of events, including "onopen", "onmessage", and "onerror".
5. **Reconnection**: If the connection is lost, the client will automatically try to reconnect to the server after a few seconds. The server can also suggest a reconnection time by including a "retry" field in the response.
6. **Closing the Connection**: Either the client or the server can choose to close the connection at any time. The client can close the connection by calling the EventSource object's "close" method. The server can close the connection by simply not sending any more events.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed46e424-5f9a-4f65-8381-413b54c109bd/Untitled.png)

SSE Stream
Server-Sent Events (SSE) are a part of the HTML5 specification that allow a web server to push updates to a client's web browser via HTTP connections. These updates are streamed to the client as they become available, allowing for real-time communication from server to client.

Assignment : Debug any websites to see the requests for SSE.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33d39463-50c2-48b1-a942-5ca2b50d645d/Untitled.png)

| Technology | Use Case | Detailed Example |
| --- | --- | --- |
| Short Polling | Useful when updates are infrequent, and immediate notification of updates is not crucial. | Weather app: A weather app could use short polling to fetch updated weather information every 10 minutes. The user doesn't need real-time updates, so the delay and overhead of short polling is acceptable. |
| Long Polling | When updates are infrequent but receiving an update as soon as it is available is important, long polling can be an effective approach. | Email client: An email client could use long polling to notify a user as soon as a new email arrives. The server holds the connection open until a new email arrives, then sends the email to the client and closes the connection. |
| SSE (Server-Sent Events) | Good for real-time applications where updates flow in one direction, from server to client. | Stock Price Update: A stock trading app could use SSE to push real-time price updates to the client. The server sends a continuous stream of updates to the client as soon as new price data becomes available. |
| WebSockets | Suitable for real-time applications where communication needs to happen in both directions between client and server. | Chat Application: A chat app could use WebSockets for real-time, bidirectional communication. When a user sends a message, it's pushed to the server, which then pushes the message to the other users. And vice versa, when the server has a new message, it can push it to all connected clients. |

________________________________________________________________________________________________________

### **Monolithic Architecture:**

In a monolithic architecture, all components of the application are tightly integrated and run in a single service. This means that if you were to update a small part of your application, you'd need to rebuild and deploy the whole thing.

*Example:* Consider a simple web application like a blog. You have users who can write posts, comment on posts, and perhaps like posts. In a monolithic architecture, all these features (user management, writing posts, commenting, liking) are developed together and run in the same service.

*When to use:* Monolithic architectures are beneficial when your application is simple, the team is small, rapid prototyping is required, or when you want to maintain consistent control over all parts of your application. They are especially good for small-to-medium-sized applications where you want to keep everything in one place.

### **Microservices Architecture:**

In a microservices architecture, an application is built as a collection of small services, each running in its own process and communicating with lightweight mechanisms (like HTTP). These services are built around business capabilities and are independently deployable by fully automated deployment machinery.

*Example:* Let's use a similar blog application example. In a microservices approach, each function of the blog might be split into separate services. User management might be one service, writing posts might be another service, commenting another, and liking posts yet another. Each of these services would be developed, deployed, and scaled independently.

*When to use:* Microservices are beneficial when your application is large and complex, you have a larger team, different components have different resource requirements, or you need to ensure high availability. They are especially good for large applications where different teams can work on different services simultaneously and each service can be scaled independently based on demand.

Both these architectures have their own benefits and drawbacks, and the decision to use one over the other depends on factors like the size and complexity of the application, the team's skills and resources, and the specific use-case requirements.
