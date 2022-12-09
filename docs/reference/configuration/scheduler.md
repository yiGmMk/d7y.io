---
id: scheduler
title: Scheduler
---

## Configure Scheduler YAML File {#configure-scheduler-yaml-file}

The default path for the scheduler yaml configuration file is `/etc/dragonfly/scheduler.yaml` in linux,
and the default path is `$HOME/.dragonfly/config/scheduler.yaml` in darwin.

```yaml
# Server scheduler instance configuration.
server:
  # # Access ip for other services,
  # # when local ip is different with access ip, advertiseIP should be set.
  # advertiseIP: 127.0.0.1
  # # Listen ip.
  # listenIP: 0.0.0.0
  # Port is the ip and port scheduler server listens on.
  port: 8002
  # # Server host.
  # host: localhost
  # WorkHome is working directory.
  # In linux, default value is /usr/local/dragonfly.
  # In macos(just for testing), default value is /Users/$USER/.dragonfly.
  workHome: ''
  # cacheDir is dynconfig cache directory.
  # In linux, default value is /var/cache/dragonfly.
  # In macos(just for testing), default value is /Users/$USER/.dragonfly/cache.
  logDir: ''
  # dataDir is the directory.
  # In linux, default value is /var/lib/dragonfly.
  # In macos(just for testing), default value is /Users/$USER/.dragonfly/data.
  cacheDir: ''
  # pluginDir is the plugin directory.
  # In linux, default value is /usr/local/dragonfly/plugins.
  # In macos(just for testing), default value is /Users/$USER/.dragonfly/plugins.
  pluginDir: ''
  # logDir is the log directory.
  # In linux, default value is /var/log/dragonfly.
  # In macos(just for testing), default value is /Users/$USER/.dragonfly/logs.
  dataDir: ''

# Scheduler policy configuration.
scheduler:
  # Algorithm configuration to use different scheduling algorithms,
  # default configuration supports "default" and "ml"
  # "default" is the rule-based scheduling algorithm,
  # "ml" is the machine learning scheduling algorithm
  # It also supports user plugin extension, the algorithm value is "plugin",
  # and the compiled `d7y-scheduler-plugin-evaluator.so` file is added to
  # the dragonfly working directory plugins.
  algorithm: default
  # backToSourceCount is single task allows the peer to back-to-source count.
  backToSourceCount: 3
  # retryBackToSourceLimit reaches the limit, then the peer back-to-source.
  retryBackToSourceLimit: 5
  # Retry scheduling limit times.
  retryLimit: 10
  # Retry scheduling interval.
  retryInterval: 50ms
  # GC metadata configuration.
  gc:
    # pieceDownloadTimeout is the timeout of downloading piece.
    pieceDownloadTimeout: 30m
    # peerGCInterval is the interval of peer gc.
    peerGCInterval: 10s
    # peerTTL is the ttl of peer.
    peerTTL: 24h
    # taskGCInterval is the interval of task gc.
    taskGCInterval: 30m
    # hostGCInterval is the interval of host gc.
    hostGCInterval: 1h

# Dynamic data configuration.
dynConfig:
  # Dynamic config refresh interval.
  refreshInterval: 1m

# Scheduler host configuration.
host:
  # idc is the idc of scheduler instance.
  idc: ''
  # netTopology is the net topology of scheduler instance.
  netTopology: ''
  # location is the location of scheduler instance.
  location: ''

# Manager configuration.
manager:
  # addr is manager access address.
  addr: manager-service:65003
  # schedulerClusterID cluster id to which scheduler instance belongs.
  schedulerClusterID: 1
  # keepAlive keep alive configuration.
  keepAlive:
    # KeepAlive interval.
    interval: 5s

# Seed peer configuration.
seedPeer:
  # Scheduler enable seed peer as P2P peer,
  # if the value is false, P2P network will not be back-to-source through
  # seed peer but by peer and preheat feature does not work.
  enable: true

# Machinery async job configuration,
# see https://github.com/RichardKnop/machinery.
job:
  # Scheduler enable job service.
  enable: true
  # Number of workers in global queue.
  globalWorkerNum: 500
  # Number of workers in scheduler queue.
  schedulerWorkerNum: 500
  # Number of workers in local queue.
  localWorkerNum: 1000
  # Redis configuration.
  redis:
    # Redis addresses.
    addrs:
      - dragonfly:6379
    # Redis sentinel master name.
    masterName: ''
    # Redis username.
    username: ''
    # Redis password.
    password: ''
    # Redis broker DB.
    brokerDB: 1
    # Redis backend DB.
    backendDB: 2

# Store task download information.
storage:
  # maxSize sets the maximum size in megabytes of storage file.
  maxSize: 100
  # maxBackups sets the maximum number of storage files to retain.
  maxBackups: 10
  # bufferSize sets the size of buffer container,
  # if the buffer is full, write all the records in the buffer to the file.
  bufferSize: 100

# Enable prometheus metrics.
metrics:
  # Scheduler enable metrics service.
  enable: true
  # Metrics service address.
  addr: ':8000'
  # Enable peer host metrics.
  enablePeerHost: false

security:
  # autoIssueCert indicates to issue client certificates for all grpc call.
  # If AutoIssueCert is false, any other option in Security will be ignored.
  autoIssueCert: false
  # caCert is the root CA certificate for all grpc tls handshake, it can be path or PEM format string.
  caCert: ''
  # tlsVerify indicates to verify certificates.
  tlsVerify: false
  # tlsPolicy controls the grpc shandshake behaviors:
  #   force: both ClientHandshake and ServerHandshake are only support tls
  #   prefer: ServerHandshake supports tls and insecure (non-tls), ClientHandshake will only support tls
  #   default: ServerHandshake supports tls and insecure (non-tls), ClientHandshake will only support insecure (non-tls)
  # Notice: If the drgaonfly service has been deployed, a two-step upgrade is required.
  # The first step is to set tlsPolicy to default, and then upgrade the dragonfly services.
  # The second step is to set tlsPolicy to prefer, and then completely upgrade the dragonfly services.
  tlsPolicy: 'prefer'
  certSpec:
    # validityPeriod is the validity period  of certificate.
    validityPeriod: 4320h

network:
  # Enable ipv6.
  enableIPv6: false

# Console shows log on console.
console: false

# Whether to enable debug level logger and enable pprof.
verbose: false

# Listen port for pprof, only valid when the verbose option is true
# default is -1. If it is 0, pprof will use a random port.
pprof-port: -1

# Jaeger endpoint url, like: http://jaeger.dragonfly.svc:14268/api/traces.
jaeger: ''
```
