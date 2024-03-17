# POST api/v1/prompt-generate


## Sequential Diagram
```mermaid
sequenceDiagram
    actor req as requestor
    participant gen as generateMessage
    participant firebase as FIREBASE_SERVICE
    participant valid as USER
    participant prompt as DB_PROMPT
    participant GPT
    participant GEMINI
    participant kafka as TP_save_history

    req ->> gen: POST /v1/prompt-generate
    activate gen
    gen ->> firebase: send token to check vaidate
    activate firebase
    firebase ->> gen : response firebase id and other
    deactivate firebase
    gen ->> valid: GetUser
    activate valid
    Note right of valid: get authorize and find firebase_id and stript_id
    valid ->> gen : response firebase_id and stripe_id if found
    deactivate valid
    opt failed validate user
        gen -->> req: 401 unauthorization
    end

    gen ->> prompt: GET /v1/prompt/models
    activate prompt
        prompt ->> gen: response all model name
    deactivate prompt

    gen ->> gen: random model from model response

    gen ->> prompt: POST /v1/prompt/adjust (send model, feature, language, tone, input_message)
    activate prompt
        Note right of prompt: will get adjust prompt to ready send to model
        prompt ->> gen: response prompt input to promptMessage API
    deactivate prompt
    alt is GPT  
        gen ->> GPT: send adjust prompt to GPT to generate
        activate GPT
        GPT ->> gen: recive a result from send
        deactivate GPT
    else is GEMINI
        gen ->> GEMINI: send adjust prompt to GEMINI to generate
        activate GEMINI
        GEMINI ->> gen: recive a result from send
        deactivate GEMINI
    end 

    gen ->> gen: adjust to struct for save prompt
    gen ->> kafka : publish data to kafka (async send only)

    gen ->> req: send result to user
    deactivate gen

    


```


## Request Body
### Request Schema

| Field         | location | Type   | Mandatory(Man/Opt/Cond) | Target | Description |
| ------------- | -------- | ------ | ----------------------- | ------ | ----------- |
| Authorization | header   | string | M                       | -      | -           |
| inputMessage  | body     | string | M                       | -      | -           |
| featureID     | body     | int    | M                       | -      | -           |
| languageID    | body     | int    | M                       | -      | -           |
| toneID        | body     | int    | M                       | -      | -           |

### Sample Request

```
{
    "inputMessage": "string",
    "featureID": 1,
    "languageID": 1,
    "toneID" : 1
}
```

