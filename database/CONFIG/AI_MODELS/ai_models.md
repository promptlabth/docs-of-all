# TABLE ai_models
---
```mermaid
erDiagram
    AI_MODELS{
        int id PK
        string model_name
    }
```

---

## Table Schema

| Column Name  | type    | Length | Constraints | Nullable | Remark           |
| ------------ | ------- | ------ | ----------- | -------- | ---------------- |
| `id`         | INT     |        | PRIMARY KEY | N        | `AUTO INCREMENT` |
| `model_name` | varchar | 20     |             | N        |                  |

## Simple Value

| Column Name  | Simple |
| ------------ | ------ |
| `id`         | 1      |
| `model_name` | GPT    |