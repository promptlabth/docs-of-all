# POST /api/v1/user-orch/login

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
    actor requestor as requestor
    box rgb(177, 250, 187) internal
    participant user-orch
    participant user-core
    end
    box rgb(255, 196, 196) adaptor
    participant firebase 
    end 

    requestor ->> user-orch: POST /api/v1/user-orch/login

    activate user-orch
        user-orch ->> firebase: validate access token
        activate firebase
            note left of firebase: Validate a user from authorization token<br/>if found will return user id to user
            firebase ->> user-orch: response user detail
        deactivate firebase

        user-orch ->> user-orch: validate 

        user-orch ->> user-core: POST /api/v1/user-core/login
        activate user-core
            user-core ->> user-core: 




    deactivate user-orch



```