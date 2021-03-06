---
title: redis_pubsub
type: input
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/input/redis_pubsub.go
-->


Consume from a Redis publish/subscribe channel using either the SUBSCRIBE or
PSUBSCRIBE commands.

```yaml
input:
  redis_pubsub:
    url: tcp://localhost:6379
    channels:
    - benthos_chan
    use_patterns: false
```

In order to subscribe to channels using the `PSUBSCRIBE` command set
the field `use_patterns` to `true`, then you can include glob-style
patterns in your channel names. For example:

- `h?llo` subscribes to hello, hallo and hxllo
- `h*llo` subscribes to hllo and heeeello
- `h[ae]llo` subscribes to hello and hallo, but not hillo

Use `\` to escape special characters if you want to match them
verbatim.

## Fields

### `url`

`string` The URL of a Redis server to connect to.

```yaml
# Examples

url: tcp://localhost:6379
```

### `channels`

`array` A list of channels to consume from.

### `use_patterns`

`bool` Whether to use the PSUBSCRIBE command.


