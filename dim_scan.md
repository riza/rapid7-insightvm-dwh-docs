# Table: `dim_scan`

**Description:** Dimension for all scans that have been run on any sites within access of the report, not only those within the scope. All scans will be made available, regardless of their scan status, including currently running scans.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `scan_id` | `bigint` | The unique identifier of the scan. |
| `site_id` | `integer` | The unique identifier of the site. |
| `started` | `timestamp without time zone` | The time at which  the scan started. |
| `finished` | `timestamp without time zone` | The time at which the scan ended, which may be NULL if a scan is still in progress. |
| `status` | `text` | The current status of the scan, one of the values: 'Aborted', 'Successful', 'Running', 'Stopped', 'Failed', or 'Paused'. |
| `type` | `text` | The type of the scan, one of the values: 'Manual' or 'Scheduled'. |
| `scan_name` | `text` | The user-driven scan name for the scan. |