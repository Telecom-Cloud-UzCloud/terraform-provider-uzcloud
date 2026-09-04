# Terraform UzCloud Provider

[🇬🇧 English](README.md) | [🇷🇺 Русский](README.ru.md) | [🇺🇿 O'zbekcha](README.uz.md)

Terraform provider for [UzCloud](https://uzcloud.uz/), built on the [Terraform Plugin Framework](https://developer.hashicorp.com/terraform/plugin/framework).

Full documentation and examples for every resource and data source are published on the [Terraform Registry](https://registry.terraform.io/providers/telecom-cloud-uzcloud/uzcloud/latest/docs).

The provider's Terraform Registry source address is `telecom-cloud-uzcloud/uzcloud`.

## Supported resources and data sources

### Resources

**Compute**

* [uzc_virtual_machine](docs/resources/virtual_machine.md) - virtual machine;
* [uzc_ssh_key](docs/resources/ssh_key.md) - SSH public key for VM login;
* [uzc_template](docs/resources/template.md) - custom template/image in the caller's own account;
* [uzc_iso](docs/resources/iso.md) - ISO image sourced from a URL;
* [uzc_vm_snapshot](docs/resources/vm_snapshot.md) - point-in-time snapshot of a virtual machine;
* [uzc_vm_snapshot_revert](docs/resources/vm_snapshot_revert.md) - reverts a virtual machine to a snapshot;
* [uzc_vm_backup](docs/resources/vm_backup.md) - recurring backup schedule for a virtual machine;
* [uzc_affinity_group](docs/resources/affinity_group.md) - VM placement (anti-)affinity group;

**Storage**

* [uzc_blockstorage](docs/resources/blockstorage.md) - block storage volume;
* [uzc_blockstorage_attachment](docs/resources/blockstorage_attachment.md) - attaches a block storage volume to a virtual machine;
* [uzc_blockstorage_snapshot](docs/resources/blockstorage_snapshot.md) - point-in-time snapshot of a block storage volume;
* [uzc_blockstorage_snapshot_revert](docs/resources/blockstorage_snapshot_revert.md) - reverts a block storage volume to a snapshot;
* [uzc_blockstorage_backup](docs/resources/blockstorage_backup.md) - recurring backup schedule for a block storage volume;

**Networking**

* [uzc_network](docs/resources/network.md) - isolated or L2 network;
* [uzc_vpc](docs/resources/vpc.md) - VPC network;
* [uzc_vpc_restart](docs/resources/vpc_restart.md) - restarts a VPC's network;
* [uzc_vpc_acl_rule](docs/resources/vpc_acl_rule.md) - network ACL rule on a VPC;
* [uzc_virtual_router](docs/resources/virtual_router.md) - virtual router;
* [uzc_ip_address](docs/resources/ip_address.md) - public or private IP address;
* [uzc_static_nat](docs/resources/static_nat.md) - static NAT from an IP address to a virtual machine;
* [uzc_firewall_rule](docs/resources/firewall_rule.md) - firewall rule on an IP address;
* [uzc_port_forwarding_rule](docs/resources/port_forwarding_rule.md) - port forwarding rule on an IP address;
* [uzc_egress_firewall_rule](docs/resources/egress_firewall_rule.md) - egress firewall rule on an isolated network;
* [uzc_remote_access_vpn](docs/resources/remote_access_vpn.md) - remote access VPN on an IP address;
* [uzc_vpn_gateway](docs/resources/vpn_gateway.md) - site-to-site VPN gateway on a VPC;
* [uzc_vpn_user](docs/resources/vpn_user.md) - remote access VPN user;
* [uzc_vpn_customer_gateway](docs/resources/vpn_customer_gateway.md) - site-to-site VPN customer gateway;
* [uzc_dns_domain](docs/resources/dns_domain.md) - DNS domain;
* [uzc_dns_record](docs/resources/dns_record.md) - DNS record on a domain;

**Managed services**

* [uzc_load_balancer](docs/resources/load_balancer.md) - load balancer;
* [uzc_load_balancer_rule](docs/resources/load_balancer_rule.md) - additional rule on an existing load balancer;
* [uzc_kubernetes_cluster](docs/resources/kubernetes_cluster.md) - managed Kubernetes cluster;
* [uzc_vm_autoscale](docs/resources/vm_autoscale.md) - VM autoscale group;
* [uzc_vm_autoscale_policy](docs/resources/vm_autoscale_policy.md) - scale-up policy on an autoscale group;
* [uzc_vm_autoscale_condition](docs/resources/vm_autoscale_condition.md) - scale-down condition on an autoscale group;

**Organization**

* [uzc_project](docs/resources/project.md) - project (organizes the resources above);

### Data sources

* [uzc_regions](docs/data-sources/regions.md) / [uzc_region](docs/data-sources/region.md) - available regions;
* [uzc_cloud_providers](docs/data-sources/cloud_providers.md) - available cloud providers;
* [uzc_projects](docs/data-sources/projects.md) / [uzc_project](docs/data-sources/project.md) - projects in the account;
* [uzc_plans](docs/data-sources/plans.md) - available sizing plans;
* [uzc_templates](docs/data-sources/templates.md) - marketplace/OS template catalog;
* [uzc_account_templates](docs/data-sources/account_templates.md) - templates in the caller's own account;
* [uzc_marketplace_apps](docs/data-sources/marketplace_apps.md) - marketplace application catalog;
* [uzc_storage_categories](docs/data-sources/storage_categories.md) - available storage categories;
* [uzc_billing_cycles](docs/data-sources/billing_cycles.md) - available billing cycles;
* [uzc_ssh_keys](docs/data-sources/ssh_keys.md) - SSH keys in the account;
* [uzc_account_balance](docs/data-sources/account_balance.md) - current account balance;
* [uzc_networks](docs/data-sources/networks.md) / [uzc_network](docs/data-sources/network.md) - isolated/L2 networks;
* [uzc_vpcs](docs/data-sources/vpcs.md) - VPC networks;
* [uzc_ip_addresses](docs/data-sources/ip_addresses.md) - allocated IP addresses;
* [uzc_network_categories](docs/data-sources/network_categories.md) - available network categories;
* [uzc_virtual_routers](docs/data-sources/virtual_routers.md) - virtual routers;
* [uzc_affinity_groups](docs/data-sources/affinity_groups.md) - VM placement (anti-)affinity groups;
* [uzc_blockstorages](docs/data-sources/blockstorages.md) / [uzc_blockstorage](docs/data-sources/blockstorage.md) - block storage volumes;
* [uzc_vm_snapshots](docs/data-sources/vm_snapshots.md) - virtual machine snapshots;
* [uzc_blockstorage_snapshots](docs/data-sources/blockstorage_snapshots.md) - block storage snapshots;
* [uzc_isos](docs/data-sources/isos.md) - ISO images in the account;
* [uzc_load_balancers](docs/data-sources/load_balancers.md) - load balancers;
* [uzc_kubernetes_clusters](docs/data-sources/kubernetes_clusters.md) - managed Kubernetes clusters;
* [uzc_autoscales](docs/data-sources/autoscales.md) - VM autoscale groups;
* [uzc_dns_domains](docs/data-sources/dns_domains.md) - DNS domains;
* [uzc_monitoring_charts](docs/data-sources/monitoring_charts.md) - available monitoring chart definitions;

## Requirements

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [Go](https://go.dev/doc/install) >= 1.23 (only needed to build the provider from source)
- A UzCloud account and API token (UzCloud portal -> Profile -> API)

## Using the provider

```hcl
terraform {
  required_providers {
    uzc = {
      source  = "telecom-cloud-uzcloud/uzcloud"
      version = ">= 1.0.2"
    }
  }
}

provider "uzc" {
  api_token = var.uzc_api_token # or set UZC_API_TOKEN
}
```

See [`examples/provider/provider.tf`](examples/provider/provider.tf) for the full set of
provider-level options, including inheritable defaults for `project`, `region`,
`cloud_provider`, and `billing_cycle`. Runnable end-to-end scenarios (a minimal VM, a full
project stack, a Kubernetes cluster, and more) live under
[`examples/`](examples), numbered roughly in order of complexity.

> [!NOTE]
> UzCloud API tokens always carry an `expires_at` and will eventually stop working. If a
> configuration that used to apply cleanly starts failing with `401 Unauthorized`, reissue
> the token from the UzCloud portal before assuming a configuration or provider bug.

### Authentication

```bash
export UZC_API_TOKEN=<api-token>     # required; UzCloud portal -> Profile -> API
export UZC_API_ENDPOINT=<base-url>   # optional; defaults to the production API
```

### Example: project, SSH key, and virtual machine

```hcl
resource "uzc_project" "example" {
  name        = "example-project"
  description = "Managed by Terraform"
}

resource "uzc_ssh_key" "example" {
  name       = "example-key"
  public_key = file("~/.ssh/id_ed25519.pub")
  project    = uzc_project.example.slug
}

resource "uzc_virtual_machine" "example" {
  name           = "example-vm"
  cloud_provider = "nimbo"
  project        = uzc_project.example.slug
  region         = "tashkent"
  billing_cycle  = "hourly"

  boot_source = "image"
  template    = "ubuntu-22-04"
  plan        = "box2cm2"

  is_public = true
  ssh_key   = uzc_ssh_key.example.slug
}
```

More complete, per-resource examples (including the `custom_plan` sizing alternative and
import instructions) live under [`examples/resources`](examples/resources).

## Building

```bash
make install
# or
go build ./...
```

## Testing

### Unit and client tests (no credentials, no network)

Hermetic tests of the provider's own logic: validators and helpers, import-id parsers, and
the HTTP client's request/response encoding and error mapping (via `httptest`). No
configuration required:

```bash
make test
# or
go test ./...
```

### Acceptance tests

Acceptance tests drive the full resource lifecycle (create, read, update, delete), an import
round-trip, and plan stability through the real Terraform runtime. As described in
HashiCorp's [acceptance testing guidance](https://developer.hashicorp.com/terraform/plugin/testing/acceptance-tests),
they create real infrastructure that may cost money, so they run only when `TF_ACC` is set.
Without it, for example in CI without secrets, they are skipped.

Set the provider credentials and test-scope variables, then run:

```bash
export TF_ACC=1
export UZC_API_TOKEN=<api-token>
export UZC_TEST_REGION=<region>
export UZC_TEST_CLOUD_PROVIDER=<cloud-provider>
export UZC_TEST_TEMPLATE=<template>
export UZC_TEST_PLAN=<plan>
export UZC_TEST_SSH_KEY=<ssh-key>

make testacc
```

## Documentation

Reference docs for every resource and data source are generated from schema
descriptions and the `examples/` directory into [`docs/`](docs) via
[`tfplugindocs`](https://github.com/hashicorp/terraform-plugin-docs):

```bash
make docs
```

Run this after changing any schema `Description` or any file under
`examples/`, and commit the resulting `docs/` diff; CI checks that the two
stay in sync.

## Developing the provider

```bash
go build ./...
go vet ./...
go test ./...
```

```bash
# Run the provider against a local Terraform configuration without publishing
# it to a registry:
go build -o terraform-provider-uzcloud
```

then point a `.terraformrc` [`dev_overrides`](https://developer.hashicorp.com/terraform/cli/config/config-file#development-overrides-for-provider-developers)
block at the built binary.

## License

[Mozilla Public License 2.0](LICENSE)
