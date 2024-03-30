# POST api/v1/historyConsumer/saveMessaage

## Sequential Diagram
```mermaid
sequenceDiagram
    participant consumer as History consumer
    participant prompt as DB_PROMPT
    participant topic as HISTORY_CREATE_TOPIC
    autonumber
    consumer ->> topic : Consume generated prompt data
    activate topic
    topic -->> consumer : Send generated prompt data
    deactivate topic
    consumer ->> prompt : Insert prompt message data
    activate prompt
    activate consumer
    prompt ->> consumer : Insert completed
    deactivate prompt
    deactivate consumer

```

## Request
### Request Schema
Data consume from Kafka topic `HISTORY_CREATE_TOPIC`

| Field            | location  | Type      | Mandatory(Man/Opt/Cond)   | Target    | Description                       |
| ------------     | --------  | ------    | -----------------------   | ------    | --------------------------------- |
| inputMessage     | body      | string    | -                         | -         | -                                 |
| resultMessage    | body      | string    | -                         | -         | -                                 |
| userId           | body      | int        | -                         | -         | -                                 |
| toneId           | body      | int       | -                         | -         | -                                 |
| featureId        | body      | int       | -                         | -         | -                                 |
| modelId          | body      | int       | -                         | -         | -                                 |

### Sample Request
```json
{
    "inputMessage": "Iphone11",
    "resultMessage": "Iphone 11 is the best iphone in the world #iphone11 #bestiphone",
    "userId" : 1,
    "toneId" : 1,
    "featureId" : 1,
    "modelId" : 1
}
```