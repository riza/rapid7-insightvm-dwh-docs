# Table: `fact_tag`

**Description:** Accumulating snapshot fact for the summary information of a tag. This is a convenience fact for rolling up the information for assets that are tagged with a tag. The summary information provided is based on the most recent data for each asset in the membership of the tag. If a tag has no assets, there will be a fact record with zero  counts.


| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| `tag_id` | `integer` | The unique identifier of the tag. |
| `assets` | `bigint` | The total number of accessible assets associated to the tag. If the tag has no accessible assets in the current scope or membership, this value can be zero. |
| `vulnerabilities` | `bigint` | The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns. |
| `critical_vulnerabilities` | `bigint` | The sum of the count of critical vulnerabilities on each asset. |
| `severe_vulnerabilities` | `bigint` | The sum of the count of severe vulnerabilities on each asset. |
| `moderate_vulnerabilities` | `bigint` | The sum of the count of moderate vulnerabilities on each asset. |
| `malware_kits` | `integer` | The sum of the count of malware kits on each asset. |
| `exploits` | `integer` | The sum of the count of exploits on each asset. |
| `vulnerabilities_with_malware_kit` | `integer` | The sum of the count of vulnerabilities with malware on each asset. |
| `vulnerabilities_with_exploit` | `integer` | The sum of the count of vulnerabilities with exploits on each asset. |
| `vulnerability_instances` | `bigint` | The sum of the vulnerabilities instances on each asset. |
| `raw_risk_score` | `double precision` | The sum of the raw risk score of each asset in the tag. |
| `risk_score` | `double precision` | The sum of the risk score of each asset in the tag. |
| `pci_status` | `text` | The overall compliance level ('Pass' or 'Fail') of the tag according to PCI standards. The status is only 'Pass' if all assets in the tag individually have a status of 'Pass'(e.g. in fact_asset) |
| `pci_failures` | `bigint` | The sum of the total PCI failures on each asset in the group. |
| `validated_vulnerabilities` | `bigint` | The number of vulnerabilities that have been validated. |