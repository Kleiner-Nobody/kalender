# Kalender-Display

E-Ink-Wanddisplay (Waveshare 7.5" e-Paper HAT (B) + ESP32-WROOM-32E Driver Board Rev3),
das über Home Assistant die Termine aller Familienmitglieder sowie den Müllkalender anzeigt.

## Hardware

- Display: Waveshare 7.5inch e-Paper HAT (B), SKU 13505, 800x480, Rot/Schwarz/Weiß
- Treiberboard: Waveshare "Universal e-Paper ESP32 Driver Board" (ESP32-WROOM-32E), Rev3
- Home Assistant Green (mit Zusatzmodul) — bereits eingerichtet

### Pinbelegung (fest verdrahtet auf dem Treiberboard)

| Signal | ESP32 GPIO |
|---|---|
| DIN (MOSI) | GPIO14 |
| SCLK | GPIO13 |
| CS | GPIO15 |
| DC | GPIO27 |
| RST | GPIO26 |
| BUSY | GPIO25 |

Das Panel wird per FPC-Flachbandkabel direkt in den entsprechenden Anschluss auf dem
Treiberboard gesteckt (kein manuelles Verdrahten der Panel-Pins nötig). Beim Öffnen/Schließen
der Klemme des FPC-Steckers vorsichtig sein — siehe Waveshare-Anleitung zur Kabel-Orientierung.

## Stand des Projekts

Siehe Roadmap unten. Aktueller Schritt: Display zum Laufen bringen (Verkabelung + ESPHome-„Hello World").

## Setup — Display zum Laufen bringen

Panel-Version ist bestätigt **(V3)** → `model: 7.50in-bV3-bwr` (volles Rot/Schwarz/Weiß-Rendering)
ist in `esphome/kalender-display.yaml` bereits eingetragen.

### Variante A: Ohne Home Assistant (aktuell kein Zugriff auf HA)

Solange kein Zugriff auf Home Assistant besteht, per ESPHome-Kommandozeile direkt vom PC aus
flashen — komplett unabhängig von HA. Die Zeilen `api:` und `ota:` in der YAML stören dabei nicht,
sie warten nur untätig auf eine spätere HA-Verbindung.

1. `esphome/secrets.yaml.example` nach `esphome/secrets.yaml` kopieren und WLAN-Zugangsdaten eintragen.
2. Python installieren (python.org, beim Setup "Add Python to PATH" anhaken), dann in der
   Kommandozeile:
   ```
   pip3 install wheel
   pip3 install esphome
   esphome version
   ```
3. ESP32-Board per USB an den PC anschließen. Falls in Windows kein neuer COM-Port auftaucht,
   fehlt evtl. der USB-Treiber für den Serienchip (CH343/CH34x) — Treiber von wch.cn (WCH) installieren.
4. Im Ordner `esphome/` ausführen:
   ```
   esphome run kalender-display.yaml
   ```
   Beim ersten Mal nach dem seriellen Port fragen lassen (USB) und bestätigen. Das kompiliert
   und flasht die Firmware direkt.
5. Prüfen, ob "Kalender-Display" / "Verbindung steht" (+ rotes Testrechteck) auf dem Panel erscheint.

Sobald Home Assistant wieder erreichbar ist, taucht das Gerät dort automatisch unter
**Einstellungen → Geräte & Dienste** als neu gefundenes ESPHome-Gerät auf (dank `api:`) und kann
adoptiert werden — kein erneutes Flashen nötig.

### Variante B: Über das Home Assistant ESPHome-Add-on (sobald HA wieder erreichbar ist)

1. ESPHome-Add-on in Home Assistant installieren (Einstellungen → Add-ons → Add-on Store → "ESPHome").
2. Im ESPHome-Add-on die Datei `kalender-display.yaml` importieren.
3. ESP32-Board per USB an den Rechner anschließen, auf dem der Browser läuft, und über den
   ESPHome-Web-Installer (WebSerial) flashen. Danach sind Updates auch drahtlos (OTA) möglich.

## Roadmap

1. [ ] Display zum Laufen bringen (dieser Schritt)
2. [ ] Familienkalender (Google Calendar / CalDAV) in Home Assistant einbinden
3. [ ] Müllkalender-Quelle finden und in HA einbinden
4. [ ] Layout des Displays gestalten (Agenda, Farbcodierung pro Person, Müll-Icons)
5. [ ] ESPHome-Konfiguration um echte Kalenderdaten aus HA erweitern
