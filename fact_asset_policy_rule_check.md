# Table: `fact_asset_policy_rule_check`

**Description:** Accumulating snapshot of policy rule check results on an asset. This fact provides a record for each policy rule check that was tested on an asset in its most recent scan.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `result_id` | `bigint` | The unique identifier of the rule check result. |
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `rule_id` | `bigint` | The unique identifier of the policy rule. |
| `scan_id` | `bigint` | The unique identifier of the scan. |
| `override_id` | `bigint` | The identifier of a policy override effectively overriding a rule test result on the asset. If there are more than one such overrides, the last submitted one will take precedent over the rest. |
| `override_ids` | `bigint[]` | The array identifiers of policy overrides potentially overriding a rule test result on an asset. |
| `date_tested` | `timestamp without time zone` | The time at which the policy rule check was tested against the asset. |
| `check_result` | `text` | The rule check result on an asset. |
| `proof` | `text` | The proof gathered during the evaluation of the rule check on the asset. |