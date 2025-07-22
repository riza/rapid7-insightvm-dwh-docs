# Table: `fact_asset_event`

**Description:** Transactional fact for every event that has occurred on an asset that may have changed the data on the asset. These events includes scans, data import, applying exceptions, etc.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `event_id` | `bigint` | The unique identifier of the event that the asset was modified by, which can be shared across multiple assets that were a part of the same event. |
| `date` | `timestamp without time zone` | The date in which the event was performed or executed. |
| `type` | `text` | The type of the event, which is one of the following values: 'SCAN' - the asset was scanned by a scan engine 'ACTIVE-SYNC' - the asset was scanned/discovered through an active sync connection 'VULNERABILITY_EXCEPTION_APPLIED' - a vulnerability exception was applied to a vulnerability 'VULNERABILITY_EXCEPTION_UNAPPLIED' - a vulnerability exception was unapplied (removed) from a vulnerability 'ASSET-IMPORT' or 'EXTERNAL-IMPORT' - the asset was imported through the external API 'EXTERNAL-IMPORT-APPSPIDER' - the asset was imported from an AppSpider scan 'SCAN-LOG-IMPORT' - the asset was import using a console command 'SCAN-LOG-INGESTOR-UPGRADE' - the asset was imported during a one-time product upgrade (deprecated) |
| `scan_id` | `bigint` | If the type is 'SCAN', 'ACTIVE-SYNC', 'SCAN-LOG-IMPORT', 'SCAN-LOG-INGESTOR-UPGRADE', or 'EXTERNAL-IMPORT-APPSPIDER' the identifier of the scan that was run. For all other types the value is NULL. |
| `vulnerability_exception_id` | `integer` | If the type is 'VULNERABILITY_EXCEPTION_APPLIED' or 'VULNERABILITY_EXCEPTION_UNAPPLIED' the identifier of the vulnerability exception that was applied or unapplied. |
| `user_name` | `text` | If the type is 'EXTERNAL-IMPORT' or 'ASSET-IMPORT' the login name of the user that performed the import. |
| `description` | `text` | A description of the event that was performed. This can include details specific to the event type. |