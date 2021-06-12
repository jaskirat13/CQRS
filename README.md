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
* Avoid **unnesscessary coupling** between service(low coupling) and software component
coupling= degree of depends of software compoent   and cohision=measure 2 or more part work together to obtain better result than individual result.
* **Independence** and automancy is more Important than code re-usablilty.
// do not repleat your self , even if code is duplicated.
* Each Microservie is **responsible for single system** function or process.
* Microservice **should not communicate directly** with each other.
they should make use of the event/message bus to communicate with each other.
