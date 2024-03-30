# GET api/v1/promptCore/getConfigCore/languages

## Sequential Diagram

```mermaid
sequenceDiagram
    actor req as requestor
    participant getConfig
    participant prompt as DB_PROMPT

    req ->> getConfig: GET /v1/getConfigCore/langugaes
    activate getConfig
    getConfig ->> prompt: query all languages
    prompt ->> getConfig: query result all languages
    getConfig ->> req: send all languages
    deactivate getConfig
```
## Request Body
`None`

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
