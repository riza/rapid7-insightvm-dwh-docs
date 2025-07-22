# Table: `fact_asset_date`

**Description:** Periodic snapshot fact for assets. This fact table is a date-based snapshot of the fact_asset table. During each export process, the current data is appended to this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date the snapshot was recorded. |
| `asset_id` | `bigint` | The unique identifier of the asset. |
| `vulnerabilities` | `bigint` | The number of vulnerability findings on the asset. This value is equal to the sum of critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns. |
| `critical_vulnerabilities` | `bigint` | The number of vulnerability findings for vulnerabilities with a critical severity. |
| `severe_vulnerabilities` | `bigint` | The number of vulnerability findings for vulnerabilities with a severe severity. |
| `moderate_vulnerabilities` | `bigint` | The number of vulnerability findings for vulnerabilities with a moderate severity. |
| `malware_kits` | `integer` | The number of distinct malware kits that can exploit any vulnerabilities on the asset. |
| `exploits` | `integer` | The number of distinct exploit modules that can exploit any vulnerabilities on the asset. |
| `vulnerabilities_with_malware_kit` | `integer` | The number of vulnerabilities on the asset that have at least one malware kit. |
| `vulnerabilities_with_exploit` | `integer` | The number of vulnerabilities on the asset that have at least one exploit module. |
| `vulnerability_instances` | `bigint` | The total number of instances of all vulnerabilities. |
| `raw_risk_score` | `double precision` | The risk score of the asset across all vulnerabilities but with no risk factor applied. |
| `risk_score` | `double precision` | The risk score of the asset across all vulnerabilities with any applicable risk factor applied.  |
| `pci_status` | `text` | The compliance level, either 'Pass' or 'Fail', of the asset according to PCI standards. |
| `pci_failures` | `bigint` | Numerical representation of the pci_status that can be used for aggregation. If pci_status is 'Pass' the value is 0, and if 'Fail' the value is 1. |
| `validated_vulnerabilities` | `bigint` | The number of vulnerabilities that have been validated. |