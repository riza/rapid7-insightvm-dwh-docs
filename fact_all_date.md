# Table: `fact_all_date`

**Description:** Periodic snapshot fact for all data. This fact table is a date-based snapshot of the fact_all table. During each export process, the current data is appended to this fact table.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `day` | `date` | The date the snapshot was recorded. |
| `assets` | `bigint` | The total number of assets. |
| `vulnerabilities` | `bigint` | The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns. |
| `critical_vulnerabilities` | `bigint` | The sum of the count of critical vulnerabilities on each asset. |
| `severe_vulnerabilities` | `bigint` | The sum of the count of severe vulnerabilities on each asset. |
| `moderate_vulnerabilities` | `bigint` | The sum of the count of moderate vulnerabilities on each asset. |
| `malware_kits` | `integer` | The sum of the count of malware kits on each asset. |
| `exploits` | `integer` | The sum of the count of exploits on each asset. |
| `vulnerabilities_with_malware_kit` | `integer` | The sum of the count of vulnerabilities with malware on each asset. |
| `vulnerabilities_with_exploit` | `integer` | The sum of the count of vulnerabilities with exploits on each asset. |
| `vulnerability_instances` | `bigint` | The sum of the vulnerabilities instances on each asset. |
| `raw_risk_score` | `double precision` | The sum of the raw risk score of each asset. |
| `risk_score` | `double precision` | The sum of the risk score of each asset. |
| `pci_status` | `text` | The overall compliance level ('Pass' or 'Fail') according to PCI standards. The status is only 'Pass' if all assets individually have a status of 'Pass' (e.g. in fact_asset) |
| `pci_failures` | `bigint` | The sum of the total PCI failures on each asset. |
| `validated_vulnerabilities` | `bigint` | The number of vulnerabilities that have been validated. |