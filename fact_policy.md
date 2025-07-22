# Table: `fact_policy`

**Description:** Accumulating snapshot fact table for a policy. This is a convenience fact to rollup assets by the policy to measure the policy's overall compliance.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `rule_compliance` | `double precision` | The ratio of rules passing across all tested assets to the total number of scorable rules across all tested assets for this policy. |
| `total_assets` | `bigint` | The total number of tested assets with applicable results. An asset has applicable results if at least one rule has a pass or fail status. An asset with all rule status. |
| `compliant_assets` | `bigint` | The number of assets passing all scorable rules in the policy. |
| `noncompliant_assets` | `bigint` | The number of assets failing at least one scorable rule in the policy. |
| `asset_compliance` | `double precision` | The ratio of assets passing all scorable rules in the policy to the total number of assets tested with the policy. |