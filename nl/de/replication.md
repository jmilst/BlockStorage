---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Daten replizieren
{: #replication}

Bei der Replikation werden Snapshots mithilfe eines Ihrer Snapshotpläne automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum kopiert. Im Fall beschädigter Daten oder einer Katastrophe können die Kopien an dem fernen Standort wiederhergestellt werden.

Die Replikation hält Ihre Daten an zwei verschiedenen Positionen synchron. Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie den Abschnitt [Duplikat des Blockdatenträgers erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).
{:tip}

Um Replikationen durchführen zu können, müssen Sie einen Snapshotplan erstellen.
{:important}


## Fernes Rechenzentrum für replizierten Speicherdatenträger ermitteln

Die Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden weltweit in Paare aus einem primären und einem fernen Datenträger unterteilt.
In Tabelle 1 finden Sie eine vollständige Liste der Verfügbarkeit der Rechenzentren und der Replikationsziele.

<table>
  <caption style="text-align: left;"><p>Tabelle 1 - In dieser Tabelle wird eine vollständige Liste der Rechenzentren mit erweiterten Leistungsmerkmalen in jeder Region aufgeführt. Jede Region wird in einer separaten Spalte angegeben. In manchen Städten, wie zum Beispiel Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London und Sydney, befinden sich mehrere Rechenzentren.</p>
  <p>&#42; Rechenzentren in der Region US 1 verfügen NICHT über erweiterten Speicher. Hosts in Rechenzentren mit erweiterten Speicherleistungsmerkmalen können die Replikation mit Replikationszielen in Rechenzentren der Region US 1 <strong>nicht</strong> einleiten.</p>
  </caption>
  <thead>
    <tr>
      <th>US 1 &#42;</th>
      <th>US 2</th>
      <th>Lateinamerika</th>
      <th>Kanada</th>
      <th>Europa</th>
      <th>Asien/Pazifik</th>
      <th>Australien</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
          TOK04<br />
          TOK05<br />
          SNG01<br />
          SEO01<br />
          CHE01<br />
	        <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
          SYD05<br />
          MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Erstreplikation erstellen

Replikationen werden auf der Basis eines Snapshotplans ausgeführt. Sie müssen zuerst über einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger verfügen, um replizieren zu können. Wenn Sie versuchen, eine Replikation zu konfigurieren und sich der eine oder andere nicht an seiner Position befindet, werden Sie aufgefordert, mehr Speicherplatz zu erwerben oder einen Zeitplan einzurichten. Replikationen werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}}![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotplan für Ihre Replikationen aus. Die Liste enthält alle Ihre aktiven Snapshotpläne. <br />
   Sie können nur einen einzigen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle seit dem letzten Replikationszyklus erfassten Snapshots werden unabhängig von dem Plan repliziert, aus dem sie stammen.<br />Wenn Sie keine Snapshots eingerichtet haben, werden Sie aufgefordert, dies vor der Bestellung der Replikation zu tun. Genauere Informationen hierzu finden Sie im Abschnitt [Mit Snapshots arbeiten](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots).
   {:important}
3. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum aus, das Sie als Standort für das Disaster-Recovery-Standort verwenden möchten.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Fenster sind mit den Standardwerten gefüllt.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
6. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.


## Vorhandene Replikation bearbeiten

Sie können entweder in der Registerkarte **Primär** oder **Replikat** unter **Speicher** **{{site.data.keyword.blockstorageshort}}** vom [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.


## Replikationszeitplan bearbeiten

Der Replikationsplan basiert auf einem vorhandenen Snapshotplan. Um den Replikationsplan von 'Stündlich' in 'Täglich' oder 'Wöchentlich' zu ändern oder umgekehrt, müssen Sie den Replikatdatenträger löschen und einen neuen einrichten. 

Wenn Sie jedoch die Tageszeit ändern möchten, zu der die **tägliche** Replikation stattfindet, können Sie den vorhandenen Zeitplan auf der Registerkarte des Primärdatenträgers oder des Replikatdatenträgers ändern. 

1. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Snapshotplan bearbeiten** aus.
3. Überprüfen Sie im Rahmen **Snapshot** die Angaben unter **Plan**, um festzustellen, welchen Plan Sie für die Replikation verwenden. Ändern Sie den gewünschten Zeitplan.
4. Klicken Sie auf **Speichern**.


## Replikationsbereich ändern

Der primäre Replikationsbereich und der Replikatsbereich müssen identisch sein. Wenn Sie den Bereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird automatisch Speicherplatz zu Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Wird der Snapshotbereich vergrößert, wird auch eine sofortige Aktualisierung der Replikation ausgelöst.

1. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Mehr Snapshotbereich hinzufügen** aus.
3. Wählen Sie in der Liste die Speichergröße aus und klicken Sie auf **Weiter**.
4. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfeld nehmen die Standardwerte an.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Bestellung aufgeben**.


## Replikatdatenträger in der Datenträgerliste anzeigen

Sie können Ihre Replikationsdatenträger auf der Seite {{site.data.keyword.blockstorageshort}} unter **Speicher > {{site.data.keyword.blockstorageshort}}** anzeigen. Der **LUN-Name** weist den Namen des Primärdatenträgers auf, an den REP angehängt ist. Der **Typ** ist 'Endurance - Replikat' oder 'Performance - Replikat'. Die **Zieladresse** ist  'Nicht zutreffend', weil der Replikatdatenträger nicht im Replikatrechenzentrum angehängt ist, und der **Status** ist 'Inaktiv'.


## Details eines replizierten Datenträgers im Replikatrechenzentrum anzeigen

Sie können die Details des Replikatdatenträgers unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** auf der Registerkarte **Replikat** anzeigen. Eine andere Option besteht darin, den Replikatdatenträger auf der Seite **{{site.data.keyword.blockstorageshort}}**  auszuwählen und auf die Registerkarte **Replikat** zu klicken.


## Snapshotbereich im Replikatrechenzentrum erhöhen, wenn der Snapshotbereich im primären Rechenzentrum erhöht wird

Ihr primärer Datenträger und der Replikatspeicherdatenträger müssen dieselbe Datenträgergröße aufweisen. Der eine darf nicht größer sein als der andere. Wenn Sie Ihren Snapshotbereich für Ihren Primärdatenträger erhöhen, wird der Replikatbereich automatisch erhöht. Wird der Snapshotbereich vergrößert, wird eine sofortige Aktualisierung der Replikation ausgelöst. Die Erhöhung der beiden Datenträger wird auf Ihrer Rechnung als Artikelposition angezeigt und bei Bedarf anteilig berechnet.

Weitere Informationen zum Vergrößern des Snapschotbereichs finden Sie unter [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:tip}


## Replikationsprotokoll anzeigen

Das Replikationsprotokoll kann im **Auditprotokoll** auf der Registerkarte **Konto** im Bereich **Verwalten** angezeigt werden. Auf dem Primär- und dem Replikatdatenträger werden identische Replikationsprotokolle angezeigt. Das Protokoll enthält die folgenden Elemente.

- Der Typ für die Replikation (Failover oder Failback).
- Der Startzeitpunkt.
- Der Snapshot, der für die Replikation verwendet wurde.
- Die Größe der Replikation.
- Der Zeitpunkt, zu dem die Replikation abgeschlossen ist.


## Duplikat eines Replikats erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.BluSoftlayer_full}}-{{site.data.keyword.blockstoragefull}}s erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.

Duplikate können sowohl für den Primär- als auch für den Replikatdatenträger erstellt werden. Das neue Duplikat wird in demselben Rechenzentrum wie der Originaldatenträger erstellt. Wenn Sie ein Duplikat von einem Replikatdatenträger erstellen, wird der neue Datenträger in demselben Rechenzentrum erstellt wie der Replikatdatenträger.

Der Lese- und Schreibzugriff auf duplizierte Datenträger kann durch einen Host erfolgen, sobald der Speicher bereitgestellt wurde. Snapshots und Replikation sind dagegen erst zulässig, nachdem die Daten vollständig vom Original auf das Duplikat kopiert wurden.

Weitere Informationen finden Sie unter [Duplikat eines Blockdatenträgers erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Verwenden von Replikaten für ein Failover bei einem Ausfall

Beim Failover 'kippen Sie den Schalter' von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Beispiel: Ihr primäres Rechenzentrum befindet sich in London und Ihr sekundäres Rechenzentrum in Amsterdam. Bei einem Fehlerereignis können Sie ein Failover nach Amsterdam durchführen und von einer Recheninstanz in Amsterdam eine Verbindung zu dem nun primären Datenträger herstellen. Nachdem Ihr Datenträger in London repariert wurde, wird von dem Datenträger in Amsterdam ein Snapshot erstellt, um von einer Recheninstanz in London eine Rückübertragung nach London und auf den nun wieder primären Datenträger durchzuführen.

* Wenn der primäre Standort sich in unmittelbarer Gefahr befindet oder erheblich beeinträchtigt ist, finden Sie weitere Informationen unter [Failover mit einem zugänglichen Primärdatenträger](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible).
* Wenn der primäre Standort inaktiv ist, finden Sie weitere Informationen unter [Failover mit einem nicht zugänglichen Primärdatenträger](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible).


## Vorhandene Replikation abbrechen

Der Abbruch kann sofort oder am Stichtag erfolgen und bewirkt das Ende der Abrechnung. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' auf den Datenträger.
2. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
3. Wählen Sie **Replikat abbrechen** aus.
4. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
5. Aktivieren Sie **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Replikat abbrechen**.


## Replikation beim Abbrechen des Primärdatenträgers abbrechen

Wenn ein Primärdatenträger abgebrochen wird, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite '{{site.data.keyword.blockstorageshort}}' abgebrochen.

 1. Heben Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' Ihren Datenträger hervor.
 2. Klicken Sie auf **Aktionen** und wählen Sie **{{site.data.keyword.blockstorageshort}} abbrechen** aus.
 3. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
 4. Aktivieren Sie **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Abbrechen**.

## SLCLI-Befehle im Zusammenhang mit der Replikation
{: #clicommands}

* Geeignete Replikationsrechenzentren für einen bestimmten Datenträger auflisten.
  ```
  # slcli block replica-locations --help
  Syntax: slcli block replica-locations [OPTIONEN] DATENTRÄGER_ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, langer Name, kurzer Name
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Blockspeicherreplikatdatenträger bestellen.
  ```
  # slcli block replica-order --help
  Syntax: slcli block replica-order [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshotplan für die Replikation,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [erforderlich]
  -l, --location TEXT             Kurzname des Rechenzentrums für den
                                  Replikatdatenträger (z. B. dal09)  [erforderlich]
  --tier [0.25|2|4|10]            Endurance-Speichertier (IOPS pro GB) des primären
                                  Datenträgers, für den ein Replikatdatenträger
                                  bestellt wird [optional]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  Betriebssystemtyp (z. B. LINUX) des primären
                                  Datenträgers, für den ein Replikat bestellt
                                  wird [optional]
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Vorhandene Replikatdatenträger für einen Blockspeicherdatenträger auflisten.
  ```
  # slcli block replica-partners --help
  Syntax: slcli block replica-partners [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Benutzername, Konto-ID,
                  Kapazität (GB), Hardware-ID, Gastsystem-ID, Host-ID
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failover eines Dateidatenträgers auf einen bestimmten Replikatdatenträger durchführen.
  ```
  # slcli block replica-failover --help
  Syntax: slcli block replica-failover [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  --immediate          Sofortige Funktionsübernahme durch den Replikatdatenträger.
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failback eines Blockdatenträgers von einem bestimmten Replikatdatenträger durchführen.
  ```
  # slcli block replica-failback --help
  Syntax: slcli block replica-failback [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  -h, --help           Diese Nachricht anzeigen und Ausführung beenden.
  ```
