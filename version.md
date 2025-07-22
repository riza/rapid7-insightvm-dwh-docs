# Table: `version`

**Description:** Stores the current version of the schema the warehouse is using. This information is used to perform upgrades over time, and is not a native part of the dimensional model.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `version` | `integer` | The current database schema version of this warehouse. |