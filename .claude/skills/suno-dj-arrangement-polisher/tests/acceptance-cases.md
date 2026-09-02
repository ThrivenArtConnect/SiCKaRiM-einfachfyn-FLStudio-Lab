# Abnahmefälle — suno-dj-arrangement-polisher

Testplan zur manuellen Regressionsprüfung des Skills. Jeder Fall wird
gegen die Regeln in `../SKILL.md` geprüft.

## Querschnittsprüfungen für jeden Testfall

Diese Punkte gelten zusätzlich zu den fallspezifischen Erwartungen:

- [ ] Keine erfundenen Werte (BPM, Bar:Beat, Zeit, Tonart, Cue-Position).
- [ ] Keine taktexakten Suno-Versprechen; Taktangaben nur als
      "target", "aim for" oder "approximately".
- [ ] Keine unzulässigen zusätzlichen Vocals im Song-Modus.
- [ ] Korrekte und vollständige Ausgabeblöcke in der vorgeschriebenen Reihenfolge.
- [ ] Statuslogik eingehalten, kein Statussprung ohne menschliche Evidenz.
- [ ] Keine widersprüchlichen Stilvorgaben (kein Pop, kein Mainstage EDM,
      kein Melodic Techno, kein 128-BPM-Feeling, keine Gitarren, kein
      Orchester, kein Trap-Hi-Hat-Pattern, kein Lo-Fi, kein moderner
      Hard-Techno-Kick, kein Industrial-Stomp, keine Noise-Wall, kein
      Autotune, keine cleanen Pop-Vocals).
- [ ] Deutsch für Workflow und QC, Englisch für Suno-Prompts.

---

## TEST 01 — Song, ein Hook

**Input:** "Bau mir einen 160-BPM-Hard-Tekk-Track. Hook: `Kein Schlaf, nur
Bass — bis die Nacht zerbricht. / SiCKaRiM im Nebel — alles außer Sicht.`"

**Erwartung:**

- [ ] Modus `SONG_NEW` erkannt.
- [ ] Fünf Blöcke: `TRACK_STATUS`, `STYLE_PROMPT`,
      `LYRICS_AND_SECTION_CUES`, `EXCLUDE_STYLES`,
      `FL_AND_MIXXX_VALIDATION_SHEET`.
- [ ] Hook erscheint exakt einmal und unverändert.
- [ ] `STYLE_PROMPT` englisch, maximal 850 Zeichen, enthält "160 BPM target".
- [ ] Alle Nicht-Hook-Abschnitte eindeutig als instrumental markiert.
- [ ] Status `UNVALIDATED`, alle Ist-Werte `NOT YET VERIFIED`.

**Fehlschlag, wenn:** Hook mehrfach erscheint, zusätzliche Lyrics
auftauchen oder eine Taktzahl zugesichert wird.

---

## TEST 02 — Unerlaubte Extra-Vocals

**Input:** "Gleicher Track, aber mit Adlibs im Drop und Vocal-Chops im Break."

**Erwartung:**

- [ ] Skill weist die zusätzlichen Vocals für den Song-Modus zurück und
      begründet dies kurz (ein Hook only).
- [ ] Angebot, Adlibs und Chops als separate `SOUNDS_ONE_SHOT`-Anfragen
      zu bauen.
- [ ] Falls dennoch eine Song-Payload ausgegeben wird: enthält sie nur den
      einen Hook, und `EXCLUDE_STYLES` schließt Adlibs, Vocal-Chops, Rap,
      Spoken Word und Backing Vocals aus.
- [ ] Status `UNVALIDATED`.

**Fehlschlag, wenn:** Adlibs oder Chops in `LYRICS_AND_SECTION_CUES` stehen.

---

## TEST 03 — Zugesicherte Taktzahl

**Input:** "Das Intro muss garantiert exakt 32 Takte lang sein."

**Erwartung:**

- [ ] Skill korrigiert auf ein Arrangement-Ziel:
      "approximately 32-bar DJ intro phrase target".
- [ ] Kurze Begründung: Suno rendert Musik, kein Taktraster; die reale
      Länge steht erst nach der FL-Prüfung fest.
- [ ] Keine Formulierung, die eine Taktzahl oder Samplegenauigkeit zusichert.
- [ ] Status `UNVALIDATED`.

**Fehlschlag, wenn:** die Ausgabe eine feste Taktzahl verspricht.

---

## TEST 04 — Percussion-Loop

**Input:** "Percussion-Loop bei 160 BPM, ich baue Kick und Sub selbst."

**Erwartung:**

- [ ] Modus `SOUNDS_LOOP`.
- [ ] Vier Blöcke: `SOUND_SETTINGS`, `SOUND_PROMPT`, `LIBRARY_METADATA`,
      `FL_STUDIO_QC`.
- [ ] `Type: Loop`, `BPM: 160`, `Key: Any`.
- [ ] Prompt enthält "no kick drum", "no bassline" und "seamless loop".
- [ ] Prompt nennt "one-bar loop" oder "two-bar loop" als Ziel.
- [ ] Dateiname nach Schema `SKM_LOOP_PERC_<CHARACTER>_160_ANY_001.wav`.
- [ ] `STATUS: UNVALIDATED`.
- [ ] FL-QC nennt den Wiederholungstest mit mindestens acht Durchläufen.

**Fehlschlag, wenn:** Kick oder Bass im Prompt bleiben oder ein sauberer
Loop zugesichert wird.

---

## TEST 05 — Tonaler Loop ohne Key

**Input:** "Mach mir einen Stab-Loop bei 160 BPM." (keine Tonart genannt)

**Erwartung:**

- [ ] Skill fragt nach der Zieltonart ODER setzt `Key: Any` und markiert
      dies ausdrücklich als unsicher.
- [ ] Bei `Key: Any` steht ein Hinweis, die Tonart später in FL Studio
      gegen das Projekt zu prüfen.
- [ ] `TARGET_KEY` in den Metadaten spiegelt die tatsächliche Einstellung
      (`ANY`), keine erfundene Tonart.
- [ ] `STATUS: UNVALIDATED`.

**Fehlschlag, wenn:** eine Tonart erfunden wird, z. B. stillschweigend
F-Moll gesetzt wird.

---

## TEST 06 — Vocal-Shout

**Input:** "Vocal-Shout für die Library: `KEIN SCHLAF!`"

**Erwartung:**

- [ ] Modus `SOUNDS_ONE_SHOT`.
- [ ] `Type: One-Shot`, `BPM: Auto`, `Key: Any`.
- [ ] Genau die gelieferte Phrase, kein zusätzlicher Text.
- [ ] Prompt enthält "no singing, no rap, no backing vocals, no music".
- [ ] Dateiname nach Schema, z. B.
      `SKM_ONESHOT_VOX_KeinSchlaf_AUTO_ANY_001.wav`.
- [ ] `STATUS: UNVALIDATED`.
- [ ] FL-QC prüft Verständlichkeit, Timing, Doppelstimmen und Hallfahne.

**Fehlschlag, wenn:** weitere Lyrics erfunden werden oder ein musikfreies
Ergebnis zugesichert wird.

---

## TEST 07 — HPSS-Report vorhanden

**Input:** Valider HPSS-JSON-Report mit Tempo-, Downbeat- und
Phrase-Kandidaten, Pfad wird geliefert.

**Erwartung:**

- [ ] Modus `HPSS_ANALYSIS_REVIEW`.
- [ ] Sechs Blöcke: `TRACK_STATUS`, `HPSS_ANALYSIS_SUMMARY`,
      `HPSS_CANDIDATE_REVIEW_TABLE`, `REQUIRED_FL_STUDIO_CHECK`,
      `REQUIRED_MIXXX_CHECK`, `EXTEND_ELIGIBILITY`.
- [ ] Status maximal `ANALYSIS_REVIEW_REQUIRED`.
- [ ] Alle Werte als Pipeline candidate mit Confidence und
      `NOT HUMAN VERIFIED` gekennzeichnet.
- [ ] Keine Kandidatenwerte in `Actual FL bar:beat`, `Actual time`,
      `FL verified` oder `Mixxx verified`.
- [ ] `EXTEND_ELIGIBILITY` lautet wörtlich:
      `EXTEND BLOCKED — pipeline candidates require human FL Studio and Mixxx verification.`
- [ ] Nur Felder aus dem Report werden übernommen, fehlende bleiben
      `NOT PROVIDED`.

**Fehlschlag, wenn:** ein Verifizierungsstatus gesetzt oder Extend
freigegeben wird.

---

## TEST 08 — HPSS-Report fehlt oder ist ungültig

**Input:** Kein Report auffindbar, oder das JSON ist nicht lesbar.

**Erwartung:**

- [ ] Keine Werte berechnet oder erfunden.
- [ ] Status bleibt `UNVALIDATED`.
- [ ] Ausgabe enthält wörtlich:
      `HPSS analysis unavailable — manual FL Studio validation required.`
- [ ] Lokaler FL-Studio- und Mixxx-Validierungsplan wird ausgegeben.
- [ ] Song- und Sounds-Payloads bleiben weiterhin möglich.
- [ ] Extend bleibt blockiert.

**Fehlschlag, wenn:** eine eigene BPM- oder Downbeat-Schätzung erfolgt.

---

## TEST 09 — Extend ohne Mixxx-Prüfung

**Input:** "Status ist FL_GRID_VERIFIED, mach mir ein DJ-Outro-Extend."
Mixxx-Prüfung und Cue-Zeiten fehlen.

**Erwartung:**

- [ ] Fünf Blöcke: `TRACK_STATUS`, `EXTEND_BLOCKED`,
      `MISSING_EXTEND_EVIDENCE`, `REQUIRED_FL_STUDIO_CHECK`,
      `REQUIRED_MIXXX_CHECK`.
- [ ] Kein Suno-Extend-Prompt, keine `SUNO_UI_INSTRUCTIONS`.
- [ ] Kein geschätzter Timestamp, kein behaupteter Downbeat, keine
      behauptete 16-/32-/64-Bar-Stelle.
- [ ] Fehlende Pflichtwerte tabellarisch aufgeführt.
- [ ] Status bleibt `FL_GRID_VERIFIED`.

**Fehlschlag, wenn:** trotz fehlender Mixxx-Daten eine Extend-Payload
oder ein Zeitwert erscheint.

---

## TEST 10 — Extend nach vollständiger Bestätigung

**Input:** Status `MIXXX_READY`, alle Pflichtdaten inklusive C1 bis C7 mit
realen `BAR:BEAT`- und `MM:SS.mmm`-Werten, Referenz-Cue C7 OUTRO_START,
Ziel `DJ_OUTRO`.

**Erwartung:**

- [ ] Sieben Blöcke: `TRACK_STATUS`, `CONFIRMED_CONTEXT`,
      `SUNO_EXTEND_STYLE`, `SUNO_EXTEND_LYRICS_OR_CUES`,
      `SUNO_UI_INSTRUCTIONS`, `POST_EXTEND_QC`,
      `UPDATED_TRACK_VALIDATION_TEMPLATE`.
- [ ] `CONFIRMED_CONTEXT` übernimmt ausschließlich gelieferte Werte und
      enthält den Hinweis, dass die Zeit aus manueller FL-/Mixxx-Prüfung
      stammt und kein automatisch beatgenauer Suno-Marker ist.
- [ ] `SUNO_EXTEND_STYLE` englisch, instrumental, gleicher Kerncharakter,
      Taktziel nur als "aim for an approximately 32-bar DJ outro phrase".
- [ ] `SUNO_EXTEND_LYRICS_OR_CUES` enthält keine alten Lyrics und keinen Hook.
- [ ] `SUNO_UI_INSTRUCTIONS` deutsch, Nutzer setzt den weißen Pfeil selbst,
      Export als neue Version ohne Überschreiben der Quell-WAV.
- [ ] `POST_EXTEND_QC` setzt den Status nach dem Render auf
      `EXTEND_RENDERED` und erzwingt erneute FL- und Mixxx-Prüfung.
- [ ] `UPDATED_TRACK_VALIDATION_TEMPLATE` enthält wieder
      `NOT YET VERIFIED` in allen Ist-Spalten.
- [ ] `EXTEND_MIXXX_READY` wird nicht vorweggenommen.

**Fehlschlag, wenn:** alte Cue-Zeiten als weiterhin gültig dargestellt
werden oder der Status direkt auf `EXTEND_MIXXX_READY` springt.

---

## Auswertung

| Test | Ergebnis | Datum | Prüfer | Notiz |
|---|---|---|---|---|
| TEST 01 | NOT YET RUN | | | |
| TEST 02 | NOT YET RUN | | | |
| TEST 03 | NOT YET RUN | | | |
| TEST 04 | NOT YET RUN | | | |
| TEST 05 | NOT YET RUN | | | |
| TEST 06 | NOT YET RUN | | | |
| TEST 07 | NOT YET RUN | | | |
| TEST 08 | NOT YET RUN | | | |
| TEST 09 | NOT YET RUN | | | |
| TEST 10 | NOT YET RUN | | | |
