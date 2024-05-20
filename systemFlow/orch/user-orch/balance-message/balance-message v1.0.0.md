# GET /api/v1/user-orch/balance-message

 | Name                     | Detail                                                              |
 | ------------------------ | ------------------------------------------------------------------- |
 | **Overview**             | inquiry a account of user when `landing` and `login` to promptlabai |
 | **Layer**                | `ORCH`                                                              |
 | **Microservice**         | `user-orch`                                                         |
 | **Related to Service**   | `user-core`                                                         |
 | **Authentication Level** | `user level`                                                        |

### Change Log
| Date       | Update By                              | Description     |
| ---------- | -------------------------------------- | --------------- |
| 2024-05-18 | [@tanachod](https://github.com/Pet002) | initial Project |

```mermaid 
%%{
    init: {
    'sequence': {
            'mirrorActors': false, 
            'rightAngles': true, 
            'messageAlign': 'center', 
            'actorFontSize': 20, 
            'actorFontWeight': 900, 
            'noteFontSize': 18, 
            'noteFontWeight': 600, 
            'messageFontSize': 14
        }
    }
}%%

%%{
    init: {
        'theme': 'base', 
        'themeVariables': {
            'labelBoxBkgColor': 'lightgrey',
            'labelBoxBorderColor': '#000000', 
            'actorBorder': '#000000', 
            'actorBkg': '#ffffff', 
            'activationBorderColor': '#232F3E', 
            'activationBkgColor': '#d9d9d9', 
            'noteBkgColor': 'rgba(255, 153, 0, .25)', 
            'noteBorderColor': '#232F3E'
        }
    }
}%%

sequenceDiagram
    actor requestor
    box rgb(177,250,187) internal
    participant user-orch
    participant user-core
    end

    box rgb(255, 196, 196) adaptor
    participant firebase 
    end 

    requestor ->> user-orch: GET /api/v1/user-orch/balance-message
    activate user-orch
        user-orch ->> firebase: validate access token
            activate firebase
                note left of firebase: Validate a user from authorization token<br/>if found will return user id to user
                firebase ->> user-orch: response user detail
            deactivate firebase
        user-orch ->> user-orch: extract firebase id

        user-orch ->> user-core: GET /api/v1/user-core/inquiry-balance-message/:firebase_id
        activate user-core
            note left of user-core: inquiry a balance message of user
            user-core ->> user-orch: response balance message of user 
        deactivate user-core

        alt IF Request Error
        user-orch -->> requestor: response 503 internal server error
        end

        user-orch ->> user-orch: build a response (detail at below)
        user-orch ->> requestor: response to requestor
    deactivate user-orch

```


## Request 
#### Header
| Field Name      | Location | Type   | Mandatory(M/O/C) | Source | Description |
| --------------- | -------- | ------ | ---------------- | ------ | ----------- |
| `Authorization` | HEADER   | string | M                |        |             |

### Request Schema
| Field Name | Location | Type | Mandatory(M/O/C) | Source | Description |
| ---------- | -------- | ---- | ---------------- | ------ | ----------- |
|            |          |      |                  |        |             |


### Sample Request
``` json
{

}
```

## Response
### Response Schema

| Field Name              | Type   | Mandatory(M/O/C) | Target | Description |
| ----------------------- | ------ | ---------------- | ------ | ----------- |
| `status`                | int    | M                |        |             |
| `code`                  | int    | M                |        |             |
| `data`                  | object | O                |        |             |
| `data`.`firebaseId`     | string | M                |        |             |
| `data`.`balanceMessage` | int    | M                |        |             |

### Sample Response
```json
{
    "status": 200,
    "code": 2000,
    "data": {
        "firebaseId": "if9012fds0asd",
        "balanceMessage": 999999999
    }
}
```

## Field to Field Mapping 

### Field mapping when call [firebase validate]()
| Target Field Name | Location | Tranformation | Mandatory (M/O/C) | Source                   | Remark       |
| ----------------- | -------- | ------------- | ----------------- | ------------------------ | ------------ |
| `Authorization`   |          | Direct        | M                 | `header`.`authorization` | Access token |

### Field mapping when call [GET /api/v1/prompt-core/balance-message/:firebase_id]()
| Target Field Name       | Location | Tranformation | Mandatory (M/O/C) | Source                                                                             | Remark |
| ----------------------- | -------- | ------------- | ----------------- | ---------------------------------------------------------------------------------- | ------ |
| `data`.`firebaseId`     |          | Direct        | M                 | [[GET /api/v1/prompt-core/balance-message/:firebase_id]()].`data`.`firebaseId`     |        |
| `data`.`balanceMessage` |          | Direct        | M                 | [[GET /api/v1/prompt-core/balance-message/:firebase_id]()].`data`.`balanceMessage` |        |