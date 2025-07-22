# Table: `dim_asset_address`

**Description:** Dimension for the network addresses of an asset. Each record represents a pair of IP and MAC that were enumerated on the asset. Since every asset will always have at least one IP address, each asset is guranteed to have on value in this table. Every "primary" address from the dim_asset dimension will be present in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `ip_address` | `inet` | The IP address of the asset. |
| `mac_address` | `macaddr` | The MAC address of the asset. |
| `primary` | `boolean` | Indicates whether this was the primary address of the asset (i.e. the address used to perform a network scan). |