# PromptHistroy DB

```mermaid
erDiagram
    PROMPT_HISTORIES{
        string id PK "UUID"
        datetime generate_date_time "'NN' convert to unix before use"
        string input_message "'NN'is a input of the message"
        string result_message "'NN', is a result of the message"
        string firebase_id "'NN', is a firebase id from DB_USER.users"
        string prompt_id "'NN', is relate to DB_PROMPT.PROMPT_CONFIGS"
        string tone "'NN', is relate to tone of prompt"
    }


```