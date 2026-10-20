# Begründung der verwendeten Beziehungen

### include-Beziehungen wurden für verpflichtende Teilfunktionen des Buchungsprozesses verwendet:

- Mietbedingungen anzeigen: Die Mietbedingungen müssen dem Kunden vor der Buchung bereitgestellt werden.
- Kunden- und Zahlungsdaten erfassen: Diese Informationen werden für eine erfolgreiche Buchung benötigt.
- Buchungsdaten und Kreditkarte validieren: Die eingegebenen Daten müssen vor dem Abschluss der Buchung überprüft werden.

### Einfache Verbindungslinien wurden verwendet, um weitere funktionale Zusammenhänge übersichtlich darzustellen, ohne sie ausdrücklich als <<include>> zu modellieren.

- Bei Mietangebote ermitteln betrifft dies die Erfassung der Suchkriterien, die Verfügbarkeitsprüfung, die Preisberechnung und die Anzeige der Suchergebnisse.
- Bei Buchung verarbeiten betrifft dies die Anzeige der Mietdaten zur Überprüfung, die Speicherung der Buchung mit der Erzeugung einer Bestätigungsnummer sowie den Versand der Bestätigungs-E-Mail.
