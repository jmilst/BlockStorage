---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Snapshots verwalten
{: #managingSnapshots}

## Snapshotplan erstellen

Sie entscheiden, wie oft und wann eine Referenz mit Zeitangabe des Speicherdatenträgers mit Snapshotplänen erstellt werden soll. Auf einem Speicherdatenträger können maximal 50 Snapshots vorhanden sein. Zeitpläne werden über die Registerkarte **Speicher** > **{{site.data.keyword.blockstorageshort}}** des [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} verwaltet.

Damit Sie einen ersten Plan konfigurieren können, müssen Sie vorher einen Snapshotbereich kaufen, sofern Sie noch keinen während der Ersteinrichtung des Speicherdatenträgers gekauft haben. Weitere Informationen finden Sie unter [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:important}

### Snapshotplan hinzufügen
{: #addingschedule}

Snapshotpläne können für stündliche, tägliche und wöchentliche Intervalle und einen bestimmten Aufbewahrungszyklus eingerichtet werden. Pro Speicherdatenträger besteht ein Grenzwert von maximal 50 (kann eine Mischung aus stündlichen, täglichen und wöchentlichen Zeitplänen) und manuellen Snapshots.

1. Klicken Sie auf den Speicherdatenträger, klicken Sie auf **Aktionen** und klicken Sie auf **Snapshot planen**.
2. Im Fenster für den neuen Snapshotzeitplan können Sie aus drei verschiedenen Snapshothäufigkeiten wählen. Verwenden Sie eine beliebige Kombination dieser drei Optionen, um einen umfassenden Snapshotplan zu erstellen
   - Stündlich
      - Geben Sie die Minute jeder Stunde an, zu der der Snapshot durchgeführt werden soll. Der Standardwert ist die aktuelle Minute.
      - Geben Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
   - Täglich
      - Geben Sie die Stunde und Minute des Zeitpunkts an, an dem der Snapshot durchgeführt werden soll. Der Standardwert ist die aktuelle Stunde und Minute.
      - Wählen Sie die Anzahl der täglichen Snapshots aus, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
   - Wöchentlich
      - Geben Sie den Tag der Woche, die Stunde und die Minute des Zeitpunkts an, an dem der Snapshot ausgeführt werden soll. Der Standardwert ist der aktuelle Tag, die aktuelle Stunde und die aktuelle Minute.
      - Wählen Sie die Anzahl der wöchentlichen Snapshots aus, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
3. Klicken Sie auf **Speichern** und erstellen Sie einen anderen Plan mit einer anderen Häufigkeit. Wenn die Gesamtzahl der geplanten Snapshots 50 übersteigt, erhalten Sie eine Warnnachricht und können nicht speichern.

Die Liste der Snapshots wird in der Reihenfolge ihrer Ausführung im Abschnitt **Snapshots** der Seite **Details** angezeigt.

Mit dem folgenden Befehl können Sie die Liste der Snapshotpläne auch über die SL-CLI anzeigen.
```
# slcli block snapshot-schedule-list --help
Syntax: slcli block snapshot-schedule-list [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -h, --help  Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

## Manuellen Snapshot erstellen

Manuelle Snapshots können an verschiedenen Punkten während der Aktualisierung oder Wartung einer Anwendung gemacht werden. Sie können auch Snapshots für mehrere Server erstellen, die temporär auf der Anwendungsebene inaktiviert wurden.

Die Anzahl der Snapshots ist auf maximal 50 pro Speicherdatenträger begrenzt.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf **Aktionen**.
3. Klicken Sie auf **Manuellen Snapshot machen**.
Der Snapshot wird erstellt und im Abschnitt **Snapshots** der Seite **Details** angezeigt. Für seinen Plan ist 'Manuell' festgelegt.

Alternativ dazu können Sie mit dem folgenden Befehl einen Snapshot über die SL-CLI erstellen.
```
# slcli block snapshot-create --help
Syntax: slcli block snapshot-create [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -n, --notes TEXT  Anmerkungen für den neuen Snapshot
  -h, --help        Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

## Alle Snapshots mit den Funktionen für belegten Speicherplatz und die Verwaltung auflisten

Eine Liste mit den aufbewahrten Snapshots und dem belegten Speicherplatz kann auf der Seite **Details** angezeigt werden.  Verwaltungsfunktionen (Bearbeiten von Plänen und Hinzufügen von zusätzlichem Speicherplatz) können auf der Seite 'Details' mithilfe des Menüs **Aktionen** oder Links im unterschiedlichen Abschnitten auf der Seite ausgeführt werden.

## Liste der aufbewahrten Snapshots anzeigen

Aufbewahrte Snapshot basieren auf der Nummer, die Sie in das Feld **Letzte aufbewahren** beim Konfigurieren der Pläne eingegeben haben. Sie können die Snapshots anzeigen, die im Abschnitt **Snapshot** erstellt wurden. Die Snapshots werden entsprechend dem Plan aufgelistet.

Alternativ dazu können Sie mit dem folgenden SL-CLI-Befehl die verfügbaren Snapshots anzeigen.
```
# slcli block snapshot-list --help
Syntax: slcli block snapshot-list [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Name, Erstellungsdatum, Größe in Byte
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

## Menge des belegten Snapshotbereichs anzeigen

Das Kreisdiagramm auf der Seite **Details** zeigt an, wieviel Speicherbereich belegt ist und wieviel Speicherbereich noch verfügbar ist. Sie erhalten Benachrichtigungen, wenn die Schwellenwerte für den Speicherplatz erreicht werden - 75 Prozent, 90 Prozent und 95 Prozent.

## Menge des Snapshotbereichs für Datenträger ändern

Es kann vorkommen, dass Sie Snapshotbereich zu einem Datenträger hinzufügen möchten, auf dem vorher keiner vorhanden war oder dass Sie zusätzlichen Snapshotbereich benötigen. Sie können je nach Bedarf zwischen 5 GB und 4.000 GB hinzufügen.

Der Snapshotbereich kann nur vergrößert werden. Es kann nicht reduziert werden. Sie können zunächst einen kleinere Speichergröße auswählen, bis Sie ermittelt haben, wieviel Speicherbereich Sie benötigen. Bedenken Sie dabei, dass der Speicherplatz gemeinsam von automatisierten und manuellen Snapshots verwendet wird.
{:note}

Der Snapshotbereich kann über **Speicher** > **{{site.data.keyword.blockstorageshort}}** geändert werden.

1. Klicken Sie auf die Speicherdatenträger, klicken Sie auf **Aktionen** und klicken Sie auf **Mehr Snapshotbereich hinzufügen**.
2. Wählen Sie an der Eingabeaufforderung einen Wert aus einem Größenbereich aus. In der Regel reichen die Größen von 0 bis zur Größe Ihres Datenträgers.
3. Klicken Sie auf **Weiter**.
4. Geben Sie gegebenenfalls den Werbeaktionscode ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Bestellung aufgeben**. Der zusätzliche Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Benachrichtigungen bei Erreichen des Grenzwerts für Snapshotbereich oder Löschen von Snapshots erhalten

Benachrichtigungen werden über Support-Tickets an den Masterbenutzer eines Kontos gesendet, wenn Sie drei unterschiedliche Schwellenwerte für den Speicherplatz erreichen - 75 Prozent, 90 Prozent und 95 Prozent.

- Bei **75 Prozent der Kapazität** wird eine Warnung gesendet, dass die Belegung des Snapshotbereichs 75 Prozent überschritten hat. Wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen, wird die Aktion notiert und das Ticket geschlossen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- Bei **90 Prozent der Kapazität** wird eine zweite Warnung gesendet, wenn die Belegung des Snapshotbereichs 90 Prozent überschritten hat. Wie bei Erreichen von 75 Prozent der Kapazität gilt, dass die Aktion notiert und das Ticket geschlossen wird, wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- Bei **95 Prozent der Kapazität** wird eine letzte Warnung gesendet. Wenn keine Aktion ausgeführt wird, um die Speicherbelegung unter den Schwellenwert zu senken, wird eine Benachrichtigung generiert und eine automatische Löschung durchgeführt, sodass zukünftige Snapshots erstellt werden können. Geplante Snapshots werden gelöscht, beginnend mit dem ältesten, bis die Belegung unter 95 Prozent liegt. Wenn die Belegung 95 Prozent überschreitet, werden Snapshots so lange gelöscht, bis die Belegung unter dem Schwellenwert liegt. Wenn der Speicherbereich manuell vergrößert oder Snapshots gelöscht werden, wird die Warnung zurückgesetzt und erneut ausgegeben, wenn der Schwellenwert wieder überschritten wird. Wenn Sie nichts unternehmen, ist dies die einzige Warnung, die Sie erhalten.

## Snapshotplan löschen

Snapshotpläne können über **Speicher** > **{{site.data.keyword.blockstorageshort}}** gelöscht werden.

1. Klicken Sie im Rahmen **Snapshotpläne** auf der Seite **Details** auf den Plan, der gelöscht werden soll.
2. Klicken Sie auf das Kontrollkästchen neben dem zu löschenden Plan und klicken Sie auf **Speichern**.<br />

Wenn Sie die Replikationsfunktion verwenden, müssen Sie sicherstellen, dass der Zeitplan, den Sie löschen, nicht der von der Replikation verwendete Zeitplan ist. Weitere Informationen zum Löschen eines Replikationszeitplans finden Sie unter [Daten replizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).
{:important}

## Snapshot löschen

Snapshots, die nicht mehr benötigt werden, können manuell entfernt werden, um Speicherplatz für künftige Snapshots freizugeben. Das Löschen erfolgt über **Speicher** > **{{site.data.keyword.blockstorageshort}}**.

1. Klicken Sie auf den Speicherdatenträger und blättern Sie bis zum Abschnitt **Snapshot**, um die Liste der vorhandenen Snapshots anzuzeigen.
2. Klicken Sie auf **Aktionen** neben dem gewünschten Snapshot und klicken Sie auf **Löschen**, um den Snapshot zu löschen. Diese Löschung hat keine Auswirkung auf zukünftige oder frühere Snapshots im selben Zeitplan, da zwischen Snapshots keine Abhängigkeit besteht.

Manuelle Snapshots, die nicht manuell im Portal gelöscht werden, werden automatisch (älteste zuerst) gelöscht, wenn Sie den Grenzwert des Speicherbereichs erreichen.

Mit dem folgenden Befehl können Sie einen Datenträger über die SL-CLI löschen.
```
# slcli block snapshot-delete
Syntax: slcli block snapshot-delete [OPTIONEN] SNAPSHOT-ID

Optionen:
  -h, --help  Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}


## Speicherdatenträger mit einem Snapshot auf dem Stand eines bestimmten Zeitpunkts wiederherstellen

Es kann vorkommen, dass Sie Ihren Speicherdatenträger aufgrund eines Benutzerfehlers oder einer Datenbeschädigung auf einen bestimmten Zeitpunkt zurücksetzen müssen.

Beim Zurücksetzen eines Datenträgers werden alle Snapshots gelöscht, die nach dem für das Zurücksetzen verwendeten Snapshot erstellt wurden.
{:important}

1. Hängen Sie Ihren Speicherdatenträger vom Host ab.
   - [Verbindung zu iSCSI-LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [Verbindung zu iSCSI-LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window}.
3. Blättern Sie nach unten und klicken Sie auf den Datenträger, der wiederhergestellt werden soll. Im Abschnitt **Snapshots** der Seite **Details** wird die Liste aller gespeicherten Snapshots mit Angabe ihrer Größe und ihres Erstellungsdatums angezeigt.
4. Klicken Sie auf **Aktionen** neben dem Snapshot, der verwendet werden soll, und klicken Sie auf **Wiederherstellen**. <br/>

   Bei der Wiederherstellung gehen die Daten verloren, die nach der Erstellung des Snapshots erstellt oder geändert wurden. Dieser Datenverlust tritt auf, weil der Speicherdatenträger in den Zustand zurückgesetzt wird, in dem er sich vor dem Snapshot befunden hat.
   {:note}
5. Klicken Sie auf **Ja**, um die Wiederherstellung zu starten.

   Quer über den Bereich der Seite wird die Nachricht angezeigt, dass der Datenträger mit dem ausgewählten Snapshot wiederhergestellt wird. Außerdem wird neben Ihrem Datenträger auf dem {{site.data.keyword.blockstorageshort}} ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie den Mauszeiger über das Symbol bewegen, wird ein Fenster mit Angaben zur Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet.
   {:note}
6. Hängen Sie Ihren Speicherdatenträger wieder an den Host an und verbinden Sie ihn erneut.
   - [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Alternativ dazu können Sie nach dem Abhängen des Datenträgers vom Host den folgenden Befehl in der SL-CLI verwenden um eine Wiederherstellung zu starten.
```
# slcli block snapshot-restore --help
Syntax: slcli block snapshot-restore [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -s, --snapshot-id TEXT  ID des Snapshots, der zur Wiederherstellung des
                          Blockspeicherdatenträgers verwendet werden soll.
  -h, --help              Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}  

Hängen Sie den Speicherdatenträger nach dem Abschluss der Wiederherstellung wieder an den Host an und verbinden Sie ihn erneut.
