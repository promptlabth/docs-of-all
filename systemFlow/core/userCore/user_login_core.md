# POST /api/v1/user-core/user-login-core

## Sequential Diagrams

```mermaid
sequenceDiagram

    actor req as requestor
    participant user_core as USER_CORE
    participant db as DB_USER

    autonumber

    req ->> user_core: POST /api/v1/user-core/user-login-core
    activate user_core

    user_core->>db: query user data by firebase_id
    activate db
    db-->>user_core: user data
    deactivate db

    opt if error data not found
        user_core -->> req: 401 unauthorization
    end

    user_core->>user_core: check if have any data that update, set datetime_last_active

    user_core ->> db: update user data
    activate db
    db-->>user_core: update user data
    opt if error
        user_core -->> req: 500 internal server error
    end

    user_core->>req: success 200
    deactivate user_core




```

## Request Body

### Request Schema

| Field        | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ------------ | -------- | ------ | ----------------------- | ------ | ----------- |
| firebase_id  | body     | string | M                       | -      | -           |
| name         | body     | string | M                       | -      | -           |
| email        | body     | string | M                       | -      | -           |
| profile_pic  | body     | string | M                       | -      | -           |
| platform     | body     | string | M                       | -      | -           |
| access_token | body     | string | M                       | -      | -           |
| stripe_id    | body     | string | M                       | -      | -           |
| plan_id      | body     | string | M                       | -      | -           |

### Sample Request

```json
{
  "firebase_id": "string",
  "name":"sirin kub",
  "email":"mamoong.namplawan@gmil.com",
  "profile_pic":"something",
  "platform":"facebook",
  "access_token":"a3s4d5f6g7hjkp[6y7uixrctvybuji",
  "stripe_id":"crtvybuimotgyhujiyujik",
  "plan_id":"1"
}
```

## Response Body

### Response Schema

| Field        | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ------------ | -------- | ------ | ----------------------- | ------ | ----------- |
| firebase_id  | body     | string | M                       | -      | -           |
| name         | body     | string | M                       | -      | -           |
| email        | body     | string | M                       | -      | -           |
| profile_pic  | body     | string | M                       | -      | -           |
| platform     | body     | string | M                       | -      | -           |
| access_token | body     | string | M                       | -      | -           |
| stripe_id    | body     | string | M                       | -      | -           |
| plan_id      | body     | string | M                       | -      | -           |

### Sample Response

```json
{
  "firebase_id": "string",
  "name":"sirin kub",
  "email":"mamoong.namplawan@gmil.com",
  "profile_pic":"something",
  "platform":"facebook",
  "access_token":"a3s4d5f6g7hjkp[6y7uixrctvybuji",
  "stripe_id":"crtvybuimotgyhujiyujik",
  "plan_id":"1"
}
```
