# Table: `dim_asset`

**Description:** Dimension for the most recent information of all assets. This is a slowly changing dimension that will change as new scans are performed on the asset.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `mac_address` | `macaddr` | The primary or best MAC address associated to  the asset in the scan. |
| `ip_address` | `inet` | The primary IP address of the asset. |
| `host_name` | `text` | The primary host name of the asset. If a host name was used to detect the asset, this name will be preferred. |
| `host_type` | `text` | The type of the host, which is one of the values: 'Virtual Machine', 'Hypervisor', 'Bare Metal', or 'Mobile'. |
| `os_type` | `text` | The type of the asset which describe the classification by purpose, such as 'Router', 'Switch', 'Workstation', 'General', etc. |
| `os_vendor` | `text` | The vendor name of the operating system detected on the asset (e.g. 'Microsoft' or 'Cisco'). |
| `os_family` | `text` | The family of the operating system detected on the asset (e.g. 'Windows' or 'IOS'). |
| `os_name` | `text` | The product name of the operating system detected on the asset (e.g. 'Windows' or 'Linux'). |
| `os_version` | `text` | The version of the operating system detected on the asset (.e.g 'SP1' or '2.2.14'). |
| `os_architecture` | `text` | The architecture of the operating system detected on the asset (e.g. 'x86'). |
| `os_description` | `text` | The full description of the operating system detected on the asset, including the  vendor, family, name, and version. |
| `os_system` | `text` | A shortened form of the operating system description that includes only the vendor, family, and product. This field is best used when grouping by related operating system families and product combination irrespective of  specified versions. |
| `os_cpe` | `text` | The common platform enumerate (CPE) value, if applicable, associated to the operating system detected on the asset. |
| `risk_modifier` | `double precision` | A modifier value that influences the overall risk score of the asset (a multiplier factor on the raw risk score). |
| `assessed_for_vulnerabilities` | `boolean` | Indicates whether this asset was assessed for vulnerabilities at least once in its history. |
| `assessed_for_policies` | `boolean` | Indicates whether this asset was assessed for policy compliance at least once in its history. |
| `credential_status` | `text` | The status of credentials on the asset used most recently. This is an aggregation of all credentials and is one of the following values: 'No credentials supplied', 'All credentials failed', 'Credentials partially successful', or 'All credentials successful'. |
| `sites` | `text` | A comma-delimited list of sites the asset is currently a part of (sorted by name ascending). This column can be used a simple and conventient alternative to querying against dim_site_asset to retrieve the individual  relationships. |
| `last_assessed_for_vulnerabilities` | `timestamp without time zone` | The date and time the asset was last assessed for vulnerabilities. |