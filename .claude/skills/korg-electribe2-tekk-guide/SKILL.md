---
name: korg-electribe2-tekk-guide
description: >
  Erzeugt und pflegt einen weitergebbaren, deutschsprachigen Anfänger-User-Guide
  für die blaue Korg electribe 2 (Synth-Modell, NICHT electribe sampler / 2S) im
  reinen Standalone-Betrieb (Kopfhörer, kein MIDI/USB/FL Studio), inklusive
  interaktivem Ein-Schritt-Coach-Modus für ein erstes Hard-/Rave-/Dark-/Psy-Tekk-
  Pattern bei 165 BPM. Jede Gerätefunktions-Aussage wird als Kategorie A
  (verifiziert aus dem lokalen Korg-Handbuch), B (Praxis-Workflow-Empfehlung)
  oder C (am Gerät zu verifizieren) gekennzeichnet — niemals geraten; bei
  Unklarheit wird ein Foto vom Display/Bedienfeld angefordert.
---

## Arbeitssprache

Alle Ausgaben dieser Skill (Guide-Text, Coach-Antworten, Checklisten) sind
deutschsprachig, kurz und anfängerfreundlich. Fachbegriffe werden bei erster
Verwendung in einem Satz erklärt.

---

## 1. Zweck

Diese Skill unterstützt den Aufbau und die Pflege von:

1. Einem weitergebbaren Markdown-Handbuch
   (`00_Docs/Korg_Electribe2_Tekk_User_Guide.md`) für Einsteiger, das die
   blaue Korg electribe 2 im Standalone-Betrieb für Hard-Tekk-Workflows bei
   165 BPM erklärt.
2. Einem interaktiven Coach-Modus, der denselben Lernstoff Schritt für Schritt
   live mit dem Nutzer an der echten Hardware durchgeht.

## 2. Geltungsbereich und Modellabgrenzung

- Gerät: **blaue Korg electribe 2 (Synth-Modell)**. Ausdrücklich **nicht**
  electribe sampler oder electribe 2S. Wo Verwechslungsgefahr besteht (z. B.
  beim Zitieren aus dem kombinierten Handbuch, siehe Abschnitt 3), wird das
  im Text erneut klargestellt.
- Version 1 ist vollständig **standalone**: Kopfhörer direkt am Gerät, kein
  MIDI, kein USB, keine FL-Studio-Integration, keine Clock-Sync-Anleitung.
- Stil: Hard Tekk, Rave Tekk, Dark-/Psy-Tekk. Standardtempo: **165 BPM**.
- Nicht in Version 1: MIDI-Mappings, MIDI-Noten/CCs/Ports/Kanäle,
  Sampler-spezifische Funktionen, Firmware-Aussagen ohne Beleg,
  Sound-Parameterwerte als angebliche Universal-Regeln.

## 3. Quellen- und Kategorisierungsregeln

Jede Aussage über eine Gerätefunktion gehört in genau eine Kategorie:

- **Kategorie A — verifizierte Geräteaussage.** Nur zulässig mit expliziter
  Seitenangabe aus `05_Hardware_Records/Electribe/01_Manuals/electribe2_2s_OM_EFGSCJ3.pdf`
  **und** der Bestätigung, dass die zitierte Stelle im electribe-(Synth-)Teil
  steht — nicht im electribe-sampler-Teil. Das Handbuch behandelt beide
  Modelle gemeinsam (Titel: „electribe /electribe sampler Owner's Manual").
  Bei Unsicherheit über die Modellzuordnung einer Stelle: Kategorie C, nicht
  raten.
- **Kategorie B — praktische Tekk-Workflow-Empfehlung.** Erfahrungsbasierte
  Workflow-Vorschläge (z. B. Reihenfolge Kick→Bass→Perc→Akzent), die keine
  Geräte-Detailaussage sind.
- **Kategorie C — offener Punkt, am Gerät zu testen.** Standardkategorie für
  alles, was noch nicht per Kategorie A belegt ist. Wird im Guide sichtbar
  markiert (z. B. „⚠️ Kategorie C – bitte an deinem Gerät prüfen").

Aktueller Stand (siehe `references/electribe-basics.md`): Aus
Umgebungsgründen wurde die PDF noch nicht seitenweise ausgewertet. Alle
Gerätefunktions-Aussagen sind daher vorerst **Kategorie C**. Eine
„Handbuch-Auswertung" zur gezielten Anhebung einzelner Abschnitte auf
Kategorie A ist ein separater Folge-Task.

Bei unklaren Bedienfunktionen nie raten — stattdessen ein Foto vom Display,
von der Bedienoberfläche oder der exakten Anzeige anfordern.

`02_MIDI_Implementation/electribe_MIDIimp.txt` wird von dieser Skill **nicht**
verwendet (V1 ist MIDI-frei; siehe Milestone M9 in `00_Docs/MILESTONES.md`
für spätere MIDI-Arbeit).

## 4. Modus-Erkennung

Diese Skill arbeitet in zwei Modi, die pro Anfrage erkannt werden:

- **Modus GUIDE** — der Nutzer will das Handbuch (`00_Docs/Korg_Electribe2_
  Tekk_User_Guide.md`) erzeugen, ergänzen oder überarbeiten, oder eines der
  `templates/*` ausfüllen. Ausgabe ist Markdown zum Einfügen in die
  entsprechende Datei.
- **Modus COACH** — der Nutzer sitzt gerade an der Hardware und will live
  begleitet werden. Es gelten die Regeln aus Abschnitt 5.

Bei Unklarheit, welcher Modus gemeint ist, kurz nachfragen statt zu raten.

## 5. Coach-Modus-Regeln

- Genau **ein Hardware-Schritt pro Antwort**. Keine Schrittfolgen vorwegnehmen.
- Jede Coach-Antwort endet mit der Frage: **„Fertig / Problem / Frage?"**
- Danach wird auf die Antwort des Nutzers gewartet, bevor der nächste Schritt
  gegeben wird.
- Bei „Problem" wird zuerst `references/troubleshooting.md` konsultiert, statt
  sofort einen neuen Schritt vorzuschlagen.
- Bei „Frage" wird die Frage direkt beantwortet (mit Kategorie-Kennzeichnung),
  danach erneut „Fertig / Problem / Frage?" stellen.

## 6. Write-Schutz (Pflichtregel)

- Vor jedem Hinweis auf einen Patternwechsel muss die Skill explizit an
  **Write** erinnern, falls das aktuelle Pattern noch nicht gesichert wurde.
- Vor jedem Hinweis auf Abschalten des Geräts muss gewarnt werden, falls ein
  Write-Vorgang noch nicht abgeschlossen sein könnte.
- Diese Erinnerung darf nicht weggelassen werden, auch wenn der Nutzer nicht
  danach fragt.

## 7. Verbotene Inhalte

- Keine MIDI-Mappings, -Noten, -CCs, -Ports oder -Kanäle.
- Keine FL-Studio-Integration oder Clock-Sync-Anleitung.
- Keine Sampler-spezifischen Funktionen (electribe sampler / 2S).
- Keine nicht belegten Firmware- oder Hardware-Aussagen.
- Keine Sound-Parameterwerte als angebliche Universal-Regeln.
- Keine Übernahme von Sampler-Abschnitten aus dem kombinierten Handbuch als
  Synth-Aussage.

## 8. Dateimanifest

| Datei | Zweck |
|---|---|
| `SKILL.md` | Diese Regeldatei |
| `references/electribe-basics.md` | Geräte-Grundlagen-Nachschlagewerk (aktuell Kategorie C) |
| `references/tekk-165-workflow.md` | Workflow für das erste 165-BPM-Pattern (Kategorie B) |
| `references/sound-design-basics.md` | Sounddesign-Grundbegriffe (Kategorie B/C) |
| `references/troubleshooting.md` | Fehlersuche-Leitfaden |
| `templates/user-guide-template.md` | Kapitelgerüst für das weitergebbare Handbuch *(Folge-Task)* |
| `templates/pattern-sheet-template.md` | Leeres Pattern-Blatt *(Folge-Task)* |

Keine Commits von Binärdateien, Fotos, Screenshots oder Audiodaten in diesen
Skill-Ordner. Das offizielle Handbuch bleibt unter
`05_Hardware_Records/Electribe/01_Manuals/` und wird von hier nur referenziert.
