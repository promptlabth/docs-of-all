# TABLE feature v1.0.0

---

```mermaid
erDiagram
    FEATURES{
        int id "auto increment"
        string feature_name "name of feature"
    }
```

## Table Schema

| Column name    | type    | Length | Constraints | Nullable | Remark         |
| -------------- | ------- | ------ | ----------- | -------- | -------------- |
| `id`           | INT     |        | PRIMARY KEY |          | AUTO_INCREMENT |
| `feature_name` | VARCHAR | 32     |             | N        |                |

## Simple Value

| Column Name    | Simple                 |
| -------------- | ---------------------- |
| `id`           | 1                      |
| `feature_name` | Create Click Bait Word |
