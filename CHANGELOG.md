# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## Unreleased

## 1.1.0 (October 16, 2026)

##### FEATURES:

* storage: **New Resource:** `uzc_object_storage`, an S3-compatible object storage (Ceph): create, import, in-place change of `size_gb` and `billing_cycle`, `s3_endpoint` exported
* kubernetes: `uzc_kubernetes_cluster` accepts `master_plan`, `worker_plan`, `blockstorage_plan` and `cloud_provider_setup`, the way the UzCloud console creates clusters; `plan` still works and is used as the worker plan
* provider: new `operation_timeout_minutes` setting (or `UZC_OPERATION_TIMEOUT`, default 20) that bounds every wait for an asynchronous operation

##### ENHANCEMENTS:

* provider: every resource waits for UzCloud to confirm the outcome of an operation (a created resource is readable, an update shows the new value, a deleted resource is gone) and fails with an explicit error after `operation_timeout_minutes`; `uzc_virtual_machine` keeps its own create/update/delete timeouts
* provider: API errors include the per-field validation details UzCloud sends, e.g. `[storage_category: The storage category field is required.]`, instead of only "Validation errors"
* storage: `uzc_vm_snapshot`, `uzc_blockstorage_snapshot`, `uzc_vm_backup` and `uzc_blockstorage_backup` default to the `hourly` billing cycle: the API rejects monthly and yearly cycles for these usage-billed services
* storage: `uzc_blockstorage_attachment` waits until the volume is reported attached (and detached on destroy); a volume snapshot is retried until the volume is Ready; deleting a volume is retried while a detach is still finishing
* compute: the wait for a public IP is capped at 10 minutes and fails with a hint about the IP address quota; a VM whose wait fails is kept in state as tainted instead of being left running unmanaged
* compute: deleting a VM is retried while its backup schedule is still being cancelled
* network: renames of `uzc_vpc`, `uzc_network`, `uzc_project` and `uzc_vpn_customer_gateway` are verified by reading the resource back
* tests: acceptance tests for every resource group, a free smoke test of all data sources, and discovery tests that print the plans, cloud providers, regions and raw API responses of an account

##### BUG FIXES:

* compute: creating a `uzc_virtual_machine` failed with "Value Conversion Error" on `offering`/`ipaddresses`
* compute: the VM create request matches the console (`username`, `null` instead of empty strings); renaming sends `vm_label`
* network: `uzc_vpc` rejected a CIDR with a prefix ("The cidr field must be a valid IP address"); `10.0.0.0/16` and `10.0.0.0` are both accepted now
* network: `region`, `project` and `billing_cycle` inherited from the provider block were left unknown after creating `uzc_vpc`, `uzc_network`, `uzc_ip_address` and `uzc_virtual_machine`
* network: an empty update response blanked the slug of `uzc_vpc`, `uzc_network` and `uzc_vpn_customer_gateway`, which then broke the delete
* project: `uzc_project` creation failed with 422 (`icon` is now always sent, `cloud-15` by default, and `status` as 1/0); its status returned as a string is parsed; deletion uses `DELETE /projects/{slug}`
* provider: multipart booleans are sent as 1/0, which Laravel's boolean validation requires (it rejects "true"/"false")
* provider: any unexpected 4xx response (400, 405, 409, ...) is an error; it was treated as success before
* vpn: `uzc_vpn_customer_gateway` sends `dead_peer_detection` (the spec's misspelling was rejected) and keeps `gateway` on read; `uzc_vpn_user` finds its slug after creation
* storage: `uzc_blockstorage` no longer forces a replacement because the API omits `storage_category` (it is read from `storage_setting`); `uzc_blockstorage_attachment` is no longer dropped from state on refresh
* data sources: `uzc_plans` and `uzc_account_balance` accept numbers sent as strings and attributes sent as empty arrays

##### WARNING:

* provider: `terraform destroy` now waits until resources are actually gone, instead of returning once a cancellation request is accepted; destroys take longer and fail after `operation_timeout_minutes`
* storage: snapshots and backups now default to `hourly`; set `billing_cycle` explicitly if a different cycle is needed
* api: known UzCloud-side issues, not fixable in the provider: renaming a VM fails (`chageLabel`), renaming a VPC is accepted but not applied, adding an SSH key answers 500, deleting a project is refused ("projects with services cannot be deleted"), and Kubernetes provisioning reports "not configured properly" on the test account

## 1.0.2 (September 4, 2026)

##### FEATURES:

* provider: initial release of the UzCloud Terraform provider, built on the Terraform Plugin Framework
* project: **New Resource:** `uzc_project`
* project: **New Data Source:** `uzc_projects`
* project: **New Data Source:** `uzc_project`

* compute: **New Resource:** `uzc_virtual_machine`
* compute: **New Resource:** `uzc_ssh_key`
* compute: **New Resource:** `uzc_template`
* compute: **New Resource:** `uzc_iso`
* compute: **New Resource:** `uzc_vm_snapshot`
* compute: **New Resource:** `uzc_vm_snapshot_revert`
* compute: **New Resource:** `uzc_vm_backup`
* compute: **New Resource:** `uzc_affinity_group`
* compute: **New Data Source:** `uzc_ssh_keys`
* compute: **New Data Source:** `uzc_templates`
* compute: **New Data Source:** `uzc_account_templates`
* compute: **New Data Source:** `uzc_marketplace_apps`
* compute: **New Data Source:** `uzc_isos`
* compute: **New Data Source:** `uzc_vm_snapshots`
* compute: **New Data Source:** `uzc_affinity_groups`

* storage: **New Resource:** `uzc_blockstorage`
* storage: **New Resource:** `uzc_blockstorage_attachment`
* storage: **New Resource:** `uzc_blockstorage_snapshot`
* storage: **New Resource:** `uzc_blockstorage_snapshot_revert`
* storage: **New Resource:** `uzc_blockstorage_backup`
* storage: **New Data Source:** `uzc_blockstorages`
* storage: **New Data Source:** `uzc_blockstorage`
* storage: **New Data Source:** `uzc_blockstorage_snapshots`

* network: **New Resource:** `uzc_network`
* network: **New Resource:** `uzc_vpc`
* network: **New Resource:** `uzc_vpc_restart`
* network: **New Resource:** `uzc_vpc_acl_rule`
* network: **New Resource:** `uzc_virtual_router`
* network: **New Resource:** `uzc_ip_address`
* network: **New Resource:** `uzc_static_nat`
* network: **New Resource:** `uzc_firewall_rule`
* network: **New Resource:** `uzc_port_forwarding_rule`
* network: **New Resource:** `uzc_egress_firewall_rule`
* network: **New Data Source:** `uzc_networks`
* network: **New Data Source:** `uzc_network`
* network: **New Data Source:** `uzc_vpcs`
* network: **New Data Source:** `uzc_ip_addresses`
* network: **New Data Source:** `uzc_network_categories`
* network: **New Data Source:** `uzc_virtual_routers`

* vpn: **New Resource:** `uzc_remote_access_vpn`
* vpn: **New Resource:** `uzc_vpn_gateway`
* vpn: **New Resource:** `uzc_vpn_user`
* vpn: **New Resource:** `uzc_vpn_customer_gateway`

* dns: **New Resource:** `uzc_dns_domain`
* dns: **New Resource:** `uzc_dns_record`
* dns: **New Data Source:** `uzc_dns_domains`

* loadbalancer: **New Resource:** `uzc_load_balancer`
* loadbalancer: **New Resource:** `uzc_load_balancer_rule`
* loadbalancer: **New Data Source:** `uzc_load_balancers`

* kubernetes: **New Resource:** `uzc_kubernetes_cluster`
* kubernetes: **New Data Source:** `uzc_kubernetes_clusters`

* autoscale: **New Resource:** `uzc_vm_autoscale`
* autoscale: **New Resource:** `uzc_vm_autoscale_policy`
* autoscale: **New Resource:** `uzc_vm_autoscale_condition`
* autoscale: **New Data Source:** `uzc_autoscales`

* monitoring: **New Data Source:** `uzc_monitoring_charts`
* provider: **New Data Source:** `uzc_regions`
* provider: **New Data Source:** `uzc_region`
* provider: **New Data Source:** `uzc_cloud_providers`
* provider: **New Data Source:** `uzc_plans`
* provider: **New Data Source:** `uzc_storage_categories`
* provider: **New Data Source:** `uzc_billing_cycles`
* provider: **New Data Source:** `uzc_account_balance`

##### ENHANCEMENTS:

* provider: `project`, `region`, `cloud_provider`, and `billing_cycle` can be set once on the provider block and inherited by any resource that leaves its own matching attribute unset
* provider: async, billing-cancellation-based deletion (`uzc_project`, `uzc_network`, `uzc_vpc`, and most other billable services) is documented per-resource, since `terraform destroy` reports success once the cancellation is accepted, not once the resource is actually gone
* compute: `uzc_virtual_machine` deletion polls until the VM is confirmed gone before returning, unlike most other resources
* network: rule-style resources with no delete endpoint (`uzc_static_nat`, `uzc_vpc_acl_rule`, `uzc_load_balancer_rule`, `uzc_dns_record`) fail loudly on destroy instead of silently drifting from state
* docs: reference documentation for every resource and data source generated via `tfplugindocs`, including runnable end-to-end example stacks under `examples/01-minimal-vm` through `examples/06-full-project-stack`
* docs: README available in English and Russian (`README.md` / `README.ru.md`)

##### WARNING:

* kubernetes: the UzCloud API currently exposes no endpoint to retrieve a cluster's kubeconfig; `uzc_kubernetes_cluster` can provision a cluster but can't hand it to a `kubernetes`/`helm` provider in the same run
