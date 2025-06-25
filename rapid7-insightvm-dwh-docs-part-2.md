fact_asset_vulnerability_instance_id	ON asset_id, vulnerability_id


fact_asset_vulnerability_instance_port_protocol	ON port, protocol


fact_asset_vulnerability_instance_service	ON service, port, protocol


fact_asset_vulnerability_instance_status	ON status


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_remediation_date


Periodic snapshot fact for vulnerability remediations on an asset. If a vulnerability has been remediated on an asset since the last export period, a row will be present in this table. Remediation includes the application of vulnerability exceptions.


Columns


day	date	The date the snapshot was recorded.


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


Indexes


fact_asset_vulnerability_remediation_date_id	ON day, asset_id, vulnerability_id


fact_asset_vulnerability_remediation_date_vuln_id	ON vulnerability_id


fact_asset_vulnerability_remediation_date_pkey	ON day, asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_policy


Accumulating snapshot fact table for a policy. This is a convenience fact to rollup assets by the policy to measure the policy's overall compliance.


Columns


policy_id	bigint	The unique identifier of the policy.


rule_compliance	float8	The ratio of rules passing across all tested assets to the total number of scorable rules across all tested assets for this policy.


total_assets	bigint	The total number of tested assets with applicable results. An asset has applicable results if at least one rule has a pass or fail status. An asset with all rule status.


compliant_assets	bigint	The number of assets passing all scorable rules in the policy.


noncompliant_assets	bigint	The number of assets failing at least one scorable rule in the policy.


asset_compliance	float8	The ratio of assets passing all scorable rules in the policy to the total number of assets tested with the policy.


Indexes


fact_policy_policy_id	ON policy_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


fact_policy_date


Periodic snapshot fact for policies. This fact table is a date-based snapshot of the fact_policy table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


policy_id	bigint	The unique identifier of the policy.


rule_compliance	float8	The ratio of rules passing across all tested assets to the total number of scorable rules across all tested assets for this policy.


total_assets	bigint	The total number of tested assets with applicable results. An asset has applicable results if at least one rule has a pass or fail status. An asset with all rule status.


compliant_assets	bigint	The number of assets passing all scorable rules in the policy.


noncompliant_assets	bigint	The number of assets failing at least one scorable rule in the policy.


asset_compliance	float8	The ratio of assets passing all scorable rules in the policy to the total number of assets tested with the policy.


Indexes


fact_policy_date_pkey	ON day, policy_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


fact_policy_rule


Accumulating snapshot fact table for a policy rule. This is a convenience fact to rollup assets by the policy rule to measure the policy rule's asset compliance.


Columns


rule_id	bigint	The unique identifier of the policy rule.


policy_id	bigint	The unique identifier of the policy.


compliant_assets	bigint	The number of assets passing the rule.


noncompliant_assets	bigint	The number of assets failing the rule.


not_applicable_assets	bigint	The number of assets not applicable to the rule. Assets not applicable to this rule are only counted if they are applicable to at least one rule in the policy.


asset_compliance	float8	The ratio of the assets passing the rule to the assets tested.


Indexes


fact_policy_rule_rule_id	ON rule_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


fact_policy_rule_date


Periodic snapshot fact for policy rules. This fact table is a date-based snapshot of the fact_policy_rule table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


rule_id	bigint	The unique identifier of the policy rule.


policy_id	bigint	The unique identifier of the policy.


compliant_assets	bigint	The number of assets passing the rule.


noncompliant_assets	bigint	The number of assets failing the rule.


not_applicable_assets	bigint	The number of assets not applicable to the rule. Assets not applicable to this rule are only counted if they are applicable to at least one rule in the policy.


asset_compliance	float8	The ratio of the assets passing the rule to the assets tested.


Indexes


fact_policy_rule_date_pkey	ON day, rule_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


fact_scan


Transaction fact table for the results of a scan and all the asset within it. This is a convenience fact to rollup assets by scans. Only scans for assets that completed fully will be included in each fact record, but the scan may be in a non-completed state (such as paused).


Columns


scan_id	integer	The unique identifier of the scan.


assets	bigint	The total number of assets in the scan.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset.


risk_score	float8	The sum of the risk score of each asset.


pci_status	text	The overall compliance level ('Pass' or 'Fail') according to PCI standards. The status is only 'Pass' if all assets individually have a status of 'Pass' (e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_scan_pkey	ON scan_id


Foreign Keys


scan_id_fk	(scan_id) references dim_scan (scan_id)


fact_site


Accumulating snapshot fact table for a site. This is a convenience fact to rollup assets by the site(s) they belong to. If an asset belongs to more than one site, its counts will be aggregated in each and every site it belongs to. If a site has no asset, it will still have a record in this fact.


Columns


site_id	integer	The unique identifier of the site.


assets	bigint	The total number of assets in the site.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset.


risk_score	float8	The sum of the risk score of each asset.


pci_status	text	The overall compliance level ('Pass' or 'Fail') according to PCI standards. The status is only 'Pass' if all assets individually have a status of 'Pass' (e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_site_pkey	ON site_id


Foreign Keys


site_id_fk	(site_id) references dim_site (site_id)


fact_site_date


Periodic snapshot fact for sites. This fact table is a date-based snapshot of the fact_site table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


site_id	integer	The unique identifier of the site.


assets	bigint	The total number of assets in the site.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset.


risk_score	float8	The sum of the risk score of each asset.


pci_status	text	The overall compliance level ('Pass' or 'Fail') according to PCI standards. The status is only 'Pass' if all assets individually have a status of 'Pass' (e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_site_date_pkey	ON day, site_id


Foreign Keys


site_id_fk	(site_id) references dim_site (site_id)


fact_tag


Accumulating snapshot fact for the summary information of a tag. This is a convenience fact for rolling up the information for assets that are tagged with a tag. The summary information provided is based on the most recent data for each asset in the membership of the tag. If a tag has no assets, there will be a fact record with zero counts.


Columns


tag_id	integer	The unique identifier of the tag.


assets	bigint	The total number of accessible assets associated to the tag. If the tag has no accessible assets in the current scope or membership, this value can be zero.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset in the tag.


risk_score	float8	The sum of the risk score of each asset in the tag.


pci_status	text	The overall compliance level ('Pass' or 'Fail') of the tag according to PCI standards. The status is only 'Pass' if all assets in the tag individually have a status of 'Pass'(e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset in the group.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_tag_pkey	ON tag_id


Foreign Keys


tag_id_fk	(tag_id) references dim_tag (tag_id)


fact_tag_date


Periodic snapshot fact for tags. This fact table is a date-based snapshot of the fact_tag table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


tag_id	integer	The unique identifier of the tag.


assets	bigint	The total number of accessible assets associated to the tag. If the tag has no accessible assets in the current scope or membership, this value can be zero.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset in the tag.


risk_score	float8	The sum of the risk score of each asset in the tag.


pci_status	text	The overall compliance level ('Pass' or 'Fail') of the tag according to PCI standards. The status is only 'Pass' if all assets in the tag individually have a status of 'Pass'(e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset in the group.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_tag_date_pkey	ON day, tag_id


Foreign Keys


tag_id_fk	(tag_id) references dim_tag (tag_id)


fact_vulnerability


Accumulating snapshot fact for a vulnerability. This convenience fact rolls up assets by the vulnerabilities they are vulnerable to. Each row represents one distinct vulnerability and the results for that vulnerability. If no assets are vulnerable to a vulnerability there will still be a record in this fact table. There will always be one row in this fact table for every vulnerability defined in the dim_vulnerability dimension.


Columns


vulnerability_id	integer	The unique identifier of the vulnerability.


affected_assets	bigint	The total number of assets vulnerable to this vulnerability.


affected_sites	bigint	The total number of sites with at least one asset vulnerable to this vulnerability.


vulnerability_instances	bigint	The total number of instances across all assets of this vulnerability.


Nullable	first_discovered	timestamp	The date at which this vulnerability was first discovered on any asset that is still presently vulnerable to the vulnerability.


Nullable	most_recently_discovered	timestamp	The data at which the vulnerability was most recently discovered on any asset that is currently vulnerable to the vulnerability.


Indexes


fact_vulnerability_pkey	ON vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_vulnerability_date


Periodic snapshot fact for vulnerabilities. This fact table is a date-based snapshot of the fact_vulnerability table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


vulnerability_id	integer	The unique identifier of the vulnerability.


affected_assets	bigint	The total number of assets vulnerable to this vulnerability.


affected_sites	bigint	The total number of sites with at least one asset vulnerable to this vulnerability.


vulnerability_instances	bigint	The total number of instances across all assets of this vulnerability.


Nullable	first_discovered	timestamp	The date at which this vulnerability was first discovered on any asset that is still presently vulnerable to the vulnerability.


Nullable	most_recently_discovered	timestamp	The data at which the vulnerability was most recently discovered on any asset that is currently vulnerable to the vulnerability.


Indexes


fact_vulnerability_date_pkey	ON day, vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


periods


Stores the historical period information indicating when the warehouse was updated during an ETL process. This can be used to determine what dates the warehouse has data for, particularly for trending and temporal-oriented queries.


Columns


day	date	The date an export took place.


Indexes


periods_id	ON day


version


Stores the current version of the schema the warehouse is using. This information is used to perform upgrades over time, and is not a native part of the dimensional model.


Columns


version	integer	The current database schema version of this warehouse.


Functions


age(date, unit)


Function that determines the age, which is the amount of units the specified date is from the current date, and returns a formatted numeric value. The age that is returned is calculated using naive calendar mathematics, and does not take into account variable month duration, daylight savings time, or leap years.


Input


date	TIMESTAMP WITHOUT TIME ZONE	The date to compute the age for.


unit	TEXT	The unit of the computation. One of the following: "years", "months", "weeks", "days", "hours", or "minutes".


Output


NUMERIC	The value of the age, in the unit specified, with a precision based on the input.


htmlToText(html, stripNewlines)


Function that removes the proprietary markup embedded within an HTML-based description so that it can be displayed as raw text within the output of a report. If the text contains embedded HTML markup, it will be unescaped for presentation as HTML/code, including stripping leading and trailing whitespace. The boolean parameter indicates whether newlines and carriage returns should be remove from the HTML (defaults to true).


Input


html	TEXT	The HTML text to convert.


stripNewlines	TEXT	Whether to remove newlines from the output. Defaults to true.


Output


TEXT	Converted text from the HTML input, with all HTML markup removed and replaced with textual equivalents.


periodAfter(date)


Returns the period that occurred on or after the specified date. This returns the last export that occurred in the warehouse after the date specified.


Input


date	DATE	The date to compute the age for.


Output


DATE	The date that the warehouse last had data exported into it on or after the specified date.


periodBefore(date)


Returns the period that occurred on or after the specified date. This returns the last export that occurred in the warehouse after the date specified.


Input


date	DATE	The date to compute the age for.


Output


DATE	The date that the warehouse last had data exported into it on or after the specified date.


Aggregates


baselineComparison(state, currentState)


Custom aggregate function that aggregates over values within two different states, old and new. The state is defined as an identifier that is historical in nature (e.g. scans). The aggregate function is passed two values. The first value is the instance of a state (old or new). The second value is the identifier of the newest state, and will be expected to be the same for each pair within the aggregation. The order in which the state is aggregated has no effect on the result of the aggregation. The result of the aggregate function is a determination of the change (or lack of change) between the two states that are provided. If a value in the old state remains the same as a value in the new state (present in both), then the result is "Same". If a value in old state does not occur within the new state (only present in old), then the result is "Old". If a value in the new state does not occur within the old state (only present in new), then the result is "New". Only one of these three values will be returned. If there are multiple values in either the old or new state with the same identifier, they are considered the same (this aggregate does not perform any counting of occurrences of a value in the state).


Input


state	BIGINT	The input state (either old or new) identifier.


currentState	BIGINT	The current state identifier.


Output


TEXT	A value indicating whether the baseline evaluates to "New", "Old", or "Same".


csv(values)


Custom aggregate function that aggregates multiple text values into a comma-separate value string. This is equivalent to manually aggregating the following way: array_to_string(array_agg(column), ',')


Input


values	TEXT	The column with the textual values to delimit.


Output


TEXT	A comma-separated list of the values for the column specified.


maximumSeverity(severity)


Aggregate function that aggregates multiple severity values to select a single maximum severity value.


Input


severity	TEXT	The severity values to aggregate over.


Output


TEXT	The maximum severity value from those specified. Will be either "Critical", "Severe", "Moderate" or NULL if no values were specified.


minimumSkillLevel(skillLevel)


Aggregate function that aggregates multiple skill level values to select the minimum skill level.


Input


skillLevel	TEXT	The skill level values to aggregate over.


Output


TEXT	The minimum skill level from the values specified. Will be either "Novice", "Intermediate", "Expert", or NULL if no values were specified.