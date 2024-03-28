# user update plan consumer

Consume USER_PLAN_UPDATE_TOPIC

```mermaid
sequenceDiagram
    participant usr_plan_topic as USER_PLAN_UPDATE_TOPIC
    participant usr_plan_consumer as USER_UPDATE_PLAN_CONSUMER
    participant db_user as DB_USER

    autonumber

    usr_plan_consumer ->> usr_plan_topic : Consumer User Plan Update Topic
    activate usr_plan_consumer
    usr_plan_consumer ->> usr_plan_consumer: unmarshal to get product_id

    usr_plan_consumer ->> db_user: query user by customer_id from DB USER
    activate db_user
    db_user ->> usr_plan_consumer : recive user data
    deactivate db_user
    
    opt if error
        usr_plan_consumer -->> usr_plan_consumer : log_error to gcp
    end

    usr_plan_consumer ->> db_user: query plan by product_id from DB USER
    activate db_user
    db_user ->> usr_plan_consumer : recive plan object
    deactivate db_user
    
    opt if error
        usr_plan_consumer -->> usr_plan_consumer : log_error to gcp
    end
    
    
    usr_plan_consumer ->> usr_plan_consumer: recreate structure of usr_plan
    usr_plan_consumer ->> db_user: update plan of user by user_id 
    activate db_user
    db_user ->> usr_plan_consumer : plan update confirmed
    deactivate db_user
    
    opt if error
        usr_plan_consumer -->> usr_plan_consumer : log_error to gcp
    end

    usr_plan_consumer -->> usr_plan_consumer : mark message and move to next partitions
    deactivate usr_plan_consumer

    
```


## Request
### Request Schema

| Field       | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ----------- | -------- | ------ | ----------------------- | ------ | ----------- |
| product_id  | body     | string | M                       | -      | -           |
| customer_id | body     | string | M                       | -      | -           |


### Simple Request 

```json
{
    "product_id": "prod_P9mGurf84Qms1K",
    "customer_id":"cus_***********" 
}
```
