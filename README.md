# 🔥 Power-to-Heat

> **PV-Überschuss intelligent in Wärme umwandeln** – mit klaren Betriebsmodi, Sicherheitslogik und modularen ioBroker-Skripten.

---

## 🎯 Ziel des Projekts

Dieses Projekt steuert einen **Heizstab (MY-PV AC ELWA 2)** so, dass primär **überschüssiger PV-Strom** in einen Pufferspeicher geladen wird. Dadurch wird der Heizkessel – vor allem in der warmen Jahreszeit – entlastet oder ganz vermieden.

Kernidee:
- PV-Überschuss erkennen
- Heizleistung passend regeln
- Warmwasser-Verfügbarkeit absichern
- Pumpen und Peripherie koordiniert schalten
- Sicherheits- und Fehlerfälle robust behandeln

So entsteht eine praxistaugliche **Power-to-Heat Automatisierung** auf Basis von ioBroker.

---

## 🧩 Projekt-Komponenten

Die Logik ist in mehrere Skripte aufgeteilt, damit jede Funktion klar abgegrenzt bleibt.

### 1) `JavaSkript` – Hauptmodul Heizstab
**Aufgabe:** Zentrale Regelung des Heizstabs inkl. Sicherheits-, Diagnose- und Statuslogik.

Enthält u. a.:
- Hauptregelung mit Intervallen und Hysterese
- Leistungsgrenzen / Temperaturgrenzen
- WW-Sicherstellung
- Rampensteuerung (weiches Hoch-/Runterfahren)
- Kalibrierung und Selbsttest
- Fehlercodes / Statusmeldungen
- Ampel-/LED-Statusanzeige
- Online-/Offline-Überwachung relevanter Geräte

### 2) `Speicherladepumpe`
**Aufgabe:** Steuerung der Speicherladepumpe inkl. Soll/Ist-Überwachung.

Enthält u. a.:
- Pumpensteuerung über GPIO
- Debounce-Logik gegen Prellen/Flattern
- Übertemperatur-Hysterese
- Bypass-/Überbrückungslogik
- Shelly-Onlineüberwachung inkl. Fehler-DP

### 3) `Heizkreispumpe`
**Aufgabe:** Ansteuerung einer Heizkreispumpe über Shelly + Stromstoßrelais.

Enthält u. a.:
- Impulssteuerung (kurzer Toggle-Puls)
- Modusabhängige Ein/Aus-Entscheidung
- Manueller Wunsch-Datenpunkt
- Rückmeldung über Pumpenstatus
- Mindestabstände zwischen Impulsen zum Schutz vor Loops

### 4) `Warm-water-circulation-pump-control`
**Aufgabe:** Zeit- oder manuelle Steuerung der Warmwasser-Zirkulationspumpe.

Enthält u. a.:
- Start per Datenpunkt / Button
- Laufzeitvorgabe mit Countdown
- Manueller Modus (`auto` / `on` / `off`)
- Offline-Handling des Shelly
- Status- und Restlaufzeitdatenpunkte

### 5) `List_of_Status_Codes`
**Aufgabe:** Referenzliste der Status- und Fehlercodes zur schnellen Diagnose.

---

## 🏗️ Wie der Code umgesetzt ist

### Architekturprinzipien
- **Modular statt monolithisch:** Jede Anlage-Komponente hat ein eigenes Skript.
- **Konfiguration am Kopf:** Zeiten, Grenzwerte, Datenpunkte und Modi stehen gesammelt oben im Skript.
- **Datenpunkt-zentriertes Design:** Zustände/Kommandos laufen über klar benannte ioBroker-DPs.
- **Sicherheitsorientiert:** Offline-Erkennung, Übertemperatur, Soll/Ist-Prüfungen und Fehler-Latches sind eingebaut.
- **Praxistaugliche Robustheit:** Debounce, Persistenzzeiten und Hysterese verhindern instabiles Schaltverhalten.

### Typischer Ablauf (vereinfacht)
1. **Messwerte einlesen** (PV, Netz, Temperaturen, Online-Status, Rückmeldungen).
2. **Betriebsmodus & Freigaben prüfen** (z. B. Heizstabbetrieb, Kesselbetrieb, Unterstützung).
3. **Sollwerte berechnen** (Leistung/Pumpenzustände je nach Logik).
4. **Ausgänge setzen** (PWM, GPIO, Shelly-States).
5. **Rückmeldungen prüfen** (Soll/Ist, Offline, Temperaturgrenzen).
6. **Status/Fehler publizieren** (Codes, Zustände, Diagnose-DPs).

### Datenpunkt-Strategie
- Bedien- und Parametrierpunkte sind schreibbar.
- Reine Ausgabepunkte (Status, Ist-Werte, Fehlermeldungen, Restlaufzeit) sind als **read-only** ausgelegt.
- Dadurch bleibt die Trennung zwischen **Steuerung** und **Anzeige/Monitoring** sauber.

---


## 🗺️ Blockdiagramm (Datenfluss)

```mermaid
flowchart LR
    subgraph SENS[Sensorik / Quellen]
        PV[PV / Netzwerte
(Überschuss, Netzbezug)]
        TEMP[Temperaturen
(intern/extern/Speicher)]
        ONLINE[Online-Status
(Modbus, MQTT, Shelly)]
        IST[Ist-Rückmeldungen
(Pumpe, Leistung, FI/LS)]
    end

    subgraph CTRL[ioBroker Logik]
        MAIN[JavaSkript
Hauptregelung Heizstab]
        SP[Speicherladepumpe]
        HK[Heizkreispumpe]
        WW[Warm-water-circulation-pump-control]
        CODES[List_of_Status_Codes]
    end

    subgraph ACT[Aktoren]
        ELWA[Heizstab
MY-PV AC ELWA 2]
        P1[Speicherladepumpe]
        P2[Heizkreispumpe]
        P3[WW-Zirkulationspumpe]
        LED[Status-LEDs / Ampel]
    end

    subgraph DP[ioBroker Datenpunkte]
        INDP[Schreibbare Steuer-/Parameter-DPs]
        OUTDP[Read-only Ausgabe-DPs
(Status, Ist, Fehler)]
    end

    PV --> MAIN
    TEMP --> MAIN
    ONLINE --> MAIN
    IST --> MAIN

    MAIN --> ELWA
    MAIN --> LED
    MAIN --> SP
    MAIN --> HK
    MAIN --> CODES

    SP --> P1
    HK --> P2
    WW --> P3

    INDP --> MAIN
    INDP --> SP
    INDP --> HK
    INDP --> WW

    MAIN --> OUTDP
    SP --> OUTDP
    HK --> OUTDP
    WW --> OUTDP
    CODES --> OUTDP
```

## ✅ Aktueller Nutzen in der Praxis

Mit diesem Setup kann der Heizstab Überschussenergie sinnvoll thermisch speichern, ohne die Betriebssicherheit aus dem Blick zu verlieren. Gleichzeitig bleiben Pumpen, Betriebsmodi und Warmwasseranforderungen transparent und steuerbar.

---

## ❓Offene Punkte / Ergänzungen

Wenn du willst, ergänze ich als Nächstes noch:
- eine **Installations-/Inbetriebnahme-Anleitung** (Schritt für Schritt)
- eine **DP-Referenz** mit allen relevanten Datenpunkten und ihrer Bedeutung
- eine **Troubleshooting-Sektion** mit typischen Fehlerbildern
