---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "confluent_cluster_link Resource - terraform-provider-confluent"
subcategory: ""
description: |-
  
---

# confluent_cluster_link Resource

[![Open Preview](https://img.shields.io/badge/Lifecycle%20Stage-Open%20Preview-%2300afba)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy)

-> **Note:** `confluent_cluster_link` resource is available in **Open Preview** for early adopters. Open Preview features are introduced to gather customer feedback. This feature should be used only for evaluation and non-production testing purposes or to provide feedback to Confluent, particularly as it becomes more widely available in follow-on editions.  
**Open Preview** features are intended for evaluation use in development and testing environments only, and not for production use. The warranty, SLA, and Support Services provisions of your agreement with Confluent do not apply to Open Preview features. Open Preview features are considered to be a Proof of Concept as defined in the Confluent Cloud Terms of Service. Confluent may discontinue providing preview releases of the Open Preview features at any time in Confluent’s sole discretion.

`confluent_cluster_link` provides a Cluster Link resource that enables creating and deleting Cluster Links on a Kafka cluster on Confluent Cloud.

-> **Note:** It is recommended to set `lifecycle { prevent_destroy = true }` on production instances to prevent accidental cluster link deletion. This setting rejects plans that would destroy or recreate the cluster link. Read more about it in the [Terraform docs](https://www.terraform.io/language/meta-arguments/lifecycle#prevent_destroy).

## Example Usage

```terraform
resource "confluent_cluster_link" "destination-outbound" {
  link_name = "destination-initiated-cluster-link"
  source_kafka_cluster {
    id                 = data.confluent_kafka_cluster.source.id
    bootstrap_endpoint = data.confluent_kafka_cluster.source.bootstrap_endpoint
    credentials {
      key    = confluent_api_key.app-manager-source-cluster-api-key.id
      secret = confluent_api_key.app-manager-source-cluster-api-key.secret
    }
  }

  destination_kafka_cluster {
    id            = data.confluent_kafka_cluster.destination.id
    rest_endpoint = data.confluent_kafka_cluster.destination.rest_endpoint
    credentials {
      key    = confluent_api_key.app-manager-destination-cluster-api-key.id
      secret = confluent_api_key.app-manager-destination-cluster-api-key.secret
    }
  }

  lifecycle {
    prevent_destroy = true
  }
}
```

<!-- schema generated by tfplugindocs -->
## Argument Reference

The following arguments are supported:

- `link_name` - (Required String) The name of the cluster link, for example, `my-cluster-link`.
- `source_kafka_cluster` - (Required Configuration Block) supports the following:
  - `id` - (Required String) The ID of the source Kafka cluster, for example, `lkc-abc123`.
  - `rest_endpoint` - (Optional String) The REST endpoint of the source Kafka cluster, for example, `https://pkc-00000.us-central1.gcp.confluent.cloud:443`).
  - `bootstrap_endpoint` - (Optional String) The bootstrap endpoint of the source Kafka cluster, for example, `SASL_SSL://pkc-00000.us-central1.gcp.confluent.cloud:9092` or `pkc-00000.us-central1.gcp.confluent.cloud:9092`).
  - `credentials` (Optional Configuration Block) supports the following:
    - `key` - (Required String) The Kafka API Key.
    - `secret` - (Required String, Sensitive) The Kafka API Secret.
- `destination_kafka_cluster` - (Required Configuration Block) supports the following:
  - `id` - (Required String) The ID of the destination Kafka cluster, for example, `lkc-abc123`.
  - `rest_endpoint` - (Optional String) The REST endpoint of the destination Kafka cluster, for example, `https://pkc-00000.us-central1.gcp.confluent.cloud:443`).
  - `bootstrap_endpoint` - (Optional String) The bootstrap endpoint of the destination Kafka cluster, for example, `SASL_SSL://pkc-00000.us-central1.gcp.confluent.cloud:9092` or `pkc-00000.us-central1.gcp.confluent.cloud:9092`).
  - `credentials` (Required Configuration Block) supports the following:
    - `key` - (Required String) The Kafka API Key.
    - `secret` - (Required String, Sensitive) The Kafka API Secret.
- `link_mode` (Optional String) The mode of the cluster link. The supported values are `"DESTINATION"` and `"SOURCE"`. Defaults to `"DESTINATION"`.
- `connection_mode` (Optional String) The connection mode of the cluster link. The supported values are `"INBOUND"` and `"OUTBOUND"`. Defaults to `"OUTBOUND"`.

-> **Note:** A Kafka API key consists of a key and a secret. Kafka API keys are required to interact with Kafka clusters in Confluent Cloud. Each Kafka API key is valid for one specific Kafka cluster.

-> **Note:** To rotate a Kafka API key, create a new Kafka API key, update the `credentials` block in all configuration files to use the new Kafka API key, run `terraform apply -target="confluent_cluster_link.example"`, and remove the old Kafka API key. Alternatively, in case the old Kafka API Key was deleted already, you might need to run `terraform plan -refresh=false -target="confluent_cluster_link.example" -out=rotate-kafka-api-key` and `terraform apply rotate-kafka-api-key` instead.

-> **Note:** Setting or updating cluster link settings is currently not supported. 

!> **Warning:** Terraform doesn't encrypt the sensitive `credentials` value of the `confluent_cluster_link` resource, so you must keep your state file secure to avoid exposing it. Refer to the [Terraform documentation](https://www.terraform.io/docs/language/state/sensitive-data.html) to learn more about securing your state file.

-> **Note:** See [Supported Cluster Types](https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html#supported-cluster-types) to determine supported options for source and destination clusters.

-> **Note:** Exactly one from the `rest_endpoint` and `bootstrap_endpoint` attributes must be specified.

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `id` - (Required String) The ID of the Cluster Link, in the format `<Kafka cluster ID>/<Cluster link name>`, for example, `lkc-abc123/my-cluster-link`.

## Import

You can import a Kafka mirror topic by using the cluster link name, cluster link mode, cluster link connection mode,
source Kafka cluster ID, and destination Kafka cluster ID, in the format `<Cluster link name>/<Cluster link mode>/<Cluster connection mode>/<Source Kafka cluster ID>/<Destination Kafka cluster ID>`, for example:

```shell
$ export IMPORT_SOURCE_KAFKA_BOOTSTRAP_ENDPOINT="<source_kafka_bootstrap_endpoint>"
$ export IMPORT_SOURCE_KAFKA_API_KEY="<source_kafka_api_key>"
$ export IMPORT_SOURCE_KAFKA_API_SECRET="<source_kafka_api_secret>"
$ export IMPORT_DESTINATION_KAFKA_REST_ENDPOINT="<destination_kafka_rest_endpoint>"
$ export IMPORT_DESTINATION_KAFKA_API_KEY="<destination_kafka_api_key>"
$ export IMPORT_DESTINATION_KAFKA_API_SECRET="<destination_kafka_api_secret>"
$ terraform import confluent_cluster_link.my_cluster_link my-cluster-link/DESTINATION/OUTBOUND/lkc-abc123/lkc-xyz456
```

!> **Warning:** Do not forget to delete terminal command history afterwards for security purposes.

## Getting Started
The following end-to-end examples might help to get started with `confluent_cluster_link` resource:
  * [`destination-initiated-cluster-link-rbac`](https://github.com/confluentinc/terraform-provider-confluent/tree/master/examples/configurations/destination-initiated-cluster-link-rbac): An example of setting up a _destination_ initiated cluster link with a mirror topic
  * [`source-initiated-cluster-link-rbac`](https://github.com/confluentinc/terraform-provider-confluent/tree/master/examples/configurations/source-initiated-cluster-link-rbac): An example of setting up a _source_ initiated cluster link with a mirror topic

See [Cluster Linking on Confluent Cloud](https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html) to learn more about Cluster Linking on Confluent Cloud.
