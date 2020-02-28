---
description: Typical BigConnect deployment scenarios
---

# Deployment

In an enterprise deployment, several things need to be taken into consideration such as disk space, I/O, bandwidth, memory, operating systems and the typical workload your system will have.

Based on the typical use cases for BigConnect, the following workload patterns are commonly observed in production environments:

#### Balanced Workload

If your workloads are distributed equally across the various job types \(CPU bound, Disk I/O bound, or Network I/O bound\), your cluster has a balanced workload pattern. This is a good default configuration for unknown or evolving workloads.

#### Compute Intensive

These workloads are CPU bound and are characterized by the need of a large number of CPUs and large amounts of memory to store in-process data. \(This usage pattern is typical for natural language processing or HPCC workloads.\)

#### I/O Intensive

A typical Spark job \(like sorting\) requires very little compute power. Instead it relies more on the I/O bound capacity of the cluster \(for example, if you have lot of cold data\). For this type of workload, we recommend investing in more disks per box.

#### Unknown or evolving workload patterns

You may not know your eventual workload patterns from the first. Typically, the first jobs submitted to BigConnect in the early days are usually very different than the actual jobs you will run in your production environment. For these reasons, we recommend that you either use the Balanced workload configuration or invest in a pilot BigConnect cluster and plan to evolve its structure as you analyze the workload patterns in your environment.

