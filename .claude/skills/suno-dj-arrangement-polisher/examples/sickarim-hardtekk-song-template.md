# Beispiel — SONG_NEW: "Kein Schlaf"

Modus: `SONG_NEW`
Zieltempo: 160 BPM (konkreter Trackwert, hat Vorrang vor dem 165-BPM-Default)
Hook: vom Nutzer geliefert, erscheint genau einmal.

Alles unterhalb der Trennlinie ist die Payload, wie der Skill sie ausgibt —
ohne Einleitung, ohne Alternativen, ohne Ist-Werte.

---

### TRACK_STATUS

```
TRACK_TITLE: Kein Schlaf
ARTIST: SiCKaRiM
TRACK_VERSION: v001
TARGET_BPM: 160
TARGET_TIME_SIGNATURE: 4/4
ACTUAL_BPM: NOT YET VERIFIED
SOURCE_WAV_FILENAME: NOT YET VERIFIED
STATUS: UNVALIDATED
FL_GRID_VERIFIED: No
MIXXX_BEATGRID_VERIFIED: No
HPSS_ANALYSIS_REPORT: NOT YET PROVIDED
EXTEND_ALLOWED: No
```

### STYLE_PROMPT

```text
Bouncy German hard tekk, rave tekk and dark psy-tekk, underground rave energy, 160 BPM target, straight 4/4 target. Punchy distorted tekk kick with controlled heavy sub and rolling rumble bass. Tight rave percussion, dry claps, short offbeat hats. Dark atmospheric pads, industrial rave stabs, controlled psy movement, raw transitions, risers, impacts, snare rolls. Loud club-ready mix, kick and sub locked and never masked, mid-forward stabs, wide dark atmosphere, no muddy low mids. DJ-friendly arrangement target: long instrumental beat intro, build, drop, single hook moment, break, second build, harder second drop, long reduced instrumental DJ outro. Constant tempo, no tempo switches, no half-time sections, no genre changes.
```

Zeichenzahl liegt unter dem 850-Zeichen-Limit; bei Anpassungen erneut prüfen.

### LYRICS_AND_SECTION_CUES

```text
[Instrumental Intro]
(instrumental, no vocals, aim for an approximately 32-bar DJ intro phrase target)
(distorted tekk kick, controlled rumble sub, closed hats, sparse rave percussion, no dominant lead)

[Build]
(instrumental, no vocals, approximately 16-bar build phrase target)
(riser, filtered noise sweep, snare roll into the drop)

[Drop]
(instrumental, no vocals, approximately 32-bar drop phrase target)
(full hard tekk kick and rolling bass, industrial rave stabs, controlled psy movement)

[Hook]
(single vocal moment, this hook appears once only, target approximately 8 bars)
Kein Schlaf, nur Bass — bis die Nacht zerbricht.
SiCKaRiM im Nebel — alles außer Sicht.

[Drop]
(instrumental, no vocals, approximately 16-bar post-hook drop phrase target)
(return to full kick and rolling bass, no further vocals)

[Break]
(instrumental, no vocals, approximately 16-bar break phrase target)
(dark atmospheric pad, filtered bass, tension FX, reverse impact)

[Build]
(instrumental, no vocals, approximately 16-bar build phrase target)
(rising noise, snare roll, hard impact into the second drop)

[Drop]
(instrumental, no vocals, aim for an approximately 32- to 64-bar second drop phrase target)
(hardest section, distorted kick, heavier rumble, industrial rave stabs, psy movement)

[DJ Outro]
(instrumental, no vocals, aim for an approximately 32-bar DJ outro phrase target)
(kick, controlled rumble sub, closed hats, sparse percussion, lead and atmosphere fading out)

[End]
```

### EXCLUDE_STYLES

```text
pop, mainstage EDM, melodic techno, 128 BPM feel, guitars, orchestral, trap hi-hat patterns, lo-fi, modern hard techno kick, industrial stomp, noise wall, autotune, clean pop vocals, extra adlibs, vocal chops, rap verses, spoken word, backing vocals
```

### FL_AND_MIXXX_VALIDATION_SHEET

Alle Ist-Werte bleiben leer, bis die WAV in FL Studio Desktop am Grid liegt
und Mixxx ein geprüftes Beatgrid hat. Die Taktziele oben sind Arrangement-
Ziele und sagen nichts über die tatsächliche Länge der Abschnitte aus.

| ID | Arrangement target | Actual FL bar:beat | Actual time | FL verified | Mixxx cue name | Mixxx verified | Notes |
|---|---|---:|---:|---|---|---|---|
| C1 | DJ Intro start | NOT YET VERIFIED | NOT YET VERIFIED | No | INTRO_START | No | First real kick downbeat |
| C2 | Build 1 start | NOT YET VERIFIED | NOT YET VERIFIED | No | BUILD_1 | No | |
| C3 | Drop 1 start | NOT YET VERIFIED | NOT YET VERIFIED | No | DROP_1 | No | |
| C4 | Hook start | NOT YET VERIFIED | NOT YET VERIFIED | No | HOOK_START | No | First audible hook syllable |
| C5 | Break start | NOT YET VERIFIED | NOT YET VERIFIED | No | BREAK_START | No | |
| C6 | Drop 2 start | NOT YET VERIFIED | NOT YET VERIFIED | No | DROP_2 | No | |
| C7 | DJ Outro start | NOT YET VERIFIED | NOT YET VERIFIED | No | OUTRO_START | No | |
| C8 | Extend seam, only if applicable | NOT APPLICABLE | NOT APPLICABLE | No | EXTEND_SEAM | No | Only after Suno Extend |

Nächste Schritte:

1. Suno-Render als WAV exportieren und Dateinamen in `TRACK_STATUS` eintragen.
2. FL-Studio-Desktop-QC nach SKILL.md Abschnitt 6 durchführen.
3. Mixxx-QC nach SKILL.md Abschnitt 7 durchführen.
4. Erst danach Statuswechsel anfordern. Extend bleibt bis `MIXXX_READY` gesperrt.
