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

more formal defination as Aric Evence the author of the book **'Domain driven design - techtic complexitty in the heart of Software'**
Aggregate is the cluster of associate object that are trade as a unit for the purpose of Data change ,external reference are restricted to one member of the aggreagate designated as the root.A set of consitency rule within the aggregate boundry.

Commands
A Commands is a **comibinatoin of expressed intent**(which describe what you want to be done)as well as the information required to undertaken action bases on that intent.
name as Verb in the **Imperative mood**. eg RegiseterUserCommand or DepositFundsCommands

Events.
Events are Objet that describe that something has Occured.
A typical source of events is the aggreagate. **when something IMP has occured within the aggreaget , it will raise an event.**
name with past participle verb eg UserRegisterdEvent or FundDepostedEvents.

Query 
express **desire for inforamation**. generally a specific representation of the state of the system.


Software Preqreuisite
* java , min 8 > JDK 16 - jdk.java.net/16 till date
* Maven
* IDE Eclispe
* Postman
* DOcker - installed as exe. from official site
* Axon Platform
* Mongo DB - docker Image 
* My SQL - docker Image

Docker Command as follow: on window 

docker ps
//error during connect: This error may indicate that the docker daemon is not running.: Get "http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.24/containers/json": open //./pipe/docker_engine: The system cannot find the file specified. 

https://www.coretechnologies.com/products/AlwaysUp/Install.html - no helped

run docker desktop in window as applicatoin getting as
Hardware assisted virtualization and data execution protection must be enabled in the BIOS. 
See https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization

try this one 
https://stackoverflow.com/questions/39684974/docker-for-windows-error-hardware-assisted-virtualization-and-data-execution-p

if not check virutilazation is up in Task Manager or system info Hypervisor enable, if not do it from BIOS and enable it

Now,Docker is running

Steps
1. Docker Network : so that our container will be able to commmunicate with on another
> docker network create --attachable -d overlay springbankNet
attachable = allow to manual attach contain to this network 
-d = specify network driver i.e overlay to span multiple docker host  since we are working with single docker host we can also change to bridge if we want.

not working do did 
> docker swarm init

reRun step 1 cmd
> docker network ls

2. need to install Axon Platfrom can do both way manual install or using docker

docker run -d --name axon-server -p 8024:8024 -p 8124:8124 \ --network springbankNet \ --restart always axoniq/axonserver:latest
// local means local to contain not localhost manchine
// restart always means restart whenever need to avoid failer later , it will restart again and again when changes made
// p means port external:internal port

3.verify in local browser  localhost:8024

4. Install Mongo DB
docker run -it -d --name mongo-container -p 27017:27017 --network springbankNet --restart always -v mongodb_data_container:/data/db mongo:latest
//- v save or persist data nd share data we need volume
the are just dir outside of file system exist as host file , so else will lose the data every thin we recreate this container

//I just spelled latest wrong and got no image found error
// I also instlled Robot 3t
check localhost:27017 work find test connection and saved it by name 'Monog in docker'

5.To install MySql
Run in Docker as
docker run -it -d --name mysql-container -p 3306:3306 --network springbankNet  -e MY_SQL_ROOT_PASSWORD=springbankRootPsw --restart always -v mysql_data_container:/var/lib/mysql mysql:latest

6. installl admire client tool
docker run -it -d --name adminer -p 8080:8080 --network springbankNet -e ADMIR_DEFAULT_SERVER=mysql-container --restart always adminer:latest







 












