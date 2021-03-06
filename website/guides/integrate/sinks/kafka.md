---
last_modified_on: "2020-04-19"
$schema: "/.meta/.schemas/guides.json"
title: "Send logs to Kafka"
description: "A simple guide to send logs to Kafka in just a few minutes."
author_github: https://github.com/binarylogic
cover_label: "Kafka Integration"
tags: ["type: tutorial","domain: sinks","sink: kafka"]
hide_pagination: true
---

import ConfigExample from '@site/src/components/ConfigExample';
import DaemonDiagram from '@site/src/components/DaemonDiagram';
import Jump from '@site/src/components/Jump';
import Steps from '@site/src/components/Steps';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

Logs are an _essential_ part of observing any
service; without them you are flying blind. But collecting and analyzing them
can be a real challenge -- especially at scale. Not only do you need to solve
the basic task of collecting your logs, but you must do it
in a reliable, performant, and robust manner. Nothing is more frustrating than
having your logs pipeline fall on it's face during an
outage, or even worse, disrupt more important services!

Fear not! In this guide we'll show you how to send send logs to [Kafka][urls.kafka]
and build a logs pipeline that will be the backbone of
your observability strategy.

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/guides/integrate/sinks/kafka.md.erb
-->

## Background

### What is Kafka?

[Apache Kafka][urls.kafka] is an open source project for a distributed publish-subscribe messaging system rethought as a distributed commit log. Kafka stores messages in topics that are partitioned and replicated across multiple brokers in a cluster. Producers send messages to topics from which consumers read. This makes it an excellent candidate for durably storing logs and metrics data.

## Strategy

### How This Guide Works

We'll be using [Vector][urls.vector_website] to accomplish this task. Vector
is a [popular][urls.vector_stars] [open-source][urls.vector_repo] utility for
building observability pipelines. It's written in [Rust][urls.rust], making it
lightweight, [ultra-fast][urls.vector_performance] and highly reliable. And
we'll be deploying Vector as a
[daemon][docs.strategies#daemon].

The [daemon deployment strategy][docs.strategies#daemon] is designed for data
collection on a single host. Vector runs in the background, in its own process,
collecting _all_ data for that host.
Typically data is collected from a process manager, such as Journald via
Vector's [`journald` source][docs.sources.journald], but can be collected
through any of Vector's [sources][docs.sources].
The following diagram demonstrates how it works.

<DaemonDiagram
  platformName={null}
  sourceName={null}
  sinkName={"kafka"} />

### What We'll Accomplish

To be clear, here's everything we'll accomplish in this short guide:

<ul className="list--icons list--icons--checks list--indent">
  <li>
    Collect your logs from one or more sources
  </li>
  <li>
    Send logs to Kafka.
    <ul>
      <li>Leverage any of AWS' IAM strategies.</li>
      <li>Optionally compress data to maximize throughput.</li>
      <li>Automatically retry failed requests, with backoff.</li>
      <li>Buffer your data in-memory or on-disk for performance and durability.</li>
    </ul>
  </li>
  <li className="list--icons--arrow text--pink text--bold">All in just a few minutes!</li>
</ul>

## Tutorial

<Tabs
  centered={true}
  className="rounded"
  defaultValue="x86_64"
  values={[{"label":"x86_64","value":"x86_64"},{"label":"ARM64","value":"arm64"},{"label":"ARMv7","value":"armv7"}]}>

<TabItem value="x86_64">
<Steps headingDepth={3}>
<ol>
<li>

### Download the Vector `.deb` package

```bash
curl --proto '=https' --tlsv1.2 -O https://packages.timber.io/vector/0.9.X/vector-amd64.deb
```

[Looking for a different version?][docs.package_managers.dpkg#versions]

</li>
<li>

### Install the downloaded package

```bash
sudo dpkg -i vector-amd64.deb
```

</li>
<li>

### Configure Vector

<ConfigExample
  format="toml"
  path={"/etc/vector/vector.toml"}
  sourceName={"file"}
  sinkName={"kafka"} />

</li>
<li>

### Start Vector

```bash
sudo systemctl start vector
```

</li>
</ol>
</Steps>
</TabItem>
<TabItem value="arm64">
<Steps headingDepth={3}>
<ol>
<li>

### Download the Vector `.deb` package

```bash
curl --proto '=https' --tlsv1.2 -O https://packages.timber.io/vector/0.9.X/vector-arm64.deb
```

[Looking for a different version?][docs.package_managers.dpkg#versions]

</li>
<li>

### Install the downloaded package

```bash
sudo dpkg -i vector-arm64.deb
```

</li>
<li>

### Configure Vector

<ConfigExample
  format="toml"
  path={"/etc/vector/vector.toml"}
  sourceName={"file"}
  sinkName={"kafka"} />

</li>
<li>

### Start Vector

```bash
sudo systemctl start vector
```

</li>
</ol>
</Steps>
</TabItem>
<TabItem value="armv7">
<Steps headingDepth={3}>
<ol>
<li>

### Download the Vector `.deb` package

```bash
curl --proto '=https' --tlsv1.2 -O https://packages.timber.io/vector/0.9.X/vector-armhf.deb
```

[Looking for a different version?][docs.package_managers.dpkg#versions]

</li>
<li>

### Install the downloaded package

```bash
sudo dpkg -i vector-armhf.deb
```

</li>
<li>

### Configure Vector

<ConfigExample
  format="toml"
  path={"/etc/vector/vector.toml"}
  sourceName={"file"}
  sinkName={"kafka"} />

</li>
<li>

### Start Vector

```bash
sudo systemctl start vector
```

</li>
</ol>
</Steps>
</TabItem>
</Tabs>

## Next Steps

Vector is _powerful_ utility and we're just scratching the surface in this
guide. Here are a few pages we recommend that demonstrate the power and
flexibility of Vector:

<Jump to="https://github.com/timberio/vector" leftIcon="github" target="_blank">
  <div className="title">Vector Github repo <span className="badge badge--primary"><i className="feather icon-star"></i> 4k</span></div>
  <div className="sub-title">Vector is free and open-source!</div>
</Jump>

<Jump to="/guides/getting-started/" leftIcon="book">
  <div className="title">Vector getting started series</div>
  <div className="sub-title">Go from zero to production in under 10 minutes!</div>
</Jump>

<Jump to="/docs/about/what-is-vector/" leftIcon="book">
  <div className="title">Vector documentation</div>
  <div className="sub-title">Thoughtful, detailed docs that respect your time.</div>
</Jump>


[docs.package_managers.dpkg#versions]: /docs/setup/installation/package-managers/dpkg/#versions
[docs.sources.journald]: /docs/reference/sources/journald/
[docs.sources]: /docs/reference/sources/
[docs.strategies#daemon]: /docs/setup/deployment/strategies/#daemon
[urls.kafka]: https://kafka.apache.org/
[urls.rust]: https://www.rust-lang.org/
[urls.vector_performance]: https://vector.dev/#performance
[urls.vector_repo]: https://github.com/timberio/vector
[urls.vector_stars]: https://github.com/timberio/vector/stargazers
[urls.vector_website]: https://vector.dev
