# POST /api/v1/user-core/user-registor-core

## Sequential Diagrams
```mermaid
sequenceDiagram
    actor req as requestor
    participant user_core
    participant DB_USER

    req ->> user_core: POST /api/v1/user-core/user-registor-core
    activate user_core

    user_core ->> DB_USER: get free plan from DB_USER
    activate DB_USER
    DB_USER ->> user_core: send a free plan object to user_core
    deactivate DB_USER

    user_core ->> user_core: build a object of user for send to DB User
    user_core ->> DB_USER: Insert user data to DB_USER
    activate DB_USER
    DB_USER ->> user_core: inserted user data to DB USER
    deactivate DB_USER
    
    opt error
    user_core -->> req: 401 unauthorization
    end 
    user_core ->> req: save success 200
    deactivate user_core


```

## Request
### Request Schema


| Field        | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description                        |
| ------------ | -------- | ------ | ----------------------- | ------ | ---------------------------------- |
| firebase_id  | body     | string | M                       | -      | -                                  |
| email        | body     | string | O                       | -      | -                                  |
| profile_pic  | body     | string | O                       | -      | -                                  |
| platform     | body     | string | M                       | -      | Is Possible `gmail` and `facebook` |
| access_token | body     | string | M                       | -      | -                                  |

### Simple Request

```json
{
    "firebase_id": "string",
    "email": "string",
    "profile_pic": "facebook.com/img",
    "platform": "google",
    "access_token": "hashed-value-token",
}
```


## Response
### Response Schema


| Field        | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description                        |
| ------------ | -------- | ------ | ----------------------- | ------ | ---------------------------------- |
| firebase_id  | body     | string | M                       | -      | -                                  |
| email        | body     | string | O                       | -      | -                                  |
| profile_pic  | body     | string | O                       | -      | -                                  |
| stripe_id    | body     | string | M                       | -      | -                                  |

### Simple Response

```json
{
    "firebase_id": "string",
    "email": "string",
    "profile_pic": "facebook.com/img",
    "stripe_id" : "string"
}
```