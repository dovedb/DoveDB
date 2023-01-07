<img align="right" width="290" height="290" src="./figs/cartoon_logo.png">

[![Slack chat][slack-img]](#get-in-touch)
[![Unit Tests][ci-img]](https://github.com/dovedb/DoveDB)

<img src="./figs/logo.jpg" width="250">

# DoveDB - a Declarative and Low-Latency Video Database

<img src="./figs/framework.png" width="560">

DoveDB, inspired by [Otif](https://favyen.com/otif.pdf) and Multi-Object Tracking, is a systematic data management platform with high usability and low latency created by [DILAB](https://dilab-zju.github.io/). It is integrated with the following desirable features:

  * Declarative query language
  * Spark data model
  * Lightweight ingestion scheme
  * Versatile query scenarios
  * Efficient query processing

The system framework of DoveDB is illustrated in the [Figure]("./figs/framework.png"). Towards uniform management of data sources in the format of video files or live streams, we build an abstract data model called VideoSource, which is essentially inherited from Spark’s RDD. A real-time video ingestion engine is developed to extract semantic information, including textual labels, visual features and spatio-temporal metadata. The output is then used to construct offline indexes to facilitate online query processing. DoveDB provides SQL-like syntax to support convenient model training and query processing. Users can train a visual model on specified table columns, where we assume the annotations are available. The trained model can be conceived as a user-defined function and deployed on a target VideoSource for online inference. We also provide built-in model compression techniques such as neural network pruning and knowledge distillation to reduce model size and accelerate inference speed. Our query processing engine, assisted by the constructed offline indexes, can support a diversified category of queries, including traditional selection, aggregation and join queries (which are referred as one-shot queries), as well as continuous queries deployed on video streams. In the following, we present the core modules in DoveDB. Due to space limit, some of the implementation details are provided in our [technical report](https://github.com/dovedb/DoveDB/blob/main/technical%20report.pdf).

## Get Involved

DoveDB is an open source project with open governance. We welcome contributions from the community, and we hope you can help improve and extend the project. Below I present a system overview of DoveDB.

## Data Model

DoveDB is built upon Spark and supports queries against historical data and streaming data. We customize RDD to derive an abstract data model called VideoSource to uniformly manage data sources in the format of video files or live streams. The following SQL statements are examples of encapsulating a disk file or a network live stream as VideoSource.

## Modern Web UI

Jaeger Web UI is implemented in Javascript using popular open source frameworks like React. Several performance
improvements have been released in v1.0 to allow the UI to efficiently deal with large volumes of data and to display
traces with tens of thousands of spans (e.g. we tried a trace with 80,000 spans).

## Cloud Native Deployment

Jaeger backend is distributed as a collection of Docker images. The binaries support various configuration methods,
including command line options, environment variables, and configuration files in multiple formats (yaml, toml, etc.).

The recommended way to deploy Jaeger in a production Kubernetes cluster is via the [Jaeger Operator](https://github.com/jaegertracing/jaeger-operator).

The Jaeger Operator provides a [CLI to generate](https://github.com/jaegertracing/jaeger-operator#experimental-generate-kubernetes-manifest-file) Kubernetes manifests from the Jaeger CR.
This can be considered as an alternative source over plain Kubernetes manifest files.

The Jaeger ecosystem also provides a [Helm chart](https://github.com/jaegertracing/helm-charts) as an alternative way to deploy Jaeger.

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

[ci-img]: https://github.com/jaegertracing/jaeger/workflows/Unit%20Tests/badge.svg?branch=main
[slack-img]: https://img.shields.io/badge/slack-join%20chat%20%E2%86%92-brightgreen?logo=slack
