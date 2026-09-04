# Changelog

[🇬🇧 English](CHANGELOG.md) | [🇷🇺 Русский](CHANGELOG.ru.md)

## Unreleased

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
