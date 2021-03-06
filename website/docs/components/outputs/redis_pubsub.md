---
title: redis_pubsub
type: output
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/output/redis_pubsub.go
-->


Publishes messages through the Redis PubSub model. It is not possible to
guarantee that messages have been received.

```yaml
output:
  redis_pubsub:
    url: tcp://localhost:6379
    channel: benthos_chan
    max_in_flight: 1
```

This output will interpolate functions within the channel field, you
can find a list of functions [here](/docs/configuration/interpolation#functions).

## Performance

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

## Fields

### `url`

`string` The URL of a Redis server to connect to.

```yaml
# Examples

url: tcp://localhost:6379
```

### `channel`

`string` The channel to publish messages to.

This field supports [interpolation functions](/docs/configuration/interpolation#functions).

### `max_in_flight`

`number` The maximum number of messages to have in flight at a given time. Increase this to improve throughput.


