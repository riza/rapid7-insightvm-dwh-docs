# Table: `dim_asset_group_account`

**Description:** Dimension for user group accounts that have been enumerated on an asset. Each record represents one user group account on an asset. If an asset has no user group accounts enumerated, there will be no records in this dimension for the asset.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `name` | `text` | The name of the group account. |