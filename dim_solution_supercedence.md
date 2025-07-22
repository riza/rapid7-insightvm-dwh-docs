# Table: `dim_solution_supercedence`

**Description:** Dimension that provides all superceding associations between solutions. Unlike dim_solution_highest_supercedence, this dimension provides access to the entire graph of superceding relationships. If a solution does not supercede any other solution, it will not have any records in this dimension.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `solution_id` | `integer` | The unique identifier of the solution. |
| `superceding_solution_id` | `integer` | The unique identifier of the superceding solution. |