# Table: `dim_policy`

**Description:** Dimension for all metadata related to a policy. It contains one record for every policy that currently exists.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `policy_id` | `bigint` | The unique identifier of a policy. |
| `benchmark_id` | `bigint` | A foreign key to the benchmark the policy is associated with. |
| `policy_name` | `text` | The natural identifier of the policy benchmark source file. Can be used to uniquely identify a policy, in conjunction with columns benchmark_name and benchmark_version from the table policy_benchmark, as an alternative from using the primary key column policy_id. |
| `policy_version` | `text` | The version of the policy. |
| `title` | `text` | The title of the policy as visible to the user. |
| `description` | `text` | A description of the policy. |
| `unscored_rules` | `integer` | The number of rules defined in the policy with a role of "unscored". These rules will not affect rule compliance scoring for the policy. |