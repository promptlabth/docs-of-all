# GET /api/v1/prompt-core/configs

| Name                     | Detail                                                  |
| ------------------------ | ------------------------------------------------------- |
| **Overview**             | inquiry all configuration when `landing` to promptlabai |
| **Layer**                | `orch`                                                  |
| **Microservice**         | `prompt-core`                                           |
| **Related to Service**   | `DB_CONFIGS`, `get-config-orch`                         |
| **Authentication Level** | `None`                                                  |



### Change Log
| Date       | Update By                              | Description     |
| ---------- | -------------------------------------- | --------------- |
| 2024-05-04 | [@tanachod](https://github.com/Pet002) | initial Project |

``` mermaid
sequenceDiagram
    actor requestor as requestor
    participant prompt-core as prompt-core
    participant DB_CONFIGS as DB_CONFIGS

    requestor ->> prompt-core: GET /api/v1/prompt-core/configs
    activate prompt-core

    prompt-core ->> DB_CONFIGS: Inquiry features
    activate DB_CONFIGS
    note left of DB_CONFIGS: SELECT * FROM features
    DB_CONFIGS ->>  prompt-core: response features
    deactivate DB_CONFIGS

    alt IF Request Error
    prompt-core -->> requestor: response 200 Not Found Data
    end

    prompt-core ->> prompt-core: mapping inquiry features


    prompt-core ->> DB_CONFIGS: Inquiry languages
    activate DB_CONFIGS
    note left of DB_CONFIGS: SELECT * FROM languages
    DB_CONFIGS ->>  prompt-core: response languages
    deactivate DB_CONFIGS

    alt IF Request Error
    prompt-core -->> requestor: response 200 Not Found Data 
    end

    prompt-core ->> prompt-core: mapping inquiry languages


    prompt-core ->> DB_CONFIGS: Inquiry tones
    activate DB_CONFIGS
    note left of DB_CONFIGS: SELECT * FROM tones
    DB_CONFIGS ->>  prompt-core: response tones
    deactivate DB_CONFIGS

    alt IF Request Error
    prompt-core -->> requestor: response 200 Not Found Data 
    end

    prompt-core ->> prompt-core: mapping inquiry tones

    prompt-core ->> prompt-core : setup schema for response to frontend
    note left of prompt-core : follow schema in below


    prompt-core ->> requestor: response to requestor follow response schema on below
    deactivate prompt-core
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
### Field mapping when call [GRPC ListLangugages]()
| Target Field Name                   | Location | Tranformation | Mandatory (M/O/C) | Target                                                                                                                      | Remark |
| ----------------------------------- | -------- | ------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------- | ------ |
| `data`.`languages[]`.`id`           |          | Direct        | M                 | [DB_CONFIGS Table languages](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/LANGUAGES).`id`           |        |
| `data`.`languages[]`.`languageName` |          | Direct        | M                 | [DB_CONFIGS Table languages](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/LANGUAGES).`languageName` |        |
| `data`.`tones[]`.`id`               |          | Direct        | M                 | [DB_CONFIGS Table tones](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/TONES).`id`                   |        |
| `data`.`tones[]`.`name`             |          | Direct        | M                 | [DB_CONFIGS Table tones](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/TONES).`name`                 |        |
| `data`.`tones[]`.`languageId`       |          | Direct        | M                 | [DB_CONFIGS Table tones](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/TONES).`languageId`           |        |
| `data`.`features[]`.`id`            |          | Direct        | M                 | [DB_CONFIGS Table features](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/FEATURES).`id`             |        |
| `data`.`features[]`.`name`          |          | Direct        | M                 | [DB_CONFIGS Table features](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/FEATURES).`name`           |        |
| `data`.`features[]`.`path`          |          | Direct        | M                 | [DB_CONFIGS Table features](https://github.com/promptlabth/docs-of-all/tree/main/database/CONFIG/FEATURES).`path`           |        |