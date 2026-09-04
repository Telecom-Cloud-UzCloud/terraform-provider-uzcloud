---
page_title: "UzCloud Provider"
subcategory: ""
description: |-
  Manages UzCloud (https://uzcloud.uz/) resources: virtual machines, networking, storage, managed services, and more.
---

# UzCloud Provider

Manages UzCloud (https://uzcloud.uz/) resources: virtual machines, networking, storage, managed services, and more.

## Quick Start

```hcl
terraform {
  required_providers {
    uzc = {
      source  = "telecom-cloud-uzcloud/uzcloud"
      version = ">= 0.1.0"
    }
  }
}

provider "uzc" {
  api_token = var.uzc_api_token # or set UZC_API_TOKEN
}

resource "uzc_project" "example" {
  name = "example-project"
}

resource "uzc_virtual_machine" "example" {
  name           = "example-vm"
  cloud_provider = "nimbo"
  project        = uzc_project.example.slug
  region         = "tashkent"
  billing_cycle  = "hourly"
  boot_source    = "image"
  template       = "ubuntu-22-04"
  plan           = "box2cm2"
  is_public      = true
}
```

More complete, runnable scenarios (a full stack with networking, storage,
and a load balancer, among others) live under
[`examples/`](https://github.com/Telecom-Cloud-UzCloud/terraform-provider-uzcloud/tree/main/examples)
in the provider's repository, numbered roughly in order of complexity.

## Authentication

The provider needs a UzCloud API bearer token, set either as `api_token` in
the provider block or via the `UZC_API_TOKEN` environment variable. Tokens
are issued (and can be reissued) from the UzCloud portal under
**Profile -> API**.

### API Token Expiry

UzCloud API tokens always carry an `expires_at` and will eventually stop
working; there is no long-lived or non-expiring token option. If a
configuration that used to apply cleanly starts failing with
`401 Unauthorized`, reissue the token from the UzCloud portal before
assuming a configuration or provider bug caused it. This provider does not
attempt to detect an approaching expiry or refresh a token automatically:
plan for token rotation the same way you would for any other short-lived
credential (e.g. a scheduled reissue, or pulling a fresh token from a
secrets manager before each run).

### Environment Variables

| Provider attribute | Environment variable | Required |
| --- | --- | --- |
| `api_token` | `UZC_API_TOKEN` | Yes |
| `api_endpoint` | `UZC_API_ENDPOINT` | No (defaults to the production API) |

## Provider-Level Defaults

`project`, `region`, `cloud_provider`, and `billing_cycle` can be set once on
the provider block and are inherited by any resource that leaves its own
matching attribute unset. If neither the resource nor the provider sets a
value, `apply` fails with a validation error naming the missing attribute,
rather than sending an empty value to the API.

## Provider Limitations

UzCloud's API shapes several parts of this provider's behavior in ways
that are worth knowing about before relying on it:

- **Deletion is usually a billing cancellation, and it can be asynchronous.**
  Most resources (`uzc_project`, `uzc_network`, `uzc_vpc`, and most other
  billable services) have no dedicated delete endpoint; destroying them
  submits a cancellation request to UzCloud's billing system instead. For
  the large majority of these, `terraform destroy` reports success as soon
  as the cancellation is *accepted*, not once the underlying resource is
  actually gone; the API gives no per-resource way to confirm the
  cancellation has finished processing. `uzc_virtual_machine` is the
  documented exception: its Delete waits (polling, with a configurable
  timeout) until the VM is confirmed gone before returning.
- **Many attributes are immutable.** Where the API documents no update
  endpoint for a field (or for an entire resource), changing that
  attribute forces Terraform to destroy and recreate the resource rather
  than update it in place. Each resource's own documentation says which of
  its attributes behave this way.
- **A few resources can't be deleted or read back through the API at all,
  and say so loudly.** For example, `uzc_static_nat`, `uzc_vpc_acl_rule`,
  `uzc_load_balancer_rule`, and `uzc_dns_record` document endpoints the
  UzCloud API doesn't currently expose (no delete, or no way to safely
  scope a delete to one record). Rather than silently drifting from
  reality, these resources fail loudly with instructions to reconcile
  state manually (`terraform state rm`) once you've confirmed the real
  state against the UzCloud portal.
- **Kubernetes clusters have no kubeconfig endpoint.** `uzc_kubernetes_cluster`
  can provision a cluster, but the UzCloud API currently exposes no way to
  retrieve its kubeconfig, so this provider can't wire a provisioned
  cluster up to a Kubernetes or Helm provider in the same run. Fetch the
  kubeconfig out of band (e.g. the UzCloud portal) until such an endpoint
  exists.

## Example Usage

```terraform
terraform {
  required_providers {
    uzc = {
      source  = "telecom-cloud-uzcloud/uzcloud"
      version = ">= 0.1.0"
    }
  }
}

variable "uzc_api_token" {
  description = "UzCloud API bearer token (Profile -> API in the UzCloud portal). Tokens carry an expires_at and must be reissued periodically."
  type        = string
  sensitive   = true
}

provider "uzc" {
  api_token = var.uzc_api_token

  # Optional provider-level defaults. Any resource that leaves the matching
  # attribute unset inherits the value configured here; if neither is set,
  # the provider returns a validation error naming the missing attribute.
  project        = "default"
  region         = "tashkent"
  cloud_provider = "nimbo"
  billing_cycle  = "hourly"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Optional

- `api_endpoint` (String) Base URL of the UzCloud API. Defaults to "https://api.uzcloud.uz/api". Can also be set via the UZC_API_ENDPOINT environment variable.
- `api_token` (String, Sensitive) UzCloud API bearer token. Can also be set via the UZC_API_TOKEN environment variable. UzCloud tokens always carry an expiration (expires_at) and WILL eventually stop working: if calls that used to succeed start failing with 401 Unauthorized, the token has most likely expired; reissue it from the UzCloud portal under Profile → API.
- `billing_cycle` (String) Default billing_cycle slug inherited by resources that don't set their own `billing_cycle`.
- `cloud_provider` (String) Default cloud_provider slug inherited by resources that don't set their own `cloud_provider`.
- `max_retries` (Number) Maximum number of retries for rate-limited (429), server-error (5xx), and network-failure responses. Defaults to 3.
- `project` (String) Default project slug inherited by resources that don't set their own `project`.
- `region` (String) Default region slug inherited by resources that don't set their own `region`.
- `timeout_seconds` (Number) HTTP request timeout, in seconds. Defaults to 60.
- `user_agent_suffix` (String) Optional suffix appended to the client's User-Agent header.
