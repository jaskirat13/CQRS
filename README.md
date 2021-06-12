# CQRS
CQRS architecture with docker and microservice in java 

CQRS - Structure Flow
client request -> API gateway-> and Route to user CMD API (modify/create/Date  change of state event)->reister on Event Store , also Handle and raise an event -> raise/Publish the Event to Event Handle BUS , once published ->Handle the event and update the Read DB 

1.event are stored immutable state and sequence in Event Store 
2. Read Database is the actual User Record

Important
Whatever may be the event , delete or update its Immutable record sequence will be there in the Event Store making it Atomic in nature.
Query you option to replay the Events Store.

Crazy
The User CMD api responsible to user Query API where production user Live.

what if the user Query api fail
The Events are still maintain in the Event Bus and there once restored can handle the remaing event in sequence.

User Query API -> will subscribe the Event Bus 
