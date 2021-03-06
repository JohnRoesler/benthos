---
title: dynamodb
type: output
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/output/dynamodb.go
-->


Inserts items into a DynamoDB table.


import Tabs from '@theme/Tabs';

<Tabs defaultValue="common" values={[
  { label: 'Common', value: 'common', },
  { label: 'Advanced', value: 'advanced', },
]}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

```yaml
output:
  dynamodb:
    table: ""
    string_columns: {}
    json_map_columns: {}
    max_in_flight: 1
    batching:
      count: 1
      byte_size: 0
      period: ""
    region: eu-west-1
```

</TabItem>
<TabItem value="advanced">

```yaml
output:
  dynamodb:
    table: ""
    string_columns: {}
    json_map_columns: {}
    ttl: ""
    ttl_key: ""
    max_in_flight: 1
    batching:
      count: 1
      byte_size: 0
      period: ""
      condition:
        static: false
        type: static
    region: eu-west-1
    endpoint: ""
    credentials:
      profile: ""
      id: ""
      secret: ""
      token: ""
      role: ""
      role_external_id: ""
    max_retries: 3
    backoff:
      initial_interval: 1s
      max_interval: 5s
      max_elapsed_time: 30s
```

</TabItem>
</Tabs>

The field `string_columns` is a map of column names to string values,
where the values are
[function interpolated](/docs/configuration/interpolation#functions) per message of a
batch. This allows you to populate string columns of an item by extracting
fields within the document payload or metadata like follows:

``` yaml
string_columns:
  id: ${!json_field:id}
  title: ${!json_field:body.title}
  topic: ${!metadata:kafka_topic}
  full_content: ${!content}
```

The field `json_map_columns` is a map of column names to json paths,
where the [dot path](/docs/configuration/field_paths) is extracted from each document and
converted into a map value. Both an empty path and the path `.` are
interpreted as the root of the document. This allows you to populate map columns
of an item like follows:

``` yaml
json_map_columns:
  user: path.to.user
  whole_document: .
```

A column name can be empty:

``` yaml
json_map_columns:
  "": .
```

In which case the top level document fields will be written at the root of the
item, potentially overwriting previously defined column values. If a path is not
found within a document the column will not be populated.

### Credentials

By default Benthos will use a shared credentials file when connecting to AWS
services. It's also possible to set them explicitly at the component level,
allowing you to transfer data across accounts. You can find out more
[in this document](/docs/guides/aws).

## Performance

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

This output benefits from sending messages as a batch for improved performance.
Batches can be formed at both the input and output level. You can find out more
[in this doc](/docs/configuration/batching).

## Fields

### `table`

`string` The table to store messages in.

### `string_columns`

`object` A map of column keys to string values to store.

This field supports [interpolation functions](/docs/configuration/interpolation#functions).

```yaml
# Examples

string_columns:
  full_content: ${!content}
  id: ${!json_field:id}
  title: ${!json_field:body.title}
  topic: ${!metadata:kafka_topic}
```

### `json_map_columns`

`object` A map of column keys to [field paths](/docs/configuration/field_paths) pointing to value data within messages.

```yaml
# Examples

json_map_columns:
  user: path.to.user
  whole_document: .

json_map_columns:
  "": .
```

### `ttl`

`string` An optional TTL to set for items, calculated from the moment the message is sent.

### `ttl_key`

`string` The column key to place the TTL value within.

### `max_in_flight`

`number` The maximum number of messages to have in flight at a given time. Increase this to improve throughput.

### `batching`

`object` Allows you to configure a [batching policy](/docs/configuration/batching).

```yaml
# Examples

batching:
  byte_size: 5000
  period: 1s

batching:
  count: 10
  period: 1s

batching:
  condition:
    text:
      arg: END BATCH
      operator: contains
  period: 1m
```

### `batching.count`

`number` A number of messages at which the batch should be flushed. If `0` disables count based batching.

### `batching.byte_size`

`number` An amount of bytes at which the batch should be flushed. If `0` disables size based batching.

### `batching.period`

`string` A period in which an incomplete batch should be flushed regardless of its size.

```yaml
# Examples

period: 1s

period: 1m

period: 500ms
```

### `batching.condition`

`object` A [`condition`](/docs/components/conditions/about) to test against each message entering the batch, if this condition resolves to `true` then the batch is flushed.

### `region`

`string` The AWS region to target.

### `endpoint`

`string` Allows you to specify a custom endpoint for the AWS API.

### `credentials`

`object` Optional manual configuration of AWS credentials to use. More information can be found [in this document](/docs/guides/aws).

### `credentials.profile`

`string` A profile from `~/.aws/credentials` to use.

### `credentials.id`

`string` The ID of credentials to use.

### `credentials.secret`

`string` The secret for the credentials being used.

### `credentials.token`

`string` The token for the credentials being used, required when using short term credentials.

### `credentials.role`

`string` A role ARN to assume.

### `credentials.role_external_id`

`string` An external ID to provide when assuming a role.

### `max_retries`

`number` The maximum number of retries before giving up on the request. If set to zero there is no discrete limit.

### `backoff`

`object` Control time intervals between retry attempts.

### `backoff.initial_interval`

`string` The initial period to wait between retry attempts.

### `backoff.max_interval`

`string` The maximum period to wait between retry attempts.

### `backoff.max_elapsed_time`

`string` The maximum period to wait before retry attempts are abandoned. If zero then no limit is used.


