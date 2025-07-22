# Table: `dim_asset_user_account`

**Description:** Dimension for user accounts that have been enumerated on an asset. Each record represents one user account on an asset. If an asset has no user accounts enumerated, there will be no records in this dimension for the asset.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `name` | `text` | The short, login name associated with the user account. This value may be null, but is typically non-null. |
| `full_name` | `text` | The longer name, or description, of the user account. |