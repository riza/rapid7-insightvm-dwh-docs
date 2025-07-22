# Table: `dim_asset_file`

**Description:** Dimension for files and directories that have been enumerated on an asset. Each record represents one file or directory discovered on an asset. If an asset has no files or groups enumerated, there will be no records in this dimension for the asset.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `file_id` | `bigint` | The unique identifier of the file or directory. |
| `type` | `text` | The type of the file. Either 'Directory', 'File' or 'Unknown'. |
| `name` | `text` | The name of the file or directory. |
| `size` | `bigint` | The size of the file or directory in bytes. If the size is unknown, or the file is a directory, the value will be -1. |