# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md) | [🇺🇿 O'zbekcha](CHANGELOG.uz.md)

## 1.2.0 (October 23, 2026)

##### FEATURES:

* network: `uzc_network` creates a tier (subnet) of a VPC: `type = "Vpc"` with `vpc`, `gateway` and `netmask`, and `acl_id` to apply one of the VPC's network ACL lists (changed in place)
* network: **New Resource:** `uzc_network_acl_list`, a network ACL list of a VPC
* network: **New Resource:** `uzc_network_acl_rule`, a rule in a network ACL list, updated in place
* network: **New Resource:** `uzc_vpc_static_route`, a static route of a VPC
* kubernetes: `uzc_kubernetes_cluster` exports the cluster's `kubeconfig` (sensitive) and API server `endpoint`, so the `kubernetes` and `helm` providers can use a cluster created in the same run

##### ENHANCEMENTS:

* compute: before creating a `uzc_virtual_machine` the provider checks `template`, `plan`, `blockstorage_plan`, `storage_category`, `networks` and `ssh_key` against the account's catalogs and reports an unknown slug with the closest match and the valid values (`ubuntu-24-04` suggests `ubuntu-2404`), instead of passing it to the API, which answers with internal errors such as "Call to a member function getTable() on null"
* compute: `server`, `template_type` and `os_family` default to `cloud-compute`, the template's image type and the template's operating system family when unset, the way the console fills them in
* compute: new `network_plan` attribute for the network UzCloud creates when `network_type` is `Isolated` and `networks` is empty; when unset, the account's default network plan (or `isolated`) is used
* network: `uzc_network` has a `billing_cycle` attribute; it falls back to the provider's `billing_cycle`, then to `hourly`
* compute: when `network_type` is unset, `uzc_virtual_machine` uses the type of the network in `networks`, the way the console does; `"Vpc"` is the documented value (`"VPC"` is still accepted), and a VPC tier with `network_type = "Isolated"` or an ordinary network with `"Vpc"` is reported before the create request
* compute: `uzc_ssh_key` has a `region` attribute (from the provider's `region` when unset), and `project` and `region` are read back on refresh and import
* network: `uzc_vpc` waits until the VPC is `Enabled`, so tiers can be added right after it, and sends `vr_plan` like the console
* kubernetes: changing `node_size` scales `uzc_kubernetes_cluster` in place instead of recreating it, and the plan change request is only sent when the plan or billing cycle changed
* storage: new `size_gb` attribute on `uzc_blockstorage`; a larger value grows the volume in place
* loadbalancer: the virtual machines of a `uzc_load_balancer_rule` can be removed as well as added, in place
* dns: `uzc_dns_record` changes `ttl` and `content` in place, reads them back on refresh, and can be imported as `{domain}/{type}/{name}`
* compute: a `uzc_ssh_key` name longer than 20 characters, which UzCloud rejects, is reported before the create request
* network: renaming a `uzc_vpc` waits up to two minutes for the new name, which UzCloud applies a few seconds after accepting the request
* compute: removing a network from `networks` of `uzc_virtual_machine` detaches it in place (`POST .../remove-network`) instead of recreating the VM

##### BUG FIXES:

* provider: `cloud_provider` is inherited from the provider block as documented; it was required on every compute resource
* provider: "exactly one of" checks (`uzc_ip_address` `network`/`vpc`, `plan`/`custom_plan`, `uzc_template` sources, Kubernetes plans) no longer reject a value that is only known after apply, such as `network = uzc_network.this.slug`
* docs: the examples used template and plan slugs that do not exist (`ubuntu-22-04`, `box2cm2`) and omitted `storage_category` and `blockstorage_plan`; all six example stacks now pass `terraform validate`
* compute: creating a VM with `network_type = "Isolated"` and no `networks` failed with 422 "The network plan field is required"
* network: `uzc_network` sent no `billing_cycle`: UzCloud then answered 403 "Something went wrong" and left a network without a CloudStack id, which failed to rename with "missing parameter id"; `billing_cycle` is now always sent
* network: `uzc_network` failed with "The network plan field is required" when `network_plan` was unset; the account's default network plan (or `isolated`) is now used
* provider: `is_vm_snapshot` is sent on `uzc_network`, `uzc_vpc` and `uzc_load_balancer` creation, as the API spec lists it
* data sources: `uzc_load_balancers` failed when the API sent a rule's ports or flags as strings
* compute: creating a `uzc_ssh_key` failed with 500 `Attempt to read property "id" on null`: the API needs the key's region, which is now sent along with the project
* network: `uzc_egress_firewall_rule` waited until the operation timeout: the API returns no rule from the create request and names the fields `cidr` and `destcidr_list`; the new rule is now found in the list, and ports reported as numbers or null are read correctly
* network: `uzc_vpc_acl_rule` waited until the operation timeout on create (the API returns no id) and always failed on destroy; it now finds the new list and deletes it
* provider: CloudStack's 403 "entity does not exist" for a deleted object is treated as not found, so destroys and refreshes do not fail on it
* loadbalancer: destroying a `uzc_load_balancer_rule` always failed; the rule is now deleted
* dns: destroying a `uzc_dns_record` always failed; the record is now deleted by sending it, as the domain lists it, the way the console does
* network: creating a `uzc_network` failed with "cannot unmarshal number into ... is_default of type bool": the create response sends the network's flags as numbers
* network: renaming a `uzc_network` failed with "cannot unmarshal array": the response carries no network, which is now read back once the new name is applied
* loadbalancer: adding a `uzc_load_balancer_rule` to an existing load balancer answered 403 "Something went wrong": the health monitor settings and the machines' `ipaddress` are now sent like the console does
* compute: a VM with several `networks` showed "Provider produced inconsistent result" because UzCloud lists them in another order; the configured order is kept
* network: deleting a `uzc_network` right after its last VM is retried while UzCloud still answers "You cannot delete the network while virtual machines are created using it"
* network: a `uzc_vpc_static_route` requested right after a new tier is accepted by UzCloud but never created; the request is re-sent when the route has not appeared after a minute

##### DEPRECATIONS:

* network: `uzc_vpc_acl_rule` is deprecated: despite its name it manages a network ACL list, not a rule. Use `uzc_network_acl_list` (`rule_id` becomes `acl_id`) and `uzc_network_acl_rule`

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
