# Table: `periods`

**Description:** Stores the historical period information indicating when the warehouse was updated during an ETL process. This can be used to determine what dates the warehouse has data for, particularly for trending and temporal-oriented queries.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date an export took place. |