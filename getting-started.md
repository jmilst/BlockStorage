---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-07"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Getting started tutorial
{: #getting-started}

{{site.data.keyword.blockstoragefull}} is persistent, high-performance iSCSI storage that is provisioned and managed independently of compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections.

{{site.data.keyword.blockstorageshort}} brings best-in-class levels of durability and availability with an unmatched feature set. It is built by using industry standards and best practices. {{site.data.keyword.blockstorageshort}} is designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures, and provide a consistent performance baseline.
{:shortdesc}

## Before you begin
{: #prereqs}

{{site.data.keyword.blockstorageshort}} LUNs can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.blockstorageshort}} offering, see [About {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Provisioning considerations

### Block size

IOPS for both Endurance and Performance is based on a 16-KB block size with a 50/50 read/write 50/50 random/sequential workload. A 16-KB block is the equivalent of one write to the volume.
{:important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

<table>
  <caption>Table 4 shows examples of how block size and IOPS affect the throughput.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Block Size (KB)</th>
            <th>IOPS</th>
            <th>Throughput (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (typical for Linux)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (typical for Oracle)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (typical for SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### Authorized hosts

Another factor to consider is the number of hosts that are using your volume. If there's a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

### Network connection

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage, and host side and application-specific tunings (IP stack or [queue depths](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings), and other settings).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server documentation](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers).
{:tip}

## Submitting your Order
{: #submitorder}

When you're ready to submit your order, you can place it through the [Console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) or the [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI).

## Connecting your new storage
{: #mountingstorage}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Connecting to LUNs on Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuring Block Storage for backup with cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring Block Storage for backup with Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Managing your new Storage

Through the portal or the SLCLI, you can manage various aspects of your File Storage such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
