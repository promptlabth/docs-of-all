# GET api/v1/getConfigOrch/tones

## Sequential Diagram
```mermaid
sequenceDiagram
    actor req as requestor
    participant getConfig
    participant prompt as DB_PROMPT

    req ->> getConfig: GET /v1/getConfigOrch/tones/:languageId
    opt Language Id is not found
        getConfig -->> req: 404 Language Id is required
    end
    activate getConfig
    getConfig ->> prompt: query by languageId
    prompt ->> getConfig: query result all tones by languageId
    getConfig ->> req: send all tones by languageId
    deactivate getConfig
```
## Request Body
### Request Schema

 Field         | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ------------- | -------- | ------ | ----------------------- | ------ | ----------- |
| languageId    | params     | int    | M                       | -      | -           |

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

----
# GET api/v1/getConfigOrch/languages
## Sequential Diagram

```mermaid
sequenceDiagram
    actor req as requestor
    participant getConfig
    participant prompt as DB_PROMPT

    req ->> getConfig: GET /v1/getConfigOrch/langugaes
    activate getConfig
    getConfig ->> prompt: query all languages
    prompt ->> getConfig: query result all languages
    getConfig ->> req: send all languages
    deactivate getConfig
```
## Request Body
None

### Response
```
[
    {
        "id": 1
        "name": "th"
    },
    {
        "id": 2
        "name": "en"
    },
    {
        "id": 3
        "name": "id"
    },
]
```
