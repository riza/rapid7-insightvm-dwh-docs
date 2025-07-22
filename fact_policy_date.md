# Table: `fact_policy_date`

**Description:** Periodic snapshot fact for policies. This fact table is a date-based snapshot of the fact_policy table. During each export process, the current data is appended to this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date the snapshot was recorded. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `rule_compliance` | `double precision` | The ratio of rules passing across all tested assets to the total number of scorable rules across all tested assets for this policy. |
| `total_assets` | `bigint` | The total number of tested assets with applicable results. An asset has applicable results if at least one rule has a pass or fail status. An asset with all rule status. |
| `compliant_assets` | `bigint` | The number of assets passing all scorable rules in the policy. |
| `noncompliant_assets` | `bigint` | The number of assets failing at least one scorable rule in the policy. |
| `asset_compliance` | `double precision` | The ratio of assets passing all scorable rules in the policy to the total number of assets tested with the policy. |