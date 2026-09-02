# CLAUDE.md

Projektweite Regeln für SiCKaRiM × einfachfyn – FL Studio Preset Lab.

## Perplexity Handover Protocol

Nach jeder abgeschlossenen relevanten Aufgabe muss Claude Code als letzten
Antwortblock einen kompakten Übergabeblock für Perplexity ausgeben.

Gilt insbesondere nach:

- Analyse oder Recherche mit Ergebnis
- Testlauf
- Dateierstellung oder Dateibearbeitung
- Commit
- Push
- Pull-Request-Vorbereitung
- Pull Request
- Bugfix
- Deployment
- HPSS-/Audioanalyse
- FL-Studio-/Mixxx-Validierung
- Suno-Arrangement- oder Extend-Entscheidung

Keine vollständigen Dateidumps.
Keine Secrets, Tokens, API-Keys, Passwörter oder Zugangsdaten.
Keine erfundenen Werte.
Keine privaten Audioinhalte oder vollständigen WAV-Daten in den Handover schreiben.

Verwende exakt dieses Format:

```text
PERPLEXITY_HANDOVER_V1

PROJECT:
REPOSITORY:
BRANCH:
UPSTREAM:
BASE_BRANCH:
BASE_COMMIT:
DIFF_SCOPE:
SESSION_URL:

TASK:
STATUS: PASS | FAIL | BLOCKED | REVIEW_REQUIRED | NOT_RUN

COMMITS:
- <SHORT_SHA> <COMMIT_MESSAGE>
- NONE, falls kein Commit erstellt wurde

WORKING_TREE:
CLEAN | MODIFIED | UNTRACKED | UNKNOWN

CHANGED_PATHS:
- <PATH>
- NONE, falls keine Datei geändert wurde

TESTS:
- <TEST_COMMAND_OR_SUITE>: <PASS | FAIL | NOT_RUN>
- Assertions: <PASSED>/<TOTAL> oder NOT_RUN
- YAML: <PASS | FAIL | NOT_RUN | NOT_APPLICABLE>
- JSON: <PASS | FAIL | NOT_RUN | NOT_APPLICABLE>
- git diff --check: <PASS | FAIL | NOT_RUN>

KEY_DECISIONS:
- <Tatsächlich getroffene Entscheidung>
- <Tatsächlich getroffene Entscheidung>
- NONE, falls keine Entscheidung getroffen wurde

RISKS_OR_OPEN_ITEMS:
- <Konkretes Risiko oder fehlende Evidenz>
- NONE, falls keine offenen Punkte existieren

REQUEST_TO_PERPLEXITY:
<Eine konkrete Frage oder Review-Anforderung für Perplexity.>

NEXT_ALLOWED_ACTION:
<Genau eine nächste erlaubte Aktion.>

WRITE_APPROVAL_REQUIRED:
YES | NO

APPROVAL_SCOPE:
<Exakte Aktion, falls YES; sonst NONE>
```

Feld-Definitionen:

- `BASE_BRANCH`:
  Zielbranch für Vergleich, Review oder späteren Pull Request.
  Wenn noch nicht bestimmt: `UNKNOWN`.

- `BASE_COMMIT`:
  Gemeinsamer Ausgangscommit oder tatsächlich ermittelter Vergleichsstand
  gegenüber `BASE_BRANCH`.
  Wenn nicht tatsächlich ermittelt:
  `UNKNOWN — not compared against BASE_BRANCH`.

- `DIFF_SCOPE`:
  Kurze, überprüfbare Eingrenzung aller Änderungen im aktuellen Task.
  Beispiel: `CLAUDE.md only`
  oder: `.claude/skills/suno-dj-arrangement-polisher/ only`

Verbindliche Regeln:

1. Der Handover-Block steht immer als letzter Inhalt der Antwort.
2. Der Block darf keine Annahmen enthalten.
3. Bei fehlender Evidenz muss `NOT_RUN`, `UNKNOWN`, `NONE` oder
   `NOT YET VERIFIED` verwendet werden.
4. `STATUS: PASS` ist nur erlaubt, wenn alle für die Aufgabe relevanten
   Tests bestanden haben und kein blockierender offener Punkt vorliegt.
5. `CHANGED_PATHS` enthält nur tatsächlich geänderte oder committe Dateien.
6. `TESTS` enthält nur tatsächlich ausgeführte Prüfungen.
7. `RISKS_OR_OPEN_ITEMS` muss konkret sein; wenn nichts offen ist,
   schreibe genau `- NONE`.
8. Bei schreibenden Aktionen wie Commit, Push, Pull Request, Merge,
   Delete, Deployment, Environment-Änderung oder Secret-Änderung:
   - `WRITE_APPROVAL_REQUIRED: YES`
   - die exakte geplante Aktion unter `APPROVAL_SCOPE` nennen
   - auf ausdrückliche Nutzerfreigabe warten.
9. Bei reinen Lese-, Analyse- oder Testschritten:
   - `WRITE_APPROVAL_REQUIRED: NO`
   - `APPROVAL_SCOPE: NONE`
10. Bei Audio-/HPSS-Aufgaben dürfen Analysewerte nur als Kandidaten
    bezeichnet werden, solange keine menschliche FL-Studio- und
    Mixxx-Bestätigung vorliegt.
11. Bei Suno Extend darf niemals `MIXXX_READY` behauptet werden,
    wenn keine bestätigten FL-Grid- und Mixxx-Beatgrid-Daten vorliegen.
12. Der Handover ersetzt keine detaillierten Projektdateien,
    sondern ist nur die kompakte Übergabe zwischen Claude Code und Perplexity.

## TOKEN-SPAR-MODUS

Zur Kontextoptimierung bei längeren Sessions kann TOKEN-SPAR-MODUS aktiviert werden.
In diesem Modus gelten folgende zusätzliche Richtlinien:

- Priorisiere Bash-Befehle statt umfassender UI-Reports
- Fasse Ausgaben auf essenzielle Informationen beschränken
- Nutze git-native Werkzeuge und vermeiden Redundanzen in der Ausgabegenerierung
- Antworte prägnant: max. 8 Bullets pro Zusammenfassung, keine ausschweifenden Erklärungen
- Dokumentiere nur nachweislich neue oder geänderte Dateien
- Handhabe Handover-Blocks mit derselben Sorgfalt wie im Standard-Modus

TOKEN-SPAR-MODUS ändert nichts an den Verbindlichen Regeln (1–12) oben.
Sicherheit, Korrektheit und Nachvollziehbarkeit bleiben oberste Priorität.
