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