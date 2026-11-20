# Temporal SDK Dashboards

## Setup

* [Golang](https://docs.temporal.io/develop/go/observability)
* [Java](https://docs.temporal.io/develop/java/observability)
* [Python](https://docs.temporal.io/develop/python/observability)
* [TypeScript](https://docs.temporal.io/develop/typescript/observability)
* [.NET](https://docs.temporal.io/develop/dotnet/observability)
* [PHP](https://docs.temporal.io/develop/php/observability)

## Available metrics
[Temporal SDK metrics](https://docs.temporal.io/references/sdk-metrics)

## Grafana dashboards
- [temporal-go-java-sdks-tally.json](temporal-go-java-sdks-tally.json) for [Go](https://github.com/temporalio/sdk-go) and [Java](https://github.com/temporalio/sdk-java) SDKs using Uber Tally to emits metrics.

- [temporal-go-sdk-otel.json](temporal-go-sdk-otel.json) for [Go](https://github.com/temporalio/sdk-go) SDK using OpenTelemetry to emit metrics.

- [temporal-core-sdks-otel.json](temporal-core-sdks-otel.json) for [Core](https://github.com/temporalio/sdk-core) based SDKs. In Core based SDKs, metrics of the type Histogram
  are measured in milliseconds by default, so the dashboard is configured accordingly to display them in milliseconds.
  Use it when Prometheus shows unsuffixed names such as `temporal_request_failure` and `temporal_request_latency_bucket`, which is what the SDK's built-in Prometheus endpoint exposes by default.

- [temporal-core-sdks-prometheus.json](temporal-core-sdks-prometheus.json) for [Core](https://github.com/temporalio/sdk-core) based SDKs when Prometheus shows suffixed names such as `temporal_request_failure_total` and `temporal_request_latency_milliseconds_bucket`.
  These names appear when metrics are exported over OTLP through an OpenTelemetry Collector or Prometheus OTLP ingestion (both add suffixes by default),
  or when the built-in Prometheus endpoint has `counters_total_suffix` and `unit_suffix` enabled.
  Gauges such as `temporal_worker_task_slots_available` and `temporal_sticky_cache_size` have no suffix in either case.

### Choosing a Core SDK dashboard

Check how `temporal_request_failure` appears in Prometheus:

| Counter name | Import |
| --- | --- |
| `temporal_request_failure` | [temporal-core-sdks-otel.json](temporal-core-sdks-otel.json) |
| `temporal_request_failure_total` | [temporal-core-sdks-prometheus.json](temporal-core-sdks-prometheus.json) |

If counters end in `_total` but the latency panels are empty, check the histogram names in Prometheus and edit the imported dashboard:

- `*_bucket` with no unit (unit suffix disabled): replace `_milliseconds_bucket` with `_bucket` and `_milliseconds_count` with `_count`.
- `*_seconds_bucket` (durations reported in seconds, for example Python `durations_as_seconds` or TypeScript `useSecondsForDurations`):
  replace `_milliseconds_bucket` with `_seconds_bucket` and `_milliseconds_count` with `_seconds_count`, and change the latency panel unit from `ms` to `s`.

The Go/Java Tally dashboard uses similar metric names but is built for the Go and Java SDKs; use one of the dashboards above for Core based SDKs.

## DataDog dashboards
DataDog dashboards and related configuration are [here](datadog).
