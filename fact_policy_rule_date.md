# Table: `fact_policy_rule_date`

**Description:** Periodic snapshot fact for policy rules. This fact table is a date-based snapshot of the fact_policy_rule table. During each export process, the current data is appended to this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date the snapshot was recorded. |
| `rule_id` | `bigint` | The unique identifier of the policy rule. |
| `policy_id` | `bigint` | The unique identifier of the policy. |
| `compliant_assets` | `bigint` | The number of assets passing the rule. |
| `noncompliant_assets` | `bigint` | The number of assets failing the rule. |
| `not_applicable_assets` | `bigint` | The number of assets not applicable to the rule. Assets not applicable to this rule are only counted if they are applicable to at least one rule in the policy. |
| `asset_compliance` | `double precision` | The ratio of the assets passing the rule to the assets tested. |