# The persistent lifecycle

- classes are unaware of their own persistence capability [+];
- *persistence lifecycle*: the states an object goes through during its life;
- *unit of work*: a set of operations you consider one (usually atomic) group;

## The persistent states

![Persistent states](imgs/states.png)

- Transient

    - objects instantiated using the new operator aren’t immediately persistent;
    - any modification of a transient instance isn’t known to a persistence context;
    - Hibernate doesn’t provide any roll-back functionality;
    - objects that are referenced only by other transient instances are transient;
    
- Persistent

    - is an entity instance with a database identity;
    - has a primary key value set as its database identifier;
    - is always associated with a persistence context;
    - may be an instance retrieved from the database by execution of a query;
    - may be objects instantiated by the application and then made persistent by calling one 
      of the methods on the persistence manager;
    
- Removed

    - can be removed by an explicit operation of the persistence manager;
    - is removed when there is no reference to it;
    
- Detached

    - *handle*: a reference to the instance that was saved;
    - its state is no longer guaranteed to be synchronized with database state;
    - there is a way to bring the detached instance back into persistent state:
        - reattachment;
        - merging;
    
## The persistent context

- is a first-level cache of managed entity instances;
- allows dirty checking and transactional write-behind;
    - Hibernate doesn’t update the database row of every single persistent object in
      memory at the end of the unit of work;
    - TODO    
- guarantees a scope of Java object identity;
- spans a whole conversation;



