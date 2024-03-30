# POST api/v1/historyConsumer/saveMessaage

## Sequential Diagram
```mermaid
sequenceDiagram
    participant topic as HISTORY_CREATE_TOPIC
    participant consumer as History consumer
    participant prompt as DB_PROMPT
    autonumber
    consumer ->> topic : Consume generated prompt data
    activate consumer
    consumer ->> consumer : Unmarshal to get generated prompt data
    consumer ->> prompt : Insert prompt message data
    activate prompt
    prompt -->> consumer : Recevied insert completed message
    opt if error
        consumer -->> consumer : log_error to gcp
    end
    consumer -->> consumer: mark message and move to next partitions
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