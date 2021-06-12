# CQRS
CQRS architecture with docker and microservice in java 

CQRS - Structure Flow
client request -> API gateway-> and Route to user CMD API (modify/create/Date  change of state event)->reister on Event Store , also Handle and raise an event -> raise/Publish the Event to Event Handle BUS , once published ->Handle the event and update the Read DB 

* event are stored immutable state and sequence in Event Store 
* Read Database is the actual User Record

Important
Whatever may be the event , delete or update its Immutable record sequence will be there in the Event Store making it Atomic in nature.
Query you option to replay the Events Store.

Crazy
The User CMD api responsible to user Query API where production user Live.

what if the user Query api fail
The Events are still maintain in the Event Bus and there once restored can handle the remaing event in sequence.

User Query API -> will subscribe the Event Bus 


Microservices 
Microservices are small, loosely couple application or service that can fail indepnedently from each other.
So , when a microservice fail only a single function or process becomes unavailable. While rest of the system remain unaffected.

Core Principle of Microservices
* Microservice **should not share code or data**.
//if not,this could lead to fail multiple application of service like **tradition service oriented architecture** that are thight cooyple with eacht other.
* Avoid **unnesscessary coupling** between service(low coupling) between software component
coupling= degree of dependency of software compoent   and cohision=measure 2 or more part work together to obtain better result than individual result.
* **Independence** and automancy is more Important than code re-usablilty.
// do not repleat your self , even if code is duplicated.
* Each Microservie is **responsible for single system** function or process.
* Microservice **should not communicate directly** with each other.
they should make use of the event/message bus to communicate with each other.

CQRS
CQRS is a software Design Patter thats stand for Command Query Responsiblity Segregation. CQRS suggests that the Application should be divided into Command and Query part. where Command is you to alter the state of an Object or Enitity and the Query is to return the Stote of an Object or Enitity.

why do we need CQRS.
this help us to scale Command and Query independently.
Advantage  where the read are more than write or Visa Versa.
So, we can optimise each for High Performance.

Event Sourcing
Event Sourcing define an approach, where all the **change** that are made to Object or Enitity, **are stored as a sequence of Immutable Events** to a event store.
as opposed to storing just the current state.
// this pattern offent used as cqpr pattern , materialing view to stored event

Axon Platform
* It consist of Axion Framework and the Axon server.
The Axon Framework is a **java framework that is used to simplify the building of Event-driven microservice that are based on CQRS**.Event-sourcing and Domain-Driven Design.
* //most important expect is the abstraction it provide.Axion is design such a ways its seperacte business login from infrastructre concerns.
* with Axion business login is implement in class that interact with different message BUS. provide a number of building block like Aggreagete, Command Handler and Event Handler.
* Axion server is **"out of the box message"** router and event store that **require no specific Configuration**.
* //Axion need to create need Message BUS or Event store.
* Axion provide you a degree of configuration
 will used Axion to use Mongo DB than Embeded Event Store.
 
 A few Important Concepts
 Domain Model
 A Domain Model describe the certain aspects of the system Domain that can be use **to solve the problems within that Domain**.
 
 Aggregate
 * Is an entity or group of entity that is **always kept in the consistent state(within a single ACID transaction)**
 * the **Aggregate Root** is the Entity within the aggreagate that is responsible to **maintaing this consistent state**.
 * This make the aggregate the **Prime building block for Implementating a command Model in any CQRS based application**.





