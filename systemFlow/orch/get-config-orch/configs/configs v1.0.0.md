# GET /api/v1/get-config-orch/configs

| Name                     | Detail                                                  |
| ------------------------ | ------------------------------------------------------- |
| **Overview**             | inquiry all configuration when `landing` to promptlabai |
| **Layer**                | `ORCH`                                                  |
| **Microservice**         | `get-config-orch`                                       |
| **Related to Service**   | `prompt-core`                                           |
| **Authentication Level** | `None`                                                  |



### Change Log
| Date       | Update By                              | Description     |
| ---------- | -------------------------------------- | --------------- |
| 2024-05-04 | [@tanachod](https://github.com/Pet002) | initial Project |

``` mermaid
sequenceDiagram
    actor requestor as requestor
    participant get-config-orch as get-config-orch
    participant prompt-core as prompt-core

    requestor ->> get-config-orch: GET /api/v1/get-config-orch/configs
    activate get-config-orch

    get-config-orch ->> prompt-core: GET /api/v1/prompt-core/configs
    activate prompt-core
    prompt-core ->>  get-config-orch: response
    deactivate prompt-core

    alt IF Request Error
    get-config-orch -->> requestor: response 503 internal server error
    end

    get-config-orch ->> get-config-orch : setup schema for response to frontend
    note left of get-config-orch : follow schema in below


    get-config-orch ->> requestor: response to requestor follow response schema on below
    deactivate get-config-orch
```

---

## Request
### Request Schema

| Field Name | Location | Type | Mandatory(M/O/C) | Source | Description |
| ---------- | -------- | ---- | ---------------- | ------ | ----------- |
| `None`     |          |      |                  |        |             |


### Sample Request 
``` json
{

}
```

## Response
### Response Schema

| Field Name                          | Type   | Mandatory(M/O/C) | Target | Description |
| ----------------------------------- | ------ | ---------------- | ------ | ----------- |
| `status`                            | int    | M                |        |             |
| `code`                              | int    | M                |        |             |
| `data`                              | object | M                |        |             |
| `data`.`languages[]`                | array  | M                |        |             |
| `data`.`languages[]`.`id`           | int    | M                |        |             |
| `data`.`languages[]`.`languageName` | string | M                |        |             |
| `data`.`tones[]`                    | array  | M                |        |             |
| `data`.`tones[]`.`id`               | int    | M                |        |             |
| `data`.`tones[]`.`name`             | string | M                |        |             |
| `data`.`tones[]`.`languageId`       | int    | M                |        |             |
| `data`.`features[]`                 | array  | M                |        |             |
| `data`.`features[]`.`id`            | int    | M                |        |             |
| `data`.`features[]`.`name`          | string | M                |        |             |
| `data`.`features[]`.`path`          | string | M                |        |             |

### Sample Response
```json
{
    "status": 200,
    "code": 2000,
    "data": {
        "languages": [
            {
                "id": 1,
                "languageName": "th"
            },
            {
                "id": 2,
                "languageName": "eng"
            },
            {
                "id": 3,
                "languageName": "id"
            }
        ], 
        "tones": [
            {
                "id" : 1,
                "name" : "สนุกสนาน",
                "languageId": 1
            },
            {
                "id" : 2,
                "toneName" : "หรูหรา",
                "languageId": 1
            }
        ],
        "features": [
            {
                "id" : 1,
                "name": "เขียนแคปชั่นขายของ",
                "path": "/createSellingPost"
            },
        ]
    }
}
```

## Field to Field Mapping 
### Field mapping when call [GET /api/v1/prompt-core/configs]()
| Target Field Name                   | Location | Tranformation | Mandatory (M/O/C) | Target                                             | Remark |
| ----------------------------------- | -------- | ------------- | ----------------- | -------------------------------------------------- | ------ |
| `data`.`languages[]`.`id`           |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`id`           |        |
| `data`.`languages[]`.`languageName` |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`languageName` |        |
| `data`.`tones[]`.`id`               |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`id`           |        |
| `data`.`tones[]`.`name`             |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`name`         |        |
| `data`.`tones[]`.`languageId`       |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`languageId`   |        |
| `data`.`features[]`.`id`            |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`id`           |        |
| `data`.`features[]`.`name`          |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`name`         |        |
| `data`.`features[]`.`path`          |          | Direct        | M                 | [GET /api/v1/prompt-core/configs]().`data`.`path`         |        |