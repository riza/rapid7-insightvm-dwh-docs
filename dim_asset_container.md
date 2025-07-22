# Table: `dim_asset_container`

**Description:** Dimension for the containers detected on a container host. Each record represents one container. If an asset has no containers or is not a container host, there will be no records for the asset in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `container_id` | `text` | The identifier of the container. |
| `name` | `text` | The name of the container. |
| `status` | `text` | The status of the container (one of 'CREATED', 'RUNNING', 'PAUSED', 'RESTARTING', 'EXITED', 'DEAD', or 'UNKNOWN;') |
| `created` | `timestamp without time zone` | The date that the container was created. |
| `started` | `timestamp without time zone` | The date the container last started. |
| `finished` | `timestamp without time zone` | The date the container last finished running. |
| `image_id` | `text` | The identifier of the image the container was based from. |
| `digest` | `text` | The digest of the image the container was based on. |
| `repository` | `text` | The repository of the image the container was based on. |