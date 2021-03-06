---
title: amqp_0_9
type: output
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/output/amqp_0_9.go
-->


Sends messages to an AMQP (0.91) exchange. AMQP is a messaging protocol used by
various message brokers, including RabbitMQ.


import Tabs from '@theme/Tabs';

<Tabs defaultValue="common" values={[
  { label: 'Common', value: 'common', },
  { label: 'Advanced', value: 'advanced', },
]}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

```yaml
output:
  amqp_0_9:
    url: amqp://guest:guest@localhost:5672/
    exchange: benthos-exchange
    key: benthos-key
    max_in_flight: 1
```

</TabItem>
<TabItem value="advanced">

```yaml
output:
  amqp_0_9:
    url: amqp://guest:guest@localhost:5672/
    exchange: benthos-exchange
    exchange_declare:
      enabled: false
      type: direct
      durable: true
    key: benthos-key
    max_in_flight: 1
    persistent: false
    mandatory: false
    immediate: false
    tls:
      enabled: false
      skip_cert_verify: false
      root_cas_file: ""
      client_certs: []
```

</TabItem>
</Tabs>

The metadata from each message are delivered as headers.

It's possible for this output type to create the target exchange by setting
`exchange_declare.enabled` to `true`, if the exchange already exists
then the declaration passively verifies that the settings match.

TLS is automatic when connecting to an `amqps` URL, but custom
settings can be enabled in the `tls` section.

The field 'key' can be dynamically set using function interpolations described
[here](/docs/configuration/interpolation#functions).

## Performance

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

## Fields

### `url`

`string` A URL to connect to.

```yaml
# Examples

url: amqp://localhost:5672/

url: amqps://guest:guest@localhost:5672/
```

### `exchange`

`string` An AMQP exchange to publish to.

### `exchange_declare`

`object` Optionally declare the target exchange (passive).

### `exchange_declare.enabled`

`bool` Whether to declare the exchange.

### `exchange_declare.type`

`string` The type of the exchange.

Options are: `direct`, `fanout`, `topic`, `x-custom`.

### `exchange_declare.durable`

`bool` Whether the exchange should be durable.

### `key`

`string` The binding key to set for each message.

This field supports [interpolation functions](/docs/configuration/interpolation#functions).

### `max_in_flight`

`number` The maximum number of messages to have in flight at a given time. Increase this to improve throughput.

### `persistent`

`bool` Whether message delivery should be persistent (transient by default).

### `mandatory`

`bool` Whether to set the mandatory flag on published messages. When set if a published message is routed to zero queues it is returned.

### `immediate`

`bool` Whether to set the immediate flag on published messages. When set if there are no ready consumers of a queue then the message is dropped instead of waiting.

### `tls`

`object` Custom TLS settings can be used to override system defaults.

### `tls.enabled`

`bool` Whether custom TLS settings are enabled.

### `tls.skip_cert_verify`

`bool` Whether to skip server side certificate verification.

### `tls.root_cas_file`

`string` The path of a root certificate authority file to use.

### `tls.client_certs`

`array` A list of client certificates to use.

```yaml
# Examples

client_certs:
- cert: foo
  key: bar

client_certs:
- cert_file: ./example.pem
  key_file: ./example.key
```


