# Fluent Bit Helm Chart

Fluent Bit chart for installing  daemonset for forwarding logs to elasticsearch and other agents.

## Choosing forward type
Change the backend type on values
```
backend:
  type: es

```

## Forwarding logs to AWS managed Elasticsearch
```
backend:
  type: es
  es:
    host: # host without https://
    port: 443
    # Elastic Index Name
    index: kubernetes_cluster
    type: flb_type
    logstash_prefix: k8s
    replace_dots: "On"
    logstash_format: "On"
    retry_limit: "False"
    time_key: "@timestamp"
    # Optional username credential for Elastic X-Pack access
    http_user:
    # Password for user defined in HTTP_User
    http_passwd:
    # Optional TLS encryption to ElasticSearch instance
    tls: "on"
    tls_verify: "on"
    # TLS certificate for the Elastic (in PEM format). Use if tls=on and tls_verify=on.
    tls_ca: ""
    # TLS debugging levels = 1-4
    tls_debug: 1

```

## Adding log exclusion
We may not want to send logs of all containers of all namespaces. For that, we can add exclusion filter. In the below example, it will not send logs of monitoring namespace.
```
input:
  tail:
    memBufLimit: 15MB
    parser: docker
    path: /var/log/containers/*.log
    ignore_older: ""
    dockerMode: false
    dockerModeFlush: 4
    exclude_path: "/var/log/containers/*monitoring*.log"

```


[This chart is initially sourced from [stable/fluent-bit](https://github.com/helm/charts/tree/master/stable/fluent-bit)]
