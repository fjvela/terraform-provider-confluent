---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "confluent_kafka_cluster_config Resource - terraform-provider-confluent"
subcategory: ""
description: |-
  
---

# confluent_kafka_cluster_config Resource

[![General Availability](https://img.shields.io/badge/Lifecycle%20Stage-General%20Availability-%2345c6e8)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy)

`confluent_kafka_cluster_config` provides a Kafka cluster config resource that enables updating configs on a Dedicated Kafka cluster on Confluent Cloud.

## Example Usage

```terraform
resource "confluent_kafka_cluster_config" "orders" {
  kafka_cluster {
    id = confluent_kafka_cluster.dedicated.id
  }
  rest_endpoint = confluent_kafka_cluster.dedicated.rest_endpoint
  config = {
    "auto.create.topics.enable" = "true"
    "log.retention.ms" = "604800123"
  }
  credentials {
    key    = confluent_api_key.app-manager-kafka-api-key.id
    secret = confluent_api_key.app-manager-kafka-api-key.secret
  }
}
```

<!-- schema generated by tfplugindocs -->
## Argument Reference

The following arguments are supported:

- `kafka_cluster` - (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Dedicated Kafka cluster, for example, `lkc-abc123`.
- `rest_endpoint` - (Optional String) The REST endpoint of the Dedicated Kafka cluster, for example, `https://pkc-00000.us-central1.gcp.confluent.cloud:443`).
- `credentials` (Optional Configuration Block) supports the following:
    - `key` - (Required String) The Kafka API Key.
    - `secret` - (Required String, Sensitive) The Kafka API Secret.

-> **Note:** Omit the `rest_endpoint` attribute and `credentials` block if the `kafka_rest_endpoint`, `kafka_api_key`, and `kafka_api_secret` attributes are all set in a `provider` block (see [option #2](https://registry.terraform.io/providers/confluentinc/confluent/latest/docs#example-usage)).

-> **Note:** A Kafka API key consists of a key and a secret. Kafka API keys are required to interact with Kafka clusters in Confluent Cloud. Each Kafka API key is valid for one specific Kafka cluster.

-> **Note:** To rotate a Kafka API key, create a new Kafka API key, update `credentials` block in all configuration files to use the new Kafka API key, run `terraform apply -target="confluent_kafka_cluster_config.orders"`, and remove the old Kafka API key. Alternatively, in case the old Kafka API Key was deleted already, you might need to run `terraform plan -refresh=false -target="confluent_kafka_cluster_config.orders" -out=rotate-kafka-api-key` and `terraform apply rotate-kafka-api-key` instead.

- `config` - (Optional Map) The custom cluster settings to set:
    - `name` - (Required String) The setting name, for example, `auto.create.topics.enable`.
    - `value` - (Required String) The setting value, for example, `true`.

-> **Note:** For more information on the cluster settings, see [Change cluster settings for Dedicated clusters](https://docs.confluent.io/cloud/current/clusters/broker-config.html#change-cluster-settings-for-dedicated-clusters).

!> **Warning:** Terraform doesn't encrypt the sensitive `credentials` value of the `confluent_kafka_cluster_config` resource, so you must keep your state file secure to avoid exposing it. Refer to the [Terraform documentation](https://www.terraform.io/docs/language/state/sensitive-data.html) to learn more about securing your state file.

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `id` - (Required String) The ID of the Kafka cluster config, in the format `<Kafka cluster ID>`, for example, `lkc-abc123`.

## Import

-> **Note:** `IMPORT_KAFKA_API_KEY` (`credentials.key`), `IMPORT_KAFKA_API_SECRET` (`credentials.secret`), and `IMPORT_KAFKA_REST_ENDPOINT` (`rest_endpoint`) environment variables must be set before importing a Kafka cluster config.

You can import a Kafka cluster config by using the Kafka cluster ID, for example:

```shell
$ export IMPORT_KAFKA_API_KEY="<kafka_api_key>"
$ export IMPORT_KAFKA_API_SECRET="<kafka_api_secret>"
$ export IMPORT_KAFKA_REST_ENDPOINT="<kafka_rest_endpoint>"
$ terraform import confluent_kafka_cluster_config.test lkc-abc123
```

!> **Warning:** Do not forget to delete terminal command history afterwards for security purposes.
