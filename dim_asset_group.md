# Table: `dim_asset_group`

**Description:** Dimension for all asset groups that any assets within the scope of the report belong to. Each asset group has metadata that define it and can be based off dynamic membership criteria.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_group_id` | `integer` | The unique identifier of the asset group. |
| `name` | `text` | The name of the asset groups which provides a human-readable identifier of the group. Asset groups names are guaranteed to be unique across multiple groups. |
| `description` | `text` | A description of the asset group that indicates the content, purpose, or composition of a group. |
| `dynamic_membership` | `boolean` | Indicates whether the assets belonging to the group are defined statically by an user, or can change automatically based on asset metadata. If true, the membership of the group is dynamically changed whenever scans are performed on assets and the metadata and vulnerabilities related to the asset change. If false, the membership is static and defined manually by a group administrator. |