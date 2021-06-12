# CQRS
CQRS architecture with docker and microservice in java 

CQRS - Structure Flow
client request -> API gateway-> and Route to user CMD API (modify/create/Date  change of state event)-> Hanlde and raise an event , also reister on Event Store -> raise/Publish the Event to Event Handle BUS , once published ->Handle the event and update the Read DB 

***event are stored immutable state and sequence in Event Store 
*** Read Database is the actual User Record

Important
Whatever may be the event , delete of update its Immutable record will be there in the Event Store making it Atomic in nature.
Query you option to replay the Events Store.

Crazy
the User CMD api responsible to user Query API where production user Live.

what if the user Query api fail
The Events are still maintain in the Event Bus and there once restored can handle the remaing event in sequence.

User Query API -> will subscribe the Event Bus 
