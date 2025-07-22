# Table: `dim_asset_unique_id`

**Description:** Dimension for the unique identifiers on an asset. Each record represents a unique identifier enumerated on an asset. Not all assets are guaranteed to have a unique identifier.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `source` | `text` | The source used to discover the unique identifier (e.g. 'WML' or 'system_profiler', etc) |
| `unique_id` | `text` | The unique identifier enumerated on the asset. |