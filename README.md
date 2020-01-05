# Software Architecture Patterns Notes
My reading notes following "Software Architecture Patterns" report by Mark Richards. The report compares few popular architecture patterns, discussing clearly each one's pros, cons, and usage.

The report is publicly available [here](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/).

## Content
#### Architecture Patterns:
1. [Layered Architecture](#layered)
2. [Event-driven Architecture](#eventdriven)
3. [Microkernel Architecture](#microkernel)
4. [Microservices Architecture](#microservices)
5. [Space-based Architecture](#spacebased)

[Comparison](#comparison)

## <a name="layered">Layered (Monolithic) Architecture</a>
<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0101.png" width="500px"/>

A group of horizontal layers, that are stacked over each other, each performing a certain role, resulting in a **separation of concerns** by design. It's a very good starting point for small businesses, that care much about the ease of deployment and testing.

The number of layers may vary, but the most common are:
- Presentation layer: responsible for handling all user interface and browser communication.
- Business layer: responsible for executing business rules.
- Persistence layer: responsible for converting business information into database queries.
- Database layer: responsible for storing, maintaining, and retrieval of the information.

By default, all the layers are *closed*, which means that the layer functionality can only be accessed by the layer above it. This results in **layers isolation**, which means that a change in a layer, should not affect any other layer.

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0103.png" width="500px"/>

A common problem for this architecture is the *"Sink-Hole Anti-Pattern"*, which is when many layers are not processing many requests, and just pass them to the next layer. A common solution is by *opening* some of the layers, which decrease the sink-hole effect, but causes lack of layer isolation.

## <a name="eventdriven">Event-driven Architecture</a>

A distributed, asynchronous, highly scalable architecture. Consists mainly of highly decoupled, single purpose units named *Event Processors*.

The architecture comes in two topologies (fashions):

#### Mediator Topology

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0201.png" width="500px"/>

Typically consists of:
- Event Queue: store the events until the mediator consumes it. 
- Event Mediator (Orchestrator): receives *initial events*, then breaks them into *processing events* that can be passed accordingly each to the suitable processor. This requires that the mediator knows the corresponding procedure (steps) for every initial event. It does not perform any business logic though.
- Event Channels: can either be a message queue, or a message topic, that passes the messages *asynchronously* to the event processors.
- Event Processors: a unit that contains and perform all the business logic. A processor should not rely on other processor.

#### Broker Topology

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0203.png" width="500px"/>

It differs from the Mediator Topology in not having an orchestrator. It is useful when the event processing logic is fairly simple, and when there is not need for a central orchestration unit.

Typically consists of:
- Broker Component: contains all the event channels needed for passing events.
- Event Processors: same as the ones in the previous topology.

The initial event goes directly into one of the event processors, which perform a certain business logic, and then publishes a new event to the broker -that may or may not be consumed by another processor- Making space for extensions, and future features.

#### Common Problems

Because of the distributed nature of this architecture, it typically faces some problems like:

- Remote processes availability
- Lack of responsiveness
- Mediator or Broker failure
- Lack of atomicity
- Difficulty in creating contracts

Some of these problems can by overcame by replication or federation, but others like atomicity problems may give a good indication that it is not the suitable architecture pattern.

## <a name="microkernel">Microkernel (Plug-in) Architecture</a>

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0301.png" width="500px"/>

An architecture that is mainly used for it's extensibility, feature separation, and isolation. It is the best option for product-based applications, operating systems, and any evolutionary design.

Consists of:

- Core system: which contains the main minimal functionality of the application. It must have some registry list of all the existing plug-ins.
- Plug-in modules: stand-alone, independent components that performs a certain task.

The communication between the plug-ins should be minimal, however some plug-ins can require the existence of other plug-ins, creating some **dependency concerns**.

The core component can communicate with the plug-ins through OSGI, message passing, web services, or even point to point binding. In all cases, a well defined contract is required.

## <a name="microservices">Microservices Architecture</a>

## <a name="spacebased">Space-based Architecture</a>

## <a name="comparison">Comparison</a>
