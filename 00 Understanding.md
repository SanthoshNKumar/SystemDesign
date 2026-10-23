

**HLD (High level Design)**
 - It is an architectuire of complete system, how different components in the sytem are interacting each other.


**LLD (Low Level Design)**
 - foucs on classes and objects within a system and how they are interacting each other
 - Goal
    - To write clean code
    - Code should be flexible and maintainable
    - code should be easy to test

Categories of LLD
 - Creational
 - Structural
 - Behavioral

**Creational**
 - It controls the creation of Objects
 - Patterns
    - Singleton
    - Builder
    - Factory
    - Abstract factory
    - Object Pool
    - Prototype

**Structural**
 - It foucs on , how different classes/objects are arranged together so that larger problem can be solved in most flexible way
 - Pattern
    - Decorator
    - Proxy
    - Composite
    - Adapter
    - Bridge
    - Facade
    - Flyweight

**Behavioral**
- It focus on , how different objects communicate or interact with each other.
- With Structural pattern we created the skeleton of the system, but how that skeleton behavas(Coordination, Responsibility and interaction) is all guided by Behavioral pattern
- Pattern
    - State
    - Strategy
    - Observer
    - Chain of Responsibility
    - Template
    - Iterator
    - Interpreter
    - Command
    - Visitor
    - Mediator
    - Memnto

System design is the skill of choosing right components and putting them right  place to solve the specific software problem.

As a senior Engineer you need to design the whole System

DataBases:
Caching
CDNS
LoadBalancing : It routes each requests 
Servers
Replica
Queue


The database overloads when user asking same thing again and again so th easy fix is palicng popular answer in memory so that user dont have to query the database again and ahain and it can be achieved by Caching.


When server or machine fails due to 'Disk Fails' 'Power Out' 'Cable Unplug' since databases and other services running on one box , you app is down. this problem is called 'Single Point of failure' and the soluction is to keep the data more than one machine.

While Building any application two things we should ask.
1. What should it DO? (Funcational)
2. How well must it so it? (Non-Functional)


Questions to ask ?
 - How many users? how fast it growing     
 - is the application read-heavy or write-heavy
    - scrolling (read)
    - posting (write)
 - What data can you never lose?
 - What data you offered to loose?
 - How much latency can you afford?
    - openingthe feed
    - Load in under 200 ms
 - What does it cost?


 A cache - because reads dominates

 Should the application be 'Monolith' or 'Microservice'

 **Monolith**
    - One codebase
    - One deployment
    - one running thing

**Microservice**
    - Split into many programs
    - Seperate services with seperate code base and deployed  its own and runs on its won server.
    - They must talk over the network

Why **Monilith** application split into **Microservices**
    - Independent Scaling
    - Team size and each team can work on own service and ship on items own schedule
    - Microservice has to make network call to talk to other services, every network call can slow, time out or fail.

    
**Veritical Scaling**
    - More CPU
    - More RAM
    - Faster DISK

**Horizontal Scaling**
    - Many Small servers
    - Each running copy of your application
    - Load balancing spreads traffic across all the machine above

**DNS:** Doamin Name System
    - DNS converts a domain name into an IP address.  
    - Example
        
        www.contoso.com
            |
        DNS Lookup
            |
        20.100.50.10
        


**CDN:** Content Delivery Network


Speed
  - **Latency** : Time for one round trip
  - **Bandwidth** : How much fits through at once

Loadbalancer is server which runs an application and this application entire job is to take each incoming request and forward the request to one of the server, so speading the load across servers.

Lets sat we have 10 servers and 100 reqeusts
So loadbalancer sends 10 request to each servers so the load is even.

Load Balancer:
    - Distributes traffic across multiple web servers
    - Improves availability
    - Prevents a single server from becoming overloaded




**Reverse Proxy:**
 - 


**Client Request:**A user's browser sends a request to a website or API.

***************************************************************************

**Web server:** A web server is software (and often the underlying hardware) designed to accept incoming network requests via HTTP or HTTPS and return responses—most commonly web pages, images, and API data.

Web Server
    - Receives HTTP requests
    - Routes requests to Read API or Write API
        - GET /abc123 Goes to Read API
        - POST /create Goes to Write API

***************************************************************************

**Application Server:** An application server (or app server) is a software framework that exposes business logic to client programs through various protocols, primarily running server-side code to generate dynamic content.It has Deep integration with databases to query, write, and update data.

**Protocol:** A protocol is a standardized set of rules that defines how data is formatted, transmitted, received, and understood between different devices on a network

**Gateway:** Single entry point or front door 

**API Gateway**
    - Request Routing
    - Authentication and Authorization
    - Rate Limiting and Throttling
    - Caching
    - Request and Response Transformation
    - Load Balancing
    - Monitoring
    - Logging
    - API versioning
    - Request/response transformation
    - Service Discovery
    - Circuit Breaker

**API Gateway** deos below the ones before the request reaches the backend.
    - Verify JWT token
    - Check user permissions
    - Apply rate limits
    - Log requests
    - Route to correct service

Caching
 - Client cache
 - DNS Cache
 - CDN Cache
 - Web Server Cache
 - App Server Cache
 - Database Cache

 ***Data Partitioning** : 
    - Horizontal partitioning (Sharding): Dividing the rows of a table into smaller tables and storing them on different server or databases instances.  
    - Vartical partitioning : Involves dividing the columns of a table into separate tables.

**Database Repliaction** :
    - Technique to maintain multiple copy of same databases across different servers or locations.
    - Focus
        - Data Availability
        - Redundancy
        - Fault Tolerance

**Distributed Messaging Systems** :

******************************************************************
**Stateless and Stateful Systems**
    - **Stateful** = Remembers previous interactions.
    - **Stateless** = Treats every request as completely new.

    Example:
    **Stateful**
        A hotel receptionist remembers you.

        Day 1:
        You: My room number is 101

        Day 2:
        Receptionist: Welcome back, Mr. Santhosh!

        The receptionist stores information about you.
        ✅ Remembers history

    **Stateless**
    A token machine at a government office.

    Visit 1 -> Take Token
    Visit 2 -> Take New Token
    Visit 3 -> Take New Token

    No memory of previous visits.
    ✅ Each interaction is independent

### Stateful Systems

-   Database connections
-   WebSocket connections
-   Online multiplayer games
-   Shopping carts stored in server memory
-   Video conferencing sessions
-   Redis Pub/Sub subscribers
-   FTP sessions

### Stateless Systems

-   REST APIs
-   HTTP requests
-   Microservices with JWT authentication
-   Azure Functions
-   Serverless applications
-   API Gateway requests
-   Azure OpenAI API calls

******************************************************************


**Token**: 
A token is a secure, lightweight digital ticket that represents a user's identity or permissions so they don't have to re-enter their password on every request.

Usecase for **Token**
• When a user types their username and password and clicks "Login," those credentials travel to your dedicated Auth Server. The Auth Server checks the database to verify the password is correct. If it is, the Auth Server generates, signs, and issues the token, sending it back to the user's browser.

When will **Token** delete?
However, if you clear your Cookies and Cache/Site Data, the tokens will be completely deleted, and you will be immediately logged out of all your apps and websites.

Why **Token**?
In a Microservices architecture, your app is broken down into dozens of tiny, independent services (e.g., Auth Service, Payment Service, Inventory Service) running on completely different servers.

In a Monolithic application, everything is built inside one big codebase running on a single server with a single database and  'Sessions' work perfectly:

**Sessions** are built for Monoliths, and **Tokens** are built for Microservices.

**JWTs** :JSON Web Tokens
- JWT is the container that holds the authentication information, and that container is the token itself

- A raw JWT is represented as a single, long string split into three distinct components separated by dots (.): Header.Payload.Signature
-  How JSON turns into a JWT
    - You write regular JSON:
    - The server encodes it (Base64URL encoding)
    - The server signs it: 

**********************************************************************************

**OAuth**: Open Authorization:
    - Its is a standard protocol that allows an application to access resources on behalf of a user without sharing the user's password.

    - Example:
        Instead of giving housekeeping your house key, you give them a temporary access card that only works for your room and only during certain hours.

        Password = House Key 🔑
        OAuth Token = Temporary Access Card 🎫

        OAuth uses access tokens instead of passwords.

        API key = Company Access Card

**********************************************************************************

**MFA (Multi Factor Authentication):**

**Pub/Sub** Architecture


**NGINX** :NGINX (pronounced "Engine-X") is a high-performance web server that is commonly used for 
    - Web Server
    - Reverse Proxy
    - Load Balancer
    - API Gateway
    - SSL/TLS Termination Server

**SSL/TLS:**   **SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are protocols used to **encrypt data** transmitted between a client (browser, mobile app) and a server.

**TLS** is the modern replacement for **SSL** and is used by HTTPS to protect data such as passwords, API requests, and personal information from eavesdropping and tampering during transmission.

**SSL/TLS Termination:**
SSL/TLS termination happens at the point where encrypted HTTPS traffic is decrypted and converted into plain HTTP (or another protocol) so the application can process it.

**Websocket?**
WebSocket is a communication protocol that provides a persistent, two-way (full-duplex) connection between a client and a server.

Unlike traditional HTTP, where the client sends a request and waits for a response, WebSocket allows both client and server to send messages anytime after the connection is established.

REST API :
    - Communication : Request/Response
    - Best For : CRUD operations

WebSocket :
    -Communication :  Two-way
    - Best For : Live data streaming

**Capacity Planning**:
    - It is the process of determining how much infrastructure, compute power, storage, memory, network bandwidth, and application resources are needed to handle current and future workloads without performance issues.

Capacity Planning answers the question: **"How many users, requests, servers, databases, and resources do I need to support my application?"**


**HTTP Methods** :
    - HTTP (HyperText Transfer Protocol) is used for communication between clients and servers. 
    - Methods
        - GET: Retrieves data from the server.
        - POST: Creates or submits new data.
        - PUT: Updates or replaces existing data.
        - PATCH: Partially updates existing data.
        - DELETE: Removes data.

**Rate Limiting** :

****************************************************************

**Throttling**:
    - **Throttling** is a technique used to **limit the rate of requests** that a user, application, or service can send to a system.

    - > **Throttling = Slowing down or restricting requests to protect a system from overload.**

Without throttling:

User
  |
10000 Requests/Second
  |
Server

Result:

❌ High CPU Usage
❌ Memory Exhaustion
❌ System Crash


With throttling:

User
|
10000 Requests/Second
|
Throttle Limit: 100 Requests/Second
|
Server

Result :

✅ Stable Performance
✅ Fair Usage
✅ Resource Protection

****************************************************************




**Paging:**

**Proxy** 
    - A **proxy** is an intermediary that sits between a client and a server and forwards requests on behalf of the client.
    - Benefits
        - Content filtering
        - Monitoring
        - Security
        - Hides client IP addresses
    - **API Gateway** is a specialized type of reverse proxy


**Entra ID**:
    - **Microsoft Entra ID** (formerly **Azure Active Directory / Azure AD**) is Microsoft's cloud-based **Identity and Access Management (IAM)** service. It helps organizations manage **users, applications, authentication, and authorization**.

**Single Sign-On (SSO)** :
    - One login gives access to multiple applications.
    - with Entra ID you can access Teams; Outlook;Azure
    - User signs in once and accesses all authorized applications.

**Multi-Factor Authentication (MFA)** 
    - Provides an extra layer of security.
    - Password + OTP / Authenticator Approval


**Redundancy**:
    - **Redundancy** means having an extra or backup component available so that if one   component fails, another can continue providing the service.


What is Request 

What is Response

Aggregare Response

Rate-limiting policies

What is Route


***************************************************************************
**Database Scaling Techniques**

**Sharding** – Splitting data across multiple servers.

**Replication** – Creating copies of data for redundancy

**Indexing** – Optimizing search queries for faster access.

***************************************************************************


***************************************************************************

***************************************************************************


***************************************************************************

***************************************************************************

***************************************************************************