# Changelog

All notable changes to this project will be documented in this file. See [commit-and-tag-version](https://github.com/absolute-version/commit-and-tag-version) for commit guidelines.

## [2.0.0](https://github.com/tonylea/helm-chart-unpoller/compare/v1.0.1...v2.0.0) (2026-11-26)

### ⚠ BREAKING CHANGES

* an install whose scraper selects on the old scope label must now set it in
  serviceMonitor.labels.

### Features

* take the ServiceMonitor's extra labels from values ([af3249a](https://github.com/tonylea/helm-chart-unpoller/commit/af3249a08d3077344a1742d70e10f03b2bd1d297))

## [1.0.1](https://github.com/tonylea/helm-chart-unpoller/compare/v1.0.0...v1.0.1) (2026-11-26)

### Bug Fixes

* drop the default requests and limits ([33595ab](https://github.com/tonylea/helm-chart-unpoller/commit/33595aba03e134112fe26c34a3140600ddab0241))

### Documentation

* drop the reason for setting no resources ([9550309](https://github.com/tonylea/helm-chart-unpoller/commit/95503098a3784e503c7600594561ec1d23cc022a))
* state the request as a recommendation ([e32acc1](https://github.com/tonylea/helm-chart-unpoller/commit/e32acc17ceccacb16974556bc2786063cd304046))

## 1.0.0 (2026-11-12)

### Features

* deploy unpoller with its metrics, ServiceMonitor and dashboards ([3a0269b](https://github.com/tonylea/helm-chart-unpoller/commit/3a0269b6096f38aa3c4c8cb8d46cf5768ef5b9f6))

### Documentation

* describe the chart, its values and how to contribute ([0e7018f](https://github.com/tonylea/helm-chart-unpoller/commit/0e7018f59b0bc4f03775c0fbb7995bc88a62b57f))
