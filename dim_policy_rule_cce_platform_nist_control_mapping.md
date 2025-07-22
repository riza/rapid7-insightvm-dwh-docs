# Table: `dim_policy_rule_cce_platform_nist_control_mapping`

**Description:** Dimension that provides all NIST SP 800-53 controls mappings for each CCE within a rule.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `rule_id` | `bigint` | The identifier of the policy rule. |
| `cce_item_id` | `text` | The identifier of the CCE item. |
| `platform` | `text` | The platform of the CCE. |
| `control_name` | `text` | The name of the control mapping. |
| `date_published` | `date` | The date published of the control mapping. |