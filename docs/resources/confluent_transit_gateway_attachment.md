
---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "confluent_transit_gateway_attachment Resource - terraform-provider-confluent"
subcategory: ""
description: |-
  
---

# confluent_transit_gateway_attachment Resource

[![Early Access](https://img.shields.io/badge/Lifecycle%20Stage-Early%20Access-%2345c6e8)](https://docs.confluent.io/cloud/current/api.html#section/Versioning/API-Lifecycle-Policy) [![Request Access To Networking v1](https://img.shields.io/badge/-Request%20Access%20To%20Networking%20v1-%23bc8540)](mailto:ccloud-api-access+networking-v1-early-access@confluent.io?subject=Request%20to%20join%20networking/v1%20API%20Early%20Access&body=I%E2%80%99d%20like%20to%20join%20the%20Confluent%20Cloud%20API%20Early%20Access%20for%20networking/v1%20to%20provide%20early%20feedback%21%20My%20Cloud%20Organization%20ID%20is%20%3Cretrieve%20from%20https%3A//confluent.cloud/settings/billing/payment%3E.)

-> **Note:** `confluent_transit_gateway_attachment` resource is available in **Early Access** for early adopters. Early Access features are introduced to gather customer feedback. This feature should be used only for evaluation and non-production testing purposes or to provide feedback to Confluent, particularly as it becomes more widely available in follow-on editions.  
**Early Access** features are intended for evaluation use in development and testing environments only, and not for production use. The warranty, SLA, and Support Services provisions of your agreement with Confluent do not apply to Early Access features. Early Access features are considered to be a Proof of Concept as defined in the Confluent Cloud Terms of Service. Confluent may discontinue providing preview releases of the Early Access features at any time in Confluent’s sole discretion.

`confluent_transit_gateway_attachment` provides a Transit Gateway Attachment resource that enables creating, editing, and deleting Transit Gateway Attachments on Confluent Cloud.

-> **Note:** It is recommended to set `lifecycle { prevent_destroy = true }` on production instances to prevent accidental Transit Gateway Attachment deletion. This setting rejects plans that would destroy or recreate the Transit Gateway Attachment, such as attempting to change uneditable attributes. Read more about it in the [Terraform docs](https://www.terraform.io/language/meta-arguments/lifecycle#prevent_destroy).

## Example Usage

### Example Transit Gateway Attachment on AWS

```terraform
resource "confluent_environment" "development" {
  display_name = "Development"
}

resource "confluent_network" "aws-transit-gateway-attachment" {
  display_name     = "AWS Transit Gateway Attachment Network"
  cloud            = "AWS"
  region           = "us-east-2"
  cidr             = "10.10.0.0/16"
  connection_types = ["TRANSITGATEWAY"]
  environment {
    id = confluent_environment.development.id
  }

  lifecycle {
    prevent_destroy = true
  }
}

resource "confluent_transit_gateway_attachment" "aws" {
  display_name = "AWS Transit Gateway Attachment"
  aws {
    ram_resource_share_arn = "arn:aws:ram:us-east-2:000000000000:resource-share/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx"
    transit_gateway_id     = "tgw-xxxxxxxxxxxxxxxxx"
    routes                 = ["192.168.0.0/16", "172.16.0.0/12", "100.64.0.0/10", "10.0.0.0/8"]
    enable_custom_routes   = true
  }
  environment {
    id = confluent_environment.development.id
  }
  network {
    id = confluent_network.aws-transit-gateway-attachment.id
  }

  lifecycle {
    prevent_destroy = true
  }
}
```

<!-- schema generated by tfplugindocs -->
## Argument Reference

The following arguments are supported:

- `display_name` - (Optional String) The name of the Transit Gateway Attachment.
- `environment` (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Environment that the Transit Gateway Attachment belongs to, for example, `env-abc123`.
- `network` (Required Configuration Block) supports the following:
    - `id` - (Required String) The ID of the Network that the Transit Gateway Attachment belongs to, for example, `n-abc123`.
- `aws` - (Required Configuration Block) The AWS-specific Transit Gateway Attachment details. It supports the following:
    - `ram_resource_share_arn` - (Required String) The Amazon Resource Name (ARN) of the Resource Access Manager (RAM) Resource Share of the transit gateway your Confluent Cloud network attaches to.
    - `transit_gateway_id` - (Required String) The ID of the AWS Transit Gateway that you want Confluent CLoud to be attached to. Must start with `tgw-`.
    - `routes` - (Optional List of String) List of destination routes for traffic from Confluent VPC to customer VPC via Transit Gateway.
    - `enable_custom_routes` - (Optional String) Enable custom destination routes in Confluent Cloud. Defaults to `false`.

-> **Note:** Learn more about Transit Gateway Attachment limitations on AWS [here](https://docs.confluent.io/cloud/current/networking/aws-transit-gateway.html#limitations).

## Attributes Reference

In addition to the preceding arguments, the following attributes are exported:

- `id` - (Required String) The ID of the Transit Gateway Attachment, for example, `tgwa-abc123`.
- `aws` - (Required Configuration Block) The AWS-specific Transit Gateway Attachment details. It supports the following:
    - `transit_gateway_attachment_id` - (Required String) The ID of the AWS Transit Gateway VPC Attachment that attaches Confluent VPC to Transit Gateway.

## Import

-> **Note:** `CONFLUENT_CLOUD_API_KEY` and `CONFLUENT_CLOUD_API_SECRET` environment variables must be set before importing a Transit Gateway Attachment.

You can import a Transit Gateway Attachment by using Environment ID and Transit Gateway Attachment ID, in the format `<Environment ID>/<Transit Gateway Attachment ID>`. The following example shows how to import a Transit Gateway Attachment:

```shell
$ export CONFLUENT_CLOUD_API_KEY="<cloud_api_key>"
$ export CONFLUENT_CLOUD_API_SECRET="<cloud_api_secret>"
$ terraform import confluent_transit_gateway_attachment.my_tgwa env-abc123/tgwa-abc123
```

!> **Warning:** Do not forget to delete terminal command history afterwards for security purposes.

## Getting Started
The following end-to-end examples might help to get started with `confluent_transit_gateway_attachment` resource:
  * [`dedicated-transit-gateway-attachment-aws-kafka-acls`](https://github.com/confluentinc/terraform-provider-confluent/tree/master/examples/configurations/dedicated-transit-gateway-attachment-aws-kafka-acls): _Dedicated_ Kafka cluster on AWS that is accessible via Transit Gateway Endpoint with authorization using ACLs
  * [`dedicated-transit-gateway-attachment-aws-kafka-rbac`](https://github.com/confluentinc/terraform-provider-confluent/tree/master/examples/configurations/dedicated-transit-gateway-attachment-aws-kafka-rbac): _Dedicated_ Kafka cluster on AWS that is accessible via Transit Gateway Endpoint with authorization using RBAC
