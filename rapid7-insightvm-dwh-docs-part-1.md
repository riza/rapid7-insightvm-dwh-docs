# InsightVM DWH Schema - MCP Documentation


dim_asset


Dimension for the most recent information of all assets. This is a slowly changing dimension that will change as new scans are performed on the asset.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	mac_address	macaddr	The primary or best MAC address associated to the asset in the scan.


Nullable	ip_address	inet	The primary IP address of the asset.


Nullable	host_name	text	The primary host name of the asset. If a host name was used to detect the asset, this name will be preferred.


Nullable	host_type	text	The type of the host, which is one of the values: 'Virtual Machine', 'Hypervisor', 'Bare Metal', or 'Mobile'.


Nullable	os_type	text	The type of the asset which describe the classification by purpose, such as 'Router', 'Switch', 'Workstation', 'General', etc.


Nullable	os_vendor	text	The vendor name of the operating system detected on the asset (e.g. 'Microsoft' or 'Cisco').


Nullable	os_family	text	The family of the operating system detected on the asset (e.g. 'Windows' or 'IOS').


Nullable	os_name	text	The product name of the operating system detected on the asset (e.g. 'Windows' or 'Linux').


Nullable	os_version	text	The version of the operating system detected on the asset (.e.g 'SP1' or '2.2.14').


Nullable	os_architecture	text	The architecture of the operating system detected on the asset (e.g. 'x86').


Nullable	os_description	text	The full description of the operating system detected on the asset, including the vendor, family, name, and version.


Nullable	os_system	text	A shortened form of the operating system description that includes only the vendor, family, and product. This field is best used when grouping by related operating system families and product combination irrespective of specified versions.


Nullable	os_cpe	text	The common platform enumerate (CPE) value, if applicable, associated to the operating system detected on the asset.


risk_modifier	float8	A modifier value that influences the overall risk score of the asset (a multiplier factor on the raw risk score).


assessed_for_vulnerabilities	bool	Indicates whether this asset was assessed for vulnerabilities at least once in its history.


assessed_for_policies	bool	Indicates whether this asset was assessed for policy compliance at least once in its history.


Nullable	credential_status	text	The status of credentials on the asset used most recently. This is an aggregation of all credentials and is one of the following values: 'No credentials supplied', 'All credentials failed', 'Credentials partially successful', or 'All credentials successful'.


Nullable	sites	text	A comma-delimited list of sites the asset is currently a part of (sorted by name ascending). This column can be used a simple and conventient alternative to querying against dim_site_asset to retrieve the individual relationships.


Nullable	last_assessed_for_vulnerabilities	timestamp	The date and time the asset was last assessed for vulnerabilities.


Indexes


dim_asset_pkey	ON asset_id


dim_asset_ip	ON ip_address


dim_asset_mac	ON mac_address


dim_asset_address


Dimension for the network addresses of an asset. Each record represents a pair of IP and MAC that were enumerated on the asset. Since every asset will always have at least one IP address, each asset is guranteed to have on value in this table. Every "primary" address from the dim_asset dimension will be present in this dimension.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	ip_address	inet	The IP address of the asset.


Nullable	mac_address	macaddr	The MAC address of the asset.


primary	bool	Indicates whether this was the primary address of the asset (i.e. the address used to perform a network scan).


Indexes


dim_asset_address_id	ON asset_id


dim_asset_address_ip	ON ip_address


dim_asset_address_mac	ON mac_address


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_container


Dimension for the containers detected on a container host. Each record represents one container. If an asset has no containers or is not a container host, there will be no records for the asset in this dimension.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	container_id	text	The identifier of the container.


Nullable	name	text	The name of the container.


Nullable	status	text	The status of the container (one of 'CREATED', 'RUNNING', 'PAUSED', 'RESTARTING', 'EXITED', 'DEAD', or 'UNKNOWN;')


Nullable	created	timestamp	The date that the container was created.


Nullable	started	timestamp	The date the container last started.


Nullable	finished	timestamp	The date the container last finished running.


Nullable	image_id	text	The identifier of the image the container was based from.


Nullable	digest	text	The digest of the image the container was based on.


Nullable	repository	text	The repository of the image the container was based on.


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_file


Dimension for files and directories that have been enumerated on an asset. Each record represents one file or directory discovered on an asset. If an asset has no files or groups enumerated, there will be no records in this dimension for the asset.


Columns


asset_id	bigint	The unique identifier of the asset.


file_id	bigint	The unique identifier of the file or directory.


Nullable	type	text	The type of the file. Either 'Directory', 'File' or 'Unknown'.


Nullable	name	text	The name of the file or directory.


Nullable	size	bigint	The size of the file or directory in bytes. If the size is unknown, or the file is a directory, the value will be -1.


Indexes


dim_asset_file_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_group


Dimension for all asset groups that any assets within the scope of the report belong to. Each asset group has metadata that define it and can be based off dynamic membership criteria.


Columns


asset_group_id	integer	The unique identifier of the asset group.


name	text	The name of the asset groups which provides a human-readable identifier of the group. Asset groups names are guaranteed to be unique across multiple groups.


Nullable	description	text	A description of the asset group that indicates the content, purpose, or composition of a group.


Nullable	dynamic_membership	bool	Indicates whether the assets belonging to the group are defined statically by an user, or can change automatically based on asset metadata. If true, the membership of the group is dynamically changed whenever scans are performed on assets and the metadata and vulnerabilities related to the asset change. If false, the membership is static and defined manually by a group administrator.


Indexes


dim_asset_group_pkey	ON asset_group_id


dim_asset_group_account


Dimension for user group accounts that have been enumerated on an asset. Each record represents one user group account on an asset. If an asset has no user group accounts enumerated, there will be no records in this dimension for the asset.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	name	text	The name of the group account.


Indexes


dim_asset_group_account_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_group_asset


Dimension for the association between an asset and an asset group. For each asset there will be one record with an association to one asset group. This dimension only provides current associations and does not indicate whether an asset once belonged to a group, but it is no longer.


Columns


asset_group_id	integer	The unique identifier of the asset group.


asset_id	bigint	The unique identifier of the asset.


Indexes


dim_asset_group_asset_pkey	ON asset_group_id, asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


asset_group_id_fk	(asset_group_id) references dim_asset_group (asset_group_id)


dim_asset_group_tag


Dimension that demonstrates the relationship between an asset group and a tag. For each tag applied to an asset group there will be one record in this dimension. If an asset group has not associated tags, no records for that group will apear in this dimension.


Columns


asset_group_id	bigint	The unique identifier of the asset group.


tag_id	integer	The unique identifier of the tag applied to the asset group.


Indexes


dim_asset_group_tag_pkey	ON asset_group_id, tag_id


Foreign Keys


asset_group_id_fk	(asset_group_id) references dim_asset_group (asset_group_id)


tag_id_fk	(tag_id) references dim_tag (tag_id)


dim_asset_host_name


Dimension for the aliases or host names of an asset. Each record represents one of the host names that were discovered during the most recent scan of the asset, including the primary name available within the other asset fact tables. This dimension is built so it includes all aliases found in any node on the asset within the latest scan. If an asset did not have a host name detected in the latest scan, an empty value will be associated with the asset.


Columns


asset_id	bigint	The unique identifier of the asset.


host_name	text	A host name that was detected on the asset.


Nullable	source_type	text	The type of the mechanism used to perform the host name detection (e.g. 'DNS', 'NetBIOS', etc).


Nullable	source	text	The name of the source used to to perform the host name detection (e.g. 'CIFS Name Service', 'uname -n', etc).


Indexes


dim_asset_host_name_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_operating_system


Dimension for the potential operating systems on an asset. Unlike dim_asset, this dimension provides access to all operating system fingerprints that have been detected. If an asset has no operating system fingerprints detected on it, there will be no records for the asset in this dimension.


Columns


asset_id	bigint	he unique identifier of the asset.


Nullable	vendor	text	The vendor name of the operating system (e.g. 'Microsoft' or 'Cisco').


Nullable	family	text	The family of the operating system (e.g. 'Windows' or 'IOS').


Nullable	name	text	The product name of the operating system (e.g. 'Windows' or 'Linux').


Nullable	version	text	The version of the operating system (e.g. 'SP1' or '2.2.14').


Nullable	architecture	text	The architecture of the operating system detected on the asset (e.g. 'x86').


Nullable	description	text	The full description of the operating system, including the vendor, family, name, and version.


Nullable	system	text	A shortened form of the operating system description that includes only the vendor, family, and product. This field is best used when grouping by related operating system families and product combination irrespective of specified versions.


Nullable	cpe	text	The common platform enumerate (CPE) value, if applicable, associated to the operating system.


Nullable	type	text	The type of the operating system which describes the classification by purpose, such as 'Router', 'Switch', 'Workstation', 'General', etc.


Nullable	certainty	real	The certainty of the fingerprint, expressed as a decimal confidence between 0 (low) and 1.0 (high).


Indexes


dim_asset_operating_system_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_service


Dimension for the set of services that have been detected on an asset. Each record represents an open port that is running a service on a protocol. If an asset has no services detected on it, there will be no records for the asset in this dimension.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	service	text	The protocol name of the service (e.g. 'HTTPS', 'RPC', 'NTP', etc).


port	integer	The port the service is running on.


Nullable	protocol	text	The procol the service is exposed through (e.g. 'TCP', or 'UDP').


Nullable	vendor	text	The vendor of the software running on the service (e.g. 'Apache', 'Microsoft', etc).


Nullable	family	text	The family of the software running on the service (e.g. 'Apache', 'IIS', etc).


Nullable	name	text	The name of the software running on the service (e.g. 'HTTPD', 'IIS', etc).


Nullable	version	text	The version of the software running on the service (e.g. '7.5').


Nullable	certainty	real	The certainty of the service fingerprint that detected the service, expressed as a decimal confidence between 0 (low) and 1.0 (high).


Nullable	credential_status	text	The status of the credential(s) used against this service, which is one of the following values: 'No credentials supplied', 'Login failed', 'Login successful', 'Allowed elevation of privileges', 'Root', 'Login as local admin'


Indexes


dim_asset_service_id	ON asset_id


dim_asset_service_service	ON service, port, protocol


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_service_configuration


Dimension for the configurations that have been detected on the services of an asset. Each record represents a configuration value that has been detected on a service. If an asset has no services detected on it, or no configurations were detected, there will be no records for the asset in this dimension.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	service	text	The protocol name of the service (e.g. 'HTTPS', 'RPC', 'NTP', etc).


port	integer	The port the service is running on.


Nullable	protocol	text	The procol the service is exposed through (e.g. 'TCP', or 'UDP').


Nullable	name	text	The name of the configuration value (e.g. 'http.banner', 'ssl.cert.sig.alg.name', etc).


Nullable	value	text	The value of the configuration (e.g. 'Microsoft-IIS/7.5', 'SHA1withRSA', etc).


Indexes


dim_asset_service_configuration_id	ON asset_id


dim_asset_service_configuration_service	ON service, port, protocol


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_software


Dimension for the software that have been detected on an asset. Each record represents a fingerprint result and multiple software instances can be associated with each asset. If an asset had no installed software detected, there will be no records in this dimension.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	vendor	text	The vendor of the software.


Nullable	family	text	The product family of the software.


Nullable	name	text	The product name of the software.


Nullable	version	text	The version of the software.


Nullable	type	text	The type of the software, indicating its purpose or classification (e.g. 'General', 'Virtualization', 'Database Server', 'Security', etc).


Nullable	cpe	text	The common platform enumerate (CPE) value, if applicable, associated to the software.


Indexes


dim_asset_software_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_tag


Dimension for the association between an asset and a tag. For each asset there will be one record with an association to only one tag. This dimension only provides current associations and does not indicate whether an asset once belonged to a tag, but it is no longer.


Columns


tag_id	integer	The unique identifier of the tag.


asset_id	bigint	The unique identifier of the asset.


association	text	The association that the tag has with the asset. It can be a direct association ("tag") or an indirect association through a site ("site"), a group ("group") or the tag dynamic search criteria ("criteria").


Nullable	site_id	bigint	The site id by which an asset indirectly associates with the tag.


Nullable	asset_group_id	bigint	The asset group id by which an asset indirectly associates with the tag.


Indexes


dim_asset_tag_pkey	ON tag_id, asset_id


dim_asset_group_tag_id	ON asset_group_id, tag_id


dim_asset_tag_asset_group	ON asset_group_id


dim_asset_tag_site	ON site_id


Foreign Keys


asset_group_id_fk	(asset_group_id) references dim_asset_group (asset_group_id)


site_id_fk	(site_id) references dim_site (site_id)


asset_id_fk	(asset_id) references dim_asset (asset_id)


tag_id_fk	(tag_id) references dim_tag (tag_id)


dim_asset_unique_id


Dimension for the unique identifiers on an asset. Each record represents a unique identifier enumerated on an asset. Not all assets are guaranteed to have a unique identifier.


Columns


asset_id	bigint	The unique identifier of the asset.


source	text	The source used to discover the unique identifier (e.g. 'WML' or 'system_profiler', etc)


unique_id	text	The unique identifier enumerated on the asset.


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_user_account


Dimension for user accounts that have been enumerated on an asset. Each record represents one user account on an asset. If an asset has no user accounts enumerated, there will be no records in this dimension for the asset.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	name	text	The short, login name associated with the user account. This value may be null, but is typically non-null.


Nullable	full_name	text	The longer name, or description, of the user account.


Indexes


dim_asset_user_account_id	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


dim_asset_validated_vulnerability


Dimension that provides access to validation results on assets. If a validation source is used to validate a vulnerability (e.g. Metasploit) it will have a record in this table detailing which vulnerability was validated, and was exploit module was used to validate.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer


Nullable	exploit_id	integer	The unique identifier of the exploit.


Nullable	module	text	The module used to validate the vulnerability, either 'metasploit' or 'other'


date	timestamp	The date the vulnerability result was validated.


Indexes


dim_asset_validated_vulnerability_asset_id	ON asset_id


dim_asset_validated_vulnerability_asset_id_vulnerability_id	ON asset_id, vulnerability_id


dim_asset_validated_vulnerability_date	ON date


dim_asset_validated_vulnerability_exploit_id	ON exploit_id


dim_asset_validated_vulnerability_vulnerability_id	ON vulnerability_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


exploit_id_fk	(exploit_id) references dim_vulnerability_exploit (exploit_id)


dim_asset_vulnerability_finding_rollup_solution


Dimension that provides access to what "best" solutions can be used to remediate a vulnerability on an asset. The solution(s) presented for an asset and vulnerability will be matched on the metadata/fingerprints of the asset and take supercedence and rollup into account. Despite this, multiple solutions may be selected and presented if a single solution cannot be selected. See dim_asset_vulnerability_finding_solution to gain access to the solutions without rollup applied.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


solution_id	integer	The unique identifier of the rollup solution.


Indexes


dim_asset_vulnerability_finding_rollup_solution_id	ON asset_id, vulnerability_id


dim_asset_vulnerability_finding_rollup_solution_solution_id	ON solution_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


solution_id_fk	(solution_id) references dim_solution (solution_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_asset_vulnerability_finding_solution


Dimension that provides access to what solutions can be used to remediate a vulnerability on an asset. The solution(s) presented for an asset and vulnerability will be matched on the metadata/fingerprints of the asset. If a single solution cannot be selected based on the fingerprints of an asset, multiple solutions may be selected and presented. The solution(s) provided represent only the most direct/immediate solutions associated with the vulnerability. See dim_asset_vulnerability_finding_rollup_solution for similar information, but with rollups and supercedence applied.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


solution_id	integer	The unique identifier of the solution.


Indexes


dim_asset_vulnerability_finding_solution_id	ON asset_id, vulnerability_id


dim_asset_vulnerability_finding_solution_solution_id	ON solution_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


solution_id_fk	(solution_id) references dim_solution (solution_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_policy


Dimension for all metadata related to a policy. It contains one record for every policy that currently exists.


Columns


policy_id	bigint	The unique identifier of a policy.


benchmark_id	bigint	A foreign key to the benchmark the policy is associated with.


policy_name	text	The natural identifier of the policy benchmark source file. Can be used to uniquely identify a policy, in conjunction with columns benchmark_name and benchmark_version from the table policy_benchmark, as an alternative from using the primary key column policy_id.


Nullable	policy_version	text	The version of the policy.


Nullable	title	text	The title of the policy as visible to the user.


Nullable	description	text	A description of the policy.


unscored_rules	integer	The number of rules defined in the policy with a role of "unscored". These rules will not affect rule compliance scoring for the policy.


Indexes


dim_policy_pkey	ON policy_id


Foreign Keys


benchmark_id_fk	(benchmark_id) references dim_policy_benchmark (benchmark_id)


dim_policy_benchmark


Dimension for all metadata related to a policy benchmark. It contains one record for every policy benchmark that currently exists.


Columns


benchmark_id	bigint	The unique identifier of a policy benchmark.


benchmark_name	text	The natural identifier of the policy benchmark source file. Can be used is conjunction with column benchmark_version to uniquely identify a policy benchmark as an alternative from using the primary key column benchmark_id.


benchmark_version	text	The version of the policy benchmark. Can be used is conjunction with column benchmark_name to uniquely identify a policy benchmark as an alternative from using the primary key column benchmark_id.


Nullable	title	text	The title of the policy benchmark.


Nullable	description	text	A description of the policy benchmark.


category	text	A grouping of similar benchmarks based on their source, purpose, or other criteria. Examples include "FDCC", "USGCB", and "CIS". Policy benchmarks created by users have a category of "Custom"


Indexes


dim_policy_benchmark_pkey	ON benchmark_id


dim_policy_group


Dimension for all the metadata for each group of rules within a policy. It contains one record for every unhidden group within each policy.


Columns


group_id	bigint	The identifier of the group.


policy_id	bigint	The identifier of the policy.


Nullable	group_name	text	The name of policy group.


Nullable	title	text	The title of the group, for each policy, that is visible to the user.


Nullable	description	text	A description of the group.


weight	numeric DEFAULT 1.0	The scoring weight of the policy group.


Nullable	parent_group_id	bigint	The identifier of a group in the policy that a group directly belongs to.


sub_groups	integer	The number of groups decending from a group


rules	integer	The number of all rules including rule of a group's sub-groups.


Indexes


dim_policy_group_pkey	ON group_id


dim_policy_group_policy_id	ON policy_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


dim_policy_rule


Dimension for all the metadata for each rule within a policy. It contains one record for every rule within each policy.


Columns


rule_id	bigint	The identifier of the policy rule.


policy_id	bigint	The identifier of the policy.


rule_name	text	The name of the rule from the selected policy.


Nullable	title	text	The title of the rule, for each policy, that is visible to the user. It describes a state or condition with which a tested asset should comply.


Nullable	description	text	A description of the rule.


Nullable	parent_group_id	bigint	The identifier of a group in the policy that a rule directly belongs to.


role	text	The rule's role in scoring and reporting: "full", "unchecked" and "unscored".


Nullable	severity	text	The severity of the rule. A textual value that can be one of the following: "low", "medium", "high", or "unknown".


Nullable	weight	numeric DEFAULT 1.0	The scoring weight of the rule.


Nullable	rationale	text	Descriptive text explaining why compliance is important to the security of the target platform.


Nullable	remediation	text	Instructions for remediating the non-compliant rule.


Nullable	enabled	bool	The boolean to determine whether this rule is enabled.


Indexes


dim_policy_rule_pkey	ON rule_id


dim_policy_rule_policy_id	ON policy_id


Foreign Keys


policy_id_fk	(policy_id) references dim_policy (policy_id)


dim_policy_rule_cce_platform_nist_control_mapping


Dimension that provides all NIST SP 800-53 controls mappings for each CCE within a rule.


Columns


rule_id	bigint	The identifier of the policy rule.


cce_item_id	text	The identifier of the CCE item.


platform	text	The platform of the CCE.


control_name	text	The name of the control mapping.


date_published	date	The date published of the control mapping.


Indexes


dim_policy_rule_cce_platform_nist_control_mapping_rule_id	ON rule_id


Foreign Keys


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


dim_policy_rule_override


Dimension for all the metadata for each policy rule that has been overriden.


Columns


override_id	bigint	The identifier of the policy rule override.


rule_id	bigint	The identifier of the policy rule.


scope	text	The system scope for which the policy rule override applies to.


scope_description	text	The description for the scope of the rule override


submitted_by	text	User name that submitted the policy rule override.


submitted_time	timestamp	Timestamp for submission of the policy rule override.


comment	text	Reporter comment that submitted the policy rule overrdie.


Nullable	reviewed_by	text	Reviewer of the policy rule override.


Nullable	review_comment	text	Reviewer comment for overriding the policy rule.


reviewed_rule_result	text	The original rule status when the override was submitted


Nullable	effective_time	timestamp	Timestamp for the policy rule override to be effective.


Nullable	expiration_time	timestamp	Timestamp for the expiration of the policy rule override.


status	text	Aggregated values for the rule status.


overridden_rule_result	text	The overridden rule status.


Nullable	asset_id	bigint	The identifier of the asset for which the policy rule override applies to.


Indexes


dim_policy_rule_override_pkey	ON override_id


dim_policy_rule_override_asset_id	ON asset_id


dim_policy_rule_override_rule_id	ON rule_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


dim_policy_rule_test


Dimension that provides all tests associated with a policy rule.


Columns


test_key	text	The unique identifier for the policy rule test


rule_id	bigint	The identifier of the policy rule.


test_type	text	The type of the policy rule test.


description	text	The description of what the rule test is checking for.


check_existence	text	The identifier of the type of condition performed in the test. Can be "all_exist", "any_exist", "at_least_one_exists", "none_exist", or "only_one_exists".


Nullable	check_results	text	The rule test check results.


state_operator	text	The logical operator that combines the evaluation results from each referenced state on a per item basis.


Indexes


dim_policy_rule_test_pkey	ON test_key, rule_id


dim_policy_rule_test_test_id	ON rule_id, test_key


Foreign Keys


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


dim_scan


Dimension for all scans that have been run on any sites within access of the report, not only those within the scope. All scans will be made available, regardless of their scan status, including currently running scans.


Columns


scan_id	bigint	The unique identifier of the scan.


Nullable	site_id	integer	The unique identifier of the site.


started	timestamp	The time at which the scan started.


Nullable	finished	timestamp	The time at which the scan ended, which may be NULL if a scan is still in progress.


status	text	The current status of the scan, one of the values: 'Aborted', 'Successful', 'Running', 'Stopped', 'Failed', or 'Paused'.


type	text	The type of the scan, one of the values: 'Manual' or 'Scheduled'.


Nullable	scan_name	text	The user-driven scan name for the scan.


Indexes


dim_scan_pkey	ON scan_id


dim_scan_site	ON site_id


Foreign Keys


site_id_fk	(site_id) references dim_site (site_id)


dim_scan_engine


Dimensions for the scan engines that may be selected to run scans, including standalone engines or pools. One record is present in this dimension for each scan engine that is defined.


Columns


scan_engine_id	integer	The unique identifier of the scan engine.


name	text	The name of the scan engine.


address	text	The address (either IP or host name) of the scan engine.


port	integer	The port the scan engine is running on.


Nullable	type	text	The type of the scan engine, one of the values: 'Standard', 'Pool', 'VMWare EPsec', or 'AWS'


Indexes


dim_scan_engine_pkey	ON scan_engine_id


dim_scan_template


Dimension for all scan templates that are defined. A record is present for each scan template in the system.


Columns


scan_template_id	text	The unique identifier of the scan template.


name	text	The short, human-readable name of the scan template.


Nullable	description	text	The verbose description of the scan template.


Indexes


dim_scan_template_pkey	ON scan_template_id


dim_site


Dimension for all sites. Each site has metadata that define it, including organization information.


Columns


site_id	integer	The unique identifier of the site.


name	text	The name of the site which provides a human-readable identifier of the site. Site names are guaranteed to be unique across multiple sites.


Nullable	description	text	An optional description of the site that indicates the content, purpose, or composition of a site.


Nullable	importance	text	A human-readable description of the importance of site, one of the values: 'Very Low', 'Low', 'Normal', 'High' or 'Very High'


dynamic_targets	bool	Indicates whether the targets defined within the site are dynamically configured based on a discovery connection. If true, a discovery connection is the means by which the targets of a site are defined and dynamically updated. If false, the target definition is static and manually configured by a site administrator.


Nullable	risk_factor	real	An adjustment factor for the risk of a site. The weighting factor defaults to 1.0 and can be adjusted up or down as the importance of a site is changed. The higher the importance, the larger the risk factor, and the lower the importance, the lower the risk factor.


Nullable	last_scan_id	bigint	The identifier of the scan that last ran for the site. If the site has not had a scan run, the value will be NULL.


Nullable	previous_scan_id	bigint	The identifier of the scan that ran prior to the last scan for the site. If the site has not had a scan run, the value will be NULL.


scan_template	text	The name of the scan template the site is currently configured to run scans using.


scan_template_id	text	The identifier of the scan template the site is currently configured to run scans using.


scan_engine	text	The name of the scan engine the site is currently configured to run scans with.


scan_engine_id	integer	The identifier of the scan engine the site is currently configured to run scans with.


Nullable	organization_name	text	The optional name of the organization the site is associated to.


Nullable	organization_url	text	The URL/website of the organization the site is associated to.


Nullable	organization_contact	text	The contact name for the contact of the organization the site is associated to.


Nullable	organization_job_title	text	The job title of the contact of the organization the site isassociated to.


Nullable	organization_email	text	The email address of the contact of the organization the site is associated to.


Nullable	organization_phone	text	The phone number of the organization the site is associated to.


Nullable	organization_address	text	The address of the organization the site is associated to.


Nullable	organization_city	text	The city/region of the organization the site is associated to.


Nullable	organization_state	text	The state/county/province/territory of the organization of the site.


Nullable	organization_country	text	The country of organization the site is associated to.


Nullable	organization_zip	text	The zip-code (if applicable) of the organization the site is associated to.


Indexes


dim_site_pkey	ON site_id


dim_site_last_scan	ON last_scan_id


dim_site_previous_scan	ON previous_scan_id


Foreign Keys


scan_engine_id_fk	(scan_engine_id) references dim_scan_engine (scan_engine_id)


scan_template_id_fk	(scan_template_id) references dim_scan_template (scan_template_id)


dim_site_asset


Dimension for the association between an asset and a site. For each asset there will be one record with an association to only one site.


Columns


site_id	integer	The unique identifier of the site.


asset_id	bigint	The unique identifier of the asset.


Indexes


dim_site_asset_pkey	ON site_id, asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


site_id_fk	(site_id) references dim_site (site_id)


dim_site_target


Dimension for all the included and excluded targets of a site. For all assets in the scope of the report, a record will be present for each unique IP range and/or host name defined as an included or excluded address in the site configuration. If any global exclusions are applied, these will also be provided at the site level.


Columns


site_id	integer	The unique identifier of the site.


type	text	Either 'host' or 'ip' to indicate the type of address.


included	bool	True if the target is included in the configuration, or false if it is excluded.


target	text	If type is 'host', this is the host name.


Nullable	ip_start	inet	If type is 'ip', this is the starting IP address of the range (if there is no range, the ip_start is the IP).


Nullable	ip_end	inet	If type is 'ip', this is the ending IP address of the range (if there is no range, ip_end is the same as the ip_start).


Nullable	scope	text	The scope of an exclusion, either 'global' if the exclusion is a global exclusion, 'site' if the exclusion is defined on the site, or NULL if included is true.


Indexes


dim_site_target_id	ON site_id


Foreign Keys


site_id_fk	(site_id) references dim_site (site_id)


dim_solution


Dimension that provides access to all solutions defined. A solution models the information, steps, and background required to remediate a vulnerability.


Columns


solution_id	integer	The unique identifier of the solution.


nexpose_id	text	The key/identifier of the solution that uniquely identifies it within Nexpose.


Nullable	estimate	interval	The amount of required time estimated to implement this solution on a single asset. This is a heuristic and may not represent the actualy time required to remediate or apply the solution, depending on the environment and tools available for remediation.


Nullable	url	text	An optional URL link defined for getting more information about the solution. When defined, this may be a web page defined by the vendor that provides more details on the solution, or it may be a download link to a patch.


solution_type	text	Type of the solution, one of the values: 'PATCH', 'ROLLUP' or 'WORKAROUND'.


Nullable	fix	text	The steps that are a part of the fix this solution prescribes. The fix will usually contain a list of procedures that must be followed to remediate the vulnerability. The fix is represented using HTML markup that can be "flattened" using the htmlToText() function.


summary	text	A short summary of solution which describes the purpose of the solution at a high level and is suitable for use as a summarization of the solution. The summary is represented using HTML markup that can be "flattened" using the htmlToText() function.


Nullable	additional_data	text	Additional information about the solution. The additional data is represented using HTML markup that can be "flattened" using the htmlToText() function.


Nullable	applies_to	text	Textual representation of the types of system, software, and/or services that the solution can be applied to. If the solution is not restricted to a certain type of system, software or service, this field will be NULL.


Indexes


dim_solution_pkey	ON solution_id


dim_solution_nexpose_id	ON nexpose_id


dim_solution_solution_type	ON solution_type


dim_solution_highest_supercedence


Dimension that provides access to the highest level superceding solution for every solution. If a solution has multiple superceding solutions that themselves are not superceded, all will be returned. Therefore a single solution may have multiple records returned. If a solution is not superceded by any other solution, it will be marked as being superceded by itself.


Columns


solution_id	integer	The unique identifier of the solution.


superceding_solution_id	integer	The identifier of a solution that is known to supercede the solution, and which itself is not superceded (the highest level of supercedence). If the solution is not superceded, this is the same identifier as solution_id.


Indexes


dim_solution_highest_supercedence_pkey	ON solution_id, superceding_solution_id


dim_solution_highest_supercedence_superceding_solution_id	ON superceding_solution_id


Foreign Keys


solution_id_fk	(solution_id) references dim_solution (solution_id)


superceding_solution_id_fk	(superceding_solution_id) references dim_solution (solution_id)


dim_solution_prerequisite


Dimension that provides an association between a solution and all the prerequisite solutions that must be applied before it. If a solution has no prerequisites, it will have no records in this dimension.


Columns


solution_id	integer	The unique identifier of the solution.


required_solution_id	integer	The unique identifier of the prerequisite solution.


Indexes


dim_solution_prerequisite_pkey	ON solution_id, required_solution_id


Foreign Keys


required_solution_id_fk	(required_solution_id) references dim_solution (solution_id)


solution_id_fk	(solution_id) references dim_solution (solution_id)


dim_solution_supercedence


Dimension that provides all superceding associations between solutions. Unlike dim_solution_highest_supercedence, this dimension provides access to the entire graph of superceding relationships. If a solution does not supercede any other solution, it will not have any records in this dimension.


Columns


solution_id	integer	The unique identifier of the solution.


superceding_solution_id	integer	The unique identifier of the superceding solution.


Indexes


dim_solution_supercedence_pkey	ON solution_id, superceding_solution_id


dim_solution_supercedence_superceding_solution_id	ON superceding_solution_id


Foreign Keys


solution_id_fk	(solution_id) references dim_solution (solution_id)


superceding_solution_id_fk	(superceding_solution_id) references dim_solution (solution_id)


dim_tag


Dimension for all tags that any assets within the scope of the report belong to. Each tag has either a direct association or indirection association to an asset based off site or asset group association or of dynamic membership criteria.


Columns


tag_id	integer	The unique identifier of the tag.


name	text	The name of the tags. Tag names are unique for tags of the same type.


type	text	The type of the tag, one of the following values: 'CRITICALITY', 'LOCATION', 'OWNER', or 'CUSTOM'.


Nullable	source	text	The original application that created the tag.


created	timestamp	The creation time.


Nullable	risk_modifier	float8	The risk modifier for a CRITICALITY typed tag.


Nullable	color	text	The optional color of the tag, in hexadecimal notation.


Indexes


dim_tag_pkey	ON tag_id


dim_tag_type	ON type


dim_vulnerability


Dimension for a vulnerability and its associated metadata, including risk scores, CVSS vector, and title. One record is present for each vulnerability that is defined.


Columns


vulnerability_id	integer	The unique identifier of the vulnerability.


nexpose_id	text	The Nexpose identifier (natural key) of the vulnerability.


title	text	A short, human-readable description of the vulnerability. The title is represented using HTML markup that can be "flattened" using the htmlToText() function.


description	text	A verbose description for the vulnerability. The description is represented using HTML markup that can be "flattened" using the htmlToText() function.


date_published	date	The date that the vulnerability was publicized by the third-party, vendor, or another authoring source. The granularity of the date is a day.


date_added	date	The date that the vulnerability was first checked by Nexpose. The granularity of the date is a day.


date_modified	date	The date that the vulnerability was last modified. The granularity of the date is a day.


severity_score	smallint	The numerical severity of the vulnerability, measured on a scale of 0 to 10 using whole numbers.


severity	text	The textual representation of the severity of the vulnerability, which is based on the severity score. The severity can be any of the following values: 'Critical', 'Severe', or 'Moderate'


critical	integer	Numerical representation of the severity of the vulnerability that can be used for aggregation purposes easily use a SUM aggregate. If the severity is 'Critical' the value of this column is 1, otherwise it is 0.


severe	integer	Numerical representation of the severity of the vulnerability that can be used for aggregation purposes easily use a SUM aggregate. If the severity is 'Severe' the value of this column is 1, otherwise it is 0.


moderate	integer	Numerical representation of the severity of the vulnerability that can be used for aggregation purposes easily use a SUM aggregate. If the severity is 'Moderate' the value of this column is 1, otherwise it is 0.


pci_severity_score	smallint	The numerical PCI severity score of the vulnerability, measured on a scale of 1 to 5 using whole numbers.


pci_status	text	The compliance level of the vulnerability according to PCI standards. 'Pass' indicates the vulnerability may be present on an asset but still pass PCI compliance. 'Fail' indicates the vulnerability must not be present in order to pass PCI compliance.


pci_failures	integer	Numerical representation of the pci_status that can be used for aggregation purposes easily using a SUM aggregate. The value is 0 if pci_status is 'Pass', and 1 if pci_status is 'Fail'.


risk_score	float8	The risk score of the vulnerability as computed by the current risk strategy/model.


cvss_vector	text	The full CVSS vector in CVSS Version 2.0 notation.


cvss_access_vector	varchar( 1 )	Access vector (AV) code that represents the CVSS access vector value of the vulnerability.


cvss_access_complexity	varchar( 1 )	Access complexity (AC) vector code that represents the CVSS access complexity vector value of the vulnerability.


cvss_authentication	varchar( 1 )	Authentication (Au) vector code that represents the CVSS authentication vector value of the vulnerability.


cvss_confidentiality_impact	varchar( 1 )	Confidentiality impact (C) vector code that represents the CVSS confidentiality impact vector value of the vulnerability.


cvss_integrity_impact	varchar( 1 )	Integrity impact (I) vector code that represents the CVSS integrity impact vector value of the vulnerability.


cvss_availability_impact	varchar( 1 )	Availability impact (A) vector code that represents the CVSS availability impact vector value of the vulnerability.


cvss_score	real	Value between 0 and 10 representing the CVSS score of the vulnerability.


pci_adjusted_cvss_score	real	Value between 0 and 10 representing the CVSS score of the vulnerability, adjusted if necessary to follow PCI rules.


cvss_exploit_score	real	Base score for the exploitability of a vulnerability that is used to compute the overall CVSS score.


cvss_impact_score	real	Base score for the impact of a vulnerability that is used to compute the overall CVSS score.


Nullable	pci_special_notes	text	Notes attached to the vulnerability following PCI rules.


denial_of_service	bool	Signifies whether the vulnerability is classified as a denial-of-service vulnerability.


exploits	bigint	The total number of distinct exploits/modules that are known to exploit the vulnerability.


exploit	integer	Numeric representation as to whether this vulnerability is exploitable. If exploits is greater than 0, this value will be 1, otherwise 0.


Nullable	exploit_skill_level	text	The minimum exploitability skill level required to exploit this vulnerability (if an exploit is known for it), one of the values 'Expert', 'Novice', 'Intermediate', or NULL.


malware_kits	bigint	The total number of distinct malware kits that are known to exploit the vulnerability.


malware_kit	integer	Numeric representation as to whether this vulnerability has a known malware kit. If mwlare_kits is greater than 0, this value will be 1, otherwise 0.


Nullable	malware_popularity	text	The maximum popularity value of the malware kits on this vulnerability (if a malware kit is known for it), one of the values: 'Uncommon', 'Occasional', 'Rare', 'Common', 'Favored', 'Popular', or 'Unknown'.


Nullable	cvss_v3_vector	text	The full CVSS vector in CVSS Version 3.0 notation.


Nullable	cvss_v3_attack_vector	varchar( 1 )	Attack Vector (AV) code that represents the CVSS attack vector value of the vulnerability.


Nullable	cvss_v3_attack_complexity	varchar( 1 )	Attack Complexity (AC) code that represents the CVSS attack complexity value of the vulnerability.


Nullable	cvss_v3_privileges_required	varchar( 1 )	Privileges Required (PR) code that represents the CVSS privilege required value of the vulnerability.


Nullable	cvss_v3_user_interaction	varchar( 1 )	User Interaction (UI) code that represents the CVSS user interaction value of the vulnerability.


Nullable	cvss_v3_scope	varchar( 1 )	Scope (S) code that represents the CVSS scope value of the vulnerability.


Nullable	cvss_v3_confidentiality_impact	varchar( 1 )	Confidentiality Impact (C) code that represents the CVSS confidentiality impact value of the vulnerability.


Nullable	cvss_v3_integrity_impact	varchar( 1 )	Integrity Impact (I) code that represents the CVSS integrity impact value of the vulnerability.


Nullable	cvss_v3_availability_impact	varchar( 1 )	Availability Impact (A) code that represents the CVSS availability impact value of the vulnerability.


Nullable	cvss_v3_score	real	Value between 0 and 10 representing the CVSS Version 3.0 score of the Vulnerability.


Nullable	cvss_v3_impact_score	real	Base score for the impact of a vulnerability that is used to compute the overall CVSS Version 3.0 score.


Nullable	cvss_v3_exploit_score	real	Base score for the exploitability of a vulnerability that is used to compute the overall CVSS Version 3.0 score.


Indexes


dim_vulnerability_pkey	ON vulnerability_id


dim_vulnerability_nexpose_id	ON nexpose_id


dim_vulnerability_severity	ON severity


dim_vulnerability_category


Dimension for categories that defines groups of related vulnerabilities by a common name. Each record represents a vulnerability and the associated category it belongs to. Each vulnerability can belong to multiple categories, in which case multiple records will be present, one for each category the vulnerability belongs to.


Columns


vulnerability_id	integer	The unique identifier of the vulnerability.


category_name	text	The human-readable name of the vulnerability category.


Indexes


dim_vulnerability_category_pkey	ON vulnerability_id, category_name


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_vulnerability_exception


Dimension for all vulnerability exceptions that are currently applied to or are pending approval to apply to any assets. This fact includes exceptions that are pending approval, those that are actively applying, and even expired exceptions.


Columns


vulnerability_exception_id	integer	The unique identifier of the vulnerability exception.


vulnerability_id	integer	The unique identifier of the vulnerability.


scope	text	The scope of the vulnerability exception, one of the values: 'Global', 'Site', 'Asset' or 'Instance'.


Nullable	scope_description	text	The description of the scope of the vulnerability exception, one of the values: 'All instances (all assets)', 'All instances in this site', 'All instances on this asset', or 'Specific instance on this asset'


reason	text	The reason the vulnerability exception was requested or applied.


Nullable	additional_comments	text	Comments field populated when a state transition, such as rejection or submission, occurs. This is a user-populated field that is optional.


Nullable	submitted_date	timestamp	The date that the exception was last submitted, or resubmitted for approval. If the exception has been rejected or recalled and is resubmitted, only the date of the last state transition is used.


Nullable	submitted_by	text	The login name of the user that submitted the vulnerability exception for approval.


Nullable	review_date	timestamp	The date when the last review of the exception request was performed. This can either be the date when the exception was last approved, or last rejected. If the exception is approved, rejected, or recalled multiple times, this is the date of the last state transition. If a review is pending, this value may be NULL.


Nullable	reviewed_by	text	The login name of the user that reviewed the vulnerability exception for approval and either approved or rejected it. If the exception is still waiting for approval, this value is NULL.


Nullable	review_comment	text	The last comment when the exception was reviewed, and either approved or rejected. If a review has yet to occur, this can be null.


Nullable	expiration_date	date	The date at which the expiration of the exception occurs. The expiration date is interpreted as midnight on the date specified. The timestamp is converted into the timezone specified within the report configuration.


Nullable	status	text	The status of the exception, one of the values: 'Under review', 'Approved', 'Rejected', 'Recalled', or 'Expired'.


Nullable	site_id	integer	If the scope is 'Site', the id of the site the exception applies to. For all other scopes, the value is NULL.


Nullable	asset_id	bigint	If the scope is 'Asset' or 'Instance' the id of the asset the exception applies to. For all other scopes, the value is NULL.


Nullable	port	integer	If the scope is 'Instance' and the exception is applying to a service, the port of the service that exception is applied to. For all other scopes, the value is NULL.


Nullable	key	text	If the scope is 'Instance' and the exception is applied to a vulnerability with a secondary key, the key of the vulnerability the exception applies to. For all other scopes, the value is NULL.


Nullable	group_id	integer	If the scope is 'Asset Group', the id of the group the exception applies to. For all other scopes, the value is NULL.


Indexes


dim_vulnerability_exception_pkey	ON vulnerability_exception_id


dim_vulnerability_exception_asset_id	ON asset_id


dim_vulnerability_exception_group_id	ON group_id


dim_vulnerability_exception_scope	ON scope


dim_vulnerability_exception_site_id	ON site_id


dim_vulnerability_exception_vulnerability_id	ON vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_vulnerability_exploit


Exploits that exploit a particular vulnerability that have been defined by external exploit data sources. Each record represents the relationship between a vulnerability and one exploit module/kit/package known to exploit that vulnerability. Each vulnerability can be associated to multiple exploits.


Columns


exploit_id	integer	The unique identifier of the exploit module.


vulnerability_id	integer	The unique identifier of the vulnerability the exploit relates to.


title	text	The short name or title of the exploit which describes the name, purpose, or target of the exploit.


Nullable	description	text	The description of the exploit that provides more detailed information on the purpose or target of the exploit.


Nullable	skill_level	text	The skill level required to perform the exploit, one of the values: 'Expert', 'Novice', or 'Intermediate'.


Nullable	source	text	The source which defined and published the exploit, one of the values: 'Exploit Database' or 'Metasploit Module'.


Nullable	source_key	text	The identifier of the exploit within the source that published the exploit. This can be an internal identifier key for the exploit within the source.


Indexes


dim_vulnerability_exploit_pkey	ON exploit_id, vulnerability_id


dim_vulnerability_exploit_vulnerability_id	ON vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_vulnerability_malware_kit


Dimension for malware kits that are known to exploit a vulnerability. Each record represents the relationship between a vulnerability and an associated malware kit known to exploit that vulnerability. Each vulnerability can be associated to multiple malware kits.


Columns


malware_kit_id	integer	The unique identifier of the malware kit.


vulnerability_id	integer	The unique identifier of the vulnerability with a malware kit


name	text	The name of the malware kit.


Nullable	popularity	text	The popularity of the malware kit, which identifies how common or accessible it is. Popularity can have the following values: 'Uncommon', 'Occasional', 'Rare', 'Common', 'Favored', 'Popular', or 'Unknown'.


Indexes


dim_vulnerability_malware_kit_pkey	ON malware_kit_id, vulnerability_id


dim_vulnerability_malware_kit_vulnerability_id	ON vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_vulnerability_reference


Dimension for references to external data source(s) that relate to, define, or that the publishing source of a vulnerability. Each record represents the relationship between a vulnerability and an external reference or link to a defining source. Each vulnerability may be associated to multiple references.


Columns


vulnerability_id	integer	The unique identifier of the vulnerability.


source	text	The name of a source of vulnerability information or metadata. The source is provided in all upper-case characters (for consistency with the user interface).


reference	text	The reference that keys or links into the source repository. If the source is 'URL', the reference is a URL. For other data sources such as CVE, BID, or SECUNIA, the reference is typically a key that indexes into those repositories.


Indexes


dim_vulnerability_referenceserence_vulnerability_id


Foreign Keys


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


dim_vulnerability_solution


Dimension that provides access to the relationship between a vulnerability and its (direct) solutions. These solutions are only those which are directly known to remediate the vulnerability, and do not include rollups or superceding solutions. If a vulnerability has more than one solution (e.g. for multiple platforms), multiple association records will be present. If a vulnerability has no known solutions, it will have no records in this dimension.


Columns


vulnerability_id	integer	The unique identifier of the vulnerability.


solution_id	integer	The unique identifier of the solution.


Indexes


dim_vulnerability_solution_pkey	ON vulnerability_id, solution_id


Foreign Keys


solution_id_fk	(solution_id) references dim_solution (solution_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_all


Accumulating snapsht fact for all assets. This convenience rollup fact aggregates across all defined assets. This fact table is guaranteed to have one and only one record at all times, even if no assets are defined.


Columns


assets	bigint	The total number of assets.


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


fact_all_date


Periodic snapshot fact for all data. This fact table is a date-based snapshot of the fact_all table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


assets	bigint	The total number of assets.


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


fact_all_date_pkey	ON day


fact_asset


Accumulating snapshot fact table for the latest state of an asset. Each fact record represents the current summary information for an asset, from all data source across all sites the asset belongs to.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerabilities	bigint	The number of vulnerability findings on the asset. This value is equal to the sum of critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a critical severity.


severe_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a severe severity.


moderate_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a moderate severity.


malware_kits	integer	The number of distinct malware kits that can exploit any vulnerabilities on the asset.


exploits	integer	The number of distinct exploit modules that can exploit any vulnerabilities on the asset.


vulnerabilities_with_malware_kit	integer	The number of vulnerabilities on the asset that have at least one malware kit.


vulnerabilities_with_exploit	integer	The number of vulnerabilities on the asset that have at least one exploit module.


vulnerability_instances	bigint	The total number of instances of all vulnerabilities.


raw_risk_score	float8	The risk score of the asset across all vulnerabilities but with no risk factor applied.


risk_score	float8	The risk score of the asset across all vulnerabilities with any applicable risk factor applied.


pci_status	text	The compliance level, either 'Pass' or 'Fail', of the asset according to PCI standards.


pci_failures	bigint	Numerical representation of the pci_status that can be used for aggregation. If pci_status is 'Pass' the value is 0, and if 'Fail' the value is 1.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_asset_pkey	ON asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


fact_asset_date


Periodic snapshot fact for assets. This fact table is a date-based snapshot of the fact_asset table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


asset_id	bigint	The unique identifier of the asset.


vulnerabilities	bigint	The number of vulnerability findings on the asset. This value is equal to the sum of critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a critical severity.


severe_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a severe severity.


moderate_vulnerabilities	bigint	The number of vulnerability findings for vulnerabilities with a moderate severity.


malware_kits	integer	The number of distinct malware kits that can exploit any vulnerabilities on the asset.


exploits	integer	The number of distinct exploit modules that can exploit any vulnerabilities on the asset.


vulnerabilities_with_malware_kit	integer	The number of vulnerabilities on the asset that have at least one malware kit.


vulnerabilities_with_exploit	integer	The number of vulnerabilities on the asset that have at least one exploit module.


vulnerability_instances	bigint	The total number of instances of all vulnerabilities.


raw_risk_score	float8	The risk score of the asset across all vulnerabilities but with no risk factor applied.


risk_score	float8	The risk score of the asset across all vulnerabilities with any applicable risk factor applied.


pci_status	text	The compliance level, either 'Pass' or 'Fail', of the asset according to PCI standards.


pci_failures	bigint	Numerical representation of the pci_status that can be used for aggregation. If pci_status is 'Pass' the value is 0, and if 'Fail' the value is 1.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_asset_date_pkey	ON day, asset_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


fact_asset_event


Transactional fact for every event that has occurred on an asset that may have changed the data on the asset. These events includes scans, data import, applying exceptions, etc.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	event_id	bigint	The unique identifier of the event that the asset was modified by, which can be shared across multiple assets that were a part of the same event.


date	timestamp	The date in which the event was performed or executed.


type	text	The type of the event, which is one of the following values: 'SCAN' - the asset was scanned by a scan engine 'ACTIVE-SYNC' - the asset was scanned/discovered through an active sync connection 'VULNERABILITY_EXCEPTION_APPLIED' - a vulnerability exception was applied to a vulnerability 'VULNERABILITY_EXCEPTION_UNAPPLIED' - a vulnerability exception was unapplied (removed) from a vulnerability 'ASSET-IMPORT' or 'EXTERNAL-IMPORT' - the asset was imported through the external API 'EXTERNAL-IMPORT-APPSPIDER' - the asset was imported from an AppSpider scan 'SCAN-LOG-IMPORT' - the asset was import using a console command 'SCAN-LOG-INGESTOR-UPGRADE' - the asset was imported during a one-time product upgrade (deprecated)


Nullable	scan_id	bigint	If the type is 'SCAN', 'ACTIVE-SYNC', 'SCAN-LOG-IMPORT', 'SCAN-LOG-INGESTOR-UPGRADE', or 'EXTERNAL-IMPORT-APPSPIDER' the identifier of the scan that was run. For all other types the value is NULL.


Nullable	vulnerability_exception_id	integer	If the type is 'VULNERABILITY_EXCEPTION_APPLIED' or 'VULNERABILITY_EXCEPTION_UNAPPLIED' the identifier of the vulnerability exception that was applied or unapplied.


Nullable	user_name	text	If the type is 'EXTERNAL-IMPORT' or 'ASSET-IMPORT' the login name of the user that performed the import.


Nullable	description	text	A description of the event that was performed. This can include details specific to the event type.


Indexes


fact_asset_event_date	ON date


fact_asset_event_exception	ON vulnerability_exception_id


fact_asset_event_id	ON asset_id, event_id


fact_asset_event_scan	ON scan_id


fact_asset_event_type	ON type


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


scan_id_fk	(scan_id) references dim_scan (scan_id)


vulnerability_exception_id_fk	(vulnerability_exception_id) references dim_vulnerability_exception (vulnerability_exception_id)


fact_asset_group


Accumulating snapshot fact for the summary information of an asset group. This is a convenience fact for rolling up the information for assets within the membership of one or more asset groups. The summary information provided is based on the most recent data for each asset in the membership of the group. If an asset group has no assets, there will be a fact record with zero counts.


Columns


asset_group_id	integer	The unique identifier of the asset group.


Nullable	assets	bigint	The total number of assets that are in the scope of associated to the group. If the group has no assets in the current scope or membership, this value can be zero.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset in the group.


risk_score	float8	The sum of the risk score of each asset in the group.


pci_status	text	The overall compliance level ('Pass' or 'Fail') of the asset group according to PCI standards. The status is only 'Pass' if all assets in the group individually have a status of 'Pass' (e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset in the group.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_asset_group_pkey	ON asset_group_id


Foreign Keys


asset_group_id_fk	(asset_group_id) references dim_asset_group (asset_group_id)


fact_asset_group_date


Periodic snapshot fact for asset groups. This fact table is a date-based snapshot of the fact_asset_group table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


asset_group_id	integer	The unique identifier of the asset group.


Nullable	assets	bigint	The total number of assets that are in the scope of associated to the group. If the group has no assets in the current scope or membership, this value can be zero.


vulnerabilities	bigint	The sum of the count of vulnerabilities on each asset. This value is equal to the sum of the critical_vulnerabilities, severe_vulnerabilities, and moderate_vulnerabilities columns.


critical_vulnerabilities	bigint	The sum of the count of critical vulnerabilities on each asset.


severe_vulnerabilities	bigint	The sum of the count of severe vulnerabilities on each asset.


moderate_vulnerabilities	bigint	The sum of the count of moderate vulnerabilities on each asset.


malware_kits	integer	The sum of the count of malware kits on each asset.


exploits	integer	The sum of the count of exploits on each asset.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset.


vulnerability_instances	bigint	The sum of the vulnerabilities instances on each asset.


raw_risk_score	float8	The sum of the raw risk score of each asset in the group.


risk_score	float8	The sum of the risk score of each asset in the group.


pci_status	text	The overall compliance level ('Pass' or 'Fail') of the asset group according to PCI standards. The status is only 'Pass' if all assets in the group individually have a status of 'Pass' (e.g. in fact_asset)


pci_failures	bigint	The sum of the total PCI failures on each asset in the group.


validated_vulnerabilities	bigint DEFAULT 0	The number of vulnerabilities that have been validated.


Indexes


fact_asset_group_date_pkey	ON day, asset_group_id


Foreign Keys


asset_group_id_fk	(asset_group_id) references dim_asset_group (asset_group_id)


fact_asset_policy


Accumulating snapshot fact for all current tested policies on an asset. This fact is a convenience rollup for the fact_asset_policy_rule fact and provides a record for each tested policy on every asset. If an asset was not applicable to any rules in the policy, it will have no records in this fact table.


Columns


asset_id	bigint	The unique identifier of the asset.


policy_id	bigint	The unique identifier of the policy.


scan_id	bigint	The unique identifier of the scan.


date_tested	timestamp	The time at which the policy was tested against the asset.


compliant_rules	integer	The total number of rules for which the asset passed in the most recent scan for this policy.


noncompliant_rules	integer	The total number of rules for which the asset failed in the most recent scan for this policy


not_applicable_rules	integer	The total number of rules that were not applicable to the asset in the most recent scan for this policy.


rule_compliance	float8	The ratio of passing results for the rules to the total number of scorable rules for this policy.


Indexes


fact_asset_policy_pkey	ON asset_id, policy_id


fact_asset_policy_asset_id	ON asset_id


fact_asset_policy_policy_id	ON policy_id


fact_asset_policy_scan_id	ON scan_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


policy_id_fk	(policy_id) references dim_policy (policy_id)


scan_id_fk	(scan_id) references dim_scan (scan_id)


fact_asset_policy_date


Periodic snapshot fact for asset policy records. This fact table is a date-based snapshot of the fact_asset_policy table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


asset_id	bigint	The unique identifier of the asset.


policy_id	bigint	The unique identifier of the policy.


scan_id	bigint	The unique identifier of the scan.


date_tested	timestamp	The time at which the policy was tested against the asset.


compliant_rules	integer	The total number of rules for which the asset passed in the most recent scan for this policy.


noncompliant_rules	integer	The total number of rules for which the asset failed in the most recent scan for this policy


not_applicable_rules	integer	The total number of rules that were not applicable to the asset in the most recent scan for this policy.


rule_compliance	float8	The ratio of passing results for the rules to the total number of scorable rules for this policy.


Indexes


fact_asset_policy_date_pkey	ON day, asset_id, policy_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


policy_id_fk	(policy_id) references dim_policy (policy_id)


scan_id_fk	(scan_id) references dim_scan (scan_id)


fact_asset_policy_rule


Accumulating snapshot of policy rules results on an asset. This fact provides a record for each policy rule that was tested on an asset in its most recent scan.


Columns


asset_id	bigint	The unique identifier of the asset.


rule_id	bigint	The unique identifier of the policy rule.


policy_id	bigint	The unique identifier of the policy.


scan_id	bigint	The unique identifier of the scan.


Nullable	override_id	bigint	The identifier of a policy override effectively overriding a rule test result on the asset. If there are more than one such overrides, the last submitted one will take precedent over the rest.


Nullable	override_ids	_int8	The array identifiers of policy overrides potentially overriding a rule test result on an asset.


date_tested	timestamp	The time at which the policy rule was tested against the asset.


status	text	The rule compliance status on an asset.


Indexes


fact_asset_policy_rule_pkey	ON asset_id, rule_id


fact_asset_policy_rule_asset_id	ON asset_id


fact_asset_policy_rule_override_id	ON override_id


fact_asset_policy_rule_policy_id	ON policy_id


fact_asset_policy_rule_rule_id	ON rule_id


fact_asset_policy_rule_scan_id	ON scan_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


policy_id_fk	(policy_id) references dim_policy (policy_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


override_id_fk	(override_id) references dim_policy_rule_override (override_id)


scan_id_fk	(scan_id) references dim_scan (scan_id)


fact_asset_policy_rule_check


Accumulating snapshot of policy rule check results on an asset. This fact provides a record for each policy rule check that was tested on an asset in its most recent scan.


Columns


result_id	bigint	The unique identifier of the rule check result.


asset_id	bigint	The unique identifier of the asset.


policy_id	bigint	The unique identifier of the policy.


rule_id	bigint	The unique identifier of the policy rule.


scan_id	bigint	The unique identifier of the scan.


Nullable	override_id	bigint	The identifier of a policy override effectively overriding a rule test result on the asset. If there are more than one such overrides, the last submitted one will take precedent over the rest.


Nullable	override_ids	_int8	The array identifiers of policy overrides potentially overriding a rule test result on an asset.


date_tested	timestamp	The time at which the policy rule check was tested against the asset.


check_result	text	The rule check result on an asset.


Nullable	proof	text	The proof gathered during the evaluation of the rule check on the asset.


Indexes


fact_asset_policy_rule_check_pkey	ON result_id


fact_asset_policy_rule_check_asset_id	ON asset_id


fact_asset_policy_rule_check_override_id	ON override_id


fact_asset_policy_rule_check_policy_id	ON policy_id


fact_asset_policy_rule_check_rule_id	ON rule_id


fact_asset_policy_rule_check_scan_id	ON scan_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


policy_id_fk	(policy_id) references dim_policy (policy_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


override_id_fk	(override_id) references dim_policy_rule_override (override_id)


scan_id_fk	(scan_id) references dim_scan (scan_id)


fact_asset_policy_rule_test


Accumulating snapshot of policy rule test results on an asset. These results include each system configuration entity tested.


Columns


test_key	text	The unique identifier of the policy rule test.


rule_id	bigint	The unique identifier of the rule.


asset_id	bigint	The unique identifier of the asset.


test_result	text	The result of the policy rule test. Possible values are 'true', 'false', 'unknown', 'error', 'not_evaluated', and 'not_applicable'


entity_name	text	The name of the object or state entity that was tested.


entity_operation	text	The operation applied to the entity to determine a result.


Nullable	entity_value	text	The expected value we are testing the entity for.


Nullable	object_key	text	The unique identifier of the policy rule test object.


Nullable	object_collection_flag	text	A flag indicating the collection status of the policy rule test object.


Nullable	state_key	text	The unique identifier of the policy rule test state.


Nullable	collected_entity_value	text	The collected value for the entity.


Nullable	location	text	The location of the system configuration entity tested on the asset.


Nullable	test_result_id	bigint	The unique identifier of a policy rule test result. Multiple rows in this table can be associated with a single policy rule test result.


Indexes


fact_asset_policy_rule_test_pkey	ON test_key, rule_id, asset_id


fact_asset_policy_rule_test_id	ON asset_id, rule_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


rule_id_fk	(rule_id) references dim_policy_rule (rule_id)


test_key_rule_id_fk	(test_key, rule_id) references dim_policy_rule_test (test_key, rule_id)


fact_asset_vulnerability_finding


Accumulating snapshot fact for all current vulnerability findings on an asset. This fact is a convenience rollup for the fact_asset_vulnerability_instance fact and provides a record for each vulnerability finding on every asset. If an asset was not vulnerable to any vulnerabilities (or all instances are excluded), it will have no records in this fact table. If multiple instances of a vulnerability are found on the same asset they will be aggregated together in the instances count. This fact table should be the preferred level of grain when instance-level details (such as the port and proof) are not required. To access exploitability information for the finding refer to fact_asset_vulnerability_finding_exploit.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


date	timestamp	The time at which the vulnerability was first found on the asset. This is the earliest date any instance on the asset was found.


Nullable	reintroduced_date	timestamp	The date on which the vulnerability was reintroduced on the asset following a previous remediation.


vulnerability_instances	bigint	The number of instances of this finding on the asset.


Nullable	critical_vulnerabilities	bigint	The number of critical vulnerabilities this finding represents. Either 1 if the vulnerablity finding is critical, 0 otherwise.


Nullable	severe_vulnerabilities	bigint	The number of severe vulnerabilities this finding represents. Either 1 if the vulnerablity finding is severe, 0 otherwise.


Nullable	moderate_vulnerabilities	bigint	The number of moderate vulnerabilities this finding represents. Either 1 if the vulnerablity finding is moderate, 0 otherwise.


Nullable	malware_kits	integer	The the count of malware kits associated to the vulnerability.


Nullable	exploits	integer	The the count of exploits associated to the vulnerability.


Nullable	vulnerabilities_with_malware_kit	integer	The number of vulnerabilities this finding represents that have malware kits. Either 1 if the vulnerablity finding has malware_kits, 0 otherwise.


Nullable	vulnerabilities_with_exploit	integer	The number of vulnerabilities this finding represents that have exploits. Either 1 if the vulnerablity finding has exploits, 0 otherwise.


Nullable	raw_risk_score	float8	The raw risk score for the vulnerability of this finding.


Nullable	risk_score	float8	The risk score for the vulnerability of this finding.


Nullable	pci_failures	bigint	The number of PCI failures for the vulnerability. Either 1 if the vulnerablity finding is would caused a PCI failure, 0 otherwise.


validated	bool DEFAULT false	Whether the vulnerability has been validated (e.g. using Metasploit).


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_finding_date


Periodic snapshot fact for vulnerability findings on an asset. This fact table is a date-based snapshot of the fact_asset_vulnerability_finding table. During each export process, the current data is appended to this fact table.


Columns


day	date	The date the snapshot was recorded.


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


date	timestamp	The time at which the vulnerability was first found on the asset. This is the earliest date any instance on the asset was found.


Nullable	reintroduced_date	timestamp	The date on which the vulnerability was reintroduced on the asset following a previous remediation.


Nullable	critical_vulnerabilities	bigint	The number of critical vulnerabilities this finding represents. Either 1 if the vulnerablity finding is critical, 0 otherwise.


Nullable	severe_vulnerabilities	bigint	The number of severe vulnerabilities this finding represents. Either 1 if the vulnerablity finding is severe, 0 otherwise.


Nullable	moderate_vulnerabilities	bigint	The number of moderate vulnerabilities this finding represents. Either 1 if the vulnerablity finding is moderate, 0 otherwise.


Nullable	malware_kits	integer	The the count of malware kits associated to the vulnerability.


Nullable	exploits	integer	The the count of exploits associated to the vulnerability.


Nullable	vulnerabilities_with_malware_kit	integer	The number of vulnerabilities this finding represents that have malware kits. Either 1 if the vulnerablity finding has malware_kits, 0 otherwise.


Nullable	vulnerabilities_with_exploit	integer	The number of vulnerabilities this finding represents that have exploits. Either 1 if the vulnerablity finding has exploits, 0 otherwise.


vulnerability_instances	bigint	The number of instances of this finding on the asset.


Nullable	raw_risk_score	float8	The raw risk score for the vulnerability of this finding.


Nullable	risk_score	float8	The risk score for the vulnerability of this finding.


Nullable	pci_failures	bigint	The number of PCI failures for the vulnerability. Either 1 if the vulnerablity finding is would caused a PCI failure, 0 otherwise.


Nullable	validated	bool DEFAULT false	Whether the vulnerability has been validated (e.g. using Metasploit).


Indexes


fact_asset_vulnerability_finding_date_pkey	ON day, asset_id


fact_asset_vulnerability_finding_date_id	ON day, asset_id, vulnerability_id


fact_asset_vulnerability_finding_date_vuln_id	ON vulnerability_id


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_finding_exploit


Accumulating snapshot fact for all current vulnerability findings on an asset that are known to be exploitable. This fact is a convenience rollup for the fact_asset_vulnerability_instance fact and provides a record for each vulnerability finding on every asset that is exploitable. If an asset was not vulnerable to an exploitable vulnerability (or all instances are excluded), it will have no records in this fact table. Each row represents one unique exploit, and either the malware_kit_id or exploit_id is guaranteed to be non-null. This fact table should be the preferred level of grain when instance-level details (such as the port and proof) are not required.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


Nullable	exploit_id	integer	The unique identifier of the exploit.


Nullable	malware_kit_id	integer	The unique identifier of the malware_kit_id.


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


exploit_id_fk	(exploit_id) references dim_vulnerability_exploit (exploit_id)


malware_kit_id_fk	(malware_kit_id) references dim_vulnerability_malware_kit (malware_kit_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_finding_exploit_remediation


Accumulating snapshot fact for all current vulnerability findings on an asset that are known to be exploitable and the solution(s) that remediate them. This fact is a convenience rollup for the fact_asset_vulnerability_finding_exploit_remediation fact and provides the best solution(s) to remediate an exploitable vulnerability finding.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


solution_id	integer	The unique identifier of the solution.


Nullable	exploit_id	integer	The unique identifier of the exploit.


Nullable	malware_kit_id	integer	The unique identifier of the malware_kit_id.


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


solution_id_fk	(solution_id) references dim_solution (solution_id)


exploit_id_fk	(exploit_id) references dim_vulnerability_exploit (exploit_id)


malware_kit_id_fk	(malware_kit_id) references dim_vulnerability_malware_kit (malware_kit_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_finding_remediation


Accumulating snapshot fact that describes the impact of applying a rollup solution to a vulnerability on an asset. For every rollup solution that is selected for an asset a record will be present in this fact that summaries the result of applying that solution to a vulnerability. Note, this fact does not calculate the impact of solutions that are not the highest level of rollup.


Columns


asset_id	bigint	The unique identifier of the asset.


Nullable	vulnerability_id	integer	The unique identifier of the vulnerability.


Nullable	solution_id	integer	The unique identifier of the solution.


Nullable	date	timestamp	The time at which the vulnerability was first found on the asset. This is the earliest date any instance on the asset was found.


Nullable	reintroduced_date	timestamp	The date on which the vulnerability was reintroduced on the asset following a previous remediation.


critical_vulnerabilities	bigint	The number of critical vulnerabilities that will be remediated. Either 1 if the vulnerablity finding is critical, 0 otherwise.


severe_vulnerabilities	bigint	The number of severe vulnerabilities that will be remediated. Either 1 if the vulnerablity finding is severe, 0 otherwise.


moderate_vulnerabilities	bigint	The number of moderate vulnerabilities that will be remediated. Either 1 if the vulnerablity finding is moderate, 0 otherwise.


malware_kits	integer	The the count of malware kits associated to the vulnerability that will be remediated.


exploits	integer	The sum of the count of vulnerabilities with exploits on each asset that will be remediated.


vulnerabilities_with_malware_kit	integer	The sum of the count of vulnerabilities with malware on each asset that will be remediated.


vulnerabilities_with_exploit	integer	The sum of the count of vulnerabilities with exploits on each asset that will be remediated.


vulnerability_instances	bigint	The sum of all the vulnerabilities instances on any asset that will be remediated.


raw_risk_score	float8	The amount of raw risk score that will be reduced by applying the remediation for the vulnerability.


risk_score	float8	The amount of risk score that will be reduced by applying the remediation for the vulnerability.


pci_failures	bigint	The number of PCI failures that will be resolved by appyling the remediation for the vulnerability.


Foreign Keys


asset_id_fk	(asset_id) references dim_asset (asset_id)


solution_id_fk	(solution_id) references dim_solution (solution_id)


vulnerability_id_fk	(vulnerability_id) references dim_vulnerability (vulnerability_id)


fact_asset_vulnerability_instance


Accumulating snapshot fact for all current vulnerability instances on an asset. This fact provides a record for each vulnerability instance on every asset. If an asset is not vulnerable to any vulnerabilities (or all vulnerabilities have been excluded) it will have no records in this fact table.


Columns


asset_id	bigint	The unique identifier of the asset.


vulnerability_id	integer	The unique identifier of the vulnerability.


date	timestamp	The time at which the vulnerability was first found on the asset. This is the earliest date any instance on the asset was found.


Nullable	reintroduced_date	timestamp	The date on which the vulnerability was reintroduced on the asset following a previous remediation.


status	text	The status of the vulnerability, one of the values: 'Confirmed vulnerability', 'Vulnerable version', 'Potential vulnerability'


Nullable	proof	text	Free-form text that describes the proof which explains why the vulnerability is present on the asset. The proof is represented using HTML markup that can be "flattened" using the htmlToText() function.


Nullable	key	text	Optional secondary identifier for a vulnerability result that can distinguish the result from other vulnerability instances of the same type on the system, but found in different locations (e.g. URLs).


Nullable	service	text	The service this vulnerability test was performed against. If the vulnerability was detected without a network-based service, the value will be NULL.


Nullable	port	integer	The port on which the service was running if the vulnerability was found through a network service. If the vulnerability was not found in the network service, the value is NULL. The data within this column will only contain valid port numbers (0 - 65535).


Nullable	protocol	text	The protocol that the service was using on which the vulnerability was found. If the vulnerability was not found on a network service, the value is NULL.


Indexes


fact_asset_vulnerability_instance_date	ON date


