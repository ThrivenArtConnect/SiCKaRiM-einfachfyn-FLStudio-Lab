# Beispiel — HPSS_ANALYSIS_REVIEW: "Kein Schlaf"

Quelle der Analyse: `ThrivenArtConnect/hard-tekk-hpss-pipeline`
Schema-Referenz: `templates/hpss-analysis-result.example.json`

Alle Werte unten sind **Platzhalter bzw. klar markierte illustrative
Kandidaten**. Es sind keine gemessenen Werte dieses Tracks. In einer echten
Antwort werden ausschließlich Felder übernommen, die im gelieferten
JSON-Report tatsächlich stehen; fehlende Felder bleiben `NOT PROVIDED`.

Ein HPSS-Report ist eine Analysequelle, keine menschliche Bestätigung.
Er kann `FL_GRID_VERIFIED`, `MIXXX_READY` oder `EXTEND_MIXXX_READY` nicht
setzen und keinen Extend freigeben.

---

### TRACK_STATUS

```
TRACK_TITLE: Kein Schlaf
ARTIST: SiCKaRiM
TRACK_VERSION: <TRACK_VERSION>
SOURCE_WAV_FILENAME: <SOURCE_WAV_FILENAME>
TARGET_BPM: 160
ACTUAL_BPM: NOT YET VERIFIED
STATUS: ANALYSIS_REVIEW_REQUIRED
FL_GRID_VERIFIED: No
MIXXX_BEATGRID_VERIFIED: No
HPSS_ANALYSIS_REPORT: <PATH_TO_REPORT_JSON>
EXTEND_ALLOWED: No
```

### HPSS_ANALYSIS_SUMMARY

```
Analyzer: hard-tekk-hpss-pipeline
Analyzer version: <ANALYZER_VERSION>
Job ID: <JOB_ID>
Report created at: <CREATED_AT>
Source SHA256: <SOURCE_SHA256>
Audio duration: <DURATION_SECONDS>
Sample rate: <SAMPLE_RATE_HZ>
Channels: <CHANNELS>

Tempo candidate: <ESTIMATED_BPM> (confidence <TEMPO_CONFIDENCE>, stability <STABILITY>)
Beat candidates reported: <BEAT_COUNT>
Downbeat candidates reported: <DOWNBEAT_COUNT>
Phrase candidates reported: <PHRASE_COUNT>
Audio QC: integrated <INTEGRATED_LUFS> LUFS, true peak <TRUE_PEAK_DBTP> dBTP, clipping suspected <CLIPPING_SUSPECTED>
Loop QC: <LOOP_QC_RECOMMENDATION>

Alle Werte sind Pipeline candidates. NOT HUMAN VERIFIED.
```

Kein Wert aus diesem Block wird in `Actual FL bar:beat`, `Actual time`,
`FL verified` oder `Mixxx verified` des Track-Validation-Sheets übernommen.

### HPSS_CANDIDATE_REVIEW_TABLE

Illustrative Zeilen mit Platzhaltern — keine gemessenen Werte:

| Pipeline candidate | Timestamp | Confidence | Candidate interpretation | Human FL check | Human Mixxx check |
|---|---:|---:|---|---|---|
| Tempo estimate | NOT APPLICABLE | `<TEMPO_CONFIDENCE>` | Kandidat `<ESTIMATED_BPM>`, gegen Zieltempo 160 BPM prüfen | NOT YET VERIFIED | NOT YET VERIFIED |
| Downbeat candidate 1 | `<TS_DB1>` | `<CONF_DB1>` | Möglicher erster echter Kick-Downbeat, Kandidat für C1 INTRO_START | NOT YET VERIFIED | NOT YET VERIFIED |
| Phrase candidate 1 | `<TS_PH1>` | `<CONF_PH1>` | Möglicher Build-1-Einsatz, Kandidat für C2 BUILD_1 | NOT YET VERIFIED | NOT YET VERIFIED |
| Phrase candidate 2 | `<TS_PH2>` | `<CONF_PH2>` | Möglicher Drop-1-Einsatz, Kandidat für C3 DROP_1 | NOT YET VERIFIED | NOT YET VERIFIED |
| Phrase candidate 3 | `<TS_PH3>` | `<CONF_PH3>` | Möglicher Break-Einsatz, Kandidat für C5 BREAK_START | NOT YET VERIFIED | NOT YET VERIFIED |
| Phrase candidate 4 | `<TS_PH4>` | `<CONF_PH4>` | Möglicher Drop-2-Einsatz, Kandidat für C6 DROP_2 | NOT YET VERIFIED | NOT YET VERIFIED |
| Phrase candidate 5 | `<TS_PH5>` | `<CONF_PH5>` | Möglicher Outro-Einsatz, Kandidat für C7 OUTRO_START | NOT YET VERIFIED | NOT YET VERIFIED |
| Hook onset candidate | `<TS_HOOK>` | `<CONF_HOOK>` | Möglicher Vocal-Einsatz, Kandidat für C4 HOOK_START | NOT YET VERIFIED | NOT YET VERIFIED |
| Audio QC | NOT APPLICABLE | NOT APPLICABLE | `<INTEGRATED_LUFS>` LUFS / `<TRUE_PEAK_DBTP>` dBTP, Clipping-Verdacht `<CLIPPING_SUSPECTED>` | NOT YET VERIFIED | NOT YET VERIFIED |

Der Hook-Kandidat ist besonders unsicher: HPSS trennt harmonische und
perkussive Anteile, es erkennt keine Silben. Die erste hörbare Hook-Silbe
wird ausschließlich am Audio bestimmt.

### REQUIRED_FL_STUDIO_CHECK

1. Finale WAV in ein neues FL-Studio-Desktop-Projekt importieren.
2. Projekt-Tempo auf 160 BPM setzen und den Tempo-Kandidaten
   `<ESTIMATED_BPM>` dagegen hören, nicht übernehmen.
3. Ersten echten Kick-Downbeat am Audio suchen; Downbeat-Kandidat
   `<TS_DB1>` nur als Suchhilfe verwenden.
4. Clip so ausrichten, dass dieser Downbeat auf Takt 1 Beat 1 liegt.
5. Jeden Phrase-Kandidaten einzeln anfahren und mit Grid/Metronom prüfen,
   ob dort wirklich ein Abschnittswechsel auf einem Downbeat liegt.
6. Hook-Kandidat `<TS_HOOK>` gegen die erste hörbare Hook-Silbe prüfen.
7. Grid zusätzlich an weit auseinanderliegenden Stellen kontrollieren:
   Intro, Drop 1, Break, Drop 2, Outro.
8. Bestätigte Marker C1 bis C7 setzen und `BAR:BEAT` sowie `MM:SS.mmm`
   notieren.
9. Bei Clipping-Verdacht `<CLIPPING_SUSPECTED>` Peaks in FL prüfen und
   gegebenenfalls neue Suno-Variante rendern.

### REQUIRED_MIXXX_CHECK

1. Nur die in FL geprüfte finale WAV importieren, Analyse abwarten.
2. Beatgrid am ersten sicheren Kick-Downbeat prüfen und korrigieren.
3. Jeden bestätigten Phasenpunkt gegen das Beatgrid prüfen — bewusst an
   weit auseinanderliegenden Stellen, nicht nur am Anfang.
4. C1 bis C7 auf reale hörbare Downbeats setzen.
5. C4 HOOK_START auf die erste hörbare Hook-Silbe setzen.
6. Bei BPM-Drift, unstetigem Grid oder unregelmäßigen Phrasen:
   kein `MIXXX_READY`.
7. Erst nach vollständiger Bestätigung `MIXXX_READY` melden.

### EXTEND_ELIGIBILITY

```
EXTEND BLOCKED — pipeline candidates require human FL Studio and Mixxx verification.
```
