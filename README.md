<img align="right" width="290" height="290" src="./figs/cartoon_logo.png">

[![Slack chat][slack-img]](#get-in-touch)
[![Unit Tests][ci-img]](https://github.com/dovedb/DoveDB)

<img src="./figs/logo.jpg" width="250">

# DoveDB - a Declarative and Low-Latency Video Database

<img src="./figs/framework.png" width="560">

DoveDB, inspired by [Otif](https://favyen.com/otif.pdf) and [Sampling-Resilient Multi-Object Tracking](https://github.com/dovedb/DoveDB/blob/main/technical%20report.pdf), is a systematic data management platform with high usability and low latency created by [DILAB](https://dilab-zju.github.io/). It is integrated with the following desirable features:

  * Declarative query language
  * Spark data model
  * Lightweight ingestion scheme
  * Versatile query scenarios
  * Efficient query processing

The system framework of DoveDB is illustrated in the [Figure]("./figs/framework.png"). Towards uniform management of data sources in the format of video files or live streams, we build an abstract data model called VideoSource, which is essentially inherited from Spark’s RDD. A real-time video ingestion engine is developed to extract semantic information, including textual labels, visual features and spatio-temporal metadata. The output is then used to construct offline indexes to facilitate online query processing. DoveDB provides SQL-like syntax to support convenient model training and query processing. Users can train a visual model on specified table columns, where we assume the annotations are available. The trained model can be conceived as a user-defined function and deployed on a target VideoSource for online inference. We also provide built-in model compression techniques such as neural network pruning and knowledge distillation to reduce model size and accelerate inference speed. Our query processing engine, assisted by the constructed offline indexes, can support a diversified category of queries, including traditional selection, aggregation and join queries (which are referred as one-shot queries), as well as continuous queries deployed on video streams. In the following, we present the core modules in DoveDB. Due to space limit, some of the implementation details are provided in our technical report1.

## Get Involved

Jaeger is an open source project with open governance. We welcome contributions from the community, and we would love your help to improve and extend the project. Here are [some ideas](https://www.jaegertracing.io/get-involved/) for how to get involved. Many of them do not even require any coding.

## Features

### High Scalability

Jaeger backend is designed to have no single points of failure and to scale with the business needs.
For example, any given Jaeger installation at Uber is typically processing several billions of spans per day.

### Native support for OpenTracing

Jaeger backend, Web UI, and instrumentation libraries have been designed from the ground up to support the [OpenTracing standard](https://opentracing.io/specification/).
  * Represent traces as directed acyclic graphs (not just trees) via [span references](https://github.com/opentracing/specification/blob/master/specification.md#references-between-spans)
  * Support strongly typed span _tags_ and _structured logs_
  * Support general distributed context propagation mechanism via _baggage_

#### OpenTelemetry

Jaeger project recommends OpenTelemetry SDKs for instrumentation, instead of Jaeger's native SDKs [that are now deprecated](https://www.jaegertracing.io/docs/latest/client-libraries/#deprecating-jaeger-clients).

The OpenTracing and OpenCensus projects have merged into a new CNCF project called [OpenTelemetry](https://opentelemetry.io). The Jaeger and OpenTelemetry projects have different goals. OpenTelemetry aims to provide APIs and SDKs in multiple languages to allow applications to export various telemetry data out of the process, to any number of metrics and tracing backends. The Jaeger project is primarily the tracing backend that receives tracing telemetry data and provides processing, aggregation, data mining, and visualizations of that data. The Jaeger client libraries do overlap with OpenTelemetry in functionality. OpenTelemetry natively supports Jaeger as a tracing backend and makes Jaeger native clients unnecessary. For more information please refer to a blog post [Jaeger and OpenTelemetry](https://medium.com/jaegertracing/jaeger-and-opentelemetry-1846f701d9f2).

### Multiple storage backends

Jaeger can be used with a growing a number of storage backends:
* It natively supports two popular open source NoSQL databases as trace storage backends: Cassandra and Elasticsearch.
* It integrates via a gRPC API with other well known databases that have been certified to be Jaeger compliant: [TimescaleDB via Promscale](https://github.com/timescale/promscale), [ClickHouse](https://github.com/jaegertracing/jaeger-clickhouse).
* There is embedded database support using [Badger](https://github.com/dgraph-io/badger) and simple in-memory storage for testing setups.
* There are ongoing community experiments using other databases, such as ScyllaDB, InfluxDB, Amazon DynamoDB.

### Modern Web UI

Jaeger Web UI is implemented in Javascript using popular open source frameworks like React. Several performance
improvements have been released in v1.0 to allow the UI to efficiently deal with large volumes of data and to display
traces with tens of thousands of spans (e.g. we tried a trace with 80,000 spans).

### Cloud Native Deployment

Jaeger backend is distributed as a collection of Docker images. The binaries support various configuration methods,
including command line options, environment variables, and configuration files in multiple formats (yaml, toml, etc.).

The recommended way to deploy Jaeger in a production Kubernetes cluster is via the [Jaeger Operator](https://github.com/jaegertracing/jaeger-operator).

The Jaeger Operator provides a [CLI to generate](https://github.com/jaegertracing/jaeger-operator#experimental-generate-kubernetes-manifest-file) Kubernetes manifests from the Jaeger CR.
This can be considered as an alternative source over plain Kubernetes manifest files.

The Jaeger ecosystem also provides a [Helm chart](https://github.com/jaegertracing/helm-charts) as an alternative way to deploy Jaeger.

### Observability

All Jaeger backend components expose [Prometheus](https://prometheus.io/) metrics by default (other metrics backends are
also supported). Logs are written to standard out using the structured logging library [zap](https://github.com/uber-go/zap).

### Security

Third-party security audits of Jaeger are available in https://github.com/jaegertracing/security-audits. Please see [Issue #1718](https://github.com/jaegertracing/jaeger/issues/1718) for the summary of available security mechanisms in Jaeger.

### Backwards compatibility with Zipkin

Although we recommend instrumenting applications with OpenTelemetry, if your organization has already invested in the instrumentation
using Zipkin libraries, you do not have to rewrite all that code. Jaeger provides backwards compatibility with Zipkin
by accepting spans in Zipkin formats (Thrift or JSON v1/v2) over HTTP. Switching from Zipkin backend is just a matter
of routing the traffic from Zipkin libraries to the Jaeger backend.

## Version Compatibility Guarantees

Occasionally, CLI flags can be deprecated due to, for example, usability improvements or new functionality.
In such situations, developers introducing the deprecation are required to follow [these guidelines](./CONTRIBUTING.md#deprecating-cli-flags).

In short, for a deprecated CLI flag, you should expect to see the following message in the `--help` documentation:
```
(deprecated, will be removed after yyyy-mm-dd or in release vX.Y.Z, whichever is later)
```

A grace period of at least **3 months** or **two minor version bumps** (whichever is later) from the first release
containing the deprecation notice will be provided before the deprecated CLI flag _can_ be deleted.

For example, consider a scenario where v1.28.0 is released on 01-Jun-2021 containing a deprecation notice for a CLI flag.
This flag will remain in a deprecated state until the later of 01-Sep-2021 or v1.30.0 where it _can_ be removed on or after either of those events.
It may remain deprecated for longer than the aforementioned grace period.

## Related Repositories

### Documentation

  * Published: https://www.jaegertracing.io/docs/
  * Source: https://github.com/jaegertracing/documentation

### Instrumentation Libraries

Jaeger project recommends OpenTelemetry SDKs for instrumentation, instead of Jaeger's native SDKs [that are now deprecated](https://www.jaegertracing.io/docs/latest/client-libraries/#deprecating-jaeger-clients).

### Deployment

  * [Jaeger Operator for Kubernetes](https://github.com/jaegertracing/jaeger-operator#getting-started)

### Components

 * [UI](https://github.com/jaegertracing/jaeger-ui)
 * [Data model](https://github.com/jaegertracing/jaeger-idl)

## Building From Source

See [CONTRIBUTING](./CONTRIBUTING.md).

## Contributing

See [CONTRIBUTING](./CONTRIBUTING.md).

Thanks to all the people who already contributed!

<a href="https://github.com/jaegertracing/jaeger/graphs/contributors">
  <img src="https://contributors-img.web.app/image?repo=jaegertracing/jaeger" />
</a>

### Maintainers

Rules for becoming a maintainer are defined in the [GOVERNANCE](./GOVERNANCE.md) document.
Below are the official maintainers of the Jaeger project.
Please use `@jaegertracing/jaeger-maintainers` to tag them on issues / PRs.

* [@albertteoh](https://github.com/albertteoh)
* [@joe-elliott](https://github.com/joe-elliott)
* [@pavolloffay](https://github.com/pavolloffay)
* [@yurishkuro](https://github.com/yurishkuro)

Some repositories under [jaegertracing](https://github.com/jaegertracing) org have additional maintainers.

### Emeritus Maintainers

We are grateful to our former maintainers for their contributions to the Jaeger project.

* [@black-adder](https://github.com/black-adder)
* [@jpkrohling](https://github.com/jpkrohling)
* [@objectiser](https://github.com/objectiser)
* [@tiffon](https://github.com/tiffon)
* [@vprithvi](https://github.com/vprithvi)

## Project Status Meetings

The Jaeger maintainers and contributors meet regularly on a video call. Everyone is welcome to join, including end users. For meeting details, see https://www.jaegertracing.io/get-in-touch/.

## Roadmap

See https://www.jaegertracing.io/docs/roadmap/

## Get in Touch

Have questions, suggestions, bug reports? Reach the project community via these channels:

 * [Slack chat room `#jaeger`][slack] (need to join [CNCF Slack][slack-join] for the first time)
 * [`jaeger-tracing` mail group](https://groups.google.com/forum/#!forum/jaeger-tracing)
 * GitHub [issues](https://github.com/jaegertracing/jaeger/issues) and [discussions](https://github.com/jaegertracing/jaeger/discussions)

## Adopters

Jaeger as a product consists of multiple components. We want to support different types of users,
whether they are only using our instrumentation libraries or full end to end Jaeger installation,
whether it runs in production or you use it to troubleshoot issues in development.

Please see [ADOPTERS.md](./ADOPTERS.md) for some of the organizations using Jaeger today.
If you would like to add your organization to the list, please comment on our
[survey issue](https://github.com/jaegertracing/jaeger/issues/207).

## License

Copyright (c) The Jaeger Authors. [Apache 2.0 License](./LICENSE).
