# Table: `dim_scan_engine`

**Description:** Dimensions for the scan engines that may be selected to run scans, including standalone engines or pools. One record is present in this dimension for each scan engine that is defined.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `scan_engine_id` | `integer` | The unique identifier of the scan engine. |
| `name` | `text` | The name of the scan engine. |
| `address` | `text` | The address (either IP or host name) of the scan engine. |
| `port` | `integer` |  The port the scan engine is running on. |
| `type` | `text` | The type of the scan engine, one of the values: 'Standard', 'Pool', 'VMWare EPsec', or 'AWS' |