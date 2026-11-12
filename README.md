# helm-chart-unpoller

Deploys [unpoller](https://github.com/unpoller/unpoller), which polls a UniFi console's API and serves its device, client and site data as Prometheus metrics.

The chart runs one replica, metrics only:

- unpoller polls every controller site (`sites = ["all"]`), without verifying the console's certificate, and refreshes the cache `/metrics` serves every 30s.
- A ClusterIP Service on 9130, and a `ServiceMonitor` scraping it every 30s, labelled `observability.deadrobot.uk/scope: external`.
- No InfluxDB, Loki, Datadog or web server output, and no events, alarms, anomalies, IDS or syslog.
- DPI and neighbouring access points aren't collected; sites and speed tests are, and client names stay readable.
- Device tags aren't fetched: on UniFi OS consoles every call to that endpoint leaves an HTTP/2 stream open in `unifi-core`, which slowly exhausts the console's memory ([unpoller#1119](https://github.com/unpoller/unpoller/issues/1119)).
- Five of unpoller's Grafana dashboards, from [`unpoller/dashboards`](https://github.com/unpoller/dashboards) `v2.0.0`, as ConfigMaps labelled `grafana_dashboard: "1"`, reading the data source with uid `prometheus`: Client Insights, Network Sites, UAP Insights, USW Insights and USG Insights.

unpoller runs as distroless's `nonroot` user (65532) with a read-only root filesystem and no capabilities, and is probed with HTTP `GET /` on its metrics port.

## Values

| Value | Default |
| --- | --- |
| `controllerUrl` | none; required, such as `https://unifi.example.com` |
| `credentialsSecret` | none; required |
| `image.repository` | `ghcr.io/unpoller/unpoller` |
| `image.tag` | `v` and the chart's `appVersion` |
| `resources` | requests `cpu: 20m`, `memory: 64Mi`; limit `memory: 256Mi` |

`credentialsSecret` names an existing Secret with the console login in `UP_UNIFI_CONTROLLER_0_USER` and `UP_UNIFI_CONTROLLER_0_PASS`.
unpoller recommends a Limited Admin with read-only access to the Network app, restricted to local access.

## Requirements

- The `ServiceMonitor` CRD, from the Prometheus Operator.
- The Secret named by `credentialsSecret`, in the release's namespace.

## Usage

```sh
helm repo add unpoller https://tonylea.github.io/helm-chart-unpoller
helm install unpoller unpoller/unpoller \
  --set controllerUrl=https://unifi.example.com \
  --set credentialsSecret=unpoller-unifi
```

## Development

Pull requests run:

- `helm lint --strict`, and the rendered chart checked by kubeconform against Kubernetes 1.37.
- helm-unittest's tests in `tests/`.
- An install and upgrade with chart-testing in a kind cluster, with the values in `ci/kind-values.yaml`.
  `ci/kind/` holds what that install needs first: the `ServiceMonitor` CRD and a stand-in Secret.

Commits follow Conventional Commits.
Releases are versioned and tagged automatically from `main`, and published to the Helm repository on this repo's GitHub Pages.
