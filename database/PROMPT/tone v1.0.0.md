# TABLE tone v1.0.0

---

```mermaid
erDiagram
    MODELS{
        int id "auto increment"
        string tone_name "name of tone"
        int language_id "language id of this tone"
    }
```

## Table Schema

| Column name   | type    | Length | Constraints | Nullable | Remark              |
| ------------- | ------- | ------ | ----------- | -------- | ------------------- |
| `id`          | INT     |        | PRIMARY KEY |          | AUTO_INCREMENT      |
| `tone_name`   | VARCHAR | 16     |             | N        |                     |
| `language_id` | INT     |        | FOREING KEY | N        | Language ID of tone |

## Simple Value

| Column Name   | Simple    |
| ------------- | --------- |
| `id`          | 1         |
| `tone_name`   | Confident |
| `language_id` | 3         |
