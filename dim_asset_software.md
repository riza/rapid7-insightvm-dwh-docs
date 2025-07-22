# Table: `dim_asset_software`

**Description:** Dimension for the software that have been detected on an asset. Each record represents a fingerprint result and multiple software instances can be associated with each asset. If an asset had no installed software detected, there will be no records in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `vendor` | `text` | The vendor of the software. |
| `family` | `text` | The product family of the software. |
| `name` | `text` | The product name of the software. |
| `version` | `text` | The version of the software. |
| `type` | `text` | The type of the software, indicating its purpose or classification (e.g. 'General', 'Virtualization', 'Database Server', 'Security', etc). |
| `cpe` | `text` | The common platform enumerate (CPE) value, if applicable, associated to the software. |