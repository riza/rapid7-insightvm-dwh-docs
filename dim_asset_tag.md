# Table: `dim_asset_tag`

**Description:** Dimension for the association between an asset and a tag. For each asset there will be one record with an association to only one tag. This dimension only provides current associations and does not indicate whether an asset once belonged to a tag, but it is no longer.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `tag_id` | `integer` | The unique identifier of the tag. |
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `association` | `text` | The association that the tag has with the asset. It can be a direct association ("tag") or an indirect association through a site ("site"), a group ("group") or the tag dynamic search criteria ("criteria"). |
| `site_id` | `bigint` | The site id by which an asset indirectly associates with the tag. |
| `asset_group_id` | `bigint` | The asset group id by which an asset indirectly associates with the tag. |