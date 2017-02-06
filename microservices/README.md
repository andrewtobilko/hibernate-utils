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