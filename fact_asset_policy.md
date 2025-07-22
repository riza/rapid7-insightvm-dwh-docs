# Table: `fact_asset_policy`

**Description:** Accumulating snapshot fact for all current tested policies on an asset. This fact is a convenience rollup for the fact_asset_policy_rule fact and provides a record for each tested policy on every asset. If an asset was not applicable to any rules in the policy, it will have no records in this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `scan_id` | `bigint` | The unique identifier of the scan. |
| `date_tested` | `timestamp without time zone` | The time at which the policy was tested against the asset. |
| `compliant_rules` | `integer` | The total number of rules for which the asset passed in the most recent scan for this policy. |
| `noncompliant_rules` | `integer` | The total number of rules for which the asset failed in the most recent scan for this policy |
| `not_applicable_rules` | `integer` | The total number of rules that were not applicable to the asset in the most recent scan for this policy. |
| `rule_compliance` | `double precision` | The ratio of passing results for the rules to the total number of scorable rules for this policy. |