# Table: `dim_tag`

**Description:** Dimension for all tags that any assets within the scope of the report belong to. Each tag has either a direct association or indirection association to an asset based off site or asset group association or of dynamic membership criteria.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `tag_id` | `integer` | The unique identifier of the tag. |
| `name` | `text` | The name of the tags. Tag names are unique for tags of the same type. |
| `type` | `text` | The type of the tag, one of the following values: 'CRITICALITY', 'LOCATION', 'OWNER', or 'CUSTOM'. |
| `source` | `text` | The original application that created the tag. |
| `created` | `timestamp without time zone` | The creation time. |
| `risk_modifier` | `double precision` | The risk modifier for a CRITICALITY typed tag. |
| `color` | `text` | The optional color of the tag, in hexadecimal notation. |