# Power-to-Heat Anlagen-Wiki

Dieses Wiki ist als GitHub-Wiki-Seite für Anwender der Anlage gedacht. Es beschreibt die Bedienung, die sichtbaren Zustände und die einstellbaren Datenpunkte der Power-to-Heat-Anlage – ohne Code-Erklärung.

> **Wichtig:** Die Anlage arbeitet mit Netzspannung, Heizstab, Pumpen und Temperaturen. Änderungen an Grenzwerten, Betriebsarten oder Hardware-Datenpunkten dürfen nur vorgenommen werden, wenn die Auswirkungen bekannt sind. Sicherheitsfunktionen wie FI/LS, Übertemperatur und Offline-Überwachung dürfen nicht dauerhaft überbrückt werden.

---

## 1. Zweck der Anlage

Die Anlage nutzt PV-Überschuss, um elektrische Energie in Wärme umzuwandeln. Der Heizstab lädt dabei den Puffer bzw. das Warmwassersystem, wenn genügend Überschuss vorhanden ist oder wenn die Warmwasser-Sicherstellung aktiv wird.

Die Steuerung übernimmt im Alltag:

- Erkennen von PV-Überschuss und Netzbezug.
- Leistungsregelung des Heizstabs bis maximal 3.500 W.
- Warmwasser-Sicherstellung bei zu niedriger Speichertemperatur.
- Schutz bei Übertemperatur, FI/LS-Ausfall, Offline-Geräten und Leistungsabweichungen.
- Koordination von Speicherladepumpe, Heizkreispumpe und Warmwasser-Zirkulationspumpe.
- PV-geführte Einbindung eines Lufttrockners mit Heizstab-Reserve.
- Anzeige von Status, Fehlern, LED-Farben und Meldungslog.

---

## 2. Visualisierung lesen

Die Visualisierung zeigt links den Energiefluss und rechts Bedienung, Meldungen und Grenzwerte.

### Energiefluss

- **Dach / Fassade:** aktuelle PV-Leistung der beiden Wechselrichter.
- **Verbrauch:** aktueller Verbrauch der Anlage bzw. des betrachteten Systems.
- **Netz:** aktueller Netzbezug oder Einspeisung.
- **Heizstab:** aktuelle Heizstableistung und Temperaturen am Heizstab.
- **Temperaturfelder unten:** relevante Anlagen- und Speicherfühler.

### Pumpenanzeige

Hinter jeder Pumpe befindet sich ein **grüner Punkt**:

- **Grüner Punkt sichtbar/an:** Die jeweilige Pumpe ist gerade eingeschaltet.
- **Kein grüner Punkt bzw. Punkt aus:** Die jeweilige Pumpe ist aktuell ausgeschaltet.

Das betrifft insbesondere:

- Speicherladepumpe / WW-Speicherladepumpe.
- Heizkreispumpe.
- Warmwasser-Zirkulationspumpe, sofern sie in der Visualisierung dargestellt ist.

### Meldungstabelle

Die Tabelle zeigt Ereignisse und Fehler in zeitlicher Reihenfolge.

| Spalte | Bedeutung |
| --- | --- |
| Zeit | Zeitpunkt der Meldung. |
| Meldung | Klartextbeschreibung, was passiert ist. |
| Fehlercode | Technischer Code zur Zuordnung. |
| Status | z. B. `QUITTIERBAR`, `NICHT_QUITTIERBAR` oder `QUITTIERT`. |

Bei quittierbaren Fehlern kann über **Quittieren** ein Reset versucht werden. Nicht quittierbare Fehler müssen zuerst physikalisch verschwinden, z. B. Temperatur wieder unter Grenzwert oder FI/LS wieder eingeschaltet.

---

## 3. Betriebsarten

Der Datenpunkt **`0_userdata.0.Heizstab.V2.Regelung.Betriebsmodus`** bestimmt die Hauptbetriebsart.

| Betriebsart | Bedeutung für Anwender |
| --- | --- |
| `Heizstabbetrieb` | Normaler PV-Heizbetrieb. Der Heizstab darf Überschuss nutzen. Die Speicherladepumpe arbeitet temperaturgeführt. Die Heizkreispumpe läuft grundsätzlich, wird aber nur bei automatischem Speicherladepumpen-Vorrang abgeschaltet. |
| `Unterstützungsbetrieb` | Unterstützender Betrieb mit externer Kessel-/Logikfreigabe. Die Heizstabregelung kann PV-Überschuss nutzen, wenn sie freigegeben ist. Die Speicherladepumpe folgt der externen Freigabe. Die Heizkreispumpe soll eingeschaltet sein. |
| `Kesselbetrieb` | Heizstab wird deaktiviert. Die manuelle Heizstabfreigabe wird ausgeschaltet. Die Speicherladepumpe folgt der externen Kessel-/Logikfreigabe. Die Heizkreispumpe soll ausgeschaltet sein. |

Zusätzlich muss für Heizstabregelung **`0_userdata.0.Heizstab.V2.Regelung.ENABLE = true`** gesetzt sein. Ist diese Freigabe aus, bleibt der Heizstab aus, auch wenn PV-Überschuss vorhanden ist.

---

## 4. Einstellbare Haupt-Datenpunkte

Alle Hauptdatenpunkte liegen unter **`0_userdata.0.Heizstab.V2.`**.

### Regelung und Bedienung

| Datenpunkt | Typ | Bedeutung |
| --- | --- | --- |
| `Regelung.ENABLE` | Ein/Aus | Manuelle Freigabe der Heizstabregelung. `false` bedeutet: Heizstab bleibt aus. |
| `Regelung.Betriebsmodus` | Auswahl | Auswahl zwischen `Heizstabbetrieb`, `Unterstützungsbetrieb` und `Kesselbetrieb`. |
| `Regelung.Fail_Reset` | Button | Fehlerquittierung für quittierbare Fehler. |
| `Kalibrierung.Start` | Button | Startet die Heizstab-Kalibrierung. Nur unter kontrollierten Bedingungen verwenden. |
| `Selbsttest.Start` | Button | Startet einen Funktionstest mit kleiner Heizleistung. |
| `Ampel.StandbyBlink_ENABLE` | Ein/Aus | Erlaubt die grüne Standby-Anzeige bei zu wenig Überschuss. |

### Temperatur- und Warmwasserparameter

| Datenpunkt | Default | Bedeutung |
| --- | ---: | --- |
| `Parameter.WW_Zieltemperatur` | 60 °C | Zieltemperatur für die Warmwasser-Sicherstellung und Speicherladepumpenlogik. |
| `Parameter.DeltaT_Regelbereich` | 5 K | Hysterese. Verhindert ständiges Ein-/Ausschalten rund um einen Grenzwert. |
| `Parameter.MinTemp` | 30 °C | Untere Warmwassergrenze. Bei aktivierter Sicherstellung wird spätestens hier geheizt. |
| `Parameter.MaxTemp` | 75 °C | Normale Maximaltemperatur. Ab hier wird der Heizstab gesperrt, bis die Temperatur um Delta-T gefallen ist oder die zusätzliche Schichtungs-Freigabe greift. |
| `Parameter.Übertemperatur_intern` | 97 °C | Harte Sicherheitsgrenze am internen Heizstabfühler. |
| `Parameter.Übertemperatur_extern` | 97 °C | Harte Sicherheitsgrenze am externen Heizstab-/Pufferfühler. |
| `Parameter.WW_Sicherstellung_AN` | aus | Aktiviert die Warmwasser-Sicherstellung. |

### Anzeige- und Diagnose-Datenpunkte

| Datenpunkt | Bedeutung |
| --- | --- |
| `Regelung.AKTIV` | Zeigt, ob die Heizstabregelung aktuell aktiv heizt. |
| `Regelung.Status` | Aktueller Klartextstatus mit Code. |
| `Regelung.Log` | Meldungslog als JSON; neueste Einträge oben. |
| `Regelung.NextRunIn_Sek` | Countdown bis zur nächsten regulären Regelberechnung. |
| `Regelung.PWM_Target_Watt` | Ziel-Leistung, auf die die Rampe fährt. |
| `Regelung.PWM_Watt` | Aktuell gerampte Sollleistung. |
| `Regelung.PWM_Prozent` | Aktueller PWM-Ausgabewert in Prozent. |
| `Regelung.Soll_Watt_Unkalibriert` | Berechnete Roh-Sollleistung vor Kalibrierkorrektur. |
| `Regelung.QuittierTaster_Blink` | Blinkt, wenn ein quittierbarer Fehler ansteht. |
| `Regelung.PumpenSkripte_OK` | Zeigt, ob die erwarteten Pumpenskriptversionen vorhanden sind. |
| `Regelung.PumpenSkripte_Fehler` | Textinformation bei Versionsabweichung. |

---

## 5. Wann heizt der Heizstab?

Der Heizstab kann nur heizen, wenn alle folgenden Bedingungen erfüllt sind:

1. Betriebsmodus ist nicht `Kesselbetrieb`.
2. `Regelung.ENABLE` steht auf `true`.
3. Kein harter Fehler ist aktiv.
4. FI/LS meldet OK.
5. Relevante Geräte sind online oder Online-Prüfung ist im Nachtfenster pausiert.
6. Temperaturgrenzen lassen den Betrieb zu.
7. Entweder ist genügend PV-Überschuss vorhanden oder die Warmwasser-Sicherstellung fordert Wärme an.

### PV-Überschussbetrieb

Die Anlage berücksichtigt, dass der Heizstab selbst bereits Strom verbraucht. Deshalb wird die verfügbare Leistung aus Netzleistung und aktueller Heizstableistung zurückgerechnet.

Schaltverhalten:

- **Einschalten:** wenn verfügbarer Überschuss über ca. 600 W liegt.
- **Ausschalten:** wenn verfügbarer Überschuss auf ca. 400 W oder darunter fällt.
- **Leistungsbereich:** 0 bis 3.500 W.
- **Rampe:** maximal 100 W pro Sekunde, damit die Leistung nicht sprunghaft wechselt.

### Warmwasser-Sicherstellung

Wenn **`Parameter.WW_Sicherstellung_AN = true`**, hat Warmwasser Vorrang vor normalem PV-Überschussbetrieb.

Mit den Defaultwerten gilt:

- Zieltemperatur: 60 °C.
- Delta-T: 5 K.
- Einschalten bei 55 °C oder darunter.
- Ausschalten bei 60 °C oder darüber.
- Feste Heizleistung während Sicherstellung: 3.450 W.

Die Sicherstellung kann auch dann heizen, wenn nicht genug PV-Überschuss vorhanden ist. Sie dient dazu, eine Mindestversorgung mit Warmwasser sicherzustellen.

---

## 6. Temperatursensoren und ihre Aufgaben

| Sensor / Datenpunkt | Anzeige / Bedeutung | Wird verwendet für |
| --- | --- | --- |
| `modbus.2.holdingRegisters.1.1001_(R)_Temperatur_Sensor_intern` | Interner Heizstabfühler | Maximaltemperatur, Zusatz-Freigabe bei Schichtung, harte Übertemperaturüberwachung intern; Plausibilitätsprüfung im Selbsttest. |
| `modbus.2.holdingRegisters.1.1030_Temp_2` | Externer Heizstab-/Pufferfühler | Maximaltemperatur, Hysterese-Freigabe, Zusatz-Freigabe bei Schichtung, harte Übertemperatur extern, PV-/WW-Regelung als Speicher-/Pufferwert. |
| `alias.0.Heizung.Speicher_Warmwasser.ACTUAL` | Warmwasser-Isttemperatur | Speicherladepumpe: Sicherheitsabschaltung ab 60 °C, Zieltemperaturbewertung und Umladelogik. |
| `0_userdata.0.Heizstab.V2.Parameter.WW_Zieltemperatur` | Einstellbarer Sollwert | Zielwert für Warmwasser-Sicherstellung und Speicherladepumpe. |
| `0_userdata.0.Heizstab.V2.Parameter.DeltaT_Regelbereich` | Einstellbare Hysterese | Freigabe nach Maximaltemperatur und Einschaltpunkt der Warmwasser-Sicherstellung. |


### Zusatz-Freigabe bei Wärmeschichtung

Im Speicher kann sich Wärme schichten: oben bzw. am externen Fühler ist es bereits sehr warm, während tiefer im Speicher bzw. am internen Heizstabfühler noch deutlich kühlere Bereiche vorhanden sind. Damit nicht nur ein kleiner Speicherbereich heiß bleibt, gibt es zusätzlich zur normalen MaxTemp-Hysterese eine Schichtungs-Freigabe.

Wenn der Heizstab wegen `Parameter.MaxTemp` ausgeschaltet wurde, darf er wieder einschalten, sobald der interne Sensor mehr als `10 K` kühler als der externe Sensor ist. Dadurch wird die Wärmeschichtung gezielt gebrochen und das komplette Speichervolumen kann besser auf eine hohe Temperatur gebracht werden. Die Sicherheitsgrenze bleibt trotzdem aktiv: Erreicht anschließend wieder einer der beiden Sensoren `Parameter.MaxTemp`, wird der Heizstab erneut ausgeschaltet.

### Unterschied Maximaltemperatur und Übertemperatur

| Schutz | Default | Wirkung |
| --- | ---: | --- |
| Normale Maximaltemperatur `Parameter.MaxTemp` | 75 °C | Heizstab wird ausgeschaltet, wenn intern oder extern die Maximaltemperatur erreicht ist. Freigabe erfolgt weiterhin über die bestehende Hysterese, z. B. bei 75 °C und 5 K wieder bei 70 °C am externen Sensor. Zusätzlich wird freigegeben, wenn intern mehr als 10 K kühler als extern ist. |
| Harte Übertemperatur intern/extern | 97 °C | Sofortabschaltung ohne Rampe. Fehler ist erst quittierbar, wenn die Temperaturen wieder unter den Grenzwerten liegen. |

---

## 7. Pumpen

### Speicherladepumpe

Die Speicherladepumpe transportiert Wärme zwischen Puffer und Warmwasserspeicher. Ihr Zustand wird über **`0_userdata.0.Heizung.Speicherladepumpe.Status`** angezeigt.

Einstellbare Bedienung:

| Datenpunkt | Bedeutung |
| --- | --- |
| `0_userdata.0.Heizung.Speicherladepumpe.ManualMode` | `AUTO`, `ON` oder `OFF`. `AUTO` ist Normalbetrieb. |
| `shelly.1.shellyprorgbwwpm#ece334f93c48#1.RGB0.Switch` | Bypass/Schlüsselschalter. Bei aktivem Bypass steuert der Kessel bzw. die externe Logik; die Skript-Soll/Ist-Abweichung wird nicht bewertet. |

Wichtige Anzeigen:

| Datenpunkt | Bedeutung |
| --- | --- |
| `0_userdata.0.Heizung.Speicherladepumpe.Status` | Sollzustand der Pumpe. |
| `0_userdata.0.Heizung.Speicherladepumpe.Ist` | Rückgemeldeter Istzustand vom Schütz. |
| `0_userdata.0.Heizung.Speicherladepumpe.SollIstFehler` | `true`, wenn Soll und Ist nicht zusammenpassen und kein Bypass aktiv ist. |
| `0_userdata.0.Heizung.Speicherladepumpe.FehlerShellyOffline` | `true`, wenn der Shelly länger als 10 s offline ist. |
| `0_userdata.0.Heizung.Speicherladepumpe.SicherheitsabschaltungAktiv` | `true`, wenn Übertemperatur oder ein ungültiger WW-Fühler die Pumpe fail-safe ausgeschaltet hat. |
| `0_userdata.0.Heizung.Speicherladepumpe.Parameter.SicherheitsabschaltungEin` | Abschalttemperatur für die WW-Sicherheitsabschaltung, Reset automatisch 2 K darunter, Default `60 °C`. |

Verhalten:

- Bei Warmwasser-Isttemperatur ab `Parameter.SicherheitsabschaltungEin` (Default 60 °C) schaltet die Pumpe aus.
- Freigabe nach dieser Sicherheitsabschaltung erst wieder 2 K unter `Parameter.SicherheitsabschaltungEin` (Default: Reset bei 58 °C) oder darunter.
- Der separate Freigabe-DP entfällt; der Reset wird immer aus der Abschalttemperatur minus 2 K berechnet.
- Wenn der Warmwasserfühler ungültig oder nicht plausibel ist, schaltet die Pumpe fail-safe aus, damit kein vorheriger EIN-Zustand weiterläuft.
- In `AUTO` schaltet die Pumpe weiterhin bei erreichter WW-Zieltemperatur ab. Nur `ManualMode = ON` darf bis zur Sicherheitsabschaltung laden.
- Liegt die WW-Zieltemperatur über der Sicherheitsabschaltung, nutzt `AUTO` die Sicherheitsabschaltung als effektive Zielgrenze; `ManualMode = ON` darf nur bis zur Sicherheitsabschaltung laden.
- Die Soll-/Ist-Überwachung wartet nach jedem Sollwertwechsel 500 ms, damit das Schütz anziehen oder abfallen kann.
- Im `Kesselbetrieb` und `Unterstützungsbetrieb` folgt sie der externen Kessel-/Logikfreigabe, solange die AUTO-Zieltemperatur noch nicht erreicht ist.
- Im `Heizstabbetrieb` arbeitet sie temperaturgeführt.
- Wenn der Heizstab aus ist und der Puffer die Zieltemperatur nicht erreichen kann, wird nicht unnötig umgeladen.

### Heizkreispumpe

Die Heizkreispumpe wird über ein Stromstoßrelais geschaltet. Die Steuerung sendet nur kurze Toggle-Impulse; der reale Zustand kommt über die Rückmeldung.

Einstellbare Bedienung:

| Datenpunkt | Bedeutung |
| --- | --- |
| `0_userdata.0.Heizung.Heizkreispumpe.manualRequest` | Manueller Wunsch `true = EIN`, `false = AUS`. |

Wichtige Anzeigen:

| Datenpunkt | Bedeutung |
| --- | --- |
| `0_userdata.0.Heizung.Heizkreispumpe.state` | Realer Zustand der Heizkreispumpe. |
| `0_userdata.0.Heizung.Heizkreispumpe.lastAction` | Letzte Aktion oder Diagnosemeldung. |
| `0_userdata.0.Heizung.Heizkreispumpe.shellyOffline` | Shelly offline; Pumpe kann nicht zuverlässig gesteuert werden. |
| `0_userdata.0.Heizung.Heizkreispumpe.speicherladepumpePriorityActive` | Warmwasser-Vorrang aktiv. |

Verhalten nach Betriebsart:

| Betriebsart | Heizkreispumpe |
| --- | --- |
| `Unterstützungsbetrieb` | Ein. |
| `Heizstabbetrieb` | Ein, außer während automatischem Speicherladepumpen-Vorrang (`ManualMode = AUTO` und Speicherladepumpe läuft). |
| `Kesselbetrieb` | Aus. |

Wenn die Speicherladepumpe im Heizstabbetrieb automatisch (`ManualMode = AUTO`) startet, wird der vorherige Zustand der Heizkreispumpe gespeichert. Danach wird die Heizkreispumpe ausgeschaltet, damit Warmwasser Vorrang hat. Wenn die Speicherladepumpe wieder aus ist, wird der vorherige Zustand wiederhergestellt. Manuelles `ON` oder `OFF` der Speicherladepumpe löst keinen Warmwasser-Vorrang aus und schaltet die Heizkreispumpe nicht um.

#### Sommer-Freilauf

Damit die Heizkreispumpe außerhalb der Heizperiode nicht festsetzt, besitzt sie einen automatischen Sommer-Freilauf:

- Prüfung alle 15 Minuten.
- Geplanter Freilauf alle 7 Tage.
- Laufzeit 15 Minuten.
- Bevorzugter Start erst bei Puffertemperatur ab 85 °C.
- Wenn diese Temperatur nach 1 Tag Wartezeit nicht erreicht wurde, wird der Freilauf zum nächsten Mittag um 12:00 Uhr erzwungen.

Anzeigen dazu liegen unter `0_userdata.0.Heizung.Heizkreispumpe.summerExercise.*`, z. B. letzter Lauf, aktiver Freilauf und nächste Fälligkeit.

### Warmwasser-Zirkulationspumpe

Die Warmwasser-Zirkulationspumpe läuft unabhängig von der Heizstableistungsregelung. Sie kann zeitgesteuert oder manuell betrieben werden.

Alle Bedien-Datenpunkte liegen unter **`0_userdata.0.Heizung.WW-Pumpe.`**.

| Datenpunkt | Bedeutung |
| --- | --- |
| `Start` | Startet die Pumpe im Automatikmodus für die eingestellte Laufzeit. |
| `LaufzeitMin` | Laufzeit in Minuten; Default 30 min. |
| `RestlaufzeitMin` | Anzeige der verbleibenden Laufzeit. |
| `Manuell` | `auto`, `on` oder `off`. |
| `Status` | `Bereit`, `EIN: x`, `Manuell AN`, `Manuell AUS` oder `Offline`. |

Verhalten:

- In `auto` startet ein Tastendruck oder `Start` den Timer.
- In `on` bleibt die Pumpe dauerhaft an, sofern der Shelly online ist.
- In `off` bleibt sie aus.
- Wenn der Shelly offline ist, startet sie nicht und zeigt `Offline`.


### Lufttrockner

Der Lufttrockner ist ein zusätzlicher PV-Verbraucher. Er wird über die VIS-Freigabe eingeschaltet, wenn genug PV-Überschuss für längere Zeit vorhanden ist.

Wichtige Bedien- und Anzeige-Datenpunkte:

| Datenpunkt | Bedeutung |
| --- | --- |
| `0_userdata.0.Heizung.Lufttrockner.Freigabe` | Anwenderfreigabe. Ohne Freigabe bleibt der Lufttrockner aus. |
| `0_userdata.0.Heizung.Lufttrockner.IstLaeuft` | Zeigt anhand der gemessenen Leistung, ob das Gerät wirklich läuft. |
| `0_userdata.0.Heizung.Lufttrockner.Status` | Klartextstatus. |
| `0_userdata.0.Heizung.Lufttrockner.LetzterSchaltgrund` | Letzter Schalt- oder Sperrgrund. |
| `0_userdata.0.Heizung.Lufttrockner.AbschaltungenHeute` | Anzahl automatischer PV-Abschaltungen am aktuellen Tag. |
| `0_userdata.0.Heizung.Lufttrockner.TagessperreAktiv` | Aktiv nach 3 automatischen PV-Abschaltungen; Reset um Mitternacht. |
| `0_userdata.0.Heizung.Lufttrockner.TankMeldungAktiv` | Hinweis auf Tank voll oder ausgeschaltetes Gerät bei Leistungsabfall; Pushover wird pro Ereignis nur einmal gesendet und erst nach wieder erkannter Leistungsaufnahme zurückgesetzt. |
| `0_userdata.0.Heizung.Lufttrockner.HeizstabPauseAktiv` | Zeigt, dass eine Heizstab-Reserve angefordert wird. |

Verhalten:

- Einschalten erst bei ausreichend PV-Überschuss über 15 Minuten.
- Ausschalten erst bei zu wenig PV über 15 Minuten.
- Nach der 3. automatischen PV-Abschaltung wird bis Mitternacht gesperrt.
- Bei Tank-/Gerätemeldung bleibt die Meldesperre auch dann aktiv, wenn der Shelly im Auto-Modus später wegen PV-Mangel ausgeschaltet wird; zurückgesetzt wird sie erst bei wieder erkannter Leistungsaufnahme.
- Ab ca. 60 °C Puffertemperatur darf der Lufttrockner beim Hauptskript eine Leistungsreserve anfordern.
- Der Heizstab kann dann reduziert weiterlaufen, solange genug PV-Strom für beide Verbraucher verfügbar ist.

---

## 8. LED- und Ampelanzeigen

Die Ampel wird intern unter **`0_userdata.0.Heizstab.V2.Ampel.*`** geführt und zusätzlich auf GPIO-Ausgänge gespiegelt.

| Farbe | GPIO | Bedeutung |
| --- | --- | --- |
| Grün | `0_userdata.0.System.GPIO.GPIO22` | Standby/Bereit bei zu wenig Überschuss, wenn Standby-Anzeige aktiviert ist. |
| Gelb | `0_userdata.0.System.GPIO.GPIO23` | Heizstab aktiv im normalen PV-Betrieb. |
| Rot | `0_userdata.0.System.GPIO.GPIO24` | Fehler, Übertemperatur, FI/LS-Fehler, PWM-Fehler oder Leistungsabweichung. |
| Grün + Gelb | GPIO22 + GPIO23 | Warmwasser-Sicherstellung aktiv oder wartend. |
| Alle blinken | GPIO22 + GPIO23 + GPIO24 | Selbsttest aktiv. |
| Alle aus | - | Regelung manuell deaktiviert oder kein besonderer Anzeigezustand. |

Zusätzlich gibt es **`Regelung.QuittierTaster_Blink`**. Dieser Datenpunkt blinkt, wenn ein Fehler quittierbar ist.

---

## 9. Fehler und typische Meldungen

| Code | Bedeutung | Was passiert? | Was tun? |
| --- | --- | --- | --- |
| `RG001` | Regelung manuell deaktiviert | Heizstab aus. | `Regelung.ENABLE` einschalten, wenn Betrieb gewünscht ist. |
| `RG003` | Regelung aktiv | Heizstab nutzt PV-Überschuss. | Normalzustand. |
| `RG004` | Überschuss zu gering | Heizstab aus. | Warten auf mehr PV-Überschuss. |
| `TP002` | Maximaltemperatur erreicht | Heizstab aus bis zur Hysterese-Freigabe oder bis zur Zusatz-Freigabe bei Schichtung. | Abkühlen lassen; bei starker Schichtung kann der Heizstab automatisch wieder anlaufen. |
| `TP003` | Warmwasser-Sicherstellung aktiv/wartend | Heizstab heizt mit Sicherstellungsleistung oder wartet auf Einschaltpunkt. | Normal, wenn WW-Sicherstellung gewünscht ist. |
| `TP004` | Harte Übertemperatur | Sofort aus, rote LED, Fehler zunächst nicht quittierbar. | Ursache prüfen; erst nach Abkühlung quittieren. |
| `FI001` | FI/LS aus | Sofort aus, rote LED, Sperre. | FI/LS und Ursache prüfen; danach quittieren. |
| `ABW001` | Istleistung weicht zu stark vom Soll ab | Heizstab gesperrt. | Heizstab/PWM/Leistung prüfen; quittieren, wenn Ursache behoben ist. |
| `PWM400` / `PWMERR` | PWM-Gerät offline oder Fehler | PWM auf 0, rote LED. | PWM-Gerät/Verbindung prüfen. |
| `OFF010` | Gerät offline, Wartezeit läuft | Noch keine harte Abschaltung bis Wartezeit abgelaufen ist. | Verbindung prüfen. |
| `OFF001` | Gerät offline nach Wartezeit | Heizstab aus. | Gerät wieder online bringen. |
| `OFF000` | Alle Geräte wieder online | Regelung freigegeben. | Normalzustand nach Offline-Ereignis. |
| `OFF090` | Nachtmodus Online-Checks pausiert | Online-Prüfung zwischen 22:00 und 04:00 pausiert. | Normal, keine Aktion nötig. |
| `MODE001` | Kesselbetrieb | Heizstab deaktiviert. | Betriebsmodus ändern, wenn Heizstabbetrieb gewünscht ist. |

---

## 10. Quittieren und Reset

Fehler werden unterschiedlich behandelt:

- **Automatische Freigabe:** z. B. zu wenig Überschuss oder Maximaltemperatur mit Hysterese.
- **Quittierbare Fehler:** z. B. Leistungsabweichung `ABW001`, wenn kein nicht quittierbarer Fehler parallel aktiv ist.
- **Nicht quittierbare Fehler:** z. B. Übertemperatur oder FI/LS aus, solange die Ursache noch anliegt.

Vorgehen:

1. Meldung und Fehlercode in der Visualisierung lesen.
2. Ursache physikalisch prüfen.
3. Warten, bis der Fehlerstatus `QUITTIERBAR` ist.
4. Button **Quittieren** bzw. `Regelung.Fail_Reset` betätigen.
5. Prüfen, ob Status und LED wieder normal sind.

---

## 11. Selbsttest und Kalibrierung

### Selbsttest

Der Selbsttest prüft grundsätzlich, ob Sensoren plausibel sind und ob der Heizstab auf eine kleine Testleistung reagiert.

Ablauf für Anwender:

1. Sicherstellen, dass keine Störung anliegt.
2. `Selbsttest.Start` betätigen.
3. Während des Tests blinken alle LEDs.
4. Ergebnis im Status/Meldungslog prüfen.

Mögliche Ergebnisse:

- `ST003`: Selbsttest erfolgreich.
- `ST002`: Selbsttest fehlgeschlagen, z. B. Leistungsabweichung.
- `TP001`: Temperatursensor unplausibel.

### Kalibrierung

Die Kalibrierung vermisst die Leistungskurve des Heizstabs. Sie fährt mehrere Leistungsstufen an und speichert Messwerte.

Nur verwenden, wenn:

- die Anlage sicher beaufsichtigt wird,
- ausreichend Wärmeabnahme möglich ist,
- keine Störung aktiv ist,
- die elektrische Installation geprüft ist.

Während der Kalibrierung wird die normale Regelung pausiert.

---

## 12. Schnellcheck bei Problemen

| Beobachtung | Mögliche Ursache | Prüfen |
| --- | --- | --- |
| Heizstab bleibt aus | `ENABLE` aus, Kesselbetrieb, zu wenig Überschuss, Sperre aktiv | `Regelung.Status`, `Regelung.ENABLE`, Betriebsmodus, Meldungslog. |
| Rote LED | Fehler aktiv | Meldungstabelle und Fehlercode prüfen. |
| Grün an, Heizstab aus | Standby / zu wenig Überschuss | PV-Leistung, Netzbezug, Status `RG004`. |
| Grün + Gelb | Warmwasser-Sicherstellung | WW-Temperatur, Zieltemperatur und Delta-T prüfen. |
| Pumpe läuft nicht | Manuell aus, Shelly offline, Bypass, Temperaturbedingung nicht erfüllt | Pumpenstatus, Online-Status, ManualMode, grüner Punkt in Visualisierung. |
| Lufttrockner bleibt aus | Freigabe aus, zu wenig PV, Tagessperre aktiv, Shelly offline oder Tank-/Gerätemeldung | Lufttrockner-Status, letzter Schaltgrund, Freigabe und Abschaltungen heute prüfen. |
| Speicherladepumpe läuft, Heizkreispumpe aus | Warmwasser-Vorrang | Normal im Heizstabbetrieb. |
| Fehler lässt sich nicht quittieren | Ursache liegt noch an | Fehlerstatus und physikalische Ursache prüfen. |
