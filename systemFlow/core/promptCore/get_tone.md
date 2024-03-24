# GET api/v1/promptCore/getConfigCore/tones/:languageId

## Sequential Diagram
```mermaid
sequenceDiagram
    actor req as requestor
    participant getConfig
    participant prompt as DB_PROMPT

    req ->> getConfig: GET /v1/getConfigCore/tones/:languageId
    activate getConfig
    opt Language Id is not found
        getConfig -->> req: 404 Language Id is required
    end
    getConfig ->> prompt: query by languageId
    activate prompt
    prompt ->> getConfig: query result all tones by languageId
    deactivate prompt
    getConfig ->> req: send all tones by languageId
    deactivate getConfig
```
## Request Body
### Request Schema

 | Field      | location | Type | Mandatory(Man/Opt/Cond) | Target | Description |
 | ---------- | -------- | ---- | ----------------------- | ------ | ----------- |
 | languageId | params   | int  | M                       | -      | -           |

### Sample Request
```
{
    "languageId": 1
}
```
### Response
```
[
    {
        "id": 1,
        "name": "สนุกสนาน",
        "languageId": 1,
    },
    {
        "id": 2,
        "name": "มืออาชีพ",
        "languageId": 1,
    }
]
```