# Autovermietung (AV) – Sprint 1: Anforderungsanalyse

**Software Engineering – Projekt 1 | Erläuterungen zu Aufgabe 1–4**

## 1. Einleitung und Modellierungsansatz

Ziel unseres Projekts ist die Anforderungsanalyse für ein webbasiertes Autovermietungssystem (AV). Kundinnen und Kunden sollen verfügbare Fahrzeugtypen für einen gewünschten Zeitraum suchen, Angebote mit Preisen und Fahrzeugdetails einsehen, sich ein Angebot per E-Mail zusenden lassen und anschließend eine Buchung durchführen können. Dabei werden unter anderem Zusatzoptionen, Mietbedingungen, persönliche Angaben und Zahlungsdaten berücksichtigt. Nach erfolgreicher Prüfung der Buchungsdaten wird die Buchung gespeichert; der Kunde erhält eine Bestätigungsnummer und eine E-Mail.

Wir haben die Modellierung bewusst **schrittweise verfeinert**. In Aufgabe 1 betrachten wir die wichtigsten Ziele aus Sicht des Kunden. Aufgabe 2 beschreibt die dafür notwendigen Systemfunktionen detaillierter. Aufgabe 3 identifiziert die zentralen fachlichen Konzepte und ihre Beziehungen. Aufgabe 4 verfeinert diese Konzepte zu persistenten Entitätsklassen mit Attributen, Datentypen und Multiplizitäten. So bleibt die Verbindung zwischen Anforderungen, Verhalten und Datenmodell nachvollziehbar.

Die Diagramme beruhen auf der Projektbeschreibung *Autovermietung – Projekt 1* und verwenden die UML-Notation aus dem bereitgestellten *UML Tutorial*. Wo die Aufgabenstellung Details offenlässt, haben wir eigene Modellierungsannahmen getroffen und begründen sie im Folgenden.

### Verwendete UML-Notation

| Notation | Bedeutung |
|---|---|
| **Akteur** | Externe Rolle, die mit dem AV-System interagiert. |
| **Use Case (Ellipse)** | Vom System bereitgestellte Funktion bzw. Interaktionsziel. |
| **Systemgrenze (Rechteck)** | Trennt das AV-System von externen Akteuren. |
| **`<<include>>`** | Der Basis-Use-Case verwendet das eingeschlossene Verhalten als festen Bestandteil. Der gestrichelte Pfeil zeigt **auf den eingeschlossenen** Use Case. |
| **`<<extend>>`** | Ein Use Case ergänzt einen Basis-Use-Case unter einer Bedingung bzw. optional. Der gestrichelte Pfeil zeigt **auf den erweiterten Basis-Use-Case**. |
| **Assoziation (durchgezogene Linie)** | Fachliche Beziehung zwischen Klassen; keine gerichtete Datenfluss-Nachricht. |
| **`1`, `0..1`, `0..*`, `1..*`** | Genau eins; optional höchstens eins; beliebig viele einschließlich null; mindestens eins. Die Multiplizität steht jeweils am betreffenden Assoziationsende. |

---

## 2. Aufgabe 1 – Anwendungsfallmodell aus Kundensicht

![Abbildung 1 – Anwendungsfallmodell aus Kundensicht](Abbildungen/Aufgabe1_UseCase_Kundensicht.png)

*Abbildung 1: Grobes Use-Case-Diagramm mit den wichtigsten Funktionen aus Sicht des Kunden.*

### 2.1 Ziel und Herleitung

Die erste Aufgabe verlangt ein Anwendungsfallmodell mit einem **hohen Abstraktionsniveau**. Deshalb zeigen wir nicht jeden technischen Verarbeitungsschritt, sondern nur Funktionen, die für den Kunden unmittelbar von Interesse sind. Aus den neun funktionalen Anforderungen haben wir sechs zentrale Anwendungsfälle abgeleitet. Als primärer Akteur wurde **`Kunde`** gewählt, da die Projektbeschreibung auf die selbstständige Online-Suche und -Buchung durch den Kunden ausgerichtet ist. Die Systemgrenze trägt die Bezeichnung **`Autovermietung (AV)`**.

### 2.2 Erklärung der Anwendungsfälle

| Anwendungsfall | Fachliche Bedeutung |
|---|---|
| **Mietangebote suchen** | Der Kunde sucht auf Grundlage von Abhol- und Rückgabeort, Zeitraum und Alter nach passenden Mietangeboten. Ergebnisse umfassen verfügbare Fahrzeugtypen und Preise. |
| **Fahrzeugdetails ansehen** | Der Kunde kann zu einem angezeigten Fahrzeugtyp zusätzliche Informationen aufrufen, insbesondere Marke, Modell, Sitzplätze und Gepäckkapazität. |
| **Angebot per E-Mail erhalten** | Der Kunde kann sich ein ausgewähltes Angebot an eine angegebene E-Mail-Adresse zusenden lassen. |
| **Fahrzeug buchen** | Der Kunde wählt ein Angebot und führt die Buchung bis zur Bestätigung durch. Dies ist neben der Suche das wichtigste Nutzerziel. |
| **Zusatzoptionen auswählen** | Der Kunde kann die Buchung freiwillig um Versicherungsoptionen, Zusatzfahrer, Kindersitze oder einen Aktionscode ergänzen. |
| **Mietbedingungen einsehen** | Der Kunde erhält die erforderlichen Informationen zu Einschränkungen, Versicherungen, Kraftstoffregelung und Abholung. |

Der Akteur `Kunde` ist direkt mit **Suche, Fahrzeugdetails, E-Mail-Angebot und Buchung** verbunden. Die Zusatzoptionen und Mietbedingungen wurden über die Beziehungen zu `Fahrzeug buchen` modelliert; sie müssen nicht zusätzlich als eigenständige Hauptziele dargestellt werden.

### 2.3 Begründung der Beziehungen

- **`Fahrzeugdetails ansehen` `<<extend>>` `Mietangebote suchen`:** Die Details sind eine freiwillig aufrufbare Vertiefung der angezeigten Suchergebnisse. Eine Suche ist auch ohne Detailansicht vollständig.
- **`Angebot per E-Mail erhalten` `<<extend>>` `Mietangebote suchen`:** Das Versenden bzw. Erhalten eines Angebots erfolgt nur auf Wunsch nach der Angebotssuche.
- **`Zusatzoptionen auswählen` `<<extend>>` `Fahrzeug buchen`:** Eine Buchung ist auch ohne zusätzliche Leistungen möglich. Die Auswahl erweitert den normalen Buchungsfall.
- **`Fahrzeug buchen` `<<include>>` `Mietbedingungen einsehen`:** Wir nehmen an, dass die relevanten Mietbedingungen im regulären Buchungsablauf immer bereitgestellt werden. Dies setzt den in der Projektbeschreibung geforderten Informationsumfang um.

**Modellierungsentscheidung:** Wir haben keine eigenen Anwendungsfälle für die Kreditkartenprüfung, das Speichern in der Datenbank oder die Erzeugung der Bestätigungsnummer aufgenommen. Diese Schritte sind für den Kunden Teil der übergeordneten Funktion `Fahrzeug buchen` und werden in Aufgabe 2 verfeinert. Ebenso modellieren wir die zuvor durchgeführte Suche als fachliche Voraussetzung der Buchung, nicht als obligatorisch neu auszuführenden `<<include>>`-Schritt.

---

## 3. Aufgabe 2 – Verfeinertes Anwendungsfallmodell aus Systemsicht

![Abbildung 2 – Verfeinertes Anwendungsfallmodell aus Systemsicht](Abbildungen/Aufgabe2_UseCase_Systemsicht.png)

*Abbildung 2: Verfeinertes Use-Case-Diagramm mit 15 Systemfunktionen für das Happy Scenario.*

### 3.1 Ziel und Herleitung

Im Unterschied zu Aufgabe 1 verlangt Aufgabe 2 **10 bis 15 Anwendungsfälle aus der Perspektive des internen Systems**. Deshalb beschreiben die Bezeichnungen nun konkrete Leistungen des AV-Systems, beispielsweise `Verfügbarkeit prüfen`, `Mietpreise berechnen` oder `Buchungsdaten und Kreditkarte validieren`. Wir haben **15 Anwendungsfälle** gewählt. Der Kunde bleibt als externer Akteur erhalten, weil er die Abläufe initiiert und mit dem System interagiert.

Wir betrachten ausschließlich das geforderte **Happy Scenario**: Die Suche liefert passende Ergebnisse, die Kundendaten sind vollständig und korrekt, die Kreditkartendaten werden erfolgreich geprüft und die Buchung kann bestätigt werden. Alternative Abläufe wie eine ungültige Kreditkarte, fehlende Suchergebnisse oder technische Fehler sind bewusst nicht Bestandteil dieses Diagramms.

### 3.2 Funktionen der Angebotssuche

| Systemfunktion | Aufgabe im System |
|---|---|
| **Mietangebote ermitteln** | Übergeordneter Anwendungsfall, der die Suche koordiniert. |
| **Suchkriterien erfassen** | Übernahme von Abholort, Rückgabeort, Abholdatum, Rückgabedatum und Fahreralter. |
| **Verfügbarkeit prüfen** | Ermittlung der anhand der Suchkriterien verfügbaren Fahrzeugtypen. |
| **Mietpreise berechnen** | Berechnung der angezeigten Preise für die gefundene Mietdauer und die zugehörigen Konditionen. |
| **Suchergebnisse anzeigen** | Ausgabe der gefundenen Fahrzeugtypen einschließlich ihrer Preise. |
| **Fahrzeugdetails anzeigen** | Anzeige von Marke, Modell, Sitzplatz- und Gepäckkapazität auf Anfrage. |
| **Angebot per E-Mail versenden** | Versand eines ausgewählten Angebots an die vom Kunden angegebene E-Mail-Adresse. |

`Mietangebote ermitteln` **inkludiert** die Erfassung der Suchkriterien, die Verfügbarkeitsprüfung, die Preisberechnung und die Ergebnisanzeige. Diese Schritte gehören zum normalen Suchverhalten. `Fahrzeugdetails anzeigen` und `Angebot per E-Mail versenden` **erweitern** die Suche, weil der Kunde diese Funktionen nur bei Bedarf nutzt.

### 3.3 Funktionen der Buchungsbearbeitung

| Systemfunktion | Aufgabe im System |
|---|---|
| **Buchung verarbeiten** | Übergeordneter Anwendungsfall für die Durchführung einer Buchung. |
| **Mietdaten zur Prüfung anzeigen** | Anzeige der ausgewählten Mietdaten, damit der Kunde sie vor Abschluss kontrollieren kann. |
| **Zusatzoptionen auswählen** | Optionale Auswahl von Versicherungen, Extras oder einem Aktionscode; die Auswahl wird vom System übernommen. |
| **Mietbedingungen anzeigen** | Bereitstellung der notwendigen Regeln, Einschränkungen und Hinweise. |
| **Kunden- und Zahlungsdaten erfassen** | Übernahme persönlicher Daten und der für die Zahlung benötigten Karteninformationen. |
| **Buchungsdaten und Kreditkarte validieren** | Prüfung auf Vollständigkeit und Korrektheit der Angaben, insbesondere der Kreditkarte. |
| **Buchung speichern und Bestätigungsnummer erzeugen** | Persistenz der erfolgreichen Buchung und Vergabe einer eindeutigen Bestätigungsnummer. |
| **Buchungsbestätigung per E-Mail versenden** | Versand der Buchungsbestätigung an die E-Mail-Adresse des Kunden. |

`Buchung verarbeiten` bindet im Normalfall die Anzeige der Mietdaten, die Bereitstellung der Mietbedingungen, die Erfassung und Validierung der Daten, die Preisberechnung sowie das Speichern und die Bestätigung der Buchung ein. **`Zusatzoptionen auswählen` verwendet `<<extend>>`**, weil optionale Zusatzleistungen nicht bei jeder Buchung gewählt werden müssen.

Die Systemfunktionen für **Speichern/Bestätigungsnummer** und **Bestätigungs-E-Mail** werden getrennt betrachtet. Dadurch wird deutlich, dass die Datenbankaktualisierung und die Kommunikation mit dem Kunden unterschiedliche Verantwortlichkeiten sind, obwohl beide zum erfolgreichen Buchungsabschluss gehören.

### 3.4 Wichtige Entwurfsentscheidungen

1. **Verfeinerung statt Wiederholung:** Während Aufgabe 1 die Nutzerziele zeigt, wird die Suche hier in Erfassung, Verfügbarkeit, Preisberechnung und Anzeige zerlegt.
2. **Wiederverwendung der Preisberechnung:** `Mietpreise berechnen` wird sowohl bei der Suche als auch im Buchungsprozess benötigt, da Zusatzoptionen oder Rabatte den endgültigen Preis beeinflussen können.
3. **Keine erneute Suche als Pflichtschritt:** Ein Kunde kann von bereits vorliegenden Ergebnissen aus buchen; deshalb ist `Mietangebote ermitteln` keine zwingend erneut auszuführende Teilfunktion von `Buchung verarbeiten`.
4. **Fachlicher Unterschied zwischen Prüfen und Validieren:** Der Kunde prüft die angezeigten Mietdaten; das System validiert anschließend die eingegebenen Daten und Zahlungsinformationen.
5. **Keine Fehlerfälle:** Fehlerbehandlung und alternative Flüsse werden aufgrund der ausdrücklichen Happy-Scenario-Vorgabe nicht modelliert.

**Hinweis zur UML-Semantik:** Ein `<<include>>`-Pfeil legt fest, dass Verhalten einbezogen wird; er definiert **keine zeitliche Reihenfolge**. Eine detaillierte Reihenfolge könnte später in einem Sequenzdiagramm dargestellt werden.

---

## 4. Aufgabe 3 – Abstraktes Klassenmodell

![Abbildung 3 – Abstraktes Klassenmodell](Abbildungen/Aufgabe3_Abstraktes_Klassenmodell.png)

*Abbildung 3: Fünf zentrale fachliche Konzepte mit ihren Assoziationen und Multiplizitäten.*

### 4.1 Ziel und Auswahl der Klassen

Für Aufgabe 3 sollen ungefähr **fünf wichtige Konzepte des Anwendungsbereichs** identifiziert werden. Im Gegensatz zu Aufgabe 4 geht es noch nicht um eine möglichst vollständige Datenbankstruktur. Wir konzentrieren uns daher auf die Objekte, die zum Verständnis eines Mietvorgangs zwingend notwendig sind:

| Klasse | Bedeutung und bewusst gewählte Attribute |
|---|---|
| **Kunde** | Die Person, die ein Fahrzeug sucht und eine Buchung durchführen kann. `name` und `email` kennzeichnen zentrale Kontaktinformationen. |
| **Angebot** | Ein vom System ermitteltes Mietangebot. `abholDatum`, `rückgabeDatum`, `fahrerAlter` und `preis` beschreiben die konkrete Anfrage und das Ergebnis. |
| **Buchung** | Die verbindliche Reservierung auf Grundlage eines Angebots; `bestätigungsnummer` dient zur späteren Identifizierung durch den Kunden. |
| **Fahrzeugtyp** | Die angebotene Fahrzeugkategorie, beschrieben durch `bezeichnung`, `sitzplätze` und `gepäckkapazität`. |
| **Standort** | Eine Mietstation mit `name` und `adresse`; sie kann sowohl Abhol- als auch Rückgabeort sein. |

Die Attribute bleiben absichtlich überschaubar. Versicherungsdetails, Zahlungen, Fahrzeugmodelle und gebuchte Extras werden erst im detaillierteren Entitätsmodell ergänzt.

### 4.2 Assoziationen und Multiplizitäten

| Assoziation | Interpretation |
|---|---|
| **Kunde – Angebot (`fordert an`)** | Ein Kunde kann `0..*` Angebote anfordern. Zu einem Angebot gehört `0..1` bereits identifizierter Kunde. |
| **Kunde – Buchung (`tätigt`)** | Ein Kunde kann `0..*` Buchungen vornehmen; jede Buchung ist genau **einem** Kunden zugeordnet. |
| **Angebot – Buchung (`führt zu`)** | Ein Angebot kann zu `0..1` Buchung führen; jede Buchung basiert auf genau **einem** Angebot. |
| **Angebot – Fahrzeugtyp (`betrifft`)** | Jedes Angebot betrifft genau **einen** Fahrzeugtyp; ein Fahrzeugtyp kann in `0..*` Angeboten vorkommen. |
| **Angebot – Standort (`Abholort`)** | Jedes Angebot hat genau **einen** Abholort; ein Standort kann Abholort für `0..*` Angebote sein. |
| **Angebot – Standort (`Rückgabeort`)** | Jedes Angebot hat genau **einen** Rückgabeort; ein Standort kann Rückgabeort für `0..*` Angebote sein. |

### 4.3 Warum haben wir diese Modellierung gewählt?

**Angebot als eigenständiges Konzept:** Ein Angebot entsteht bereits aus Suchkriterien und ist nicht mit einer verbindlichen Buchung gleichzusetzen. Erst die Entscheidung des Kunden kann zu einer `Buchung` führen. Daher wird die Beziehung `Angebot–Buchung` auf der Buchungsseite mit `0..1` modelliert.

**Optionaler Bezug zum Kunden:** Die Projektbeschreibung erlaubt es, verfügbare Fahrzeugtypen und Preise anzusehen, bevor persönliche Daten für eine Buchung erfasst werden. Deshalb kann ein `Angebot` zunächst ohne eindeutig identifizierten `Kunde` vorliegen (`0..1`). Nach einer erfolgreichen Buchung ist die Person hingegen eindeutig zugeordnet.

**Fahrzeugtyp statt Einzelauto:** Im Such- und Buchungsprozess wird ein **Fahrzeugtyp** ausgewählt, nicht zwingend ein Fahrzeug mit bestimmtem Kennzeichen. Ein Objekt für jedes konkrete Fahrzeug wäre für diese Abstraktionsebene unnötig.

**Standort mit zwei Rollen:** Für Abhol- und Rückgabeort verwenden wir dieselbe Klasse `Standort`, aber **zwei getrennte Assoziationen**. So können beide Orte identisch oder verschieden sein, ohne zwei fast identische Klassen zu erzeugen.

**Bewusste Annahme:** Wir gehen davon aus, dass ein bestimmtes Angebot höchstens zu einer Buchung führt. Die Projektbeschreibung erzwingt diese Einschränkung nicht ausdrücklich; sie vereinfacht das Modell und sollte als Entwurfsentscheidung dokumentiert werden.

---

## 5. Aufgabe 4 – Detailliertes Entitätsklassenmodell

![Abbildung 4 – Entitätsklassenmodell](Abbildungen/Aufgabe4_Entity_Klassenmodell.png)

*Abbildung 4: Verfeinerung des abstrakten Klassenmodells um persistente Entitäten, Attribute, Datentypen und weitere Beziehungen.*

### 5.1 Ziel und Herleitung

Aufgabe 4 baut auf Aufgabe 3 auf. Nun sollen die Objekte modelliert werden, deren Informationen später **in der Datenbank gespeichert** werden können. Deshalb beginnen alle Klassennamen mit **`E`** für *Entity*. Die fünf bisherigen Konzepte bleiben erhalten (`EKunde`, `EAngebot`, `EBuchung`, `EFahrzeugtyp`, `EStandort`) und werden um sechs zusätzliche Entitäten erweitert. Insgesamt enthält das Modell **elf Entitätsklassen**; dies entspricht der Vorgabe von ungefähr zehn Klassen.

Im Unterschied zum abstrakten Modell geben wir nun auch **Attribute und Datentypen** (`Integer`, `String`, `Date`, `Decimal`) an. Die Beziehungen werden weiterhin als UML-Assoziationen mit Multiplizitäten modelliert; eine gesonderte Darstellung von Fremdschlüsselspalten ist auf dieser Ebene nicht erforderlich.

### 5.2 Erläuterung aller elf Entitäten

| Entität | Attribute und Zweck |
|---|---|
| **`EKunde`** | `kundenId: Integer` identifiziert den Kunden. `name`, `straße`, `plz`, `ort`, `telefonnummer` und `email` (`String`) enthalten die für die Buchung erforderlichen persönlichen Angaben. |
| **`EAngebot`** | `angebotId: Integer`, `abholDatum: Date`, `rückgabeDatum: Date`, `fahrerAlter: Integer`, `angebotspreis: Decimal`, `emailFürAngebot: String`. Die Entität hält die konkreten Such-/Angebotsdaten fest; die E-Mail-Adresse für ein Angebot kann von der späteren Kundenadresse unabhängig sein. |
| **`EBuchung`** | `buchungsId: Integer` ist der interne Identifikator, `bestätigungsnummer: String` die kundenbezogene Referenz. `status: String` hält den Buchungszustand fest, `gesamtpreis: Decimal` den endgültigen Preis. |
| **`EFahrzeugtyp`** | `typId: Integer`, `bezeichnung: String`, `sitzplätze: Integer`, `gepäckkapazität: Integer`, `preisProTag: Decimal`, `mindestAlter: Integer`, `kraftstoffregelung: String`, `allgemeineBeschränkungen: String`. Damit können sowohl Suchergebnisse als auch wichtige Mietbeschränkungen beschrieben werden. |
| **`EFahrzeugmodell`** | `modellId: Integer`, `marke: String`, `modellName: String`. Die Trennung von Fahrzeugtyp und Modell ermöglicht es, verschiedene Marken und Modelle einer Fahrzeugkategorie zuzuordnen. |
| **`EStandort`** | `standortId: Integer`, `name: String`, `adresse: String`, `abholhinweise: String`. Die Standortdaten werden sowohl für die Suche als auch für spätere Abholinformationen verwendet. |
| **`EVersicherung`** | `versicherungId: Integer`, `bezeichnung: String`, `beschreibung: String`, `preisProTag: Decimal`. Modelliert angebotene Versicherungsoptionen und ihre Zusatzkosten. |
| **`EExtra`** | `extraId: Integer`, `bezeichnung: String`, `preisProTag: Decimal`. Beschreibt zusätzliche Leistungen, zum Beispiel Kindersitze oder Zusatzfahrer. |
| **`EAktionscode`** | `code: String`, `rabattProzent: Decimal`, `gültigBis: Date`. Repräsentiert einen zeitlich begrenzten prozentualen Rabatt. |
| **`EZahlungsinformation`** | `zahlungsId: Integer`, `karteninhaber: String`, `letzteVierZiffern: String`, `gültigBis: Date`, `zahlungsToken: String`, `prüfstatus: String`. Hält eine reduzierte Zahlungsreferenz und das Prüfergebnis fest. |
| **`EBuchungExtra`** | `buchungExtraId: Integer`, `menge: Integer`. Verknüpft eine konkrete Buchung mit einem Extra und speichert dessen Anzahl. |

### 5.3 Assoziationen und Multiplizitäten

Die **sechs grundlegenden Assoziationen** aus Aufgabe 3 bleiben erhalten, nun mit `E`-Präfix:

| Beziehung | Multiplizitäten und Bedeutung |
|---|---|
| `EKunde – EAngebot` | Pro Angebot `0..1` identifizierter Kunde; pro Kunde `0..*` Angebote. |
| `EKunde – EBuchung` | Pro Buchung genau `1` Kunde; pro Kunde `0..*` Buchungen. |
| `EAngebot – EBuchung` | Pro Buchung genau `1` Angebot; pro Angebot `0..1` Buchung. |
| `EAngebot – EFahrzeugtyp` | Pro Angebot genau `1` Fahrzeugtyp; pro Fahrzeugtyp `0..*` Angebote. |
| `EAngebot – EStandort` als **Abholort** | Pro Angebot genau `1` Abholort; pro Standort `0..*` Angebote. |
| `EAngebot – EStandort` als **Rückgabeort** | Pro Angebot genau `1` Rückgabeort; pro Standort `0..*` Angebote. |

Hinzu kommen **sechs zusätzliche Assoziationen** für den detaillierten Buchungsprozess:

| Beziehung | Multiplizitäten und Bedeutung |
|---|---|
| `EFahrzeugtyp – EFahrzeugmodell` (`umfasst`) | Ein Fahrzeugtyp umfasst `1..*` Fahrzeugmodelle; jedes Modell gehört im vereinfachten Modell zu `1` Typ. |
| `EBuchung – EZahlungsinformation` (`nutzt`) | Jede als erfolgreich gespeicherte Buchung hat `1` Zahlungsinformation; jede solche Zahlungsinformation gehört zu `1` Buchung. |
| `EBuchung – EVersicherung` (`wählt`) | `0..*` auf beiden Seiten: Eine Buchung kann mehrere Versicherungen auswählen, eine Versicherung kann in vielen Buchungen verwendet werden. |
| `EBuchung – EAktionscode` (`verwendet`) | Pro Buchung `0..1` Aktionscode; derselbe Code kann in `0..*` Buchungen verwendet werden. |
| `EBuchung – EBuchungExtra` (`enthält`) | Eine Buchung besitzt `0..*` BuchungExtra-Einträge; jeder Eintrag gehört zu genau `1` Buchung. |
| `EBuchungExtra – EExtra` (`bezeichnet`) | Jeder BuchungExtra-Eintrag referenziert genau `1` Extra; ein Extra kann in `0..*` BuchungExtra-Einträgen vorkommen. |

### 5.4 Begründung nicht offensichtlicher Modellierungsentscheidungen

**1. Warum trennen wir `EFahrzeugtyp` und `EFahrzeugmodell`?** Die Suchergebnisse und die Buchung beziehen sich auf eine Fahrzeugkategorie, während die detaillierte Ansicht verschiedene Marken und Modelle dieser Kategorie nennen kann. Die Trennung deckt beides ab, ohne konkrete Einzelfahrzeuge modellieren zu müssen.

**2. Warum gibt es `EBuchungExtra`?** Die direkte Viele-zu-Viele-Beziehung zwischen Buchung und Extra wäre nicht ausreichend, wenn derselbe Zusatz in einer bestimmten **Menge** gebucht wird (beispielsweise zwei Kindersitze). `EBuchungExtra` bildet daher die Auswahl und ihre Anzahl explizit ab. Wir modellieren es als Entität, weil diese Zuordnung eigene Daten besitzt.

**3. Warum speichern wir keine vollständigen Kreditkartennummern?** Die Aufgabenstellung verlangt die Eingabe und Prüfung der Kartendaten, nicht deren vollständige dauerhafte Speicherung. Wir gehen deshalb von einer Zahlungsprüfung durch einen geeigneten Zahlungsdienst aus. `EZahlungsinformation` enthält nur eine Referenz (Token), maskierte Karteninformationen und den Prüfstatus. Der Kartenprüfcode wird nicht gespeichert. Dies ist eine zusätzliche Sicherheitsentscheidung unseres Modells.

**4. Warum ist `EAngebot` unabhängig von `EKunde` möglich?** Suchergebnisse müssen auch vor der Eingabe vollständiger Kundendaten entstehen können. Die E-Mail-Adresse zum Versand eines Angebots kann deshalb direkt in `EAngebot` angegeben werden. Erst die abgeschlossene Buchung ist verbindlich einem Kunden zugeordnet.

**5. Warum gibt es sowohl `angebotspreis` als auch `gesamtpreis`?** Das Angebot enthält den für die ursprüngliche Suche errechneten Preis. Bei der Buchung können beispielsweise Versicherungen, Extras oder Aktionscodes hinzukommen. Deshalb wird der endgültige Gesamtpreis getrennt in `EBuchung` festgehalten.

**6. Warum besitzt `EBuchung` ein `status`-Attribut?** Der Status repräsentiert den Zustand einer Buchung. Im späteren Sprint kann darauf ein Zustandsdiagramm für `EBuchung` aufbauen. Die konkrete Menge möglicher Statuswerte wird hier noch nicht vollständig festgelegt.

**7. Warum nehmen wir für die Zahlung eine Eins-zu-eins-Beziehung an?** Dieses Entitätsmodell betrachtet die **erfolgreich bestätigte Buchung** des Happy Scenario. Dafür wird eine eindeutige zugehörige Zahlungsinformation angenommen. Würden wir auch fehlgeschlagene Zahlungen oder mehrere Zahlungsversuche speichern, müsste diese Beziehung in einer späteren Modellverfeinerung angepasst werden.

**8. Warum darf ein Aktionscode mehrfach verwendet werden?** Die Aufgabenstellung nennt keinen allgemeinen Einmalgebrauch. Daher modellieren wir **höchstens einen Code je Buchung**, erlauben aber dessen Wiederverwendung bei mehreren Buchungen. Individuelle oder nur einmal einlösbare Codes würden zusätzliche Einschränkungen erfordern.

**9. Warum werden keine konkreten Fahrzeuge modelliert?** Die vorliegenden Anforderungen konzentrieren sich auf die Verfügbarkeit und Auswahl von **Fahrzeugtypen**. Eine Fahrzeugflotte mit Kennzeichen, Wartungszustand und konkreter Zuordnung ist nicht ausdrücklich gefordert und würde den Umfang der Analyse unnötig vergrößern.

### 5.5 Abdeckung der Anforderungen

| Anforderungen aus der Projektbeschreibung | Modellierung im Sprint 1 |
|---|---|
| **1–2:** Suchkriterien, Verfügbarkeit, Fahrzeugtypen und Preise | `Mietangebote ermitteln`; `EAngebot`, `EFahrzeugtyp`, `EStandort` |
| **3:** Marke, Modell, Sitzplätze und Gepäck | `Fahrzeugdetails anzeigen`; `EFahrzeugtyp`, `EFahrzeugmodell` |
| **4:** Angebot per E-Mail | `Angebot per E-Mail versenden`; `EAngebot.emailFürAngebot` |
| **5:** Weiterleitung zur Buchung und Kontrolle der Mietdaten | `Buchung verarbeiten`, `Mietdaten zur Prüfung anzeigen`; `EBuchung` |
| **6:** Versicherungen, Zusatzoptionen und Rabattcodes | `Zusatzoptionen auswählen`; `EVersicherung`, `EExtra`, `EBuchungExtra`, `EAktionscode` |
| **7:** Einschränkungen, Kraftstoffbedingungen und Abholhinweise | `Mietbedingungen anzeigen`; `EFahrzeugtyp`, `EStandort`, `EVersicherung` |
| **8:** Persönliche Daten und Zahlungsangaben | `Kunden- und Zahlungsdaten erfassen`; `EKunde`, `EZahlungsinformation` |
| **9:** Datenprüfung, Speicherung, Bestätigungsnummer und E-Mail | Validierung und Bestätigungs-Use-Cases; `EBuchung`, `EZahlungsinformation` |

---

## 6. Konsistenz der vier Diagramme und Zusammenfassung

Alle vier Diagramme beschreiben **dasselbe AV-System auf unterschiedlichen Abstraktionsebenen**. Die Kundenziele aus Aufgabe 1 werden in Aufgabe 2 in interne Systemfunktionen aufgeteilt. Die für diese Funktionen zentralen Fachobjekte erscheinen in Aufgabe 3. Aufgabe 4 ergänzt die für die Datenhaltung und die Buchungsoptionen erforderlichen Entitäten. Die Namen, Rollen und Multiplizitäten der fünf Basiskonzepte bleiben dabei möglichst konsistent.

Die Modellierung folgt einem bewusst begrenzten Umfang: Wir betrachten Buchungen von **Fahrzeugtypen**, keine vollständige Fahrzeugflottenverwaltung. Angebote können ohne bereits identifizierten Kunden entstehen; abgeschlossene Buchungen sind einem Kunden zugeordnet. Die eigentliche Validierung der Kreditkartendaten erfolgt im Buchungsprozess, während langfristig nur reduzierte Zahlungsdaten gespeichert werden.

Diese vier Modelle bilden somit die Grundlage für Sprint 2, insbesondere für Sequenzdiagramme, Klassendesign und das Zustandsdiagramm von `EBuchung`.

### Vor der endgültigen Abgabe: Abgleich mit den Bildern

Die **PlantUML-Definitionen sind für Beziehungen und Multiplizitäten maßgeblich**, nicht automatisch erzeugte Bilder. Insbesondere ist in **Abbildung 4** vor der endgültigen PDF-Abgabe zu prüfen, dass die beiden Assoziationen `EAngebot–EStandort` die Multiplizitäten **`0..*` am Ende von `EAngebot`** und **`1` am Ende von `EStandort`** zeigen. Die beigefügte Bildfassung stellt diese beiden Werte vertauscht dar. Auch die anderen Diagramme sollten vor Abgabe mit ihren aktuellen PlantUML-Dateien verglichen werden.

### Verwendete Unterlagen

- *Software Engineering – Projekt 1: Autovermietung*, insbesondere Problemstellung und „Sprint 1 – Anforderungsanalyse“ (S. 1–3).
- *UML Tutorial*, insbesondere Use-Case-Diagramme, `<<include>>`/`<<extend>>`, Klassendiagramme, Assoziationen und Multiplizitäten.
