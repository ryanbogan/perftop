[![CD](https://github.com/opensearch-project/perftop/workflows/CD/badge.svg)](https://github.com/opensearch-project/perftop/actions?query=workflow%3ACD)
[![Documentation](https://img.shields.io/badge/api-reference-blue.svg)](https://opensearch.org/docs/monitoring-plugins/pa/dashboards/)
[![Chat](https://img.shields.io/badge/chat-on%20forums-blue)](https://discuss.opendistrocommunity.dev/c/performance-analyzer/)
![PRs welcome!](https://img.shields.io/badge/PRs-welcome!-success)

<img src="https://opensearch.org/assets/brand/SVG/Logo/opensearch_logo_default.svg" height="64px"/>

<!-- TOC -->

- [OpenSearch Perftop](#opensearch-perftop)
- [Preset Dashboards](#preset-dashboards)
- [Installation](#installation)
- [Usage](#usage)
- [Build](#build)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Code of Conduct](#code-of-conduct)
- [Security](#security)
- [License](#license)
- [Copyright](#copyright)

<!-- /TOC -->

# OpenSearch PerfTop

The PerfTop CLI provides pre-configured dashboards for analyzing cluster, node, shard performance, and more. Use custom JSON templates to create the dashboards you need to diagnose your cluster performance.

## Preset Dashboards

* All sorts are in decreasing order.
* Bar graphs show aggregated metrics on cluster-level unless stated otherwise.
* Line graphs generate random colors. If no data shows up, it's likely that the data is 0.

### ClusterOverview
<img src="./images/ClusterOverview.png" alt="term" width="1000">
This dashboard can be used to see what operations are running on cluster-level and on shard-level.
With this, users can measure which operation/node is consuming the most CPU and what latency the cluster is experiencing.

* "Resource Metrics" is sorted by CPU_Utilization.
* "Shard Operation Metrics" is sorted by ShardEvents.
* "Workload" is sorted by HTTP_RequestDocs.

### ClusterNetworkMemoryAnalysis
<img src="./images/ClusterNetworkMemoryAnalysis.png" alt="term" width="1000">
This dashboard shows shard-level operation, the network, and memory metrics.
It can be used to analyze which shard is doing the most workload, the amount of data being transmitted by the network,
which disk is performing poorly, and which circuit breaker type is experiencing OutOfMemory exceptions.

* "Shard Operation Metrics" is sorted by ShardEvents.
* "Circuit Breaker - Tripped Events / Estimated and Configured Limits" is sorted by CB_TrippedEvents.

### ClusterThreadAnalysis
<img src="./images/ClusterThreadAnalysis.png" alt="term" width="1000">
This dashboard shows low-level metrics about threads/threadpools, which can be used to analyze
which threadpool type is rejecting operations due to its queue being too large,
which thread is running/waiting for too long and results in blocks,
and which thread operation is having issues with memory and is having to load it from the disk.

* "Thread Pool - Queue Size and Rejected Requests" is sorted by ThreadPool_RejectedReqs.
* "Thread - Blocked Time" is sorted by Thread_Blocked_Time.
* "Page Faults" is sorted by Paging_MajfltRate.
* All "Context Switch" tables are sorted by Sched_*.

### NodeAnalysis
<img src="./images/NodeAnalysis.png" alt="term" width="1000">
This dashboard has the most wide ranges of metric types.
It shows shard-level operation metrics, thread metrics, JVM-related metrics
(e.g. heap usage, garbage collection), and network packet drop rate metrics.
After gaining some insights from the previous dashboards, users can specify which node to fetch metrics for.

This dashboard supports `--nodename $NODENAME` command-line argument for displaying metric data for
ONLY the node that starts with `$NODENAME`. If not provided, this dashboard will include all nodes.
Users can also define different node names for each type of graphs from the JSON dashboard config.

* "Shard Operation Metrics" is sorted by ShardEvents.
* "Shard Request Cache Miss" is sorted by Cache_Request_Miss.
* "Thread Pool - Queue Size and Rejected Requests" is sorted by ThreadPool_RejectedReqs.
* "Heap Usage" is sorted by Heap_Used.
* If no `--nodename $NODENAME` is provided, the bar graphs will be aggregated metrics on cluster-level.

## Installation
Excutables:

Download the executables and preset JSON dashboard configs [here](https://github.com/opensearch-project/perftop/releases/tag/1.1.0.0).

Supported platforms: Linux, macOS

## Usage

Excutables:

```
./opensearch-perf-top-${PLATFORM} --dashboard $JSON --endpoint $ENDPOINT
```

## Build

Prerequisites:
- `node` (version >= v10.0 < v11.0)
- `npm`

1. Clone/download from Github
2. Run `./gradlew build -Dbuild.linux={true/false} -Dbuild.macos={true/false}`. This will run the following:
   1. `npm install` - locally installs dependencies
   2. `npm run build-{linux/macos}` - creates "opensearch-perf-top-{linux/macos}" executables.
3. For cleaning, run `./gradlew clean` which will run:
   1. `npm run clean` - deletes locally installed dependencies and executables

To run PerfTop without (re)creating the executables every code change:
```
node ./lib/bin.js --dashboard $JSON
```

## Documentation

Please refer to the [technical documentation](https://opensearch.org/docs/monitoring-plugins/pa/index/) for detailed information on installing and configuring Perftop.

## Contributing

See [developer guide](DEVELOPER_GUIDE.md) and [how to contribute to this project](CONTRIBUTING.md).

## Code of Conduct

This project has adopted the [Amazon Open Source Code of Conduct](CODE_OF_CONDUCT.md). For more information see the [Code of Conduct FAQ](https://aws.github.io/code-of-conduct-faq), or contact [opensource-codeofconduct@amazon.com](mailto:opensource-codeofconduct@amazon.com) with any additional questions or comments.

## Security

If you discover a potential security issue in this project we ask that you notify AWS/Amazon Security via our [vulnerability reporting page](http://aws.amazon.com/security/vulnerability-reporting/). Please do **not** create a public GitHub issue.

## License

This project is licensed under the [Apache v2.0 License](LICENSE).

## Copyright

Copyright 2020-2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
