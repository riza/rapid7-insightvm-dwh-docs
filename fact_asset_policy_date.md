# Table: `fact_asset_policy_date`

**Description:** Periodic snapshot fact for asset policy records. This fact table is a date-based snapshot of the fact_asset_policy table. During each export process, the current data is appended to this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date the snapshot was recorded. |
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `scan_id` | `bigint` | The unique identifier of the scan. |
| `date_tested` | `timestamp without time zone` | The time at which the policy was tested against the asset. |
| `compliant_rules` | `integer` | The total number of rules for which the asset passed in the most recent scan for this policy. |
| `noncompliant_rules` | `integer` | The total number of rules for which the asset failed in the most recent scan for this policy |
| `not_applicable_rules` | `integer` | The total number of rules that were not applicable to the asset in the most recent scan for this policy. |
| `rule_compliance` | `double precision` | The ratio of passing results for the rules to the total number of scorable rules for this policy. |