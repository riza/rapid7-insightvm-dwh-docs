# Table: `dim_policy_rule_test`

**Description:** Dimension that provides all tests associated with a policy rule.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `rule_id` | `bigint` | The identifier of the policy rule. |
| `test_key` | `text` | The unique identifier for the policy rule test |
| `test_type` | `text` | The type of the policy rule test. |
| `description` | `text` | The description of what the rule test is checking for. |
| `check_existence` | `text` | The identifier of the type of condition performed in the test. Can be "all_exist", "any_exist", "at_least_one_exists", "none_exist", or "only_one_exists". |
| `check_results` | `text` | The rule test check results. |
| `state_operator` | `text` | The logical operator that combines the evaluation results from each referenced state on a per item basis. |