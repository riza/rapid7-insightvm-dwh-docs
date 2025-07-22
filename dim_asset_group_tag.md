# Table: `dim_asset_group_tag`

**Description:** Dimension that demonstrates the relationship between an asset group and a tag. For each tag applied to an  asset group there will be one record in this dimension. If an asset group has not associated tags, no records for that group will apear in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_group_id` | `bigint` | The unique identifier of the asset group. |
| `tag_id` | `integer` | The unique identifier of the tag applied to the asset group. |