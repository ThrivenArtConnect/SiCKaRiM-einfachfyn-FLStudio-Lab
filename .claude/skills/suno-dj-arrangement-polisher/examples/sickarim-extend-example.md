# Beispiele — Suno Extend (SiCKaRiM)

Zwei Fälle: einmal gesperrt, einmal freigegeben. Beide zeigen exakt die
Blockstruktur, die der Skill ausgibt.

Alle Werte in Spitzklammern sind Platzhalter. In einer echten Antwort
stehen dort ausschließlich Werte, die der Nutzer bestätigt geliefert hat
oder die in einer bestätigten Track-Validation-Datei dokumentiert sind.

---

# A. BLOCKIERTES EXTEND

Ausgangslage: Der Track wurde in FL Studio am Grid ausgerichtet, es gibt
aber noch keine Mixxx-Prüfung und keine bestätigten Cue-Zeiten. Damit ist
`MIXXX_READY` nicht erreicht — kein Extend, kein geschätzter Timestamp.

### TRACK_STATUS

```
TRACK_TITLE: <TRACK_TITLE>
TRACK_VERSION: <TRACK_VERSION>
SOURCE_WAV_FILENAME: <CONFIRMED_SOURCE_WAV>
TARGET_BPM: 160
ACTUAL_BPM: NOT YET VERIFIED
STATUS: FL_GRID_VERIFIED
FL_GRID_VERIFIED: Yes
MIXXX_BEATGRID_VERIFIED: No
EXTEND_ALLOWED: No
```

### EXTEND_BLOCKED

```
EXTEND BLOCKED — required status is MIXXX_READY or EXTEND_MIXXX_READY.
Current status is FL_GRID_VERIFIED.
```

Es wird kein Extend-Prompt, kein Timestamp, kein Downbeat und keine
16-/32-/64-Bar-Stelle genannt. Diese Werte existieren erst nach der
Mixxx-Prüfung.

### MISSING_EXTEND_EVIDENCE

| Pflichtwert | Status | Quelle |
|---|---|---|
| TRACK_TITLE | NOT YET VERIFIED | Nutzer |
| TRACK_VERSION | NOT YET VERIFIED | Nutzer |
| SOURCE_WAV_FILENAME | NOT YET VERIFIED | Nutzer |
| TRACK_STATUS = MIXXX_READY | NOT REACHED | Mixxx-QC |
| TARGET_BPM | Vorhanden (160) | Nutzer |
| ACTUAL_BPM | NOT YET VERIFIED | FL Studio |
| FL_GRID_VERIFIED | Yes | FL Studio |
| MIXXX_BEATGRID_VERIFIED | No | Mixxx |
| C1 INTRO_START BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C2 BUILD_1 BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C3 DROP_1 BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C4 HOOK_START BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C5 BREAK_START BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C6 DROP_2 BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| C7 OUTRO_START BAR:BEAT + MM:SS.mmm | NOT YET VERIFIED | FL Studio / Mixxx |
| Extend-Ziel (DJ_OUTRO / DROP_2 / BREAK / REPLACEMENT_SECTION) | NOT YET SELECTED | Nutzer |
| Referenz-Cue für Extend-Start | NOT YET SELECTED | Nutzer |

### REQUIRED_FL_STUDIO_CHECK

1. Finale WAV in ein neues FL-Studio-Desktop-Projekt importieren.
2. Projekt-Tempo auf 160 BPM setzen.
3. Ersten echten Kick-Downbeat suchen und den Clip auf Takt 1 Beat 1 legen.
4. Mit Grid/Metronom Intro, Drop 1, Break, Drop 2 und Outro prüfen.
5. Bei konstanter Abweichung das reale Tempo ermitteln und als
   `ACTUAL_BPM` notieren.
6. Playlist-Marker C1 bis C7 auf reale Positionen setzen.
7. Für jeden Marker `BAR:BEAT` und `MM:SS.mmm` notieren.

### REQUIRED_MIXXX_CHECK

1. Nur die geprüfte finale WAV importieren und Analyse abwarten.
2. Beatgrid am ersten sicheren Kick-Downbeat prüfen und korrigieren.
3. Weit auseinanderliegende Stellen gegenprüfen: Intro, Drop 1, Break,
   Drop 2, Outro.
4. C1 bis C7 auf reale hörbare Downbeats setzen.
5. C4 HOOK_START auf die erste hörbare Hook-Silbe setzen.
6. Bei BPM-Drift oder unstetigem Grid: kein `MIXXX_READY`.
7. Erst mit vollständigen Werten `MIXXX_READY` melden — dann ist Extend offen.

---

# B. ERLAUBTES EXTEND — DJ_OUTRO

Ausgangslage: FL-Grid und Mixxx-Beatgrid sind geprüft, C1 bis C7 sind mit
realen Werten dokumentiert, Ziel ist ein instrumentales, DJ-mixbares Outro
ab C7 OUTRO_START.

### TRACK_STATUS

```
TRACK_TITLE: <TRACK_TITLE>
TRACK_VERSION: <TRACK_VERSION>
SOURCE_WAV_FILENAME: <CONFIRMED_SOURCE_WAV>
TARGET_BPM: 160
ACTUAL_BPM: <CONFIRMED_ACTUAL_BPM>
STATUS: MIXXX_READY
FL_GRID_VERIFIED: Yes
MIXXX_BEATGRID_VERIFIED: Yes
EXTEND_ALLOWED: Yes
EXTEND_TARGET: DJ_OUTRO
```

### CONFIRMED_CONTEXT

```
Trackname: <TRACK_TITLE>
Version: <TRACK_VERSION>
Quell-WAV: <CONFIRMED_SOURCE_WAV>
Bestätigtes reales BPM: <CONFIRMED_ACTUAL_BPM>
Status: MIXXX_READY
Extend-Ziel: DJ_OUTRO
Referenz-Cue: C7 OUTRO_START
Bestätigte FL-Position: <CONFIRMED_FL_BAR_BEAT>
Bestätigte Zeit: <CONFIRMED_TIMESTAMP>
```

Die Zeit stammt aus manueller FL-/Mixxx-Prüfung und ist kein automatisch
beatgenauer Suno-Marker.

### SUNO_EXTEND_STYLE

```text
Continue the same bouncy German hard tekk and dark psy-tekk track into a long instrumental DJ outro, 160 BPM target, straight 4/4 target, constant tempo. Keep the same distorted tekk kick, controlled rumble sub and rolling bass character. Reduce to kick, controlled rumble sub, closed hats and sparse rave percussion. Gradually remove lead, stabs, hook and atmosphere. Aim for an approximately 32-bar DJ outro phrase. No new melody, no new vocals, no tempo switch, no half-time section, no genre change.
```

### SUNO_EXTEND_LYRICS_OR_CUES

```text
[DJ Outro]
(instrumental, no vocals, aim for an approximately 32-bar DJ outro phrase)
(kick, controlled rumble sub, closed hats, sparse percussion, lead and atmosphere fading out)

[End]
```

Keine alten Lyrics und kein Hook werden wiederholt.

### SUNO_UI_INSTRUCTIONS

1. Song in Suno öffnen.
2. `(...)` auswählen.
3. Remix/Edit auswählen.
4. Extend auswählen.
5. Weißen Extend-Pfeil manuell auf oder minimal vor `<CONFIRMED_TIMESTAMP>`
   setzen. Suno verlängert nicht automatisch beatgenau an diesem Punkt —
   die Position kommt aus deiner FL-/Mixxx-Prüfung.
6. Übergang an dieser Stelle probehören, bevor du generierst.
7. `SUNO_EXTEND_STYLE` und `SUNO_EXTEND_LYRICS_OR_CUES` einfügen.
8. Mindestens zwei Varianten erzeugen und Naht, Kick, Bass und Übergang
   bewerten.
9. Gewählte Variante über `(...) → Create → Get Whole Song` zusammensetzen.
10. Neue Datei als neue Version exportieren, `<CONFIRMED_SOURCE_WAV>`
    nicht überschreiben.

### POST_EXTEND_QC

- Status direkt nach der Erzeugung: `EXTEND_RENDERED`.
- Neue Whole-Song-WAV in FL Studio Desktop importieren.
- Downbeat, reales Tempo, Grid, ursprüngliche Abschnitte, Extend-Naht und
  neuen Abschnitt erneut prüfen.
- In Mixxx neu analysieren.
- Beatgrid und C1 bis C7 vollständig neu kontrollieren — der Extend kann
  die gesamte Zeitachse verschieben.
- C8 EXTEND_SEAM optional auf die Naht setzen.
- Erst nach vollständiger Bestätigung: `EXTEND_MIXXX_READY`.

### UPDATED_TRACK_VALIDATION_TEMPLATE

| ID | Arrangement target | Actual FL bar:beat | Actual time | FL verified | Mixxx cue name | Mixxx verified | Notes |
|---|---|---:|---:|---|---|---|---|
| C1 | DJ Intro start | NOT YET VERIFIED | NOT YET VERIFIED | No | INTRO_START | No | First real kick downbeat |
| C2 | Build 1 start | NOT YET VERIFIED | NOT YET VERIFIED | No | BUILD_1 | No | |
| C3 | Drop 1 start | NOT YET VERIFIED | NOT YET VERIFIED | No | DROP_1 | No | |
| C4 | Hook start | NOT YET VERIFIED | NOT YET VERIFIED | No | HOOK_START | No | First audible hook syllable |
| C5 | Break start | NOT YET VERIFIED | NOT YET VERIFIED | No | BREAK_START | No | |
| C6 | Drop 2 start | NOT YET VERIFIED | NOT YET VERIFIED | No | DROP_2 | No | |
| C7 | DJ Outro start | NOT YET VERIFIED | NOT YET VERIFIED | No | OUTRO_START | No | |
| C8 | Extend seam, only if applicable | NOT YET VERIFIED | NOT YET VERIFIED | No | EXTEND_SEAM | No | Seam of this Suno Extend |
