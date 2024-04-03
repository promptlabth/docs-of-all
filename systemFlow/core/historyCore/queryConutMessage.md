# GET api/v1/historyCore/getCountMessage/:firebaseId/:startDate

## Sequential Diagram
```mermaid
sequenceDiagram
    actor req as requestor
    participant countService as getCountMessage
    participant hist as DB_HISTORY

    req ->> countService: GET api/v1/historyCore/getCountMessage/:firebaseId/:startDate

    countService ->> countService: make a struct <br/> {firebaseID, start_plan_date(conv unix to datetime), now} <br/>free plan will start from first month

    countService ->> hist: query a history
    note right of countService: SELECT<br/>	COUNT(*) AS count_message<br/>FROM<br/>	prompt_histories<br/>WHERE<br/>	prompt_histories.firebase_id = firebaseId<br/>	AND prompt_histories.created_date BETWEEN startDate<br/>	AND CURRENT_TIMESTAMP;

    hist ->> countService : response count history 

    opt if error
    countService -->> req: response 501 error
    end

     countService ->> req: response count and status 200 ok

```



## Request Body

### Request Schema

| Field      | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ---------- | -------- | ------ | ----------------------- | ------ | ----------- |
| firebaseId | param    | string | M                       | -      | -           |
| startDate  | param    | int64  | M                       | -      | -           |

### Sample Request

```json
{
  "firebaseId": "string",
  "startDate": 1712162101,
}
```

## Response Body

### Response Schema

| Field        | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ------------ | -------- | ------ | ----------------------- | ------ | ----------- |
| messageCount | body     | int | M                       | -      | -           |

### Sample Response

```json
{
  "messageCount": 59
}
```

