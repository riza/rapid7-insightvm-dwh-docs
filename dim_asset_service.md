# Table: `dim_asset_service`

**Description:** Dimension for the set of services that have been detected on an asset. Each record represents an open port that is  running a service on a protocol. If an asset has no services detected on it, there will be no records for the asset in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `service` | `text` | The protocol name of the service (e.g. 'HTTPS', 'RPC', 'NTP', etc). |
| `port` | `integer` | The port the service is running on. |
| `protocol` | `text` | The procol the service is exposed through (e.g. 'TCP', or 'UDP'). |
| `vendor` | `text` | The vendor of the software running on the service (e.g. 'Apache', 'Microsoft', etc). |
| `family` | `text` | The family of the software running on the service (e.g. 'Apache', 'IIS', etc). |
| `name` | `text` | The name of the software running on the service (e.g. 'HTTPD', 'IIS', etc). |
| `version` | `text` | The version of the software running on the service (e.g. '7.5'). |
| `certainty` | `real` | The certainty of the service fingerprint that detected the service, expressed as a decimal confidence between 0 (low) and 1.0 (high). |
| `credential_status` | `text` | The status of the credential(s) used against this service, which is one of the following values: 'No credentials supplied', 'Login failed', 'Login successful', 'Allowed elevation of privileges', 'Root', 'Login as local admin' |