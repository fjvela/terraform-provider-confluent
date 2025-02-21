# confluent_ksql_cluster Data Source

[![Open Preview](https://img.shields.io/badge/Lifecycle%20Stage-Open%20Preview-%2300afba)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy)

-> **Note:** `confluent_ksql_cluster` data source is available in **Open Preview** for early adopters. Open Preview features are introduced to gather customer feedback. This feature should be used only for evaluation and non-production testing purposes or to provide feedback to Confluent, particularly as it becomes more widely available in follow-on editions.  
**Open Preview** features are intended for evaluation use in development and testing environments only, and not for production use. The warranty, SLA, and Support Services provisions of your agreement with Confluent do not apply to Open Preview features. Open Preview features are considered to be a Proof of Concept as defined in the Confluent Cloud Terms of Service. Confluent may discontinue providing preview releases of the Open Preview features at any time in Confluent’s sole discretion.

`confluent_ksql_cluster` describes a ksqlDB cluster data source.

## Example Usage

```terraform
data "confluent_ksql_cluster" "example_using_id" {
  id = "lksqlc-abc123"
  environment {
    id = "env-xyz456"
  }
}

output "example_using_id" {
  value = data.confluent_ksql_cluster.example_using_id
}

data "confluent_ksql_cluster" "example_using_name" {
  display_name = "ksqldb_cluster"
  environment {
    id = "env-xyz456"
  }
}

output "example_using_name" {
  value = data.confluent_ksql_cluster.example_using_name
}
```

## Argument Reference

The following arguments are supported:

- `id` - (Optional String) The ID of the ksqlDB cluster, for example, `lksqlc-abc123`.
- `display_name` - (Optional String) The name of the ksqlDB cluster.
- `environment` (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Environment that the ksqlDB cluster belongs to, for example, `env-xyz456`.

-> **Note:** Exactly one from the `id` and `display_name` attributes must be specified.

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `api_version` - (Required String) An API Version of the schema version of the ksqlDB cluster, for example, `ksqldbcm/v2`.
- `kind` - (Required String) A kind of the ksqlDB cluster, for example, `Cluster`.
- `csu` - (Required Number) The number of CSUs (Confluent Streaming Units) in the ksqlDB cluster.
- `use_detailed_processing_log` (Optional Boolean) Controls whether the row data should be included in the processing log topic.
- `topic_prefix` - (Required String) Topic name prefix used by this ksqlDB cluster. Used to assign ACLs for this ksqlDB cluster to use, for example, `pksqlc-00000`.
- `rest_endpoint` - (Required String) The API endpoint of the ksqlDB cluster, for example, `https://pksqlc-00000.us-central1.gcp.glb.confluent.cloud`.
- `kafka_cluster` (Optional Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Kafka cluster that the ksqlDB cluster belongs to, for example, `lkc-abc123`.
- `credential_identity` (Optional Configuration Block) supports the following:
    - `id` - (Required String) The ID of the service or user account that the ksqlDB cluster belongs to, for example, `sa-abc123`.
- `storage` - (Required Integer) The amount of storage (in GB) provisioned to this cluster.
