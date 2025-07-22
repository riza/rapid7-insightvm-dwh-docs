# Table: `fact_policy_rule`

**Description:** Accumulating snapshot fact table for a policy rule. This is a convenience fact to rollup assets by the policy rule to measure the policy rule's asset compliance.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `rule_id` | `bigint` | The unique identifier of the policy rule. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `compliant_assets` | `bigint` | The number of assets passing the rule. |
| `noncompliant_assets` | `bigint` | The number of assets failing the rule. |
| `not_applicable_assets` | `bigint` | The number of assets not applicable to the rule. Assets not applicable to this rule are only counted if they are applicable to at least one rule in the policy. |
| `asset_compliance` | `double precision` | The ratio of the assets passing the rule to the assets tested. |