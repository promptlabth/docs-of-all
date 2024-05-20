# GET /api/v1/user-core/inquiry-balance-message/:firebase_id

| Name                     | Detail                                                              |
| ------------------------ | ------------------------------------------------------------------- |
| **Overview**             | inquiry a account of user when `landing` and `login` to promptlabai |
| **Layer**                | `Core`                                                              |
| **Microservice**         | `user-core`                                                         |
| **Related to Service**   | `DB_USER`                                                           |
| **Authentication Level** | `None`                                                              |

### Change Log
| Date       | Update By                              | Description     |
| ---------- | -------------------------------------- | --------------- |
| 2024-05-20 | [@tanachod](https://github.com/Pet002) | initial Project |

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
    participant user-core
    participant db_user as DB_USER
    end

    requestor ->> user-core: GET /api/v1/user-core/inquiry-balance-message/:firebase_id
    activate user-core
        user-core ->> user-core: get value firebase id from params
        user-core ->> db_user: inquiry balance message from table balance_messages
        activate db_user
            note left of db_user: SELECT * FROM balance_messages
            db_user ->> user-core: response a balance message
        deactivate db_user

        alt IF NOT FOUND data in Database
            user-core ->> db_user: insert data to table
            activate db_user
            note left of db_user: INSERT INTO balance_messages(firebase_id, balance_message)<br/> VALUES('{:firebase_id}', 0)
                db_user ->> user-core: response value back to variable
            deactivate db_user

        else OTHER ERROR FROM DB_USER
            user-core -->> requestor : response 200 with code error
        end
        

        user-core ->> user-core: mapping value by field to field mapping (at below)
        user-core ->> requestor: response a value to requestor
    deactivate user-core

```

## Request
### Header 
| Field Name     | Location | Type   | Mandatory (M/O/C) | Source | Description        |
| -------------- | -------- | ------ | ----------------- | ------ | ------------------ |
| `x-request-id` | HEADER   | string | M                 |        | generate from orch |

### Request Schema
| Field Name    | Location | Type   | Mandatory (M/O/C) | Source | Description |
| ------------- | -------- | ------ | ----------------- | ------ | ----------- |
| `firebase_id` | Param    | string | M                 |        |             |

### Sample Request
```json
`None`
```

## Response
### Response Schema
| Field Name              | type   | Mandatory (M/O/C) | target | Description       |
| ----------------------- | ------ | ----------------- | ------ | ----------------- |
| `status`                | int    | M                 |        |                   |
| `code`                  | int    | M                 |        |                   |
| `data`                  | object | O                 |        |                   |
| `data`.`firebaseId`     | string | M                 |        |                   |
| `data`.`balanceMessage` | int    | M                 |        |                   |
| `message`               | string | O                 |        | message for error |

### Sample Response 
```json
{
    "status": 200,
    "code": 2000,
    "data": {
        "firebaseId" : "if9012fds0asd",
        "balanceMessage": 999999999
    }
}
```
## Field to Field Mapping 

### Field mapping when Inquiry and Insert [DB_USER TABLE balance_messages]()
| Target Field Name       | Location | Tranformation | Mandatory | Source                                               | Remark |
| ----------------------- | -------- | ------------- | --------- | ---------------------------------------------------- | ------ |
| `data`.`firebaseId`     |          | Direct        | M         | [DB_USER TABLE balance_messages]().`firebase_id`     |        |
| `data`.`balanceMessage` |          | Direct        | M         | [DB_USER TABLE balance_messages]().`balance_message` |        |