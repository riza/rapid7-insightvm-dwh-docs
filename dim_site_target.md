# Table: `dim_site_target`

**Description:** Dimension for all the included and excluded targets of a site. For all assets in the scope of the report, a record will be present for each unique IP range and/or host name defined as an included or excluded address in the site configuration. If any global exclusions are applied, these will also be provided at the site level.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `site_id` | `integer` | The unique identifier of the site. |
| `type` | `text` | Either 'host' or 'ip' to indicate the type of address. |
| `included` | `boolean` | True if the target is included in the configuration, or false if it is excluded. |
| `target` | `text` | If type is 'host', this is the host name. |
| `ip_start` | `inet` | If type is 'ip', this is the starting IP address of the range (if there is no range, the ip_start is the IP). |
| `ip_end` | `inet` | If type is 'ip', this is the ending IP address of the range (if there is no range, ip_end is the same as the ip_start). |
| `scope` | `text` | The scope of an exclusion, either 'global' if the exclusion is a global exclusion, 'site' if the exclusion is defined on the site, or NULL if included is true. |