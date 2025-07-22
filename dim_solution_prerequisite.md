# Table: `dim_solution_prerequisite`

**Description:** Dimension that provides an association between a solution and all the prerequisite solutions that must be applied before it. If a solution has no prerequisites, it will have no records in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `solution_id` | `integer` | The unique identifier of the solution. |
| `required_solution_id` | `integer` | The unique identifier of the prerequisite solution. |