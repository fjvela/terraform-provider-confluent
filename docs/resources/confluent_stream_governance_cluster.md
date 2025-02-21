---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "confluent_stream_governance_cluster Resource - terraform-provider-confluent"
subcategory: ""
description: |-
  
---

# confluent_stream_governance_cluster Resource

[![Open Preview](https://img.shields.io/badge/Lifecycle%20Stage-Open%20Preview-%2300afba)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy)

!> **WARNING:** `confluent_stream_governance_cluster` resource is deprecated and will be removed in the next version. Use `confluent_schema_registry_cluster` instead.

-> **Note:** `confluent_stream_governance_cluster` resource is available in **Open Preview** for early adopters. Open Preview features are introduced to gather customer feedback. This feature should be used only for evaluation and non-production testing purposes or to provide feedback to Confluent, particularly as it becomes more widely available in follow-on editions.  
**Open Preview** features are intended for evaluation use in development and testing environments only, and not for production use. The warranty, SLA, and Support Services provisions of your agreement with Confluent do not apply to Open Preview features. Open Preview features are considered to be a Proof of Concept as defined in the Confluent Cloud Terms of Service. Confluent may discontinue providing preview releases of the Open Preview features at any time in Confluent’s sole discretion.

`confluent_stream_governance_cluster` provides a Stream Governance cluster resource that enables creating, editing, and deleting Stream Governance clusters on Confluent Cloud.

-> **Note:** It is recommended to set `lifecycle { prevent_destroy = true }` on production instances to prevent accidental cluster deletion. This setting rejects plans that would destroy or recreate the cluster, such as attempting to change uneditable attributes. Read more about it in the [Terraform docs](https://www.terraform.io/language/meta-arguments/lifecycle#prevent_destroy).

## Example Usage

```terraform
resource "confluent_environment" "development" {
  display_name = "Development"

  lifecycle {
    prevent_destroy = true
  }
}

data "confluent_stream_governance_region" "example" {
  cloud   = "AWS"
  region  = "us-east-2"
  package = "ESSENTIALS"
}

resource "confluent_stream_governance_cluster" "essentials" {
  package = data.confluent_stream_governance_region.example.package

  environment {
    id = confluent_environment.development.id
  }

  region {
    # See https://docs.confluent.io/cloud/current/stream-governance/packages.html#stream-governance-regions
    # Stream Governance and Kafka clusters can be in different regions as well as different cloud providers,
    # but you should to place both in the same cloud and region to restrict the fault isolation boundary.
    id = data.confluent_stream_governance_region.example.id
  }

  lifecycle {
    prevent_destroy = true
  }
}
```

<!-- schema generated by tfplugindocs -->
## Argument Reference

The following arguments are supported:

- `package` - (Required String) The type of the billing package. Accepted values are: `ESSENTIALS` and `ADVANCED`.
- `environment` (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Environment that the Stream Governance cluster belongs to, for example, `env-abc123`.
- `region` (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Stream Governance region that the Stream Governance cluster belongs to, for example, `sgreg-1`. See [Stream Governance Regions](https://docs.confluent.io/cloud/current/stream-governance/packages.html#stream-governance-regions) to find a corresponding region ID based on desired cloud provider region and types of the billing package.

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `id` - (Required String) The ID of the Stream Governance cluster, for example, `lsrc-abc123`.
- `api_version` - (Required String) An API Version of the schema version of the Stream Governance cluster, for example, `stream-governance/v2`.
- `kind` - (Required String) A kind of the Stream Governance cluster, for example, `Cluster`.
- `http_endpoint` - (Required String) The HTTP endpoint of the Stream Governance cluster, for example, `https://psrc-00000.us-west-2.aws.confluent.cloud`.
- `display_name` - (Required String) The name of the Stream Governance cluster, for example, `Stream Governance Package`.
- `resource_name` - (Required String) The Confluent Resource Name of the Stream Governance cluster, for example, `crn://confluent.cloud/organization=1111aaaa-11aa-11aa-11aa-111111aaaaaa/environment=env-abc123/schema-registry=lsrc-abc123`.

!> **Warning:** Stream Governance clusters can be upgraded from `ESSENTIALS` to `ADVANCED`, but cannot be downgraded from `ADVANCED` to `ESSENTIALS`.

## Import

-> **Note:** `CONFLUENT_CLOUD_API_KEY` and `CONFLUENT_CLOUD_API_SECRET` environment variables must be set before importing a Stream Governance cluster.

You can import a Stream Governance cluster by using Environment ID and Stream Governance cluster ID, in the format `<Environment ID>/<Stream Governance cluster ID>`, for example:

```shell
$ export CONFLUENT_CLOUD_API_KEY="<cloud_api_key>"
$ export CONFLUENT_CLOUD_API_SECRET="<cloud_api_secret>"
$ terraform import confluent_stream_governance_cluster.example env-abc123/lsrc-abc123
```

!> **Warning:** Do not forget to delete terminal command history afterwards for security purposes.
