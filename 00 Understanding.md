

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

Speed
  - **Latency** : Time for one round trip
  - **Bandwidth** : How much fits through at once

Loadbalancer is server which runs an application and this application entire job is to take each incoming request and forward the request to one of the server, so speading the load across servers.

Lets sat we have 10 servers and 100 reqeusts
So loadbalancer sends 10 request to each servers so the load is even.

**Reverse Proxy:**
 - 












