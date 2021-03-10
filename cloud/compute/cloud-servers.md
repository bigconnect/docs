# Cloud Servers

Cloud Servers are Linux-based virtual machines \(VMs\) that run on top of virtualized hardware. Each Cloud Server you create is a new server you can use, either standalone or as part of a larger, cloud-based infrastructure.

## Plans and Pricing

We offer two different kinds of Cloud Server plans: one shared CPU plan and one dedicated CPU plan.

Choosing the right Cloud Server plan depends on your workload. A oversized Cloud Server would underuse its resources and cost more, but an undersized Cloud Server running at full CPU or memory would suffer from degraded performance or errors.

You can also resize a Cloud Server to a larger plan after creation, including resizing to a larger plan of a different kind.

## Shared CPU vs. Dedicated CPU

{% hint style="success" %}
All Cloud Servers run on the latest generation of Intel Xeon CPUs \(Skylake\) with DDR4 ECC RAM. We use NVMe SSDs for local Cloud Server disks.
{% endhint %}

* A Cloud Server is a virtual machine \(VM\) that is allocated resources, like CPU, RAM, and disk storage, from a physical host.
* A hypervisor, also known as a virtual machine monitor, makes sure that the multiple Cloud Servers running on a physical host each receive their virtual resources, like vCPU.
* A vCPU is a unit of processing power corresponding to a single hyperthread on a processor core. A modern, multicore processor has several vCPUs.

The plan you choose determines the amount of resources allocated to the Cloud Server. Resources like RAM, disk storage, and network bandwidth are always dedicated, but you can choose between shared CPU and dedicated CPU plans for dedicated vCPU.

Dedicated CPU plans have guaranteed access to the full hyperthread at all times. With shared CPU plans, the hyperthread allocated to the Cloud Server may be shared between multiple other Cloud Servers. When a shared CPU Cloud Serve experiences heavier load, the hypervisor dynamically allocates more hyperthread\(s\) to it.

However, the amount of CPU cycles available for the hypervisor to allocate depends on the workload of the other Cloud Servers sharing that host. If these neighboring Cloud Servers have high load, a Cloud Server could receive fractions of hyperthreads instead of dedicated access to the underlying physical processors. In practice, this means that shared CPU Cloud Servers can have access to full hyperthreads, but it’s not guaranteed.

### Shared Plan

* Low traffic web servers
* Blogs
* Discussion forums
* Content Management Systems \(CMS\)
* Small databases
* Dev/test servers
* Microservices
* Code repositories

Basic Cloud Servers come in a wide variety of configurations, from 1 vCPU / 1 GB memory to 16 vCPUs / 32 GB of memory. They also give you the flexibility to choose the memory to vCPU ratio most appropriate for your application.

Basic Cloud Servers are shared CPU, which are ideal for apps that mostly run at low to medium load, and occasionally burst for brief periods of time. For production workloads where time is of the essence or variable performance is intolerable, you should choose dedicated CPU Cloud Servers.

Basic Cloud Servers are also guaranteed to use one of the latest two generations of CPUs we have and NVMe SSDs. NVMe SSDs take advantage of parallelism to deliver disk performance that can be much faster than regular SSDs. Workloads that require a large number of transactions will have much lower latency with NVMe SSDs.

### **Dedicated Plan**

Dedicated Cloud Servers have a balanced ratio of memory to dedicated CPU, suitable for a wide variety of production workloads.  Dedicated Cloud Servers are available in six configurations ranging from 2 vCPUs up to 8 vCPUs, with 8 GB up to 32 GB of RAM. This 4:1 memory to CPU ratio is optimal for standard workloads like:

* Medium-to-high-traffic web servers
* E-commerce sites
* Medium-sized databases
* Enterprise Software as a Service \(SaaS\)

They are also a good default choice if you’re not sure which Cloud Server type is best for your application. They’re best for general purpose production workloads that require dedicated compute power.

## Bandwidth

Each Cloud Server includes free outbound data transfer, starting at 20 TB/month and scaling up. Additional transfer is billed at 1.5 €/TB + VAT, but most users don’t exceed the amount included with their plan.

You can view your accumulated outbound monthly transfer allowance for each Cloud Server on the [Cloud Server details page](https://console.cloud.bigconnect.io).

## Features

### Linux Distributions

We offer the following Linux distributions:

* Ubuntu 16.04, 18.04, 20.04
* Debian 9, 10
* CentOS 7, 8
* Fedora 32, 33

All Linux distributions are 64 bit \(x64\).

### Windows Distributions

* Windows Server 2019 \(DE/EN\)
* Windows Server 2016 \(DE/EN/RU/Language Pack\)
* Windows Server 2012 R2 \(DE/EN/RU/Language Pack\)

{% hint style="info" %}
Windows installation must be performed manually and you should bring your own license. BigConnect Cloud does not provide licenses for Microsoft Windows.
{% endhint %}

### Block Storage Volumes

BigConnect Block Storage is a flexible, convenient way of managing additional storage \(in units called volumes\) for your Cloud Servers. Volumes are independent resources that you can move between Cloud Servers within the same region. You can increase the size of a volume without powering down the Cloud Servers it’s attached to. They’re most useful when you need more storage space but don’t need the additional processing power or memory that a larger Cloud Servers would provide.

### Load Balancers

BigConnect Load Balancers are a fully-managed, highly available load balancing service. Load balancers distribute traffic to groups of Cloud Servers, which decouples the overall health of a backend service from the health of a single server to ensure that your services stay online.

## Limits

* Network traffic not matching a Cloud Server’s IP address/MAC address













#### 











