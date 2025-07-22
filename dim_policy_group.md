# Table: `dim_policy_group`

**Description:** Dimension for all the metadata for each group of rules within a policy. It contains one record for every unhidden group within each policy.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `group_id` | `bigint` | The identifier of the group. |
| `policy_id` | `bigint` | The identifier of the policy. |
| `group_name` | `text` | The name of policy group. |
| `title` | `text` | The title of the group, for each policy, that is visible to the user. |
| `description` | `text` | A description of the group. |
| `weight` | `numeric` | The scoring weight of the policy group. |
| `parent_group_id` | `bigint` | The identifier of a group in the policy that a group directly belongs to. |
| `sub_groups` | `integer` | The number of groups decending from a group |
| `rules` | `integer` | The number of all rules including rule of a group's sub-groups. |