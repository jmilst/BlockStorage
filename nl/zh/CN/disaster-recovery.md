---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-01"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}


# 复制副本卷以用于灾难恢复

发生导致主站点停运的灾难性故障或状况时，客户可以执行以下操作，以快速访问辅助站点上的数据。 

## 在辅助站点上通过复制副本卷实现故障转移

1. 登录到 [IBM Cloud 控制台](https://console.bluemix.net/catalog/){:new_window}，然后单击左上方的**菜单**图标。选择**经典基础架构**。 

   也可以登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}。
2. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
3. 单击列表中 LUN 的副本，以查看其**详细信息**页面。
4. 向下滚动**详细信息**页面，选择现有快照，然后单击**操作** > **复制**。
5. 对新卷的容量（增加大小）或 IOP 进行必要的更新。
6. 根据需要，更新新卷的快照空间。
7. 单击**继续**，以下订单购买复制卷。

创建卷后，您可以将创建的卷连接到主机上，然后在该卷上执行读/写操作。将数据从原始卷复制到复制卷时，详细信息页面会显示正在进行复制。复制过程完成后，新卷将完全独立于原始卷，您可以如常通过快照和复制对其进行管理。

## 故障转移到原始主站点

如果要将生产返回给原始主站点，那么必须执行以下步骤。

1. 登录到 [IBM Cloud 控制台](https://console.bluemix.net/catalog/){:new_window}，然后单击左上方的**菜单**图标。选择**经典基础架构**。 

   也可以登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}。
2. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
3. 单击 LUN 名称，然后创建快照安排（如果尚不存在）。 

   有关快照安排的更多信息，请参阅[管理快照](working-with-snapshots.html#adding-a-snapshot-schedule)。
   {:tip}
4. 单击**副本**，然后单击**购买复制**。
5. 选择复制要遵循的现有快照安排。列表中包含所有有效的快照安排。 
6. 单击**位置**，然后选择原始生产站点的数据中心。
7. 单击**继续**。
8. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。

复制完成后，您需要创建新副本的复制卷。
{:important}

1. 返回到**存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 单击列表中 LUN 的副本，以查看其**详细信息**页面。
3. 向下滚动**详细信息**页面，选择现有快照，然后单击**操作** > **复制**。
4. 对新卷的容量（增加大小）或 IOP 进行必要的更新。
5. 根据需要，更新新卷的快照空间。
6. 单击**继续**，以下订单购买复制卷。

复制过程完成后，您可以取消复制，以及用于将数据返回到原始主站点的卷。复制卷将成为主存储，而且您可以再次建立对原始辅助站点的复制。