# suno-dj-arrangement-polisher

Projektlokaler Claude-Code-Skill für SiCKaRiM. Er baut und prüft
Suno-Prompts für Hard Tekk / RaveTekk / Dark Psy-Tekk, entscheidet über
Suno-Extends, ordnet HPSS-Analysen ein und gibt Export- und
Credit-Empfehlungen.

Die vollständigen Regeln stehen in [`SKILL.md`](SKILL.md). Diese README
erklärt, wofür der Skill gedacht ist, was rein- und rausgeht und wo seine
Grenzen liegen.

---

## Zweck

Vier Dinge, bewusst getrennt gehalten:

| Aufgabe | Was der Skill tut |
|---|---|
| **Prompts polieren** | Baut Style-Prompt, Section-Cues und Exclude-Liste nach festem Format; entfernt Widersprüche aus vorhandenen Prompts |
| **Extend-Gate** | Prüft fünf Bedingungen und gibt ein Extend erst frei, wenn alle erfüllt sind |
| **HPSS-Status** | Ordnet Analyse-Reports als Kandidaten ein und verhindert, dass Maschinenwerte als geprüft durchgehen |
| **Export & Credits** | Sagt, welche Export-Option und welcher Split für den jeweiligen Zweck sinnvoll ist |

Der Leitgedanke: **FL Studio Desktop und Mixxx sind die einzige gültige
Audio-Referenz.** Alles, was der Skill über einen Track sagt, ist ein Ziel
oder ein Kandidat, bis ein Mensch es am Grid geprüft hat.

---

## Eingabe-Format

Der Skill braucht nicht alles auf einmal. Was fehlt, wird erfragt oder
bleibt `NOT YET VERIFIED` — geraten wird nichts.

### Für einen neuen Song

```
Track-Titel:   Kein Schlaf
BPM:           160            (ohne Angabe: Projekt-Default 165)
Hook:          <genau eine Textzeile oder ein Zweizeiler, optional>
Besonderheit:  <optional, z. B. "härteres Drop 2">
```

### Für einen Suno-Sound

```
Typ:   Loop | One-Shot
Rolle: PERC | STAB | LEAD | BASS | FX | VOX
BPM:   160 | Auto
Key:   F minor | Any        (bei tonalem Material erforderlich)
Phrase: "KEIN SCHLAF!"      (nur bei Vocal-Shouts)
```

### Für ein Extend

```
TRACK_TITLE, TRACK_VERSION, SOURCE_WAV_FILENAME
TARGET_BPM, ACTUAL_BPM
FL_GRID_VERIFIED: Yes|No
MIXXX_BEATGRID_VERIFIED: Yes|No
C1–C7 je mit BAR:BEAT und MM:SS.mmm
Extend-Ziel: DJ_OUTRO | DROP_2 | BREAK | REPLACEMENT_SECTION
Referenz-Cue, Style-String des Quelltracks, aktuelle Länge
```

Vorlage: [`templates/extend-validation-input.yaml`](templates/extend-validation-input.yaml)

### Für eine HPSS-Prüfung

Pfad zu einem JSON-Report aus `ThrivenArtConnect/hard-tekk-hpss-pipeline`.
Schema: [`templates/hpss-analysis-result.example.json`](templates/hpss-analysis-result.example.json)

---

## Ausgabe-Format

Feste Blöcke je nach Modus, damit die Antworten vergleichbar bleiben:

| Modus | Blöcke |
|---|---|
| Song | `TRACK_STATUS` · `STYLE_PROMPT` · `LYRICS_AND_SECTION_CUES` · `EXCLUDE_STYLES` · `FL_AND_MIXXX_VALIDATION_SHEET` |
| Sound | `SOUND_SETTINGS` · `SOUND_PROMPT` · `LIBRARY_METADATA` · `FL_STUDIO_QC` |
| HPSS | `TRACK_STATUS` · `HPSS_ANALYSIS_SUMMARY` · `HPSS_CANDIDATE_REVIEW_TABLE` · `REQUIRED_FL_STUDIO_CHECK` · `REQUIRED_MIXXX_CHECK` · `EXTEND_ELIGIBILITY` |
| Extend blockiert | `TRACK_STATUS` · `EXTEND_CONDITION_CHECK` · `EXTEND_BLOCKED` · `MISSING_EXTEND_EVIDENCE` · `REQUIRED_FL_STUDIO_CHECK` · `REQUIRED_MIXXX_CHECK` |
| Extend erlaubt | `TRACK_STATUS` · `EXTEND_CONDITION_CHECK` · `CONFIRMED_CONTEXT` · `SUNO_EXTEND_STYLE` · `SUNO_EXTEND_LYRICS_OR_CUES` · `SUNO_UI_INSTRUCTIONS` · `POST_EXTEND_QC` · `UPDATED_TRACK_VALIDATION_TEMPLATE` |

Workflow-Text und QC-Listen auf Deutsch, Suno-Prompts auf Englisch.

---

## Before / After

### Beispiel 1 — Prompt mit Emotions- und Garantiesprache

**Vorher**

```text
Epic dark hard tekk banger, exactly 32 bars intro guaranteed, sounds like
[Künstlername], emotional and powerful, radio ready, with adlibs and vocal
chops throughout
```

**Nachher**

```text
### STYLE_PROMPT
Bouncy German hard tekk, rave tekk and dark psy-tekk, 165 BPM target,
straight 4/4 target. Punchy distorted tekk kick, controlled heavy sub,
rolling rumble bass, tight rave percussion, industrial rave stabs.
Loud club-ready mix, kick and sub locked and never masked. DJ-friendly
arrangement target: aim for an approximately 32-bar instrumental intro,
build, drop, break, second drop, long instrumental DJ outro.
```

Was passiert ist: „exactly 32 bars guaranteed" → Arrangement-Ziel, weil Suno
Musik rendert und kein Taktraster. Emotion-Words und der Künstlername raus,
Klangmerkmale rein. Adlibs und Vocal-Chops raus aus dem Song — sie gehören
als separate One-Shots in die Library.

### Beispiel 2 — Extend blockiert

**Eingabe:** Status `FL_GRID_VERIFIED`, Mixxx noch nicht geprüft, Style-String
neu formuliert, Länge unbekannt.

**Ausgabe (gekürzt)**

```
### EXTEND_CONDITION_CHECK
| A | structure_confidence >= 0.85 oder menschliche Bestätigung | NOT PROVIDED | kein HPSS-Report geliefert |
| B | Style-String 1:1 übernommen                              | NOT MET      | Style wurde umformuliert |
| C | Vocal anchor wiederholt                                  | NOT APPLICABLE | instrumentales DJ-Outro |
| D | Länge < 4:00 min oder [Outro] fehlt                      | NOT PROVIDED | Länge nicht angegeben |
| E | FL Studio und Mixxx vom Menschen bestätigt               | NOT MET      | MIXXX_BEATGRID_VERIFIED: No |

### EXTEND_BLOCKED
EXTEND BLOCKED — vier von fünf Bedingungen nicht erfüllt.
```

Kein Timestamp, kein Downbeat, keine Extend-Payload. Stattdessen steht
konkret da, welche Angabe fehlt und woher sie kommt.

### Beispiel 3 — Split-Entscheidung

**Frage:** „Ich brauche die Vocals separat für einen Shout."

**Antwort:** Advanced Split für die Lead-Vocal, 10 Credits — nicht Auto Split
für 50. Ein einzelnes Instrument rechtfertigt keinen vollen Satz, und weniger
Stems heißt auch weniger HPSS-Prüfaufwand. Danach `HPSS_STATUS: CANDIDATE`,
bis FL Studio und Mixxx bestätigt sind.

---

## Grenzen

**Was der Skill nicht kann:**

- Kein Audio hören, analysieren oder messen. Er implementiert kein HPSS,
  keine BPM-Erkennung und keine Downbeat-Schätzung.
- Keine Taktlänge, keine Songzeit und keine Cue-Position zusichern. Was Suno
  aus einem Prompt macht, steht erst am Render fest.
- Keinen Status selbst hochsetzen. Jede Statusanhebung braucht neue, vom
  Nutzer gelieferte Evidenz.
- Nicht garantieren, dass ein Loop nahtlos oder ein Vocal-Shout musikfrei
  wird. Der Prompt lenkt, das Ergebnis wird geprüft.

**Wo ein menschlicher Check Pflicht ist:**

| Schritt | Ohne Bestätigung |
|---|---|
| Erster Kick-Downbeat auf Takt 1 Beat 1 | kein `FL_GRID_VERIFIED` |
| Mixxx-Beatgrid und C1–C7 auf hörbaren Downbeats | kein `MIXXX_READY` |
| Stems in FL Studio und Mixxx geprüft | `HPSS_STATUS` bleibt `CANDIDATE` |
| Alle fünf Extend-Bedingungen | kein Extend |
| Neue Whole-Song-Datei nach einem Extend | kein `EXTEND_MIXXX_READY` |

**Was mit Verifikationsstatus versehen ist** — im Skill dokumentiert, aber
nicht als Suno-Zusage belegt: die High-Fidelity-Metatags (4a.5), die
Export-Tabelle inklusive MIDI-Download und 32-bit (4b.1) und die
Credit-Preise (4c.1). Alle drei vor dem ersten Einsatz einmal an der
Suno-Oberfläche gegenprüfen.

---

## Dateien

| Pfad | Inhalt |
|---|---|
| `SKILL.md` | Vollständige Regeln |
| `templates/track-validation.md` | Validierungsblatt pro Song |
| `templates/suno-sounds-library-entry.md` | Library-Eintrag pro Sample |
| `templates/extend-validation-input.yaml` | Eingabeformat für Extend |
| `templates/hpss-analysis-result.example.json` | HPSS-Report-Schema |
| `examples/` | Song-, Sounds-, Extend- und HPSS-Beispiele |
| `tests/acceptance-cases.md` | Zehn Abnahmefälle |
