# GET api/v1/user/query

## Sequential Diagram

```mermaid
sequenceDiagram
    actor req as requestor
    participant api as USER_QUERY_API
    participant db as USER_DB

    req->>api: GET api/v1/user/query
    activate api
    api->>db: get name, profile_pic, stripe_id, plan_type, product_id by firebase id
    activate db
    db-->>api: return name, email, profile_pic, stripe_id, plan_type, product_id
    deactivate db
    opt if error data not found
        api -->> req: 404 Not Found
    end
    api->>req: send result
    deactivate api
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
