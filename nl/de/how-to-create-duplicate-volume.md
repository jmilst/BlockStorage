---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Duplikat eines Blockdatenträgers erstellen
{: #duplicatevolume}

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.blockstoragefull}}s erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.   

Da das Duplikat auf den Daten eines Snapshots zu einem bestimmten Zeitpunkt basiert, wird ein Snapshotbereich auf dem Originaldatenträger benötigt, damit das Duplikat erstellt werden kann. Weitere Informationen zu Snapshots und zum Bestellen von Snapshotbereichen finden Sie in der [Snapshotdokumentation](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots).  

Duplikate können sowohl für **Primärdatenträger** als auch **Replikatdatenträger** erstellt werden. Das neue Duplikat wird in demselben Rechenzentrum wie der Originaldatenträger erstellt. Wenn Sie ein Duplikat aus einem Replikatdatenträger erstellen, wird der neue Datenträger in demselben Rechenzentrum erstellt wie der Replikatdatenträger.

Der Lese- und Schreibzugriff auf duplizierte Datenträger kann durch einen Host erfolgen, sobald der Speicher bereitgestellt wurde. Snapshots und Replikation sind dagegen erst zulässig, nachdem die Daten vollständig vom Original auf das Duplikat kopiert wurden.

Sobald die Datenkopie abgeschlossen ist, kann das Duplikat als unabhängiger Datenträger verwaltet und verwendet werden.

Diese Funktion steht an den meisten Standorten zur Verfügung. Klicken Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-news), um eine Liste mit den verfügbaren Rechenzentren anzuzeigen.

Als Benutzer mit einem dedizierten Konto für {{site.data.keyword.containerlong}} finden Sie in der [{{site.data.keyword.containerlong_notm}}-Dokumentation](/docs/containers?topic=containers-block_storage#block_backup_restore) Informationen zu Optionen für die Duplizierung eines Datenträgers.
{:tip}

Nachstehend einige gängige Anwendungen für duplizierte Datenträger:
- **Disaster-Recovery-Test**: Erstellen Sie ein Duplikat des Replikatdatenträgers, um zu überprüfen, ob die Daten intakt sind und im Fall einer Katastrophe ohne Unterbrechung der Replikation verwendet werden können.
- **Goldene Kopie**: Verwenden Sie einen Speicherdatenträger als goldene Kopie, um damit mehrere Instanzen für verschiedene Anwendungen zu erstellen.
- **Datenaktualisierung**: Erstellen Sie eine Kopie Ihrer Produktionsdaten, um diese zu Testzwecken an die nicht für die Produktion verwendete Umgebung anzuhängen.
- **Wiederherstellung aus Snapshot**: Stellen Sie Daten auf dem Originaldatenträger mit bestimmten Dateien und Datumsangaben aus einem Snapshot wieder her, ohne den gesamten Originaldatenträger mit einer Snapshotwiederherstellungsfunktion zu überschreiben.
- **Entwicklung und Test**: Erstellen Sie gleichzeitig bis zu vier simultane Duplikate eines Datenträgers, um duplizierte Daten zu Entwicklungs- und Testzwecken zu erstellen.
- **Größenänderung des Speichers**: Erstellen Sie einen Datenträger mit der neuen Größe und/oder den IOPS-Raten, ohne die Daten verschieben zu müssen.  

Sie können einen duplizierten Datenträger über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} erstellen.


## Duplikat von einem bestimmten Datenträger in der Speicherliste erstellen

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste:
    - Klicken Sie im Kundenportal auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
    - Klicken Sie in der {{site.data.keyword.BluSoftlayer_full}}-Konsole auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste einen Datenträger aus und klicken Sie auf **Aktionen** > **Duplizierte LUN (Datenträger)**.
3. Wählen Sie Ihre Snapshotoption aus:
    - Wenn Sie von einem **Nicht-Replikat**-Datenträger bestellen:
      - Wählen Sie **Aus neuem Snapshot erstellen** aus: Damit wird ein neuer Snapshot für das Duplikat erstellt. Verwenden Sie diese Option, wenn der Datenträger keine aktuellen Snapshots aufweist oder wenn Sie zu diesem Zeitpunkt ein Duplikat erstellen wollen.<br/>
      - Wählen Sie **Aus letztem Snapshot erstellen** aus: Damit wird ein Duplikat aus dem letzten für diesen Datenträger vorhandenen Snapshot erstellt.
    - Bei einer Bestellung über einen **Replikat**-Datenträger: Die einzige Option für einen Snapshot ist die Verwendung des neuesten verfügbaren Snapshots.
4. Speichertyp und Position bleiben die gleichen wie beim ursprünglichen Datenträger.
5. Stündliche oder monatliche Abrechnung: Sie können wählen, ob die duplizierte LUN mit stündlicher oder monatlicher Abrechnung bereitgestellt wird. Der Abrechnungstyp für den Originaldatenträger wird automatisch ausgewählt. Wenn Sie jedoch für Ihren neuen duplizierten Speicher einen anderen Abrechnungstyp wählen möchten, können Sie das hier tun.
5. Bei Bedarf können Sie IOPS oder ein IOPS-Tier für den neuen Datenträger angeben. Die IOPS-Bezeichnung wird für den Originaldatenträger standardmäßig festgelegt. Die verfügbaren Kombinationen aus Leistung und Größe werden angezeigt.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 0,25 IOPS ist, können Sie keine neue Auswahl treffen.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 2, 4 oder 10 IOPS ist, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Tiers auswählen.
6. Bei Bedarf können Sie die Größe des neuen Datenträgers aktualisieren, sodass er größer als der Originaldatenträger ist. Die Größe des Originaldatenträgers wird standardmäßig festgelegt.

   {{site.data.keyword.blockstorageshort}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden
.
   {:tip}
7. Bei Bedarf können Sie den Snapshotbereich für den neuen Datenträger aktualisieren und mehr, weniger oder keinen Snapshotbereich hinzufügen. Der Snapshotbereich wird für den Originaldatenträger standardmäßig festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung abzusetzen.

## Duplikat aus einem bestimmten Snapshot erstellen

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste.
2. Klicken Sie in der Liste auf ein LUN, um die Detailseite anzuzeigen. (Es kann sich um einen Replikatdatenträger oder einen Nichtreplikatdatenträger handeln.)
3. Blättern Sie abwärts, wählen Sie in der Liste auf der Detailseite einen vorhandenen Snapshot aus und klicken Sie auf **Aktionen** > **Duplikate**.   
4. Speichertyp (Endurance oder Performance) und Position bleiben mit dem Originaldatenträger identisch.
5. Die verfügbaren Kombinationen aus Leistung und Größe werden angezeigt. Die IOPS-Bezeichnung wird für den Originaldatenträger standardmäßig festgelegt. Sie können IOPS oder ein IOPS-Tier für den neuen Datenträger angeben.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 0,25 IOPS ist, können Sie keine neue Auswahl treffen.
    - Wenn Ihr Originaldatenträger ein Endurance-Tier mit 2, 4 oder 10 IOPS ist, können Sie für den neuen Datenträger einen beliebigen Wert zwischen diesen Tiers auswählen.
6. Bei Bedarf können Sie die Größe des neuen Datenträgers aktualisieren, sodass er größer als der Originaldatenträger ist. Die Größe des Originaldatenträgers wird standardmäßig festgelegt.

   {{site.data.keyword.blockstorageshort}} kann bis auf das Zehnfache der ursprünglichen Größe des Datenträgers erhöht werden
.
   {:tip}
7. Bei Bedarf können Sie den Snapshotbereich für den neuen Datenträger aktualisieren und mehr, weniger oder keinen Snapshotbereich hinzufügen. Der Snapshotbereich wird für den Originaldatenträger standardmäßig festgelegt.
8. Klicken Sie auf **Weiter**, um Ihre Bestellung des Duplikats abzusetzen.


## Duplikat über die SL-CLI erstellen
Mit dem folgenden SL-CLI-Befehl können Sie ein Duplikat eines {{site.data.keyword.blockstorageshort}}-Datenträgers erstellen.

```
# slcli block volume-duplicate --help
Syntax: slcli block volume-duplicate [OPTIONEN] AUSGANGSDATENTRÄGER-ID

Optionen:
  -o, --origin-snapshot-id INTEGER
                                  ID eines Ausgangsdatenträgersnapshots
                                  für die Duplizierung.
  -c, --duplicate-size INTEGER    Größe des Duplikats des Blockspeicherdatenträgers in GB. ***Wird keine Größe angegeben, wird die Größe des
                                  Ausgangsdatenträgers verwendet.***
                                  Mögliche Größen:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [Größe des
                                  Ausgangsdatenträgers]
  -i, --duplicate-iops INTEGER    Performance-Speicher-IOPS, zwischen 100 und
                                  6000 in Vielfachen von 100 [nur für
                                  Performance-Datenträger] ***Wird kein IOPS-Wert
                                  angegeben, wird der IOPS-Wert des
                                  Ausgangsdatenträgers verwendet.***
                                  Voraussetzungen: [Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger niedriger als 0,3 ist, muss
                                  der IOPS/GB-Wert für das Duplikat ebenfalls niedriger
                                  als 0,3 sein. Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger größer-gleich 0,3 ist,
                                  muss der IOPS/GB-Wert für das Duplikat ebenfalls
                                  größer-gleich 0,3 sein. ]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance-Speichertier (IOPS pro GB) [nur
                                  für Endurance-Datenträger] ***Wird kein Tier
                                  angegeben, wird das Tier des Ausgangsdatenträgers
                                  verwendet.***
                                  Voraussetzungen: [Wenn der IOPS/GB-Wert für den
                                  Ausgangsdatenträger 0,25 ist muss der IOPS/GB-
                                  Wert für das Duplikat ebenfalls 0,25 sein. Wenn der
                                  IOPS/GB-Wert für den Ausgangsdatenträger größer
                                  als 0,25 ist, muss der IOPS/GB-Wert für das Duplikat
                                  ebenfalls größer als 0,25 sein.]
  -s, --duplicate-snapshot-size INTEGER
                                  Größe des Snapshotbereichs, der für das
                                  Duplikat zu bestellen ist. ***Wird keine Größe
                                  für den Snapshotbereich angegeben, wird die Größe
                                  des Snapshotbereichs des Ausgangsblockspeicher-
                                  datenträgers verwendet.***
                                  Den Wert "0" für diesen Parameter angeben, um ein
                                  Duplikat ohne Snapshotbereich zu bestellen.
  --billing [hourly|monthly]        Optionaler Parameter für den Abrechnungssatz
                                  (Standardwert: monatlich)
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

## Duplizierten Datenträger verwalten

Während die Daten vom Originaldatenträger auf das Duplikat kopiert werden, wird auf der Detailseite der Status angezeigt, der angibt, dass die Duplizierung in Bearbeitung ist. In dieser Zeit können Sie eine Verbindung zu einem Host herstellen, von dem Datenträger lesen und auf ihn schreiben, jedoch keine Snapshotpläne erstellen. Wenn der Duplizierungsprozess abgeschlossen ist, ist der neue Datenträger unabhängig vom Original und kann mit Snapshots und Replikation normal verwaltet werden.
