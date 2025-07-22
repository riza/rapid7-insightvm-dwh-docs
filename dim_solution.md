# Table: `dim_solution`

**Description:** Dimension that provides access to all solutions defined. A solution models the information, steps, and background required to remediate a vulnerability.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `solution_id` | `integer` | The unique identifier of the solution. |
| `nexpose_id` | `text` | The key/identifier of the solution that uniquely identifies it within Nexpose. |
| `estimate` | `interval` | The amount of required time estimated to implement this solution on a single asset. This is a heuristic and may not represent the actualy time required to remediate or apply the solution, depending on the environment and tools available for remediation. |
| `url` | `text` | An optional URL link defined for getting more information about the solution. When defined, this may be a web page defined by the vendor that provides more details on the solution, or it may be a download link to a patch. |
| `solution_type` | `text` | Type of the solution, one of the values: 'PATCH', 'ROLLUP' or 'WORKAROUND'. |
| `fix` | `text` | The steps that are a part of the fix this solution prescribes. The fix will usually contain a list of procedures that must be followed to remediate the vulnerability. The fix is represented using HTML markup that can be "flattened" using the htmlToText() function. |
| `summary` | `text` | A short summary of solution which describes the purpose of the solution at a high level and is suitable for use as a summarization of the solution. The summary is represented using HTML markup that can be "flattened" using the htmlToText() function. |
| `additional_data` | `text` | Additional information about the solution. The additional data is represented using HTML markup that can be "flattened" using the htmlToText() function. |
| `applies_to` | `text` | Textual representation of the types of system, software, and/or services that the solution can be applied to. If the solution is not restricted to a certain type of system, software or service, this field will be NULL. |