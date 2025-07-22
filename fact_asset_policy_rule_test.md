# Table: `fact_asset_policy_rule_test`

**Description:** Accumulating snapshot of policy rule test results on an asset. These results include each system configuration entity tested.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `rule_id` | `bigint` | The unique identifier of the rule. |
| `test_key` | `text` | The unique identifier of the policy rule test. |
| `test_result` | `text` | The result of the policy rule test. Possible values are 'true', 'false', 'unknown', 'error', 'not_evaluated', and 'not_applicable' |
| `entity_name` | `text` | The name of the object or state entity that was tested. |
| `entity_operation` | `text` | The operation applied to the entity to determine a result. |
| `entity_value` | `text` | The expected value we are testing the entity for. |
| `object_key` | `text` | The unique identifier of the policy rule test object. |
| `object_collection_flag` | `text` | A flag indicating the collection status of the policy rule test object. |
| `state_key` | `text` | The unique identifier of the policy rule test state. |
| `collected_entity_value` | `text` | The collected value for the entity. |
| `location` | `text` | The location of the system configuration entity tested on the asset. |
| `test_result_id` | `bigint` | The unique identifier of a policy rule test result. Multiple rows in this table can be associated with a single policy rule test result. |