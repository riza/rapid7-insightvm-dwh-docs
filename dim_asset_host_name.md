# Table: `dim_asset_host_name`

**Description:** Dimension for the aliases or host names of an asset. Each record represents one of the host names that were discovered during the most recent scan of the asset, including the primary name available within the other asset fact tables. This dimension is built so it includes all aliases found in any node on the asset within the latest scan. If an asset did not have a host name detected in the latest scan, an empty value will be associated with the asset.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `host_name` | `text` | A host name that was detected on the asset. |
| `source_type` | `text` | The type of the mechanism used to perform the host name detection (e.g. 'DNS', 'NetBIOS', etc). |
| `source` | `text` | The name of the source used to to perform the host name detection (e.g. 'CIFS Name Service', 'uname -n', etc). |