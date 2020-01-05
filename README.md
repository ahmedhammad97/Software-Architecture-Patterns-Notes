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

## <a name="layered">Layered (Monolithic) Architecture</a>
<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0101.png" width="50%"/>

A group of horizontal layers, that are stacked over each other, each performing a certain role, resulting in a **separation of concerns** by design. It's a very good starting point for small businesses, that care much about the ease of deployment and testing.

The number of layers may vary, but the most common are:
- Presentation layer: responsible for handling all user interface and browser communication.
- Business layer: responsible for executing business rules.
- Persistence layer: responsible for converting business information into database queries.
- Database layer: responsible for storing, maintaining, and retrieval of the information.

By default, all the layers are *closed*, which means that the layer functionality can only be accessed by the layer above it. This results in **layers isolation**, which means that a change in a layer, should not affect any other layer.

<img src="https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/assets/sapr_0103.png" width="50%"/>

A common problem for this architecture is the *"Sink-Hole Anti-Pattern"*, which is when many layers are not processing many requests, and just pass them to the next layer. A common solution is by *opening* some of the layers, which decrease the sink-hole effect, but causes lack of layer isolation.

## <a name="eventdriven">Event-driven Architecture</a>

## <a name="microkernel">Microkernel Architecture</a>

## <a name="microservices">Microservices Architecture</a>

## <a name="spacebased">Space-based Architecture</a>

## <a name="comparison">Comparison</a>
