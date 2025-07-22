# Table: `dim_asset_service_configuration`

**Description:** Dimension for the configurations that have been detected on the services of an asset. Each record represents a configuration value that has been detected on a service. If an asset has no services detected on it, or no configurations were detected, there will be no records for the asset in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `service` | `text` | The protocol name of the service (e.g. 'HTTPS', 'RPC', 'NTP', etc). |
| `port` | `integer` | The port the service is running on. |
| `protocol` | `text` | The procol the service is exposed through (e.g. 'TCP', or 'UDP'). |
| `name` | `text` | The name of the configuration value (e.g. 'http.banner', 'ssl.cert.sig.alg.name', etc). |
| `value` | `text` | The value of the configuration (e.g. 'Microsoft-IIS/7.5', 'SHA1withRSA', etc). |