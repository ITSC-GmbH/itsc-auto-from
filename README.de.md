---
subject : "Browser-Erweiterungen"
title : "ITSC Auto-From (Thunderbird MailExtension)"
author : "ITSC IT Systems Consulting GmbH"
abstract: "Überträgt beim Beantworten von E-Mails automatisch die ursprüngliche Empfängeradresse (TO) als Absenderadresse (FROM), falls für das E-Mail-Konto keine exakt passende Identität existiert."
keywords: ["Thunderbird"]
lang: "de"
---

**ITSC Auto-From** ist eine Thunderbird-Erweiterung (WebExtension / MailExtension nach Manifest V3), die beim Beantworten von E-Mails automatisch die ursprüngliche Empfängeradresse (`TO`) als Absenderadresse (`FROM`) einträgt, falls für das E-Mail-Konto keine exakt passende Identität existiert.

# Problemstellung & Einsatzzweck

Zur Spam-Prävention werden häufig individuelle E-Mail-Adressen für unterschiedliche Partner/Dienste eingerichtet (z. B. `partner-dienst@meinedomain.de`). Alle diese E-Mails laufen in einem gemeinsamen IMAP-Konto zusammen.

- Wenn in Thunderbird für eine Adresse eine eigene Identität existiert, wählt Thunderbird diese beim Antworten automatisch aus.
- Existiert **keine** passende Identität, fiel Thunderbird bisher auf die Standard-Absenderadresse zurück und die korrekte Partner-Adresse musste manuell eingetragen werden.

**Lösung:** Dieses Add-on prüft beim Erstellen der Antwort-Mail, ob eine passende Identität existiert. Falls nicht, übernimmt es automatisch die `TO`-Adresse der Originalnachricht als Absender (`FROM`).

# Funktionen

1. **Automatische Absenderanpassung (FROM)**:
   - Erkennt Antworten auf eingegangene Nachrichten.
   - Gleicht die Empfängeradressen (`TO`) mit allen für das Konto konfigurierten Identitäten ab.
   - Findet Thunderbird keine passende Identität, wird die `TO`-Adresse automatisch als Absender (`FROM`) gesetzt.
   - Optional wird der konfigurierte Anzeigename der Standardidentität übernommen (z. B. `Max Mustermann <partner-alias@meinedomain.de>`).

2. **Visuelle Signalisierung**:
   - **Symbolleisten-Badge**: Im Verfassen-Fenster wird auf dem Add-on-Symbol eine grüne `AUTO`-Markierung angezeigt.
   - **Tooltip / Infoleiste**: Zeigt die zugewiesene Absenderadresse an.
   - **Status-Popup**: Klick auf das Symbol öffnet ein Detail-Popup mit ursprünglicher `TO`-Adresse, gesetzter `FROM`-Adresse und Kontoname.
   - **System-/Desktopbenachrichtigung**: Kann in den Einstellungen optional aktiviert werden.

3. **Konto-spezifische Steuerung**:
   - Die Funktionalität lässt sich in den Add-on-Einstellungen pro Thunderbird-Konto aktivieren oder deaktivieren.
   - Standardmäßig für alle Konten aktiv.

4. **Mehrsprachigkeit (i18n)**:
   - Vollständige Unterstützung für **Deutsch** (`de`) und **Englisch** (`en`).

# Konfiguration

Die Einstellungen des Add-ons ermöglichen es, die automatische Absenderzuordnung für jedes E-Mail-Konto individuell ein- oder auszuschalten sowie das Signalisierungsverhalten anzupassen.

## Aufrufen der Einstellungen in Thunderbird

Es gibt zwei Wege, um die Einstellungsseite zu öffnen:

1. **Über den Add-on-Manager (Standardweg)**:
   - Öffnen Sie in Thunderbird das Menü **Extras** (oder das Thunderbird-Anwendungsmenü `≡`) -> **Add-ons und Themes** (Tastenkombination: `Strg + Umschalt + A` bzw. `Cmd + Shift + A` unter macOS).
   - Wählen Sie in der linken Seitenleiste die Kategorie **Erweiterungen** aus.
   - Suchen Sie den Eintrag **ITSC Auto-From**.
   - Klicken Sie entweder direkt auf das **Schraubenschlüssel-Symbol** (Einstellungen) oder auf das **Drei-Punkte-Menü (`...`)** rechts neben dem Schalter und wählen Sie **Einstellungen**.
   - Die Einstellungsseite öffnet sich daraufhin als eigener Tab in Thunderbird.

2. **Über das Verfassen-Fenster (Schnellzugriff)**:
   - Klicken Sie in der Symbolleiste des E-Mail-Verfassen-Fensters auf das Add-on-Symbol (**ITSC Auto-From**).
   - Klicken Sie im geöffneten Status-Popup unten auf die Schaltfläche **Einstellungen öffnen**.

## Konfigurationsmöglichkeiten

Auf der Einstellungsseite stehen folgende Optionen zur Verfügung:

### Allgemeine Einstellungen
- **Für neue Konten standardmäßig aktivieren**:
  - Legt fest, ob neu hinzugefügte E-Mail-Konten standardmäßig für die automatische Absenderzuordnung aktiviert sein sollen.
- **Anzeigenamen der Identität beibehalten**:
  - Ist diese Option aktiv (Standard), wird der Realname der Standard-Identität übernommen (z. B. `Max Mustermann <partner@beispiel.de>`). Ist sie deaktiviert, wird ausschließlich die reine E-Mail-Adresse eingesetzt.
- **Badge im Verfassen-Fenster anzeigen**:
  - Zeigt eine grüne `AUTO`-Markierung auf dem Symbol in der Werkzeugleiste des Verfassen-Fensters an, sobald eine Absenderadresse automatisch angepasst wurde.
- **Desktop-/Systembenachrichtigung anzeigen**:
  - Sendet optional eine Systembenachrichtigung beim Öffnen des Verfassen-Fensters, die über die vorgenommene Absenderanpassung informiert.

### E-Mail-Konten (Konto-spezifische Aktivierung/Deaktivierung)
- **Kontenliste**: Alle in Thunderbird konfigurierten E-Mail-Konten (z. B. IMAP- oder POP3-Konten) werden mit ihrem Namen, Kontotyp und den hinterlegten Identitäten aufgelistet.
- **Konto-Schalter (Aktiv / Inaktiv)**:
  - **Aktiviert (Standard)**: Beim Antworten auf E-Mails dieses Kontos greift das Add-on ein, wenn keine passende Identität existiert, und setzt die ursprüngliche `TO`-Adresse automatisch als Absender (`FROM`).
  - **Inaktiv**: Für dieses Konto wird keine automatische Absenderersetzung durchgeführt; Thunderbird verwendet sein Standardverhalten.
- **Identitäten-Vorschau (aufklappbar)**: Zeigt die für jedes Konto bekannten Absenderidentitäten (`Name <adresse@domain.de>`) an, die vom Add-on beim Abgleich berücksichtigt werden.

### Automatische Speicherung
- Alle Änderungen an Schaltern und Optionen werden sofort und automatisch wirksam und im Speicher gesichert – eine manuelle Bestätigung über eine Speichern-Schaltfläche ist nicht erforderlich.

# Projektstruktur

```
thunderbird-auto-from/
├── manifest.json              # WebExtension-Manifest (Manifest V3)
├── package.json               # Projekt- und Test-Konfiguration
├── README.md                  # Dokumentation (Englisch)
├── README.de.md               # Dokumentation (Deutsch)
├── LICENSE                    # MIT-Lizenz
├── _locales/
│   ├── de/messages.json       # Deutsche Lokalisierung
│   └── en/messages.json       # Englische Lokalisierung
├── icons/                     # Erweiterungssymbole (SVG, 16, 32, 48, 64, 128 px)
├── src/
│   ├── background/
│   │   ├── auto-from-handler.js # Kernlogik für Abgleich & Adressanpassung
│   │   └── background.js        # WebExtension Hintergrund-Eventlistener
│   ├── options/               # Einstellungsseite (Konten & Optionen)
│   │   ├── options.html
│   │   ├── options.css
│   │   └── options.js
│   ├── popup/                 # Status-Popup im Verfassen-Fenster
│   │   ├── popup.html
│   │   ├── popup.css
│   │   └── popup.js
│   └── utils/
│       ├── email-utils.js     # E-Mail-Parser & Adress-Hilfsfunktionen
│       ├── storage-utils.js   # Konfigurations- & Speicherverwaltung
│       └── i18n-helper.js     # Lokalisierungshelfer
├── test/                      # Automatisierte Unit-Tests (Node.js Test-Runner)
│   ├── email-utils.test.js
│   ├── storage-utils.test.js
│   ├── auto-from-handler.test.js
│   ├── options.test.js
│   ├── background.test.js
│   └── build.test.js
└── scripts/
    └── build.js               # Build-Skript für .xpi- und Quellcode-ZIP-Archive
```

# Entwicklung & Tests

## Voraussetzungen
- Node.js (Version 18+)

## Tests ausführen
```bash
npm test
```

## Syntax- und Linter-Prüfung
```bash
npm run lint
```

## Release-Pakete erstellen
```bash
# Erstellt sowohl das installierbare XPI als auch das Quellcode-ZIP
npm run build

# Alternativ nur das XPI-Paket erstellen
npm run build:xpi

# Alternativ nur das Quellcode-ZIP-Archiv erstellen
npm run build:source
```
Die generierten Release-Artefakte befinden sich im Verzeichnis `dist/`:
- `dist/itsc-auto-from.xpi` (Installierbares Thunderbird-Add-on)
- `dist/itsc-auto-from-source.zip` (Vollständiger Quellcode zur Einreichung / Archivierung)

# Installation in Mozilla Thunderbird

1. Erstelle das Add-on-Paket via `npm run build:xpi` (oder `npm run build`).
2. Öffne Thunderbird.
3. Öffne das Menü **Extras** (bzw. Hamburger-Menü) -> **Add-ons und Themes**.
4. Klicke auf das Zahnrad-Symbol (oben rechts) und wähle **Add-on aus Datei installieren...**.
5. Wähle die Datei `dist/itsc-auto-from.xpi` aus und bestätige die Installation.
6. Zur temporären Entwicklung: Unter **Extras** -> **Entwickler-Werkzeuge** -> **Add-on-Entwickler-Werkzeuge** kann das Verzeichnis als temporäres Add-on geladen werden.

# Probleme und Änderungswünsche

- Bei Problemen oder Änderungswünschen erstellen Sie ein Ticket im Github-Projekt `thunderbird-auto-from`:
  [https://github.com/ITSC-GmbH/itsc-auto-from](https://github.com/ITSC-GmbH/itsc-auto-from)
- Prüfen Sie zuvor, ob bereits ein anderes Ticket zum selben Thema existiert.
- Bei Fehlern beschreiben Sie, wie das Problem nachvollzogen werden kann; Screenshots sind hilfreich.

# Verfügbarkeit des Quellcodes

Der Quellcode jedes Releases steht frei zur Verfügung; Sie können ihn auf der GitHub-Projektseite im Abschnitt ["Releases"](https://github.com/ITSC-GmbH/itsc-auto-from/releases) herunterladen.

# Lizenz

Dieses Projekt ist unter der **MIT-Lizenz** lizenziert – siehe die Datei [LICENSE](LICENSE) für Details.

Copyright (c) 2026 ITSC IT Systems Consulting GmbH
