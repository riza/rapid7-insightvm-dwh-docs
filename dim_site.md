# Table: `dim_site`

**Description:** Dimension for all sites. Each site has metadata that define it, including organization information.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `site_id` | `integer` | The unique identifier of the site. |
| `name` | `text` | The name of the site which provides a human-readable identifier of the site. Site names are guaranteed to be unique across multiple sites. |
| `description` | `text` | An optional description of the site that indicates the content, purpose, or composition of a site. |
| `importance` | `text` |  A human-readable description of the importance of site, one of the values: 'Very Low', 'Low', 'Normal', 'High' or 'Very High' |
| `dynamic_targets` | `boolean` | Indicates whether the targets defined  within the site are dynamically configured based on a discovery connection. If true, a discovery connection is the means by which the targets of a site are defined and dynamically updated. If false, the target definition is static and manually configured by a site administrator. |
| `risk_factor` | `real` | An adjustment factor for the risk of a site. The weighting factor defaults to 1.0 and can be adjusted up or down as the importance of a site is changed. The higher the importance, the larger the risk factor, and the lower the  importance, the lower the risk factor. |
| `last_scan_id` | `bigint` | The identifier of the scan that last ran for the site. If the site has not had a scan run, the value will be NULL. |
| `previous_scan_id` | `bigint` | The identifier of the scan that ran prior to the last scan for the site. If the site has not had a scan run, the value will be NULL. |
| `scan_template` | `text` | The name of the scan template the site is currently configured to run scans using. |
| `scan_template_id` | `text` | The identifier of the scan template the site is currently configured to run scans using. |
| `scan_engine` | `text` | The name of the scan engine the site is currently configured to run scans with. |
| `scan_engine_id` | `integer` | The identifier of the scan engine the site is currently configured to run scans with. |
| `organization_name` | `text` | The optional name of the organization the site is associated to. |
| `organization_url` | `text` | The URL/website of the organization the site is associated to.  |
| `organization_contact` | `text` | The contact name for the contact of the  organization the site is associated to. |
| `organization_job_title` | `text` | The job title of the contact of the organization the site isassociated to. |
| `organization_email` | `text` | The email address of the contact of the organization the site is associated to. |
| `organization_phone` | `text` | The phone number of the organization the site is associated to.  |
| `organization_address` | `text` | The address of the organization the site is associated to. |
| `organization_city` | `text` | The city/region of the organization the site is associated to. |
| `organization_state` | `text` | The state/county/province/territory of the organization of the site. |
| `organization_country` | `text` | The country of organization the site is associated to. |
| `organization_zip` | `text` | The zip-code (if applicable) of the organization the site is associated to. |