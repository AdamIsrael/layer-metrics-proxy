# Overview

This is an example of a VNF charm that collecting metrics against a remote machine. This can be used with [Open Source Mano](https://osm.etsi.org/) (OSM).

Please refer to [Creating your own VNF charm](https://osm.etsi.org/wikipub/index.php/Creating_your_own_VNF_charm_(Release_FOUR)) for more information.

# Usage

Metrics collection is included in the `vnfproxy` layer. The functionality is available to any proxy charm, if the metrics to be collected are defined.

## Defining metrics

The metrics to be collected are defined in `metrics.yaml` in the root directory of your charm layer.

```yaml
metrics:
    uptime:
        type: gauge
        description: Uptime of the VNF
        command: awk '{print $1}' /proc/uptime
```

## How it works

Normally, Juju will collect metrics from the unit where the charm is running. When implementing a proxy charm, however, this collection needs to be run from the virtual machine running your VNF.

The `metrics-proxy` layer implements a modified version of `hooks/collect-metrics` that will perform the command specified in `metrics.yaml` remotely via SSH.

## Limitations

Proxy metric collection has only been tested using SSH to remotely access your VNF. If you need to collect metrics via another method (REST API, etc), it will require a custom `hooks/collect-metrics` to use that mechanism.
