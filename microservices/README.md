## Microservices

### Monolithic Hell

- [+] is simple to develop;
- [+] easy to test;
- [-] slows down development due to frequent restarts;
- [-] you must redeploy the entire application in order to update any one part of it;
- [-] can also be difficult to scale when different modules have conflicting resource requirements;
- [-] reliability:  a bug in any module can potentially bring down the entire process;
- [-] is difficult to adopt new frameworks and languages;

### Microservices

- [+] it tackles the problem of complexity (decomposes into a set of services);
- [+] services are much faster to develop;
- [+] services are much easier to understand and maintain;
- [+] enables each service to be scaled independently;
- [+] enables each microservice to be deployed independently;
- [-] you need to choose and implement an inter-process communication mechanism;
- [-] the partitioned database architecture;
- [-] hard to test;
- [-] implementing changes that span multiple services;
- [-] deploying is much more complex;

## API Gateway

### Direct client‑to‑microservice communication

- [-] the mismatch between the needs of the client and the fine‑grained APIs;
- [-] makes the client code much more complex;
- [-] some services might use protocols that are not web‑friendly;
- [-] makes it difficult to refactor the microservices;

### API gateway

- is a server that is the single entry point into the system;
- encapsulates the internal system architecture and provides an API that is tailored to each client;
- might have other responsibilities:
    - authentication;
    - monitoring;
    - load balancing
    - caching;
    - request shaping and management;
    - static response handling;
- [-] is yet another highly available component that must be developed, deployed, and managed;

### Implementing

##### Performance and Scalability

- build the API Gateway on a platform that supports asynchronous, nonblocking I/O;

##### Using a Reactive Programming Model

- write API Gateway code in a declarative style using a reactive approach
(e.g. `CompletableFuture` in Java 8)

##### Service Invocation

- use an asynchronous, messaging‑based mechanism (e.g. JMS);
- use a synchronous mechanism (e.g. HTTP);
- you might use multiple implementations of each style;

##### Service Discovery

- use to determine dynamically assigned locations of the service instances;

##### Handling Partial Failures

- try to return a part of the result;
- return cached data if that was available;
- stop cascading failures;

## Service Discovery

- service instances have dynamically assigned network locations;
- the set of service instances changes dynamically because of auto-scaling, failures, and upgrades;

### The Client-Side Discovery

1. queries a service registry (a database of available service instances);
2. uses a load-balancing algorithm to select one of the available service instances;

- benefits:
    - is relatively straightforward;
    - can make intelligent, application-specific load-balancing decisions such as using hashing;
- drawbacks:
    - couples the client with the service registry;
    - must be implemented client-side service discovery logic;
    
### The Server-Side Discovery

1. requests to a a load balancer;
2. the load balancer queries the service registry;
3. the load balancer routes each request to an available service instance;

- benefits:
    - details of discovery are abstracted away from the client;
    - eliminates the need to implement discovery logic by clients;
- drawbacks:
    - is highly available system component that you need to set up and manage;
    
### The Service Registry

- is a key part of service discovery;
- is a database containing the network locations of service instances;
- should be highly available and up to date;
- provides an API for registering and querying service instances;

### Service Registration Options

- the self-registration pattern:
    -  a service instance is responsible for registering and deregistering itself with
    the service registry;
    - a service instance sends heartbeat requests to prevent its registration from
    expiring;
    - [+] it is relatively simple and doesn’t require any other system components;
    - [-] it couples the service instances to the service registry;
    - [-] you must implement the registration code in each service;
- the third-party registration pattern:
    - the service registrar handles the registration by either *polling the deployment
    environment* or *subscribing to events*;
    - the service registrar also deregisters terminated instances;
    - [+] you don’t need to implement service-registration logic for each service;
    - [+] registration is handled in a centralized manner within a dedicated service;
    - [+] services are decoupled from the service registry;
    - [-] it is yet another highly available system component;
    
## Event-Driven Data Management

### The problems

- the data owned by each microservice is private to that microservice and can
only be accessed via its API;
- different microservices often use different kinds of databases (the  polyglot
persistence approach):
    - [+] loosely coupled services
    - [+] better performance and scalability;
    - [-] how to implement business transactions that maintain consistency across
    multiple services?
    - [-] how to implement queries that retrieve data from multiple services?
    
### Event-Driven Architecture

- [+] provides eventual consistency;
- [+] enables an application to maintain materialized views;
- [-] is more complex than when using ACID;
- [-] the application can also see inconsistencies;
- [-] subscribers must detect and ignore duplicate events;

#### Achieving Atomicity

- local transactions:
    - there are an EVENT table and an Event Publisher;
    - the application begins a local transaction, updates the state of the business
    entities, inserts an event into the EVENT table, and commits the transaction;
    -  an Event Publisher process queries the EVENT table, publishes
    the events to the Message Broker, and then uses a local transaction to mark the
    events as published;
    - [+] it guarantees an event is published for each update without using 2PC;
    - [-] the developer must remember to publish events;
    - [-] it is hard to implement with some NoSQL databases;
- transaction log:
    - the application updates the database, which results in changes being recorded
    in the database’s transaction log;
    - the Transaction Log Miner thread or process reads the transaction log and
    publishes events to the Message Broker;
    - [+] it guarantees an event is published for each update without using 2PC;
    - [+] separating event publishing from the application’s business logic;
    - [-] the format of the transaction log is proprietary to each database and can
    be changed;
- event sourcing:
    - the application stores a sequence of state-changing events;
    - each event contains sufficient data to reconstruct the data state;
    - [+] makes possible to implement temporal queries that determine the state of
    an entity at any point in time;
    - [+] your business logic consists of loosely coupled business entities that
    exchange events;
    
## Deployment Strategies

- multiple service instances per host pattern;
- service instance per host pattern;
- serverless deployment;