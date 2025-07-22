# Table: `dim_solution_highest_supercedence`

**Description:** Dimension that provides access to the highest level superceding solution for every solution. If a solution has multiple superceding solutions that themselves are not superceded, all will be returned. Therefore a single solution may have multiple records returned. If a solution is not superceded by any other solution, it will be marked as being superceded by itself.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `solution_id` | `integer` | The unique identifier of the solution. |
| `superceding_solution_id` | `integer` | The identifier of a solution that is known to supercede the solution, and which itself is not superceded (the highest level of supercedence). If the solution is not superceded, this is the same identifier as solution_id. |