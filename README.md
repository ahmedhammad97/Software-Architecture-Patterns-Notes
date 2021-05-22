# Software Architecture Patterns Notes
My reading notes following "Software Architecture Patterns" report by Mark Richards. The report compares few popular architecture patterns, discussing clearly each ones pros, cons, and usage.

The report is publicly available [here](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/).

## Content
#### Architecture Patterns:
1. [Layered Architecture](#layered)
2. [Event-driven Architecture](#eventdriven)
3. [Microkernel Architecture](#microkernel)
4. [Microservices Architecture](#microservices)
5. [Space-based Architecture](#spacebased)

[Comparison](#comparison)
[More Reading Notes](#more)

## <a name="layered">Layered (Monolithic) Architecture</a>
<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0101.png" width="500px"/>

A group of horizontal layers, that are stacked over each other, each performing a certain role, resulting in a **separation of concerns** by design. It's a very good starting point for small businesses, that care much about the ease of development and testing.

The number of layers may vary, but the most common are:

- **Presentation layer**: responsible for handling all user interface and browser communication.

- **Business layer**: responsible for executing business rules.

- **Persistence layer**: responsible for converting business information into database queries.

- **Database layer**: responsible for storing, maintaining, and retrieval of the information.

By default, all the layers are *closed*, which means that the layer functionality can only be accessed by the layer above it. This results in **layers isolation**, which means that a change in a layer, should not affect any other layer.

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0103.png" width="500px"/>

A common problem for this architecture is the *"Sink-Hole Anti-Pattern"*, which is when many layers are not processing many requests, and just pass them to the next layer. A common solution is by *opening* some of the layers, which decrease the sink-hole effect, but causes lack of layer isolation.

## <a name="eventdriven">Event-driven Architecture</a>

A distributed, asynchronous, highly scalable architecture. Consists mainly of highly decoupled, single purpose units named *Event Processors*.

The architecture comes in two topologies (fashions):

### Mediator Topology

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0201.png" width="500px"/>

Typically consists of:

- **Event queue**: store the events until the mediator consumes it. 

- **Event mediator (orchestrator)**: receives *initial events*, then breaks them into *processing events* that can be passed accordingly each to the suitable processor. This requires that the mediator knows the corresponding procedure (steps) for every initial event. It does not perform any business logic though.

- **Event channels**: can either be a message queue, or a message topic, that passes the messages *asynchronously* to the event processors.

- **Event processors**: a unit that contains and perform all the business logic. A processor should not rely on other processor.

### Broker Topology

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0203.png" width="500px"/>

It differs from the Mediator Topology in not having an orchestrator. It is useful when the event processing logic is fairly simple, and when there is not need for a central orchestration unit.

Typically consists of:

- **Broker Component**: contains all the event channels needed for passing events.

- **Event Processors**: same as the ones in the previous topology.

The initial event goes directly into one of the event processors, which perform a certain business logic, and then publishes a new event to the broker -that may or may not be consumed by another processor- Making space for extensions, and future features.

### Common Problems

Because of the distributed nature of this architecture, it typically faces some problems like:

- Remote processes unavailability
- Lack of responsiveness
- Mediator or Broker failure
- Lack of atomicity
- Difficulty in creating contracts

Some of these problems can by overcame by replication or federation, but others like atomicity problems may give a good indication that it is not the suitable architecture pattern.

## <a name="microkernel">Microkernel (Plug-in) Architecture</a>

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0301.png" width="500px"/>

An architecture that is mainly used for it's extensibility, feature separation, and isolation. It is the best option for product-based applications, operating systems, and any evolutionary design.

Consists of:

- **Core system**: which contains the main minimal functionality of the application. It must have some registry list of all the existing plug-ins.

- **Plug-in modules**: stand-alone, independent components that performs a certain task.

The communication between the plug-ins should be minimal, however some plug-ins can require the existence of other plug-ins, creating some **dependency concerns**.

The core component can communicate with the plug-ins through OSGI, message passing, web services, or even point to point binding. In all cases, a well defined contract is required.

## <a name="microservices">Microservices Architecture</a>

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0401.png" width="500px"/>

An architecture that focuses on the ease of real time deployment, and therefore, scalability. It consists of separately distributed deployed units, called *service components* that each has a single purpose, and are transparent to the user, who uses them through an interface layer. Those components can vary in size, from a single module, to a whole sub-system.

The microservices architecture usually evolves to solve problems in other architectures, rather than being the initial architecture.

There are three famous types:

- **API REST-based**: service components are fine-grained, typically a module or two, and accessed through REST interface that is usually implemented through separately deployed web API layer.

- **APP REST-based**: service components are course-grained, contains more business logic, and are accessed through a web-based application screen. 

- **Centralized messaging**: a lot similar to the APP REST-based, except that instead of the REST protocol, it uses a lightweight centralized message broker. This allows more advanced queuing mechanisms, asynchronous messaging, monitoring, error handling, and better load balancing and scalability. Yet, creating a single point of failure, that can be avoided by clustering or / and federation.

### Common Problems

- Transactions (Atomicity) is very hard
- Remote service unavailability
- Remote access authentication
- Remote access authorization
- Lack of orchestration

### Tips

1. If you found yourself in a need for an orchestrator, chances are that the service components are too fine grained.

2. Inter-service communication between components can be avoided through:
	- Shared database that stores the common information needed by both components.
	- Copying the desired functionality to both components (violating DRY rule).

## <a name="spacebased">Space-based (Cloud) Architecture</a>

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0501.png" width="500px"/>

An architecture that is specifically designed to address and solve scalability and concurrency issues *"architecturally"*, instead of trying to scale out bottleneck layers, which only moves the problem to a deeper layer (which is usually harder to scale).

It is suitable for applications with *unpredictable* concurrent user volume.

This high scalability is achieved by removing the central database constraint, and using replicated in-memory data grids instead. Some centralized data store can still be used to load and asynchronously persist data updates.

It typically consists of:

- **Processing unit**: contains all applications, and perform all the business logic.

- **Virtual middleware**: the controller of the architecture, and usually consists of four main components:
	- **Message grid**: manages input requests and session information.
	- **Data grid**: manages data replication between processing units.
	- **Processing grid**: manages distributed requests processing.
	- **Deployment manager**: manages the dynamic startup and shutdown of the unit.

Cloud architecture applications usually resides inside cloud hosted services *(PAAS)*, but it is not a must.

## <a name="comparison">Comparison</a>

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_aa01.png" width="500px"/>

## <a name="more">More Reading Notes</a>
- [Designing Data Intensive Applications](https://github.com/ahmedhammad97/Designing-Data-Intensive-Applications-Notes)
- [Clean Code](https://github.com/ahmedhammad97/Clean-Code-Do-And-Dont)
