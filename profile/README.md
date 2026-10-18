# KafScale

**Kafka-compatible streaming that keeps your log in S3. Stateless brokers, standard Kafka clients, Apache 2.0.**

[![Release](https://img.shields.io/github/v/release/KafScale/platform?sort=semver)](https://github.com/KafScale/platform/releases)
[![Coverage](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/KafScale/platform/gh-pages/badges/coverage.json)](https://github.com/KafScale/platform/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com/KafScale/platform/blob/main/LICENSE)

Most Kafka clusters are durable pipes. Producers write, consumers read, and teams replay when something breaks. Yet every one of them still runs stateful brokers with local disks, replica rebalancing and capacity held back for peak load.

KafScale starts from a simple premise: durable logs belong in object storage, not in stateful brokers. Brokers speak the Kafka protocol and hold no durable state. S3 is the source of truth. Your existing Kafka clients and tools keep working.

## How it works

- **Stateless brokers.** Brokers handle Kafka traffic and buffer segments in memory. Scale them up and down without moving data or rebalancing partitions.
- **S3 as the log.** Immutable segments and index files go straight to S3 or any S3-compatible store.
- **etcd for metadata.** Topics, offsets and consumer group state live in etcd.
- **One endpoint.** A proxy rewrites Kafka metadata responses, so clients see one DNS name while brokers scale behind it.
- **Processors that skip the broker.** Processors read finished segments from S3 directly, so analytics never competes with streaming. The `.kfs` segment format is documented.
- **Kubernetes operator.** Install the operator, define a topic, produce with the Kafka tools you already use.

## When it fits, and when it does not

KafScale fits ETL, logs, async events, backup and long retention, where 200 to 500 ms latency is fine and you want to run it yourself without managing disks.

It is not for you if you need sub-10 ms latency, Kafka transactions and exactly-once, compacted topics, or a fully managed service.

## Get started

Follow the [quickstart](https://kafscale.io/quickstart/) to install the operator, create a topic and stream messages. To try it on your laptop with a local MinIO bucket:

```bash
git clone https://github.com/KafScale/platform.git && cd platform
make demo
```

The [platform README](https://github.com/KafScale/platform#readme) walks through backing up an existing Kafka cluster to S3 with kaf-mirror in about ten minutes.

## Links

- Website and docs: [kafscale.io](https://kafscale.io) and [kafscale.io/docs](https://kafscale.io/docs/)
- Architecture: [kafscale.io/architecture](https://kafscale.io/architecture/)
- Comparison with other Kafka-compatible platforms: [kafscale.io/comparison](https://kafscale.io/comparison/)
- Releases: [GitHub releases](https://github.com/KafScale/platform/releases) and [release notes](https://kafscale.io/releases/)
- Questions and ideas: [Discussions](https://github.com/KafScale/platform/discussions)

## Repositories

| Repo | What it is |
|---|---|
| [platform](https://github.com/KafScale/platform) | Broker, proxy, operator, processors, console, Helm chart and docs |
| [onboarding-tutorials](https://github.com/KafScale/onboarding-tutorials) | Examples for developers, including Spring Boot, Flink and Spark |
| [k6suite](https://github.com/KafScale/k6suite) | k6 load and stress tests for KafScale |
| [kaf-mirror](https://github.com/KafScale/kaf-mirror) | Kafka-to-Kafka replication on franz-go with a web dashboard, REST API and CLI. Fork of [scalytics/kaf-mirror](https://github.com/scalytics/kaf-mirror) |

The latest release is [v1.6.2](https://github.com/KafScale/platform/releases/tag/v1.6.2).

## Related: KafClaw

[KafClaw](https://github.com/KafClaw) is our sister project: agents for your data streaming. [KafClaw](https://github.com/KafClaw/KafClaw) is a compact agent runtime written in Go. It uses Kafka topics to connect agents into groups and swarms, and works with KafScale as the event transport. Docs are at [kafclaw.scalytics.io](https://kafclaw.scalytics.io).

More open source from the same team is listed at [scalytics.io/open-source](https://www.scalytics.io/open-source). Why KafScale exists, in the words of its author: [KafScale on novatechflow](https://www.novatechflow.com/p/kafscale.html).

## Get involved

Bugs and feature requests go to [platform issues](https://github.com/KafScale/platform/issues). Pull requests are welcome, and no CLA is required. Read [CONTRIBUTING](https://github.com/KafScale/platform/blob/main/CONTRIBUTING.md) and the [code of conduct](https://github.com/KafScale/platform/blob/main/CODE_OF_CONDUCT.md) first. Report security problems as described in the [security policy](https://github.com/KafScale/platform/blob/main/SECURITY.md), not in public issues.

KafScale is Apache 2.0 licensed and sponsored by [Scalytics, Inc.](https://www.scalytics.io) Need it run or supported for you? See [Lascaris](https://www.scalytics.io/lascaris).
