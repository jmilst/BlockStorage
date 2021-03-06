---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:codeblock: .codeblock} 
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 订购快照
{: #orderingsnapshots}

为了自动或手动创建存储卷的快照，您需要购买空间来保存快照。可以购买的最高容量等于存储卷容量（在初始卷购买期间或之后使用在此描述的步骤）。

## 确定要订购的快照空间量

在一般情况下，根据以下两个关键因素来确定快照使用的快照空间：
- 活动文件系统在一段时间内的更改量有多大，
- 计划将快照保留多长时间。  

所需空间量的计算方法是**（更改比率）**x**（数据保留小时数/天数/周数/月数）**。

第一个快照使用的空间量可忽略不计，因为此快照只是用于指示活动文件系统块的元数据（指针）的副本。
{:note}

对于具有大量更改和很长保留期的卷，需要的空间要大于具有中等更改比率和适中保留时间安排的卷。第一种类型的示例是高更改比率的数据库。第二种类型的示例为 VMware 数据存储。

如果有 500 GB 实际数据，要生成 12 个每小时快照，并且各快照之间的更改比率为 1%，那么得出的快照空间量为：60 GB。

*（5 GB 更改比率）x（12 个每小时快照）=（60 GB 已用空间）*

反过来说，如果有 500 GB 实际数据，要生成 12 个每小时快照，看到的每小时更改比率为 10%，那么使用的快照空间量为 600 GB。

*（50 GB 更改比率）x（12 个每小时快照）=（600 GB 已用空间）*

因此，在确定所需快照空间量时，请仔细考虑更改比率。更改比率对所需快照空间量有着极大影响。卷越大，更频繁地更改的可能性越高。但是，更改比率为 5 GB 的 500 GB 卷和更改比率为 5 GB 的 10 TB 卷所使用的快照空间量相同。

此外，对于大多数工作负载，卷越大，初始需要预留的空间越少。这主要是由于底层数据效率以及环境中快照工作方式的性质。

## 通过 {{site.data.keyword.cloud_notm}} 控制台来订购快照空间

1. 登录到 [{{site.data.keyword.cloud_notm}} 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/catalog){:new_window}，然后单击左上角的菜单图标。选择**经典基础架构**。

   或者，您可以登录到 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window}。
2. 通过**存储** > **{{site.data.keyword.blockstorageshort}}**，访问存储器 LUN。
2. 单击“快照”框架上的**更改快照空间**。
3. 选择所需的空间量和付款方式。
4. 单击**继续**。
5. 输入您拥有的任何**促销码**，然后单击**重新计算**。缺省情况下，已填写“此订单的费用”和“订单复查”字段。

   处理订单时会应用折扣。
   {:note}
6. 选中**我已阅读主服务协议并同意其中的条款**框，然后单击**下订单**。快照空间将在几分钟后供应。

## 通过 SLCLI 来订购快照空间

```
# slcli block snapshot-order --help
用法：slcli block snapshot-order [OPTIONS] VOLUME_ID

选项：
  --capacity INTEGER    要创建的快照空间的大小（以 GB 为单位）[必需]
  --tier [0.25|2|4|10]  为其订购空间的块卷的耐久性存储器层 (IOPS/GB)
                        [可选且仅针对耐久性存储器卷有效]
  --upgrade             指示订单为升级订单的标志
  -h, --help            显示此消息并退出。
```
{:codeblock}
