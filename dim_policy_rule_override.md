# Table: `dim_policy_rule_override`

**Description:** Dimension for all the metadata for each policy rule that has been overriden.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `override_id` | `bigint` | The identifier of the policy rule override. |
| `rule_id` | `bigint` | The identifier of the policy rule. |
| `scope` | `text` | The system scope for which the policy rule override applies to. |
| `scope_description` | `text` | The description for the scope of the rule override |
| `submitted_by` | `text` | User name that submitted the policy rule override. |
| `submitted_time` | `timestamp without time zone` | Timestamp for submission of the policy rule override. |
| `comment` | `text` | Reporter comment that submitted the policy rule overrdie. |
| `reviewed_by` | `text` | Reviewer of the policy rule override. |
| `review_comment` | `text` | Reviewer comment for overriding the policy rule. |
| `reviewed_rule_result` | `text` | The original rule status when the override was submitted |
| `effective_time` | `timestamp without time zone` | Timestamp for the policy rule override to be effective. |
| `expiration_time` | `timestamp without time zone` | Timestamp for the expiration of the policy rule override. |
| `status` | `text` | Aggregated values for the rule status. |
| `overridden_rule_result` | `text` | The overridden rule status. |
| `asset_id` | `bigint` | The identifier of the asset for which the policy rule override applies to. |