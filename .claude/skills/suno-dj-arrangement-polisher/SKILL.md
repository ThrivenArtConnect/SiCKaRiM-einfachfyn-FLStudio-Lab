---
name: suno-dj-arrangement-polisher
description: >
  Erzeugt und prüft projektkonforme Suno-v5/v5.5/ReMi Prompt-Payloads
  für SiCKaRiM Hard-Tekk/RaveTekk/Psy-Tekk, Suno Sounds Library-Prompts,
  HPSS-Analyse-Review-Reports, FL-Studio-/Mixxx-Validierungspläne und
  statusgesicherte Suno-Extend-Pakete. Keine BPM-, Downbeat-, Cue- oder
  Taktlängen-Freigabe ohne bestätigte menschliche Evidenz.
---

# Suno DJ Arrangement Polisher

Arbeitssprache: **Deutsch** für Workflow, QC-Listen und Rückfragen.
**Englisch** ausschließlich innerhalb von `STYLE_PROMPT`, `EXCLUDE_STYLES`,
Suno-Section-Tags, `SOUND_PROMPT` und `SUNO_EXTEND_STYLE`.

---

# 1. Zweck und Aufruf

Dieser Skill wird verwendet, sobald es um SiCKaRiM-Material für Suno
(v5, v5.5, ReMi), um die Suno-Sounds-Sample-Library, um FL-Studio-/Mixxx-
Validierung oder um Suno Extend geht.

Vier Dinge sind bewusst getrennt und dürfen nicht vermischt werden:

- **Song-Payloads** — komplette Tracks für den Suno-Song-Generator.
- **Suno Sounds** — einzelne Loops und One-Shots für die Sample-Library.
- **HPSS-Analyse** — maschinelle Analyse-Kandidaten aus einem externen Repo.
- **Extend** — Verlängerung eines bereits real geprüften Songs.

Der Grund für diese Trennung: Ein Song-Prompt darf höchstens einen Hook
enthalten, ein Library-Shout dagegen genau eine kurze Phrase. Ein Extend
setzt reale, gemessene Cue-Zeiten voraus, ein neuer Song nicht. Wer die
Modi vermischt, produziert entweder unbrauchbare Samples oder Extends an
falschen Stellen.

**FL Studio Desktop und Mixxx sind die einzige gültige Audio-Referenz.**
Was Suno im Prompt versprochen bekommt, ist ein kreatives Ziel. Was im
Track wirklich passiert, steht erst fest, wenn die WAV in FL Studio am
Grid liegt und in Mixxx ein sauberes Beatgrid hat. Deshalb sind alle
Taktangaben in diesem Skill Ziele ("target", "aim for", "approximately")
und niemals Zusagen. Suno-Section-Tags steuern die Musik, sie garantieren
keine Taktlängen und keine samplegenauen Positionen.

**HPSS erzeugt Kandidaten, keine Freigaben.** Ein Analyse-Report aus
`ThrivenArtConnect/hard-tekk-hpss-pipeline` ist eine Vorsortierung, die
dem Menschen Prüfarbeit spart. Er ersetzt die DJ-Validierung nicht und
darf keinen Verifizierungsstatus setzen.

Wenn Angaben fehlen, wird nachgefragt oder `NOT YET VERIFIED` gesetzt —
nie geschätzt. Ein erfundener Timestamp kostet im Club einen Mix.

---

# 2. Modus-Erkennung

Bestimme zuerst den Modus aus der Anfrage. Bei Mehrdeutigkeit kurz
nachfragen, statt zwei Modi in einer Ausgabe zu mischen.

## A. SONG_NEW

- Neuer Suno-Song wird angefragt.
- Ausgabe: vollständige Song-Payload (Abschnitt 4).
- Status: immer `UNVALIDATED`.

## B. SONG_POLISH

- Nutzer liefert einen vorhandenen Suno-Prompt, Lyrics, Hook oder Arrangement.
- Aufgabe: Widersprüche entfernen (Genre-Mischungen, Tempo-Switches,
  überzählige Vocals, Taktgarantien) und eine saubere Song-Payload bauen.
- Status: `UNVALIDATED`, solange keine Audio-Evidenz vorliegt.

## C. SOUNDS_LOOP

- Einzelner rhythmischer oder tonaler Suno-Sounds-Loop.
- Ausgabe: Loop-Settings, Prompt, Library-Metadaten, FL-QC (Abschnitt 5).
- Status: immer `UNVALIDATED`.

## D. SOUNDS_ONE_SHOT

- One-Shots: FX, Impacts, Riser, Snare Fills oder Vocal-Shouts.
- Ausgabe: Settings, Prompt, Library-Metadaten, FL-QC (Abschnitt 5).
- Status: immer `UNVALIDATED`.

## E. HPSS_ANALYSIS_REVIEW

- Nutzer liefert einen JSON-Report oder einen lokalen Pfad zu einem
  Analyseergebnis aus `ThrivenArtConnect/hard-tekk-hpss-pipeline`.
- Nur vorhandene Werte lesen und formatieren, nichts ergänzen.
- Maximal erreichbarer Status: `ANALYSIS_REVIEW_REQUIRED`.
- Keine menschliche Freigabe simulieren.

## F. VALIDATION_UPDATE

- Nutzer liefert echte FL-Studio-/Mixxx-Prüfdaten (Werte, Screenshots,
  ausgefülltes Validation-Sheet).
- Track-Validation-Dokument nur mit explizit gelieferten Werten aktualisieren.
- Alles, was nicht geliefert wurde, bleibt `NOT YET VERIFIED`.

## G. EXTEND_REQUEST

- Suno-Extend-Paket nur bei vollständigen Pflichtdaten (Abschnitt 9).
- Fehlen Daten: Extend strikt blockieren.

---

# 3. Verbindliche Statuslogik

Ausschließlich diese Stati sind zulässig.

## UNVALIDATED

- Suno-Prompt oder Suno-Audio existiert.
- Reale BPM, Downbeats, Taktpositionen, Phrasen und Cues sind nicht
  ausreichend bestätigt.
- Keine Extend-Payload ausgeben.

## ANALYSIS_PENDING

- HPSS-Pipeline-Analyse wurde angefordert.
- Ergebnis liegt noch nicht vor.
- Kein Extend.

## ANALYSIS_REVIEW_REQUIRED

- Ein HPSS-Report liegt vor.
- Alle BPM-, Beat-, Downbeat-, Phrase- und QC-Daten bleiben Kandidaten.
- Kein Extend.

## FL_GRID_VERIFIED

Nur zulässig, wenn explizit bestätigt oder nachweisbar dokumentiert:

- erster echter Kick-Downbeat liegt auf Playlist Takt 1 Beat 1,
- Grid/Metronom über mehrere weit auseinanderliegende Songabschnitte geprüft,
- reale Phasenmarker dokumentiert.

Noch kein `MIXXX_READY` und noch kein Extend.

## MIXXX_READY

Nur zulässig, wenn zusätzlich zu `FL_GRID_VERIFIED` bestätigt ist:

- Mixxx-Beatgrid geprüft und korrigiert,
- Intro, Drop 1, Break, Drop 2 und Outro an weit auseinanderliegenden
  Punkten kontrolliert,
- C1 bis C7 mit realen Daten gesetzt,
- Cues liegen auf tatsächlich hörbaren Downbeats,
- Hook-Cue liegt auf der ersten hörbaren Hook-Silbe.

Erst ab diesem Status darf `EXTEND_REQUEST` ein Extend-Paket erzeugen.

## EXTEND_RENDERED

- Suno Extend wurde erzeugt bzw. als Whole Song zusammengesetzt.
- Neue Audiodatei ist noch nicht in FL/Mixxx validiert.
- Alte Cues und Zeitwerte niemals ungeprüft übernehmen.

## EXTEND_MIXXX_READY

- Extend-Whole-Song wurde in FL Studio Desktop UND Mixxx erneut geprüft.
- Neue tatsächliche Cue-/Grid-Daten sind dokumentiert.

## Erlaubte Statusfolge

```
UNVALIDATED
→ ANALYSIS_PENDING
→ ANALYSIS_REVIEW_REQUIRED
→ FL_GRID_VERIFIED
→ MIXXX_READY
→ EXTEND_RENDERED
→ EXTEND_MIXXX_READY
```

Ein Status wird nur durch neue, explizit gelieferte menschliche Evidenz
höher gesetzt. Der Skill hebt niemals selbst an.

---

# 4. Song-Payload-Regeln

Gilt für `SONG_NEW` und `SONG_POLISH`.

Ausgabe exakt in diesem Format, ohne Einleitung, ohne Alternativen und
ohne zusätzliche Erklärungen innerhalb der Payload:

```
### TRACK_STATUS
### STYLE_PROMPT
### LYRICS_AND_SECTION_CUES
### EXCLUDE_STYLES
### FL_AND_MIXXX_VALIDATION_SHEET
```

## Projektprofil

- Artist: SiCKaRiM.
- Stil: Bouncy Hard Tekk, RaveTekk, Dark Psy-Tekk, Underground German Rave.
- Projekt-Default: 165 BPM. Ein konkret genannter Trackwert hat Vorrang,
  z. B. 160 BPM.
- Taktart-Ziel: 4/4.
- Klangbild: punchy distorted tekk kick, controlled heavy sub / rumble bass,
  rolling bass, tight rave percussion, dark atmospheric pads, industrial
  rave stabs, kontrollierte Psy-Bewegung, rohe Übergänge, Riser, Impacts,
  Snare Rolls.

## STYLE_PROMPT

- Englisch.
- Maximal 850 Zeichen.
- Enthält Stil, BPM-Ziel, 4/4-Ziel, Groove, Sound-Design, Mix-Ziel und
  DJ-Arrangement-Ziel.
- Keine Songlyrics.
- Formulierung immer `160 BPM target` bzw. `165 BPM target`, niemals
  eine Formulierung, die ein exaktes Tempo zusichert.

## LYRICS_AND_SECTION_CUES

- Englische Tags in eckigen Klammern.
- Einfache Tags: `[Instrumental Intro]`, `[Build]`, `[Drop]`, `[Hook]`,
  `[Break]`, `[DJ Outro]`, `[End]`.
- Phrasenlängen nur als "aim for", "target" oder
  "approximately <N>-bar phrase target".
- Alle nicht-vokalen Abschnitte eindeutig als instrumental beschreiben.
- Der User-Hook erscheint exakt einmal und unverändert.

## EXCLUDE_STYLES

Eine kurze komma-separierte englische Liste, nur Ausschlüsse. Basis:

`pop, mainstage EDM, melodic techno, 128 BPM feel, guitars, orchestral,
trap hi-hat patterns, lo-fi, modern hard techno kick, industrial stomp,
noise wall, autotune, clean pop vocals`

## Song-Vocal-Regel

Für vollständige Suno-Songs gilt:

- Nur ein explizit vom Nutzer gelieferter Hook darf Lyrics enthalten.
- Dieser Hook wird exakt einmal und unverändert ausgegeben.
- Keine weiteren Vocals, Vocal-Chops, Adlibs, Rap, Spoken Word,
  Backing Vocals oder unaufgeforderten Gesang erzeugen.

Verlangt der Nutzer zusätzliche Vocals, weise darauf hin und biete an,
sie als separate Suno-Sounds-One-Shots zu bauen (Modus `SOUNDS_ONE_SHOT`).
Library-Shouts heben die "ein Hook only"-Regel für Songs nicht auf.

## DJ-Arrangement-Ziel

1. **DJ Intro** — approximately 32-bar target; Kick/Rumble/Hats/Percussion;
   ohne Vocals und ohne dominanten Lead.
2. **Build 1** — approximately 16-bar target; ohne Vocals.
3. **Drop 1** — approximately 32-bar target; Anfang instrumental.
4. **Hook** — exakt einmal, nach der ersten instrumentalen Drop-Hälfte.
5. **Post-Hook Drop** — approximately 16-bar target; instrumental.
6. **Break** — approximately 16-bar target; keine Vocals.
7. **Build 2** — approximately 16-bar target; keine Vocals.
8. **Drop 2** — approximately 32- oder 64-bar target; instrumental.
9. **DJ Outro** — approximately 32-bar target; Kick/Rumble/Hats,
   vocal-frei und lead-arm.

Keine Tempo-Switches, keine Half-Time-Parts, keine Genrewechsel.

Verlangt der Nutzer eine zugesicherte Taktzahl, korrigiere auf ein
Arrangement-Ziel und erkläre einmal kurz, warum: Suno rendert Musik, keine
Taktraster. Die reale Taktzahl steht erst nach der FL-Prüfung fest.

---

# 4a. Prompt-Grammatik — Standardvorlagen

Die Vorlagen hier sind copy-paste-fähig. Sie stehen unter den Regeln aus
Abschnitt 4; wo sich beides widerspricht, gewinnt Abschnitt 4.

## 4a.1 Style-Template

```text
Hard Tekk, 165 BPM, dark distorted kick, rolling bass, rave lead, psy movement, raw transitions, [Mastered for 48kHz], [High Dynamic Range], [Ultra-Clean Vocals], [Pre-amp: Balanced], [Headroom: High]
```

165 BPM ist der Projekt-Default. Nennt der Nutzer einen Trackwert, ersetzt
dieser ihn — der Rest der Zeile bleibt unverändert.

### Verifikationsstatus der Metatags

Die eckigen Klammern am Ende — `[Mastered for 48kHz]`, `[High Dynamic Range]`,
`[Ultra-Clean Vocals]`, `[Pre-amp: Balanced]`, `[Headroom: High]` — sind
**unverifizierte Prompt-Konditionierung**, keine dokumentierten Suno-Regler.

Was das praktisch heißt:

- Sie sind freier Text im Style-Feld. Suno liest sie als Beschreibung, nicht
  als Einstellung.
- `[Mastered for 48kHz]` erzwingt **keine** 48-kHz-Ausgabe. Die tatsächliche
  Sample-Rate der Datei entscheidet sich beim Export, nicht im Prompt, und
  wird in FL Studio geprüft.
- `[Ultra-Clean Vocals]` ist kein Garant für saubere Stimmen und hebt die
  Vocal-Regeln aus Abschnitt 4 nicht auf.
- Sie dürfen im Prompt stehen und schaden nicht. Sie dürfen dem Nutzer
  gegenüber nie als zugesicherte technische Wirkung dargestellt werden.

Wer belastbare 48 kHz und Headroom will, holt sie über den Export-Workflow
in Abschnitt 4b und die FL-Studio-QC — nicht über Klammern im Prompt.

## 4a.2 Lyrics-Template mit Struktur-Tags

```text
[Intro] (Instrumental)
[Verse 1] (warm male lead vocal, close and restrained)
[Chorus]
[Bridge]
[Outro] (Instrumental fade)
```

### Wann dieses Template gilt — und wann nicht

Dies ist die **generische** Song-Struktur für Suno. Für einen SiCKaRiM-
Track im Modus `SONG_NEW` oder `SONG_POLISH` gilt sie **nicht** unverändert,
weil sie mit `[Verse 1]`, `[Chorus]` und `[Bridge]` drei singende Abschnitte
vorsieht. Die Song-Vocal-Regel aus Abschnitt 4 erlaubt genau einen Hook.

Daraus folgt die Vorrangregel:

| Fall | Welches Template |
|---|---|
| SiCKaRiM-Track, Modus SONG_NEW / SONG_POLISH | Struktur aus Abschnitt 4 (DJ Intro → Build → Drop → **ein** Hook → Break → Build 2 → Drop 2 → DJ Outro), niemals dieses generische Template |
| Generischer Suno-Track außerhalb des SiCKaRiM-Profils, ausdrücklich vom Nutzer verlangt | Template oben, mit den vom Nutzer gelieferten Lyrics |
| Unklar | nachfragen, nicht raten |

Die Klammerregie wie `(warm male lead vocal, close and restrained)` ist ein
Beispiel für eine Stimmbeschreibung. Sie erfindet keine Lyrics und ersetzt
keine: Text kommt ausschließlich vom Nutzer.

Der `[Outro] (Instrumental fade)`-Tag ist zusätzlich für Bedingung D des
Extend-Gates relevant — siehe 9.1a.

## 4a.3 Production-Words statt Emotion-Words

Prompts beschreiben, was im Signal passiert, nicht wie es sich anfühlen soll.
Emotionsvokabular und Künstlernamen streuen das Ergebnis und machen zwei
Renders schwer vergleichbar; technische Begriffe engen es ein.

| Nicht verwenden | Stattdessen |
|---|---|
| epic, emotional, powerful, energetic, hypnotic feeling | distorted kick, rolling bass, sidechained pad, gated release |
| dark and sad, aggressive mood | minor key, detuned saw, hard filter bite |
| Künstler- oder Bandnamen, "im Stil von X" | die konkreten Klangmerkmale, die gemeint sind |
| radio-ready, professional, banger | loud club mix, kick and sub locked, mid-forward stabs |

Künstlernamen bleiben auch deshalb draußen, weil sie fremde Arbeit als
Zielvorgabe benutzen. Die gewünschten Merkmale lassen sich benennen, ohne
jemanden zu kopieren.

## 4a.4 Style-String bei jedem Extend wiederholen

Der Style-String des Quelltracks wird bei jedem Extend wortgleich
übernommen. Kein Rewrite, keine Paraphrase, kein "leicht aufgeräumt".
Das ist Bedingung B des Extend-Gates (9.1a) und dort begründet.

## 4a.5 Metatag-Liste High-Fidelity

| Metatag | Zweck laut Prompt-Praxis | Verifikationsstatus |
|---|---|---|
| `[Mastered for 48kHz]` | Sample-Rate-Hinweis | UNVERIFIED — kein dokumentierter Suno-Regler |
| `[High Dynamic Range]` | weniger Kompression | UNVERIFIED |
| `[Ultra-Clean Vocals]` | Artefaktarme Stimmen | UNVERIFIED |
| `[Pre-amp: Balanced]` | ausgewogener Pegel | UNVERIFIED |
| `[Headroom: High]` | Reserve vor 0 dBFS | UNVERIFIED |

Alle fünf sind zulässig und dürfen zusammen im Style-Feld stehen. Keiner
darf als zugesicherte Eigenschaft der Ausgabedatei ausgegeben werden. Was
die Datei wirklich hat, steht erst nach dem Export und der FL-Studio-Prüfung
fest.

---

# 5. Suno Sounds Library Builder

Pro Anfrage wird nur **ein** klarer Sound-Typ erzeugt. Mehrere
Klangquellen in einem Prompt liefern unbrauchbares Library-Material,
weil sich die Layer im Sample nicht mehr trennen lassen.

## 5.1 Ausgabeformat für Sounds

Bei `SOUNDS_LOOP` und `SOUNDS_ONE_SHOT` exakt:

```
### SOUND_SETTINGS
### SOUND_PROMPT
### LIBRARY_METADATA
### FL_STUDIO_QC
```

## 5.2 LOOP-Regeln

Für Drum/Percussion:

- Type: Loop
- BPM: konkreter Nutzerwert oder Projekt-Default
- Key: Any

Für tonale Bass-, Stab- und Lead-Loops:

- Type: Loop
- BPM: konkreter Nutzerwert oder Projekt-Default
- Key: nur setzen, wenn der Nutzer eine Zieltonart nennt.
- Fehlt die Tonart: bei klar tonalem Material nachfragen ODER `Key: Any`
  verwenden und dies ausdrücklich als unsicher markieren, mit dem Hinweis,
  die Tonart später in FL Studio gegen das Projekt zu prüfen.

Prompt-Pflichtbestandteile:

- `seamless loop`
- `one-bar loop` oder `two-bar loop` als Ziel, niemals als Zusicherung.

Kein Kick und kein Bass in Percussion-Loops, wenn sie später mit einem
eigenen Kick-/Sub-System kombiniert werden sollen — sonst kollidieren
zwei Kicks und der Sub wird unkontrollierbar.

Diese Ausschlüsse gehören verbindlich als englische Strings in den
`SOUND_PROMPT`, nicht nur als deutsche Umschreibung in der Erklärung.
Für jeden Percussion-Loop, der mit einem eigenen Kick-/Sub-System
kombiniert werden soll, muss der Prompt wörtlich enthalten:

- `no kick drum`
- `no bassline`

Der Grund für die Wortgleichheit: Formulierungen wie "no kick" oder
"without bass" lenken Suno erfahrungsgemäß schwächer, und ein
maschineller QC-Durchlauf über die Library findet sie nicht wieder.

Diese beiden Strings ergänzen die Pflichtbestandteile oben, sie ersetzen
sie nicht — `seamless loop` und `one-bar loop target` bzw.
`two-bar loop target` bleiben zusätzlich erforderlich. Auch mit allen
vier Strings bleibt es dabei: der Prompt lenkt Suno, er sichert keinen
nahtlosen Loop zu. Ob die Naht trägt, entscheidet der Wiederholungstest
in FL Studio.

Bei Bass-/Rumble-Loops klar vermerken: Sub-Anteil später in FL Studio
prüfen; im Zweifel nur als Mid-/Texture-Layer verwenden.

## 5.3 ONE-SHOT-Regeln

Für Drop Impacts, Reverse FX, Riser, Snare Fills:

- Type: One-Shot
- BPM: Auto
- Key: Any

Für Vocal-Shouts:

- Type: One-Shot
- BPM: Auto
- Key: Any
- Nur eine kurze, explizit vom Nutzer gelieferte Phrase.
- Prompt enthält: `no singing, no rap, no backing vocals, no music`.
- Keine zusätzlichen Lyrics erfinden.

Keine Zusagen wie "perfekt clean" oder "kommt garantiert ohne Musik".
Der Prompt lenkt Suno, kontrolliert wird am Audio in FL Studio.

## 5.4 Library-Metadaten

Jeder Sound erhält:

```
SAMPLE_ID:
TYPE:
ROLE:
TARGET_BPM:
TARGET_KEY:
TARGET_LENGTH:
PROMPT_VERSION:
FILENAME:
STATUS: UNVALIDATED
```

Benennung:

```
SKM_<TYPE>_<ROLE>_<CHARACTER>_<BPM-or-AUTO>_<KEY-or-ANY>_<3digit-version>.wav
```

Beispiele:

```
SKM_LOOP_PERC_DarkRave_160_ANY_001.wav
SKM_LOOP_STAB_RaveTekk_160_FMIN_001.wav
SKM_ONESHOT_FX_DropImpact_AUTO_ANY_001.wav
SKM_ONESHOT_VOX_KeinSchlaf_AUTO_ANY_001.wav
```

---

# 6. FL Studio Desktop QC

Dies ist eine Checkliste für den Nutzer, niemals eine Feststellung. Ohne
gelieferte Evidenz gilt kein Punkt als erledigt.

## Für komplette Suno-Songs

1. Finale Suno-WAV in ein neues FL-Studio-Desktop-Projekt importieren.
2. Projekt-Tempo auf Zieltempo setzen, z. B. 160 BPM.
3. Nicht den Dateianfang raten: ersten echten Kick-Downbeat suchen.
4. Audio-Clip so ausrichten, dass dieser Downbeat auf Takt 1 Beat 1 liegt.
5. Mit Grid/Metronom mindestens Intro, Drop 1, Break, Drop 2 und Outro prüfen.
6. Bei konstant abweichendem Tempo das reale Tempo manuell ermitteln.
7. Bei lokalem Drift zuerst eine neue Suno-Variante bevorzugen;
   Time-Stretch/Warp nur bewusst und sparsam einsetzen.
8. Reale Playlist-Marker setzen:
   - C1 INTRO_START
   - C2 BUILD_1
   - C3 DROP_1
   - C4 HOOK_START
   - C5 BREAK_START
   - C6 DROP_2
   - C7 OUTRO_START
9. Zeitwerte und FL-Bar:Beat erst nach manueller Prüfung dokumentieren.

## Für Suno Sounds Loops

1. Am ersten sinnvollen Transient sauber schneiden.
2. In ein Projekt mit Zieltempo importieren.
3. Mindestens acht direkte Wiederholungen testen.
4. Loop-Naht auf Klicks, Timing-Versatz, wechselnde Hallfahne und
   Phasenprobleme prüfen.
5. Bei tonal spielendem Material Tonart gegen das Projekt prüfen.
6. Bei Bass/Rumble den Sub kontrollieren; bei Unsicherheit nur als
   Mid-/Texture-Layer verwenden.
7. `FL_GRID_VERIFIED` erst mit expliziter Nutzerbestätigung setzen.

## Für One-Shots

1. Start am gewünschten Transient schneiden.
2. Fade-In/Fade-Out gegen Klicks prüfen.
3. Lautheit und störende Hintergrundmusik prüfen.
4. Vocal-Shout: Verständlichkeit, Timing, ungewollte Doppelstimmen und
   Hallfahne prüfen.
5. Status ohne Nutzerbestätigung nicht hochsetzen.

---

# 7. Mixxx QC

Ebenfalls reine Checkliste, niemals als erledigt darstellen.

1. Nur die geprüfte finale WAV-Version nach Mixxx importieren.
2. Analyse abwarten.
3. Beatgrid am ersten sicheren Kick-Downbeat prüfen und bei Bedarf korrigieren.
4. Danach weit entfernte Songstellen prüfen: Intro, Drop 1, Break,
   Drop 2 und Outro.
5. C1 bis C7 nur auf reale hörbare Downbeats setzen.
6. C4 HOOK_START auf die erste tatsächlich hörbare Hook-Silbe setzen.
7. Bei BPM-Drift, unstetigem Grid oder unregelmäßigen Phrasen:
   kein `MIXXX_READY`.
8. Nach einem Suno Extend alte Cues nie blind übernehmen; sämtliche Cues
   und das Grid erneut prüfen.
9. Optionaler Extend-Naht-Marker: C8 EXTEND_SEAM.

---

# 8. Pflicht-Track-Validation-Format

Für vollständige Songs exakt dieses Tabellenmodell verwenden:

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

Vorlage: `templates/track-validation.md`.

---

# 9. Extend Gate — harte Sperre

## 9.1 Bedingung

Bei `EXTEND_REQUEST` darf nur dann ein Suno-Extend-Paket entstehen, wenn
`TRACK_STATUS` exakt `MIXXX_READY` oder `EXTEND_MIXXX_READY` ist.

Bei `UNVALIDATED`, `ANALYSIS_PENDING`, `ANALYSIS_REVIEW_REQUIRED` oder
`FL_GRID_VERIFIED`:

- Kein Suno-Extend-Prompt.
- Kein geschätzter Extend-Timestamp.
- Kein behaupteter Downbeat.
- Keine behauptete passende 16-/32-/64-Bar-Stelle.
- Stattdessen exakt diese Ausgabe:

```
### TRACK_STATUS
### EXTEND_BLOCKED
### MISSING_EXTEND_EVIDENCE
### REQUIRED_FL_STUDIO_CHECK
### REQUIRED_MIXXX_CHECK
```

## 9.1a Die fünf Extend-Bedingungen

Der Statuscheck aus 9.1 ist notwendig, aber nicht hinreichend. Ein Extend
entsteht nur, wenn **alle fünf** Bedingungen erfüllt sind. Jede einzelne
nicht erfüllte Bedingung blockiert.

| # | Bedingung | Erfüllt, wenn | Nicht erfüllt → |
|---|---|---|---|
| A | Strukturvertrauen | `structure_confidence >= 0.85` im HPSS-Report ODER der Nutzer bestätigt die Struktur ausdrücklich | blockieren |
| B | Style-String 1:1 | Der Style-String des Quelltracks wurde wortgleich übernommen, ohne Rewrite und ohne Paraphrase | blockieren |
| C | Vocal Anchor | Die erste Gesangszeile des aktuellen Parts ist im Extend-Text wörtlich wiederholt — nur bei Vocal-Extends; bei instrumentalem Extend gilt die Bedingung als nicht anwendbar | blockieren, sofern anwendbar |
| D | Längenfenster | Aktuelle Länge < 4:00 min ODER `[Outro]`-Tag fehlt noch | blockieren |
| E | Menschliche Freigabe | Nutzer hat FL-Studio- **und** Mixxx-Check bestätigt; bis dahin gilt `MIXXX_READY: false` | blockieren |

### Zu Bedingung A — `structure_confidence`

`structure_confidence` ist ein Feld des HPSS-Reports aus
`ThrivenArtConnect/hard-tekk-hpss-pipeline`, das die Zuverlässigkeit der
erkannten Songstruktur als Zahl zwischen 0 und 1 angibt.

Wichtig, damit hier keine Scheinsicherheit entsteht:

- Fehlt das Feld im Report, ist es `null`, oder ist der Report nicht
  lesbar, gilt Bedingung A als **nicht erfüllt**. Nie einen Wert schätzen,
  nie aus `tempo.confidence` oder `beats.confidence` ableiten — das sind
  andere Größen.
- Ein hoher `structure_confidence` ersetzt Bedingung E nicht. A und E sind
  eigenständige UND-Bedingungen: Der Maschinenwert darf die Struktur-Frage
  beantworten, niemals die Frage, ob ein Mensch den Track am Grid geprüft hat.
- Der Schwellwert 0.85 ist eine Projektfestlegung, kein von der Pipeline
  vorgegebener Wert.

### Zu Bedingung B — Style-String 1:1

Suno konditioniert die Fortsetzung auf den Style-String. Wird er
umformuliert, driftet der Extend hörbar vom Quelltrack weg, auch wenn die
Umformulierung inhaltlich dasselbe meint. Deshalb: kopieren, nicht
neu schreiben. Auch nicht "leicht aufräumen".

Soll der Style bewusst geändert werden, ist das kein Extend im Sinne dieses
Skills — dann ist es ein neuer Track oder ein `REPLACEMENT_SECTION`, und der
Nutzer muss die Änderung ausdrücklich anweisen.

### Zu Bedingung C — Vocal Anchor

Bei einem Vocal-Extend wird die erste Gesangszeile des aktuellen Parts
wörtlich an den Anfang des Extend-Textes gesetzt. Sie dient Suno als
Anker für Stimmfarbe und Phrasierung.

Zwei Einschränkungen:

- Der Anker ist eine Wiederholung bereits vorhandener, vom Nutzer
  gelieferter Lyrics. Er ist **keine** Lizenz, neue Lyrics zu erfinden.
- Bei einem instrumentalen Extend — dem Regelfall für DJ-Outro, Drop 2 und
  Break — gibt es keine Gesangszeile. Dann ist Bedingung C nicht anwendbar
  und blockiert nicht. Das ausdrücklich so vermerken, statt sie stillschweigend
  als erfüllt zu behandeln.

### Zu Bedingung D — Längenfenster

Die aktuelle Länge stammt aus der Datei oder aus einer ausdrücklichen
Nutzerangabe, nie aus einer Schätzung. Ist sie unbekannt, gilt D als nicht
erfüllt.

Der Sinn: Ein Track jenseits von 4:00 min mit gesetztem `[Outro]` ist
arrangementseitig abgeschlossen. Ein Extend hängt dort meist einen zweiten
Schluss an, statt den Track zu verlängern.

## 9.1b Ausgabe der Bedingungsprüfung

Jede Extend-Anfrage gibt die Prüfung sichtbar aus, auch wenn sie durchgeht.
So sieht der Nutzer, worauf die Entscheidung beruht:

```
### EXTEND_CONDITION_CHECK

| # | Bedingung | Status | Evidenz |
|---|---|---|---|
| A | structure_confidence >= 0.85 oder menschliche Bestätigung | MET / NOT MET / NOT PROVIDED | <Quelle> |
| B | Style-String 1:1 übernommen | MET / NOT MET | <Quelle> |
| C | Vocal anchor wiederholt | MET / NOT MET / NOT APPLICABLE | <Quelle> |
| D | Länge < 4:00 min oder [Outro] fehlt | MET / NOT MET / NOT PROVIDED | <Quelle> |
| E | FL Studio und Mixxx vom Menschen bestätigt | MET / NOT MET | <Quelle> |
```

Ergebnis:

- **Alle fünf erfüllt** (C darf `NOT APPLICABLE` sein) → `EXTEND_STATUS: EXTEND_ALLOWED`,
  Extend-Paket nach 9.5 erzeugen.
- **Mindestens eine nicht erfüllt** → `EXTEND_STATUS: EXTEND_BLOCKED`,
  Ausgabe nach 9.1, und in `MISSING_EXTEND_EVIDENCE` jede nicht erfüllte
  Bedingung mit Begründung nennen — nicht nur auflisten, sondern sagen,
  welche konkrete Angabe fehlt und woher sie kommt.

`EXTEND_ALLOWED` und `EXTEND_BLOCKED` sind Entscheidungen über eine einzelne
Extend-Anfrage, keine Track-Stati. Der `TRACK_STATUS` aus Abschnitt 3 bleibt
davon unberührt: Er ändert sich erst durch neue menschliche Evidenz, nie
dadurch, dass eine Extend-Prüfung durchgeht.

## 9.2 Pflichtdaten für Extend

Diese Werte müssen explizit vom Nutzer geliefert oder in einer bestätigten
Track-Validation-Datei dokumentiert sein:

- `TRACK_TITLE`
- `TRACK_VERSION`
- `SOURCE_WAV_FILENAME`
- `TRACK_STATUS`: `MIXXX_READY` oder `EXTEND_MIXXX_READY`
- `TARGET_BPM`
- `ACTUAL_BPM`
- `FL_GRID_VERIFIED: Yes`
- `MIXXX_BEATGRID_VERIFIED: Yes`
- C1 bis C7 mit Cue-ID, Cue-Name, realer FL-Position `BAR:BEAT`, realer
  Zeit `MM:SS.mmm` und Verifizierungsstatus.
- gewünschtes Extend-Ziel: `DJ_OUTRO` | `DROP_2` | `BREAK` |
  `REPLACEMENT_SECTION`
- Referenz-Cue für den Extend-Start.

Fehlt irgendein Wert:

- Extend blockieren.
- Fehlende Werte konkret tabellarisch aufführen.
- Keine Werte ergänzen oder schätzen.

Eingabevorlage: `templates/extend-validation-input.yaml`.

## 9.3 Extend-Startpunkt-Regel

Wenn alle Daten vorliegen:

- Nur den bestätigten Cue als musikalische Referenz verwenden.
- Der Nutzer setzt den weißen Suno-Extend-Pfeil selbst auf bzw. minimal
  vor den bestätigten Timestamp.
- Nie behaupten, Suno verlängere automatisch beatgenau an diesem Punkt.
- Der echte Downbeat stammt aus FL Studio/Mixxx, nicht aus einer
  unbestätigten Suno-Timeline.
- Der Nutzer hört die Naht in Suno vor dem Generieren ab.

## 9.4 Erlaubte Extend-Ziele

### A. DJ_OUTRO

- Nur mit bestätigt dokumentiertem C7 OUTRO_START.
- Instrumental.
- Kick, controlled rumble/sub, closed hats, sparse percussion.
- Lead, Hook, Vocals und neue Melodie schrittweise entfernen.
- `approximately 32-bar DJ outro target`.
- Kein Tempo-Wechsel, keine Half-Time, keine neue Vocal-Line.

### B. DROP_2

- Nur mit bestätigt dokumentiertem C6 DROP_2.
- Peak-Energy Hard-Tekk-Abschnitt.
- Instrumental, falls kein neuer expliziter Hook geliefert wurde.
- `approximately 32- or 64-bar drop target`.
- Keine zusätzlichen Vocals, keine Genrewechsel.

### C. BREAK

- Nur mit bestätigt dokumentiertem C5 BREAK_START.
- Dunkler tension break, gefilterter Bass, FX, danach Build.
- `approximately 16-bar break target`.
- Keine Stimmen ohne explizite Nutzerfreigabe.

### D. REPLACEMENT_SECTION

- Nur wenn der Nutzer einen bestätigten Start-Cue UND End-Cue nennt.
- Alle nach dem Ersatz liegenden alten Cue-Zeitwerte werden ungültig.
- Nach `Get Whole Song` komplette neue FL-/Mixxx-Prüfung erzwingen.

## 9.5 Ausgabe bei erlaubtem Extend

Wenn alle Daten vollständig und bestätigt sind, exakt:

```
### TRACK_STATUS
### CONFIRMED_CONTEXT
### SUNO_EXTEND_STYLE
### SUNO_EXTEND_LYRICS_OR_CUES
### SUNO_UI_INSTRUCTIONS
### POST_EXTEND_QC
### UPDATED_TRACK_VALIDATION_TEMPLATE
```

### CONFIRMED_CONTEXT

- Trackname
- Version
- Quell-WAV
- bestätigtes reales BPM
- Status
- Extend-Ziel
- Referenz-Cue
- bestätigte FL-Bar:Beat-Position
- bestätigte Zeit
- klarer Hinweis: "Die Zeit stammt aus manueller FL-/Mixxx-Prüfung und ist
  kein automatisch beatgenauer Suno-Marker."

### SUNO_EXTEND_STYLE

- Englisch, kompakt.
- Nur die Fortsetzung der gewünschten nächsten Phase.
- Gleicher Hard-Tekk/RaveTekk/Psy-Tekk-Kerncharakter.
- Taktziele nur als "aim for", "target" oder "approximately".
- Keine Zusage auf taktgenaue Suno-Ausgabe.

### SUNO_EXTEND_LYRICS_OR_CUES

- Bei instrumentalem Extend: nur `[DJ Outro]`, `[Drop]`, `[Break]`,
  `[Build]`, `[End]` und Kurzregie.
- Bei Vocal-Extend: nur neue, explizit vom Nutzer gelieferte Lyrics.
- Keine alten Lyrics oder Hooks wiederholen, außer der Nutzer fordert
  dies ausdrücklich.

### SUNO_UI_INSTRUCTIONS

Deutsch, als Schrittfolge:

1. Song öffnen.
2. `(...)` auswählen.
3. Remix/Edit auswählen.
4. Extend auswählen.
5. Weißen Pfeil manuell auf oder minimal vor `<CONFIRMED_TIMESTAMP>` setzen.
6. Übergang probehören.
7. Style und Cues einfügen.
8. Mindestens zwei Varianten erzeugen und Naht, Kick, Bass und Übergang
   bewerten.
9. Gewählte Variante über `(...) → Create → Get Whole Song` zusammensetzen.
10. Neue Datei als neue Version exportieren, nicht die Quell-WAV überschreiben.

### POST_EXTEND_QC

- Status nach Erzeugung: `EXTEND_RENDERED`.
- Neue Whole-Song-WAV in FL Studio Desktop importieren.
- Downbeat, reales Tempo, Grid, ursprüngliche Abschnitte, Extend-Naht und
  neuen Abschnitt erneut prüfen.
- In Mixxx neu analysieren.
- Beatgrid und C1–C7 vollständig neu kontrollieren.
- C8 EXTEND_SEAM optional setzen.
- Erst nach vollständiger Bestätigung: `EXTEND_MIXXX_READY`.

### UPDATED_TRACK_VALIDATION_TEMPLATE

Frische Kopie der Tabelle aus Abschnitt 8, alle Ist-Werte zurück auf
`NOT YET VERIFIED`, weil der Extend die alten Zeitachsen verschiebt.

---

# 10. HPSS-Pipeline-Integration

## Referenz-Repository

Die optionale Audioanalyse-Quelle ist:

```
ThrivenArtConnect/hard-tekk-hpss-pipeline
```

Dieses Repository ist die autoritative Quelle für:

- HPSS-Verarbeitung,
- Audio-Feature-Extraktion,
- Tempo-/Beat-/Onset-Kandidaten,
- Harmonic-/Percussive-Energie-Daten,
- Loop-Naht-Kandidaten,
- Loudness-, Peak- und Clipping-Kandidaten,
- Analyse-Reports.

Dieser Skill implementiert **kein** HPSS, keine Audio-DSP und keine eigene
BPM-/Downbeat-Schätzung.

## Grundsatz

Die HPSS-Pipeline liefert ausschließlich unverifizierte Analyse-Kandidaten.

Sie darf niemals automatisch setzen:

- `FL_GRID_VERIFIED`
- `MIXXX_READY`
- `EXTEND_MIXXX_READY`
- bestätigte BPM
- bestätigte Downbeats
- bestätigte Songphasen
- bestätigte Cue-Positionen
- bestätigte Loop-Naht
- Suno-Extend-Freigabe

Maximal zulässiger automatischer Status: `ANALYSIS_REVIEW_REQUIRED`.

## HPSS_STATUS — eigene Statusachse für Stems

`HPSS_STATUS` beschreibt den Zustand der **Stems** aus der Pipeline.
`TRACK_STATUS` aus Abschnitt 3 beschreibt den Zustand des **Songs**. Beide
laufen getrennt und dürfen nie gegeneinander verrechnet werden: geprüfte
Stems machen keinen geprüften Song, und ein `MIXXX_READY`-Song sagt nichts
über die Stems.

Um die Verwechslung im Text zu vermeiden, wird der Stem-Status immer mit
Präfix geschrieben: `HPSS_STATUS: MIXXX_READY`, nie bloß `MIXXX_READY`.

### HPSS_STATUS: CANDIDATE

Der Startzustand jedes Reports und jedes Stem-Satzes. Alle Werte sind
Kandidaten, nichts ist bestätigt. Bleibt so, bis alle drei Checks unten
erfüllt sind.

### HPSS_STATUS: MIXXX_READY

Nur erreichbar, wenn **alle drei** Checks erfüllt und vom Nutzer bestätigt sind:

| # | Check | Erfüllt, wenn |
|---|---|---|
| 1 | FL Studio | Stems als 32-bit / 48 kHz exportiert und in der Playlist geladen |
| 2 | Mixxx | Stems importiert, BPM- und Key-Sync geprüft, keine Phasenprobleme |
| 3 | Mensch | Nutzer bestätigt beide Checks ausdrücklich |

Check 3 ist keine Formalie, sondern die eigentliche Bedingung: Der Skill
sieht die Stems nicht und hört sie nicht. Ohne die ausdrückliche Bestätigung
bleibt der Status `CANDIDATE`, auch wenn der Nutzer Check 1 und 2 beschreibt.
Ein HPSS-Report allein hebt den Status nie an.

### HPSS_STATUS: FAILED

Zu setzen, sobald einer dieser Befunde vorliegt:

| Befund | Kriterium |
|---|---|
| Vocal-Bleed im Drum-Stem | > 5 % — objektiv gemessen oder vom Nutzer klar hörbar berichtet |
| Phasenprobleme Kick/Bass | Kick verliert im Zusammenspiel mit dem Bass hörbar Punch |

Zur 5-%-Schwelle: Sie ist eine Projektfestlegung. Der Skill misst nichts
selbst — er übernimmt entweder einen gemessenen Wert aus dem Report oder die
Höreinschätzung des Nutzers. Liegt keins von beidem vor, ist der Befund
`NOT ASSESSED`, nicht "unter der Schwelle".

`FAILED` ist kein Endzustand: Nach einem neuen Split oder korrigierten
Export beginnt die Prüfung wieder bei `CANDIDATE`.

### Was HPSS_STATUS niemals darf

- Sich selbst von `CANDIDATE` hochsetzen.
- `TRACK_STATUS` verändern — auch `HPSS_STATUS: MIXXX_READY` gibt kein
  Extend frei und erfüllt Bedingung E aus 9.1a nicht.
- Als "final", "fertig" oder "abgenommen" bezeichnet werden, solange die
  menschliche Bestätigung fehlt.

## Verhalten bei vorliegendem HPSS-Report

Wenn ein valider HPSS-JSON-Report existiert:

1. `TRACK_STATUS` maximal auf `ANALYSIS_REVIEW_REQUIRED` setzen.
2. Gemeldete Werte nur als Pipeline candidate, mit Confidence und dem
   Vermerk `NOT HUMAN VERIFIED` übernehmen.
3. Kandidaten niemals blind in `Actual FL bar:beat`, `Actual time`,
   `FL verified` oder `Mixxx verified` schreiben.
4. Stattdessen eine zusätzliche Tabelle erzeugen:

| Pipeline candidate | Timestamp | Confidence | Candidate interpretation | Human FL check | Human Mixxx check |
|---|---:|---:|---|---|---|

5. Eine kurze FL-Studio-Prüfliste erzeugen, die besonders die
   Pipeline-Kandidaten am Audio prüft.
6. Eine Mixxx-Prüfliste erzeugen, die die Kandidaten an weit
   auseinanderliegenden Songstellen gegen das Beatgrid prüft.
7. Extend weiterhin blockieren.

Schema-Beispiel: `templates/hpss-analysis-result.example.json`.

## Verhalten bei fehlendem oder fehlerhaftem HPSS-Report

Wenn keine Pipeline vorhanden ist, kein lokaler Report auffindbar ist,
kein valides JSON lesbar ist oder eine Pipeline-Analyse fehlschlägt:

- Keine Werte berechnen oder erfinden.
- Status bleibt `UNVALIDATED`.
- Song- und Sounds-Payloads bleiben verfügbar.
- Extend bleibt blockiert.
- Den lokalen FL-Studio- und Mixxx-Validierungsplan ausgeben.

Auch der Fehlerfall nutzt verbindlich dieselbe Blockstruktur wie eine
erfolgreiche Analyse — sonst sieht jede fehlgeschlagene Prüfung anders aus
und lässt sich weder vergleichen noch maschinell abnehmen:

```
### TRACK_STATUS
### HPSS_ANALYSIS_SUMMARY
### HPSS_CANDIDATE_REVIEW_TABLE
### REQUIRED_FL_STUDIO_CHECK
### REQUIRED_MIXXX_CHECK
### EXTEND_ELIGIBILITY
```

Inhaltliche Vorgaben für diesen Fehlerfall:

- `TRACK_STATUS`: `UNVALIDATED`.
- `HPSS_ANALYSIS_SUMMARY` enthält wörtlich:
  `HPSS analysis unavailable — manual FL Studio validation required.`
  Dazu ein Satz, woran die Analyse gescheitert ist (Datei nicht gefunden,
  JSON nicht parsebar, Pipeline-Lauf fehlgeschlagen) — ohne Spekulation
  über die Audiodatei selbst.
- `HPSS_CANDIDATE_REVIEW_TABLE` behält den Spaltenkopf, enthält aber keine
  erfundenen Kandidaten. Jede Zelle steht auf `NOT YET PROVIDED` oder
  `NOT AVAILABLE`.
- `REQUIRED_FL_STUDIO_CHECK` enthält den lokalen FL-Studio-Validierungsplan
  aus Abschnitt 6.
- `REQUIRED_MIXXX_CHECK` enthält den lokalen Mixxx-Validierungsplan aus
  Abschnitt 7.
- `EXTEND_ELIGIBILITY` lautet wörtlich:
  `EXTEND BLOCKED — pipeline candidates require human FL Studio and Mixxx verification.`

Ausdrücklich nicht erlaubt, auch nicht als Schätzung oder Vorschlag:

- geschätztes BPM,
- bestätigter Downbeat,
- Cue-Zeit oder FL-Bar:Beat-Position,
- Extend-Paket.

## Netzwerk-Regel

Die HPSS-Pipeline darf lokal über eine CLI oder später remote über eine
HTTPS-API verwendet werden. Lokal ist Standard und bevorzugt.

Eine externe API darf nur verwendet werden, wenn alle folgenden Werte
explizit konfiguriert und verfügbar sind:

- `HPSS_API_BASE_URL`
- `HPSS_API_TOKEN`
- erlaubte HTTPS-Domain in der Claude-Code-Cloud-Umgebung
- Nutzerfreigabe für die Übertragung der Audio-Datei oder einer
  signierten Audio-URL

Diese Werte niemals in SKILL.md, README, Markdown-Templates,
Beispieldateien, Git-Commits, Terminal-Logs oder Antworten schreiben oder
ausgeben — hier stehen bewusst nur die Namen, nie Inhalte.

## Cloud-Verhalten

Der Skill erstellt, ändert oder konfiguriert keine Cloud-Umgebung
eigenständig. Er erklärt lediglich:

- **Lokal:** HPSS-CLI/Script auf dem Mac ausführen, JSON-Report im
  Projektordner ablegen, Skill liest den Report.
- **Remote:** Cloud-Umgebung mit Custom Network Access verwenden, nur
  GitHub-Domains und die konkrete HPSS-API-Domain erlauben, Base-URL und
  Token ausschließlich als Environment Variables setzen.

Für den aktuellen Skill sind keine Netzwerk-Umgebungsvariablen erforderlich.

## Erweiterte Ausgabe bei HPSS-Analyse

Bei einer Anfrage mit vorhandenem Analyse-Report genau diese Ausgabe:

```
### TRACK_STATUS
### HPSS_ANALYSIS_SUMMARY
### HPSS_CANDIDATE_REVIEW_TABLE
### REQUIRED_FL_STUDIO_CHECK
### REQUIRED_MIXXX_CHECK
### EXTEND_ELIGIBILITY
```

`EXTEND_ELIGIBILITY` muss bei `ANALYSIS_REVIEW_REQUIRED` immer lauten:

```
EXTEND BLOCKED — pipeline candidates require human FL Studio and Mixxx verification.
```

---

# 11. Pflichtausgaben für jede Anfrage

Wenn Informationen fehlen:

- Nur die minimal nötigen Angaben erfragen.
- `NOT YET VERIFIED` statt Annahmen verwenden.
- Zeitwerte, Tonarten, BPM oder Cues nicht spekulativ füllen.

## Komplette Song-Payload

```
### TRACK_STATUS
### STYLE_PROMPT
### LYRICS_AND_SECTION_CUES
### EXCLUDE_STYLES
### FL_AND_MIXXX_VALIDATION_SHEET
```

## Suno Sound

```
### SOUND_SETTINGS
### SOUND_PROMPT
### LIBRARY_METADATA
### FL_STUDIO_QC
```

## HPSS-Report-Prüfung

Gilt für beide Fälle — gültiger Report und fehlender, unlesbarer oder
fehlgeschlagener Report. Nur die Inhalte unterscheiden sich, die Blöcke nicht.

```
### TRACK_STATUS
### HPSS_ANALYSIS_SUMMARY
### HPSS_CANDIDATE_REVIEW_TABLE
### REQUIRED_FL_STUDIO_CHECK
### REQUIRED_MIXXX_CHECK
### EXTEND_ELIGIBILITY
```

## Extend blockiert

Vorangestellt immer `### EXTEND_CONDITION_CHECK` nach Abschnitt 9.1b.

```
### TRACK_STATUS
### EXTEND_CONDITION_CHECK
### EXTEND_BLOCKED
### MISSING_EXTEND_EVIDENCE
### REQUIRED_FL_STUDIO_CHECK
### REQUIRED_MIXXX_CHECK
```

## Extend erlaubt

Vorangestellt immer `### EXTEND_CONDITION_CHECK` nach Abschnitt 9.1b.

```
### TRACK_STATUS
### EXTEND_CONDITION_CHECK
### CONFIRMED_CONTEXT
### SUNO_EXTEND_STYLE
### SUNO_EXTEND_LYRICS_OR_CUES
### SUNO_UI_INSTRUCTIONS
### POST_EXTEND_QC
### UPDATED_TRACK_VALIDATION_TEMPLATE
```

---

# 12. Mitgelieferte Dateien

| Datei | Zweck |
|---|---|
| `templates/track-validation.md` | Validierungsblatt pro Song |
| `templates/suno-sounds-library-entry.md` | Library-Eintrag pro Sample |
| `templates/extend-validation-input.yaml` | Eingabeformat für Extend-Freigabe |
| `templates/hpss-analysis-result.example.json` | Erwartetes HPSS-Report-Schema |
| `examples/sickarim-hardtekk-song-template.md` | Song-Payload-Beispiel |
| `examples/sickarim-sounds-library-examples.md` | Sechs Library-Beispiele |
| `examples/sickarim-extend-example.md` | Extend blockiert und Extend erlaubt |
| `examples/sickarim-hpss-analysis-review-example.md` | HPSS-Review-Beispiel |
| `tests/acceptance-cases.md` | Abnahmefälle für Regressionsprüfung |

Keine WAV-Dateien, Stems, Binärdateien, Tokens oder Secrets in dieses
Repository einchecken.
