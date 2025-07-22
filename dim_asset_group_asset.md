# Table: `dim_asset_group_asset`

**Description:** Dimension for the association between an asset and an asset group. For each asset there will be one record with an association to one asset group. This dimension only provides current associations and does not indicate whether an asset once belonged to a group, but it is no longer.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_group_id` | `integer` | The unique identifier of the asset group. |
| `asset_id` | `bigint` | The unique identifier of the asset. |