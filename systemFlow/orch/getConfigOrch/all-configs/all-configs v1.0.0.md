# GET /api/v1/get-config-orch/all-configs

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

    requestor ->> get-config-orch: GET /api/v1/get-config-orch/all-configs
    activate get-config-orch

    get-config-orch ->> prompt-core: GET /api/v1/prompt-core/languages
    activate prompt-core
    prompt-core ->>  get-config-orch: response
    deactivate prompt-core

    alt IF ListLanguage Error
    get-config-orch -->> requestor: response 503 internal server error
    end

    get-config-orch ->> prompt-core: GET /api/v1/prompt-core/tones
    activate prompt-core
    prompt-core ->>  get-config-orch: response
    deactivate prompt-core

    alt IF ListTones Error
    get-config-orch -->> requestor: response 503 internal server error
    end

    get-config-orch ->> prompt-core: GET /api/v1/prompt-core/features
    activate prompt-core
    prompt-core ->>  get-config-orch: response
    deactivate prompt-core

    alt IF ListFeatures Error
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

| Field Name                          | Type   | Mandatory(M/O/C) | Source                                          | Description |
| ----------------------------------- | ------ | ---------------- | ----------------------------------------------- | ----------- |
| `status`                            | int    | M                |                                                 |             |
| `data`                              | object | M                |                                                 |             |
| `data`.`languages[]`                | array  | M                |                                                 |             |
| `data`.`languages[]`.`id`           | int    | M                | [GRPC ListLangugages]().`data[]`.`id`           |             |
| `data`.`languages[]`.`languageName` | string | M                | [GRPC ListLangugages]().`data[]`.`languageName` |             |
| `data`.`tones[]`                    | array  | M                |                                                 |             |
| `data`.`tones[]`.`id`               | int    | M                | [GRPC ListTones]().`data[]`.`id`                |             |
| `data`.`tones[]`.`toneName`         | string | M                | [GRPC ListTones]().`data[]`.`toneName`          |             |
| `data`.`tones[]`.`languageId`       | int    | M                | [GRPC ListTones]().`data[]`.`languageId`        |             |
| `data`.`features[]`                 | array  | M                |                                                 |             |
| `data`.`features[]`.`id`            | int    | M                | [GRPC ListFeatures]().`data[]`.`id`             |             |
| `data`.`features[]`.`featureName`   | string | M                | [GRPC ListFeatures]().`data[]`.`featureName`    |             |

### Sample Response
```json
{
    "status" : 200,
    "data": {
        "languages":[
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
        "tones" : [
            {
                "id" : 1,
                "toneName" : "สนุกสนาน",
                "languageId": 1
            },
            {
                "id" : 2,
                "toneName" : "หรูหรา",
                "languageId": 1
            }
        ],
        "features" : [
            {
                "id" : 1,
                "featureName": "เขียนแคปชั่นขายของ"
            },
            {
                "id" : 2,
                "featureName": "เขียนบทความ"
            }
        ]
    }
}
```

## Field to Field Mapping 
### Field mapping when call [GRPC ListLangugages]()
| Target Field Name                   | Location | Tranformation | Mandatory (M/O/C) | Source Field                           | Remark |
| ----------------------------------- | -------- | ------------- | ----------------- | -------------------------------------- | ------ |
| `data`.`languages[]`.`id`           |          | Direct        | M                 | [GRPC ListLangugages]().`id`           |        |
| `data`.`languages[]`.`languageName` |          | Direct        | M                 | [GRPC ListLangugages]().`languageName` |        |


### Field mapping when call [GRPC ListTones]()
| Target Field Name | Location | Tranformation | Mandatory (M/O/C) | Source Field | Remark |
| ----------------- | -------- | ------------- | ----------------- | ------------ | ------ |
|                   |          |               |                   |              |        |

### Field mapping when call [GRPC ListFeatures]()
| Target Field Name | Location | Tranformation | Mandatory (M/O/C) | Source Field | Remark |
| ----------------- | -------- | ------------- | ----------------- | ------------ | ------ |
|                   |          |               |                   |              |        |