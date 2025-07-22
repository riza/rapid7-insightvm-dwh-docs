# Table: `fact_asset`

**Description:** Accumulating snapshot fact table for the latest state of an asset. Each fact record represents the current summary information for an asset, from all data source across all sites the asset belongs to.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
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