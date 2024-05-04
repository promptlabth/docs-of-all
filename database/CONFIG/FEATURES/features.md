# TABLE features
---
```mermaid
erDiagram
    FEATURES{
        int id PK
        string name "name of the feature" 
    }
```

## Table Schema

| Column name | type    | Length | Constraints | Nullable | remark |
| ----------- | ------- | ------ | ----------- | -------- | ------ |
| `id`        | int     |        | Primary Key | N        |        |
| `name`      | varchar | 50     |             | N        |        |

## Simple Value

| Column Name | Simple          |
| ----------- | --------------- |
| `id`        | 1               |
| `name`      | เขียนแคปชั่นขายของ |