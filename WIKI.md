# Power-to-Heat – Anwender-Wiki

Dieses Wiki erklärt die Bedienung der Anlage, ihre Anzeigen und das Vorgehen bei Störungen. Die Bedienung erfolgt über die Anlagenvisualisierung.

> **Sicherheit:** Die Anlage arbeitet mit Netzspannung und hohen Temperaturen. Arbeiten an elektrischen Bauteilen gehören in die Hände einer Fachkraft. Schutzfunktionen dürfen nicht überbrückt werden. Bei wiederkehrenden Störungen die Ursache klären lassen.

## 1. Was macht die Anlage?

Die Anlage nutzt überschüssigen Strom der Photovoltaikanlage, um mit einem Heizstab Wärme zu erzeugen. Diese Wärme wird im Puffer gespeichert und kann für die Warmwasserversorgung genutzt werden.

Im Alltag regelt die Anlage die Heizleistung automatisch. Sie steuert die Pumpen, überwacht Temperaturen und Geräte und zeigt Betriebszustände sowie Störungen an. Ein Lufttrockner kann ebenfalls überschüssigen PV-Strom nutzen.

Wenn die Warmwasser-Sicherstellung eingeschaltet ist, kann der Heizstab auch Strom aus dem Netz beziehen.

## 2. Die Visualisierung verstehen

### Energie und Temperaturen

| Anzeige | Bedeutung |
| --- | --- |
| Dach / Fassade | Aktuelle PV-Leistung der beiden Wechselrichter. |
| Verbrauch | Aktueller Stromverbrauch des angezeigten Systems. |
| Netz | Aktueller Netzbezug oder Einspeisung. |
| Heizstab | Aktuelle Heizleistung und Temperaturen am Heizstab. |
| Temperaturfelder | Temperaturen der angezeigten Anlagen- und Speicherfühler. |

Die Heizleistung kann sich langsam ändern. Das ist normal: Die Anlage passt sie schrittweise an den verfügbaren Überschuss an.

### Pumpen

Ein grüner Punkt hinter einer Pumpe bedeutet, dass diese eingeschaltet ist. Ist der Punkt aus, ist die Pumpe ausgeschaltet. Bei Problemen zusätzlich die zugehörige Statusmeldung lesen.

### Meldungen

Die Meldungstabelle zeigt Ereignisse mit Zeitpunkt, Beschreibung, Fehlercode und Status. Der Fehlercode hilft bei Rückfragen an den Anlagenbetreuer.

| Status | Bedeutung |
| --- | --- |
| QUITTIERBAR | Die Meldung kann über **Quittieren** zurückgesetzt werden, wenn die Ursache behoben ist. |
| NICHT_QUITTIERBAR | Die Ursache liegt noch an oder verhindert eine Rücksetzung. |
| QUITTIERT | Die Meldung wurde bestätigt beziehungsweise zurückgesetzt. |

Nicht jede Meldung ist eine Störung. Zu wenig PV-Überschuss oder eine erreichte Zieltemperatur sind normale Betriebszustände.

## 3. Betriebsart wählen

| Betriebsart | Verhalten der Anlage |
| --- | --- |
| Heizstabbetrieb | Der Heizstab nutzt PV-Überschuss. Die Speicherladepumpe arbeitet abhängig von den Temperaturen. Die Heizkreispumpe läuft grundsätzlich; bei automatischer Warmwasserladung kann sie vorübergehend ausgeschaltet werden. |
| Unterstützungsbetrieb | Der Heizstab kann bei vorhandener Freigabe PV-Überschuss nutzen. Die Speicherladepumpe richtet sich nach der externen Kesselfreigabe und den Temperaturbedingungen. Die Heizkreispumpe soll eingeschaltet sein. |
| Kesselbetrieb | Der Heizstab ist deaktiviert und seine manuelle Freigabe wird ausgeschaltet. Die Speicherladepumpe richtet sich nach der externen Kesselfreigabe und den Temperaturbedingungen. Die Heizkreispumpe soll ausgeschaltet sein. |

Damit der Heizstab arbeiten kann, muss zusätzlich die **Heizstabfreigabe** eingeschaltet sein. Nach einem Wechsel aus dem Kesselbetrieb diese Freigabe prüfen.

## 4. Wichtige Einstellungen

Die folgenden Werte beschreiben die Standardeinstellungen. Maßgeblich sind die tatsächlich eingestellten Werte der Anlage.

| Einstellung | Standard oder Auswahl | Bedeutung |
| --- | --- | --- |
| Heizstabfreigabe | Ein/Aus | Erlaubt den Heizbetrieb. Bei ausgeschalteter Freigabe bleibt der Heizstab aus. |
| Warmwasser-Sicherstellung | Standard: Aus | Erlaubt das Nachheizen für die Warmwasserversorgung auch ohne ausreichenden PV-Überschuss. |
| Warmwasser-Zieltemperatur | 60 °C | Zielwert für die Warmwasser-Sicherstellung und die automatische Speicherladung. |
| Temperaturabstand für das Wiedereinschalten | 5 °C | Verhindert häufiges Ein- und Ausschalten nahe der Temperaturgrenze. |
| Untere Warmwassergrenze | 30 °C | Untere Grenze für die aktivierte Warmwasser-Sicherstellung. |
| Maximaltemperatur | 75 °C | Temperaturgrenze für den normalen Heizbetrieb. |
| Grüne Standby-Anzeige | Ein/Aus | Legt fest, ob die Bereitschaft bei zu wenig Überschuss grün angezeigt wird. |

Die Übertemperaturabschaltung ist eine Sicherheitsfunktion und keine Einstellung für den täglichen Betrieb. Die beschriebenen Standardgrenzen liegen bei 97 °C am internen und externen Heizstabfühler. Änderungen an Sicherheitsgrenzen mit dem Anlagenbetreuer abstimmen.

## 5. Wann heizt der Heizstab?

Für den Heizbetrieb müssen die Heizstabfreigabe eingeschaltet, eine passende Betriebsart gewählt und die Temperatur- und Sicherheitsbedingungen erfüllt sein. Eine aktive Störung kann den Betrieb sperren.

### Betrieb mit PV-Überschuss

Der Heizstab startet bei ungefähr 600 W verfügbarem Überschuss und schaltet bei ungefähr 400 W oder weniger wieder aus. Die unterschiedlichen Ein- und Ausschaltschwellen verhindern häufige Starts und Stopps.

Die Heizleistung wird automatisch angepasst und beträgt höchstens 3.500 W. Sie verändert sich mit maximal 100 W pro Sekunde. Bei wechselnder Bewölkung sind Änderungen der Heizleistung daher normal.

### Warmwasser-Sicherstellung

Ist die Warmwasser-Sicherstellung eingeschaltet, fordert sie bei zu niedriger Temperatur Wärme an.

Mit den Standardeinstellungen gilt:

- Start bei 55 °C oder darunter.
- Ende bei 60 °C oder darüber.
- Heizleistung während der Sicherstellung: 3.450 W.

Die Funktion kann Netzstrom verbrauchen. Sie sollte eingeschaltet sein, wenn diese zusätzliche Warmwasserversorgung gewünscht ist. Sicherheitsabschaltungen bleiben wirksam.

## 6. Temperaturen richtig einordnen

| Temperaturanzeige | Bedeutung |
| --- | --- |
| Interner Heizstabfühler | Temperatur im Bereich des Heizstabs. |
| Externer Heizstab-/Pufferfühler | Temperatur am externen Messpunkt des Puffers. |
| Warmwasserspeicher | Tatsächliche Temperatur am Fühler des Warmwasserspeichers. |

Die Werte können unterschiedlich sein, weil sich warmes Wasser im Speicher schichtet und die Fühler an unterschiedlichen Stellen messen. Die Temperatur am Puffer ist deshalb nicht automatisch gleich der Warmwassertemperatur.

### Maximaltemperatur erreicht

Bei Erreichen der normalen Maximaltemperatur stoppt der Heizstab automatisch. Mit 75 °C Maximaltemperatur und 5 °C Temperaturabstand ist eine erneute Freigabe bei 70 °C am externen Fühler möglich.

Eine zusätzliche automatische Freigabe ist möglich, wenn der interne Fühler mehr als 10 °C kühler als der externe Fühler ist. Daher kann der Heizstab nach einem temperaturbedingten Stopp wieder anlaufen. Die Temperaturüberwachung bleibt aktiv.

### Übertemperatur

Übertemperatur führt zur sofortigen Abschaltung und einer Störungsmeldung. Erst nach ausreichender Abkühlung ist eine Quittierung möglich. Bei wiederholter Übertemperatur die Anlage prüfen lassen.

## 7. Pumpen bedienen

### Speicherladepumpe

Die Speicherladepumpe transportiert Wärme zwischen Puffer und Warmwasserspeicher.

| Auswahl | Bedeutung |
| --- | --- |
| AUTO | Normalbetrieb: Die Anlage entscheidet anhand von Betriebsart, Freigabe und Temperaturen. |
| ON | Manuell eingeschaltet; die Sicherheitsabschaltung bleibt wirksam. |
| OFF | Manuell ausgeschaltet. |

Für den Alltag **AUTO** verwenden.

Im Automatikbetrieb endet die Ladung bei erreichter Warmwasser-Zieltemperatur. Die Sicherheitsabschaltung begrenzt die Ladung zusätzlich, standardmäßig auf 60 °C. Die Freigabe erfolgt nach Abkühlung um 2 °C, standardmäßig bei 58 °C oder darunter. Auch im manuellen Betrieb bleibt diese Abschaltung wirksam. Ist eine höhere Zieltemperatur eingestellt, bleibt die Sicherheitsgrenze maßgeblich.

Bei einem ungültigen Warmwasser-Temperaturwert schaltet die Pumpe zum Schutz aus. Wenn der Heizstab aus ist und der Puffer nicht genügend Wärme liefern kann, vermeidet die Automatik unnötiges Umladen.

Ist der Bypass beziehungsweise Schlüsselschalter aktiv, übernimmt der Kessel beziehungsweise die externe Steuerung die Ansteuerung.

### Heizkreispumpe und Warmwasser-Vorrang

Im Heizstabbetrieb hat die automatische Warmwasserladung Vorrang: Während die Speicherladepumpe in **AUTO** läuft, kann die Heizkreispumpe ausgeschaltet sein. Nach dem Ende der Ladung wird ihr vorheriger Zustand wiederhergestellt.

Eine manuell ein- oder ausgeschaltete Speicherladepumpe löst diesen automatischen Vorrang nicht aus. Der tatsächliche Zustand der Heizkreispumpe ist an der Pumpenanzeige erkennbar.

### Automatischer Sommerlauf

Außerhalb der Heizperiode läuft die Heizkreispumpe regelmäßig kurz, damit sie nicht festsetzt:

- Vorgesehen ist ein Lauf alle sieben Tage für 15 Minuten.
- Bevorzugt startet er bei einer Puffertemperatur ab 85 °C.
- Wird diese Temperatur nach einem Tag Wartezeit nicht erreicht, wird der Lauf zum nächsten Mittag um 12:00 Uhr erzwungen.

Ein kurzer Pumpenlauf ohne aktuellen Heizbedarf kann deshalb normal sein.

### Warmwasser-Zirkulationspumpe

Die Zirkulationspumpe sorgt für die Umwälzung des Warmwassers und arbeitet unabhängig von der Heizstableistung.

| Auswahl oder Anzeige | Bedeutung |
| --- | --- |
| Automatik | Ein Start beziehungsweise Tastendruck lässt die Pumpe für die eingestellte Laufzeit laufen. |
| Manuell an | Die Pumpe bleibt eingeschaltet, sofern das Steuergerät erreichbar ist. |
| Manuell aus | Die Pumpe bleibt ausgeschaltet. |
| Laufzeit | Dauer eines automatischen Laufs; standardmäßig 30 Minuten. |
| Restlaufzeit | Verbleibende Zeit des aktuellen Laufs. |
| Offline | Das Steuergerät ist nicht erreichbar; ein Start ist nicht möglich. |

## 8. Lufttrockner

Der Lufttrockner nutzt PV-Überschuss, wenn seine Freigabe eingeschaltet ist. Ohne Freigabe bleibt er aus.

Die Automatik wartet bei ausreichendem Überschuss 15 Minuten bis zum Einschalten. Bei zu wenig Überschuss wartet sie ebenfalls 15 Minuten bis zum Ausschalten. Dadurch führen kurze Wetterwechsel nicht sofort zu einem Schaltvorgang.

Nach der dritten automatischen Abschaltung wegen fehlendem PV-Überschuss bleibt der Lufttrockner bis Mitternacht gesperrt. Der Grund wird im Status angezeigt; diese Abschaltungen und die Tagessperre lösen keine Push-Benachrichtigung aus.

Ab ungefähr 60 °C Puffertemperatur kann die Anlage einen Teil des PV-Stroms für den Lufttrockner reservieren. Der Heizstab kann mit verringerter Leistung weiterarbeiten, wenn genügend Strom für beide verfügbar ist.

### Hinweis auf vollen Tank oder ausgeschaltetes Gerät

Bei entsprechendem Rückgang der Leistungsaufnahme kann eine Meldung auf einen vollen Tank oder ein ausgeschaltetes Gerät hinweisen. Tank und Gerätezustand prüfen.

Pro Ereignis wird die Push-Benachrichtigung einmal gesendet. Erst wenn das Gerät wieder eine Leistungsaufnahme zeigt, kann ein neues Ereignis erneut gemeldet werden.

Bei ausbleibendem Start die Freigabe, den angezeigten Schaltgrund, den PV-Überschuss und eine mögliche Tagessperre prüfen.

## 9. LED- und Ampelanzeigen

| Anzeige | Bedeutung |
| --- | --- |
| Grün | Bereit beziehungsweise Standby bei zu wenig PV-Überschuss, sofern die Standby-Anzeige eingeschaltet ist. |
| Gelb | Heizstab arbeitet im normalen PV-Betrieb. |
| Rot | Störung aktiv. Die genaue Ursache steht in der Meldungstabelle. |
| Grün und Gelb | Warmwasser-Sicherstellung aktiv oder wartend. |
| Alle blinken | Selbsttest läuft. |
| Alle aus | Regelung manuell deaktiviert oder kein besonderer Anzeigezustand. |

Blinkt die Quittieranzeige, liegt eine quittierbare Störung vor. Vor dem Quittieren die Meldung lesen und ihre Ursache prüfen.

## 10. Meldungen und Störungen

| Meldung | Bedeutung | Was tun? |
| --- | --- | --- |
| Regelung manuell deaktiviert | Der Heizstab ist nicht freigegeben. | Heizstabfreigabe einschalten, wenn Heizbetrieb gewünscht ist. |
| Regelung aktiv | Der Heizstab nutzt PV-Überschuss. | Keine Aktion erforderlich. |
| Überschuss zu gering | Für den PV-Heizbetrieb fehlt Leistung. | Auf mehr PV-Überschuss warten. |
| Maximaltemperatur erreicht | Der Heizstab pausiert temperaturbedingt. | Keine Quittierung nötig; die Freigabe erfolgt automatisch, wenn die Temperaturbedingungen erfüllt sind. |
| Warmwasser-Sicherstellung aktiv oder wartend | Die Sicherstellung heizt oder wartet auf ihren Einschaltpunkt. | Prüfen, ob diese Funktion gewünscht ist. |
| Übertemperatur | Der Heizstab wurde sofort abgeschaltet. | Abkühlen lassen und Ursache prüfen lassen; anschließend quittieren, sobald möglich. |
| FI/LS aus | Der elektrische Schutzschalter meldet keine Freigabe. | Ursache durch eine fachkundige Person prüfen lassen. Nach Behebung gegebenenfalls quittieren. |
| Heizleistung weicht vom erwarteten Wert ab | Der Heizstab ist gesperrt. | Bei erneutem Auftreten nach automatischem Rücksetzversuch den Anlagenbetreuer hinzuziehen. Nach Ursachenbehebung quittieren. |
| Heizstabsteuerung nicht erreichbar oder gestört | Die Heizleistung wird abgeschaltet. | Verbindung und Gerät durch den Anlagenbetreuer prüfen lassen. |
| Gerät offline, Wartezeit läuft | Ein Gerät ist vorübergehend nicht erreichbar. | Bei anhaltender Meldung Verbindung prüfen lassen. |
| Gerät offline nach Wartezeit | Der Heizstab bleibt aus. | Erreichbarkeit wiederherstellen lassen. |
| Alle Geräte wieder online | Die Verbindung ist wieder vorhanden. | Status auf weitere Meldungen prüfen. |
| Nachtmodus | Zwischen 22:00 und 04:00 Uhr sind bestimmte Erreichbarkeitsprüfungen pausiert. | Keine Aktion erforderlich. |
| Kesselbetrieb | Der Heizstab ist aufgrund der Betriebsart aus. | Bei gewünschtem Heizstabbetrieb Betriebsart ändern und Heizstabfreigabe prüfen. |

Für eine Rückfrage den genauen Meldungstext, Fehlercode und Zeitpunkt bereithalten.

## 11. Störung quittieren

1. Die Meldung in der Visualisierung lesen.
2. Die Ursache beheben beziehungsweise durch eine Fachkraft beheben lassen.
3. Bei Übertemperatur ausreichende Abkühlung abwarten.
4. Sobald der Status **QUITTIERBAR** erscheint, **Quittieren** betätigen.
5. Prüfen, ob die Störung verschwunden ist und die Anlage wieder den erwarteten Zustand zeigt.

Eine noch anstehende Ursache lässt sich nicht durch Quittieren beseitigen. Tritt die Störung erneut auf, die Anlage prüfen lassen.

Zu wenig Überschuss und die normale Maximaltemperatur benötigen keine manuelle Quittierung.

## 12. Selbsttest und Kalibrierung

### Selbsttest

Der Selbsttest prüft die Temperaturmessung und die Reaktion des Heizstabs auf eine kleine Testleistung.

Wenn die Funktion in der Bedienoberfläche angeboten wird:

1. Sicherstellen, dass keine Störung anliegt.
2. Den Selbsttest starten.
3. Während des Tests blinken alle LEDs.
4. Das Ergebnis in der Statusanzeige beziehungsweise Meldungstabelle lesen.

Bei fehlgeschlagenem Test oder einer unplausiblen Temperaturmessung den Anlagenbetreuer hinzuziehen.

### Kalibrierung

Bei der Kalibrierung misst die Anlage das Verhalten des Heizstabs bei mehreren Leistungsstufen. Die normale Regelung pausiert dabei.

Diese Funktion gehört zur Einrichtung und Betreuung der Anlage. Sie sollte nur nach Abstimmung mit dem Anlagenbetreuer, unter Aufsicht und bei ausreichender Wärmeabnahme verwendet werden. Für den täglichen Betrieb ist keine Kalibrierung erforderlich.

## 13. Schnellhilfe

| Beobachtung | Zuerst prüfen |
| --- | --- |
| Heizstab bleibt aus | Heizstabfreigabe, Betriebsart, PV-Überschuss, Temperaturen und Meldungstabelle. |
| Rote LED leuchtet | Meldungstext und Fehlerstatus lesen. |
| Grün leuchtet, Heizstab ist aus | Möglicherweise normaler Standby wegen zu wenig PV-Überschuss. |
| Grün und Gelb leuchten | Status der Warmwasser-Sicherstellung und Temperatur prüfen. |
| Speicherladepumpe läuft, Heizkreispumpe ist aus | Im Heizstabbetrieb kann der automatische Warmwasser-Vorrang aktiv sein. |
| Pumpe läuft nicht | Automatik-/Handbetrieb, Temperaturbedingungen, Bypass und Störungsmeldungen prüfen. |
| Heizkreispumpe läuft kurz ohne Heizbedarf | Möglicherweise automatischer Sommerlauf. |
| Lufttrockner bleibt aus | Freigabe, Schaltgrund, Überschuss, Tagessperre und Gerätezustand prüfen. |
| Störung lässt sich nicht quittieren | Prüfen, ob die Ursache noch anliegt und der Status bereits QUITTIERBAR ist. |
