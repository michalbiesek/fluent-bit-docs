# AppScope

The **AppScope** input plugin allows to collect AppScope messages through a TCP Unix socket server.

## Getting Started

The plugin supports the following configuration parameters:

| Key | Description | Default |
| :--- | :--- | :--- ||
| Path | Set the absolute path to the Unix socket file. |  |
| Unix\_Perm | Set the permission of the Unix socket file. | 0644 |
| Buffer\_Chunk\_Size | By default the buffer to store the incoming AppScope messages, do not allocate the maximum memory allowed, instead it allocate memory when is required. The rounds of allocations are set by _Buffer\_Chunk\_Size_. If not set, _Buffer\_Chunk\_Size_ is equal to 32000 bytes \(32KB\).
| Buffer\_Max\_Size | Specify the maximum buffer size to receive a AppScope message. If not set, the default size will be the value of _Buffer\_Chunk\_Size_. |  |

## Getting Started

In order to receive AppScope messages, you can run the plugin from the command line or through the configuration file:

### Command Line

From the command line you can let Fluent Bit listen for _JSON_ messages with the following options:

```bash
$ fluent-bit -i appscope -p path=/tmp/in_appscope -o stdout
```

### Configuration File

In your main configuration file append the following _Input_ & _Output_ sections:

```python
[INPUT]
    Name                appscope
    Path                /tmp/in_appscope
    Buffer_Chunk_Size   32000
    Buffer_Max_Size     64000

[OUTPUT]
    Name   stdout
    Match  *
```

## Testing

Once Fluent Bit is running, you can send some messages using the AppScope tool:

```bash
$ LD_PRELOAD=lib/linux/x86_64/libscope.so SCOPE_EVENT_DEST=unix://@abstract_socket /usr/bin/top
```

In [Fluent Bit](http://fluentbit.io) we should see the following output:

```bash
Fluent Bit v1.x.x
* Copyright (C) 2019-2020 The Fluent Bit Authors
* Copyright (C) 2015-2018 Treasure Data
* Fluent Bit is a CNCF sub-project under the umbrella of Fluentd
* https://fluentbit.io

[2021/09/15 15:59:28] [ info] [engine] started (pid=3039332)
[2021/09/15 15:59:28] [ info] [storage] version=1.1.1, initializing...
[2021/09/15 15:59:28] [ info] [storage] in-memory
[2021/09/15 15:59:28] [ info] [storage] normal synchronization mode, checksum disabled, max_chunks_up=128
[2021/09/15 15:59:28] [ info] [cmetrics] version=0.2.1
[2021/09/15 15:59:28] [ info] [sp] stream processor started
[0] appscope.0: [1610637112.1728340865, {"format"=>"ndjson", "info"=>{"process"=>{"libscopever"=>"0.x.x", "pid"=>"3039329," "ppid"=>"3029347", "hostname"=>"x", "procname"=>"top", "cmd"=>"/usr/bin/top", "id"=>"x-top-/usr/bin/top"}, "configuration"=>{"current"=>{"metric"=>{"enable"=>"true", "transport"=>{"type"=>"udp", "host"=>"127.0.0.1", "port"=>"8125", "tls"=>{"enable"=>"false", "validateserver"=>"true", "cacertpath"=>""}}, "format"=>{"type"=>"statsd", "statsdprefix"=>"", "statsdmaxlen"=>512, "verbosity"=>4}}, "libscope"=>{"log"=>{"level"=>"warning", "transport"=>{"type"=>"file", "path"=>"/tmp/scope.log", "buffering"=>"line"}}, "configevent"=>"true", "summaryperiod"=>10, "commanddir"=>"/tmp"}, "event"=>{"enable"=>"true", "transport"=>{"type"=>"unix", "path"=>"@abstract_socket"}, "format"=>{"type"=>"ndjson", "maxeventpersec"=>10000, "enhancefs"=>"true"}, "watch"=>[{"type"=>"file", "name"=>"(\/logs?\/)|(\.log$)|(\.log[.\d])", "field"=>".*", "value"=>".*"}, {"type"=>"console", "name"=>"(stdout)|(stderr)", "field"=>".*", "value"=>".*"}, {"type"=>"http", "name"=>".*", "field"=>".*", "value"=>".*", "headers"=>[]}, {"type"=>"net", "name"=>".*", "field"=>".*", "value"=>".*"}, {"type"=>"fs", "name"=>".*", "field"=>".*", "value"=>".*"}, {"type"=>"dns", "name"=>".*", "field"=>".*", "value"=>".*"}]}, "payload"=>{"enable"=>"false", "dir"=>"/tmp"}, "tags"=>{}, "protocol"=>[]}}, "environment"=>{}}}]

```
