# GET user_core/v1/user-core/user-query-core

## Sequential Diagram

```mermaid
sequenceDiagram
    actor req as requestor
    participant user_core as USER_CORE
    participant db as USER_DB

    autonumber
    req->>user_core: GET user_core/v1/user-core/user-query-core
    activate user_core
    user_core->>db: get name,email, profile_pic, platform, access_token,stripe_id, plan_id by firebase id
    activate db
    db-->>user_core: return name,email, profile_pic, platform, access_token,stripe_id, plan_id
    deactivate db
    opt if error data not found
        user_core -->> req: 404 Not Found
    end
    user_core->>req: send result
    deactivate user_core
```

## Request Body

### Request Schema

| Field       | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ----------- | -------- | ------ | ----------------------- | ------ | ----------- |
| firebase_id | body     | string | M                       | -      | -           |

### Sample Request

```json
{
    "firebase_id": "string"
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
