# Table: `dim_asset_operating_system`

**Description:** Dimension for the potential operating systems on an asset. Unlike dim_asset, this dimension provides access to all operating system fingerprints that have been detected. If an asset has no operating system fingerprints detected on  it, there will be no records for the asset in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | he unique identifier of the asset. |
| `vendor` | `text` | The vendor name of the operating system (e.g. 'Microsoft' or 'Cisco'). |
| `family` | `text` | The family of the operating system (e.g. 'Windows' or 'IOS'). |
| `name` | `text` | The product name of the operating system (e.g. 'Windows' or 'Linux'). |
| `version` | `text` | The version of the operating system (e.g. 'SP1' or '2.2.14'). |
| `architecture` | `text` | The architecture of the operating system detected on the asset (e.g. 'x86'). |
| `description` | `text` | The full description of the operating system, including the  vendor, family, name, and version. |
| `system` | `text` | A shortened form of the operating system description that includes only the vendor, family, and product. This field is best used when grouping by related operating system families and product combination irrespective of  specified versions. |
| `cpe` | `text` | The common platform enumerate (CPE) value, if applicable, associated to the operating system. |
| `type` | `text` | The type of the operating system which describes the classification by purpose, such as 'Router', 'Switch',  'Workstation', 'General', etc. |
| `certainty` | `real` | The certainty of the fingerprint, expressed as a decimal confidence between 0 (low) and 1.0 (high). |