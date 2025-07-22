# Table: `dim_policy_rule`

**Description:** Dimension for all the metadata for each rule within a policy. It contains one record for every rule within each policy.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `rule_id` | `bigint` | The identifier of the policy rule. |
| `policy_id` | `bigint` | The identifier of the policy. |
| `rule_name` | `text` | The name of the rule from the selected policy. |
| `title` | `text` | The title of the rule, for each policy, that is visible to the user. |
| `description` | `text` | A description of the rule. |
| `parent_group_id` | `bigint` | The identifier of a group in the policy that a rule directly belongs to. |
| `role` | `text` | The rule's role in scoring and reporting: "full", "unchecked" and "unscored". |
| `severity` | `text` | The severity of the rule. A textual value that can be one of the following: "low", "medium", "high", or "unknown". |
| `weight` | `numeric` | The scoring weight of the rule. |
| `rationale` | `text` | Descriptive text explaining why compliance is important to the security of the target platform. |
| `remediation` | `text` | Instructions for remediating the non-compliant rule. |
| `enabled` | `boolean` | The boolean to determine whether this rule is enabled. |