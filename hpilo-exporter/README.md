# prometheus-helm-addons

## HP ILO Metrics Exporter

This chart provides HP Integrated Lights Out (ILO) metrics for prometheus. It is using
[Prometheus HP iLO exporter](https://github.com/IDNT/hpilo-exporter) to provide the metrics.

### Configuration

By default the pods are started on nodes labeled metrics-plane=enabled

There are two ways you can use this chart:

#### a) One pod per iLO target

If you have deployed prometheus from openstack-helm-infra, this is the fastest because there is no additional
prometheus configuration required. Example:

```
helm install --namespace=metrics --name=metrics-HostA hpilo-exporter --set monitoring.ilo.user=Administrator --set monitoring.ilo.password=TOPSECRET --set monitoring.ilo.host=HostA-ILO.mynetwork.local --set monitoring.ilo.port=443 --set endpoints.hpilo_metrics.name=ilo-HostA
```

Make sure the specified name is unique.

#### b) One pod and a custom prometheus configuration

In this case prometheus must provide the ILO host, port, username and password when sending the request 
to the metrics endpoint. Example:

```
helm install --namespace=metrics --name=metrics-ilo hpilo-exporter --set endpoints.hpilo_metrics.name=ilo
```

In case you have deployed prometheus using the openstack-helm-infra charts, you should disble automatic scraping by adding

```
--set monitoring.prometheus.hpilo_exporter.srcape=false
```

