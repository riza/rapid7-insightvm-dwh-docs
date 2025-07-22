# Table: `dim_site_asset`

**Description:** Dimension for the association between an asset and a site. For each asset there will be one record with an association to only one site.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `site_id` | `integer` | The unique identifier of the site. |
| `asset_id` | `bigint` | The unique identifier of the asset. |