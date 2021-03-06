---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Mandatos de SLCLI para {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

Puede utilizar SLCLI para realizar acciones que normalmente se manejan a través del [{{site.data.keyword.slportal}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){:new_window}. Por ejemplo, con SLCLI puede realizar pedidos de volúmenes, espacio de instantáneas, réplica, actualizar autorizaciones, cancelar volúmenes, etc.

Para obtener más información sobre cómo instalar y utilizar SLCLI, consulte [Cliente de API de Python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Mandatos de SLCLI relacionados con el acceso
* [Gestión de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## Mandatos de SLCLI relacionados con la réplica

* [Mandatos de SLCLI relacionados con la réplica](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Mandatos de SLCLI relacionados con las instantáneas

* [Solicitud de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [Gestión de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## Mandatos de SLCLI relacionados con los volúmenes

* [Solicitud de un volumen de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Creación de un volumen duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [Ajuste del IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli block volume-modify
  ```
* [Expansión de la capacidad](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#resizingsteps)
  ```
  slcli block volume-modify
  ```
* [Gestión de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Gestión de los límites de almacenamiento](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
