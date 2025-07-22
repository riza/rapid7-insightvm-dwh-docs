# Table: `dim_policy_benchmark`

**Description:** Dimension for all metadata related to a policy benchmark. It contains one record for every policy benchmark that currently exists.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `benchmark_id` | `bigint` | The unique identifier of a policy benchmark. |
| `benchmark_name` | `text` | The natural identifier of the policy benchmark source file. Can be used is conjunction with column benchmark_version to uniquely identify a policy benchmark as an alternative from using the primary key column benchmark_id. |
| `benchmark_version` | `text` | The version of the policy benchmark. Can be used is conjunction with column benchmark_name to uniquely identify a policy benchmark as an alternative from using the primary key column benchmark_id. |
| `title` | `text` | The title of the policy benchmark. |
| `description` | `text` | A description of the policy benchmark. |
| `category` | `text` | A grouping of similar benchmarks based on their source, purpose, or other criteria. Examples include "FDCC", "USGCB", and "CIS". Policy benchmarks created by users have a category of "Custom" |