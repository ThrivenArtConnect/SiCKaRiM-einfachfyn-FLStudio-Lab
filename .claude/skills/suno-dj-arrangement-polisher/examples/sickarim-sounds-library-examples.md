# Beispiele — Suno Sounds Library (SiCKaRiM)

Sechs copy-paste-fähige Library-Anfragen. Pro Beispiel genau ein Sound-Typ.
Alle Einträge starten auf `STATUS: UNVALIDATED`; der Status steigt erst
nach FL-Studio-QC und ausdrücklicher Bestätigung.

Die Taktangaben in den Prompts sind Ziele. Ob ein Loop wirklich sauber
rundläuft, entscheidet der Wiederholungstest in FL Studio.

---

## 1. Dark Hard-Tekk Percussion Loop

### SOUND_SETTINGS

```
Type: Loop
BPM: 160
Key: Any
```

### SOUND_PROMPT

```text
Dark underground hard tekk percussion loop, 160 BPM target, straight 4/4 target, seamless loop, one-bar loop target. Tight dry rave percussion only: short closed hats, offbeat open hat accents, dry rimshot, metallic industrial tick, subtle shaker. Raw analog character, light saturation, tight transients, small dry room, no long reverb tail, no kick drum, no bassline, no sub, no vocals, no melody, no chords, no pads.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_LOOP_PERC_DarkRave_160_ANY_001
TYPE: LOOP
ROLE: PERC
TARGET_BPM: 160
TARGET_KEY: ANY
TARGET_LENGTH: ONE_BAR
PROMPT_VERSION: 001
FILENAME: SKM_LOOP_PERC_DarkRave_160_ANY_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Am ersten sinnvollen Transient sauber geschnitten
- [ ] In 160-BPM-Projekt importiert
- [ ] Mindestens acht direkte Wiederholungen getestet
- [ ] Loop-Naht klickfrei, kein Timing-Versatz, keine springende Hallfahne
- [ ] Kein Kick- und kein Bassanteil vorhanden (soll mit eigenem Kick-/Sub-System laufen)
- [ ] Einsatzbereich dokumentiert
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung

---

## 2. Dark Rave-Tekk Stab Loop

### SOUND_SETTINGS

```
Type: Loop
BPM: 160
Key: F minor
```

### SOUND_PROMPT

```text
Dark rave tekk stab loop in F minor, 160 BPM target, straight 4/4 target, seamless loop, one-bar loop target. Short aggressive industrial rave stabs, detuned saw and hoover character, hard filter bite, gritty saturation, tight gated release, dark and mid-forward. Rhythmic stab pattern only, no long melody line. No drums, no kick, no hats, no percussion, no bassline, no sub, no vocals, no pads.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_LOOP_STAB_RaveTekk_160_FMIN_001
TYPE: LOOP
ROLE: STAB
TARGET_BPM: 160
TARGET_KEY: FMIN
TARGET_LENGTH: ONE_BAR
PROMPT_VERSION: 001
FILENAME: SKM_LOOP_STAB_RaveTekk_160_FMIN_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Am ersten sinnvollen Transient sauber geschnitten
- [ ] In 160-BPM-Projekt importiert
- [ ] Mindestens acht direkte Wiederholungen getestet
- [ ] Loop-Naht klickfrei, Timing stabil
- [ ] Tonart gegen Projekt geprüft (Ziel F-Moll)
- [ ] Keine Drums, kein Kick, kein Bass im Sample
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung

---

## 3. Psy-Tekk Motion Lead Loop

### SOUND_SETTINGS

```
Type: Loop
BPM: 160
Key: F minor
```

### SOUND_PROMPT

```text
Dark psy-tekk motion lead loop in F minor, 160 BPM target, straight 4/4 target, seamless loop, two-bar loop target. Hypnotic repetitive psy movement, narrow acid-style filter motion, subtle pitch modulation, controlled resonance, dry aggressive tone with light delay. Short repeating motif only, no long melodic phrase, no chord progression. No drums, no kick, no percussion, no bassline, no sub, no vocals.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_LOOP_LEAD_PsyMotion_160_FMIN_001
TYPE: LOOP
ROLE: LEAD
TARGET_BPM: 160
TARGET_KEY: FMIN
TARGET_LENGTH: TWO_BAR
PROMPT_VERSION: 001
FILENAME: SKM_LOOP_LEAD_PsyMotion_160_FMIN_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Am ersten sinnvollen Transient sauber geschnitten
- [ ] In 160-BPM-Projekt importiert
- [ ] Mindestens acht direkte Wiederholungen getestet
- [ ] Loop-Naht klickfrei, kein hörbarer Sprung in der Filterbewegung
- [ ] Tonart gegen Projekt geprüft (Ziel F-Moll)
- [ ] Keine Drums und kein Bassanteil im Sample
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung

---

## 4. Hard-Tekk Drop Impact

### SOUND_SETTINGS

```
Type: One-Shot
BPM: Auto
Key: Any
```

### SOUND_PROMPT

```text
Hard tekk drop impact one-shot, single hit, dark and heavy. Distorted low boom with tight sub drop, layered metallic industrial crash, short compressed tail, aggressive saturation, club-ready weight. Sharp attack, controlled decay. No vocals, no melody, no chords, no drum loop, no rhythmic pattern, no music bed.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_ONESHOT_FX_DropImpact_AUTO_ANY_001
TYPE: ONE_SHOT
ROLE: FX
TARGET_BPM: AUTO
TARGET_KEY: ANY
TARGET_LENGTH: SHORT_ONE_SHOT
PROMPT_VERSION: 001
FILENAME: SKM_ONESHOT_FX_DropImpact_AUTO_ANY_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Start am gewünschten Transient geschnitten
- [ ] Fade-In/Fade-Out gegen Klicks geprüft
- [ ] Lautheit gegen Referenz-Kick geprüft
- [ ] Keine störende Hintergrundmusik im Sample
- [ ] Sub-Anteil kontrolliert, bei Bedarf Hochpass
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung

---

## 5. Dark Psy Reverse Riser

### SOUND_SETTINGS

```
Type: One-Shot
BPM: Auto
Key: Any
```

### SOUND_PROMPT

```text
Dark psy reverse riser one-shot, single rising transition effect. Reversed noise sweep with resonant filter motion, metallic psy texture, rising tension, gritty saturation, abrupt cut at the peak. Dry and mid-forward, no long reverb wash. No vocals, no melody, no chords, no drum loop, no rhythmic pattern, no music bed.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_ONESHOT_FX_ReverseRiser_AUTO_ANY_001
TYPE: ONE_SHOT
ROLE: FX
TARGET_BPM: AUTO
TARGET_KEY: ANY
TARGET_LENGTH: SHORT_ONE_SHOT
PROMPT_VERSION: 001
FILENAME: SKM_ONESHOT_FX_ReverseRiser_AUTO_ANY_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Ende exakt am gewünschten Peak geschnitten
- [ ] Fade-In/Fade-Out gegen Klicks geprüft
- [ ] Lautheitsverlauf gegen Build-Referenz geprüft
- [ ] Keine störende Hintergrundmusik im Sample
- [ ] Position vor dem Drop im Projekt getestet
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung

---

## 6. German Hard-Tekk Vocal Shout

Nur die vom Nutzer gelieferte Phrase: `KEIN SCHLAF!`
Keine weiteren Lyrics ergänzen. Ein Library-Shout hebt die
"ein Hook only"-Regel für vollständige Songs nicht auf.

### SOUND_SETTINGS

```
Type: One-Shot
BPM: Auto
Key: Any
```

### SOUND_PROMPT

```text
Single German rave vocal shout, one short phrase only: "KEIN SCHLAF!". Dark, raw, forceful, dry close vocal, subtle saturation, short reverb tail. Aggressive underground rave delivery, shouted not sung, no singing, no rap, no backing vocals, no music.
```

### LIBRARY_METADATA

```
SAMPLE_ID: SKM_ONESHOT_VOX_KeinSchlaf_AUTO_ANY_001
TYPE: ONE_SHOT
ROLE: VOX
TARGET_BPM: AUTO
TARGET_KEY: ANY
TARGET_LENGTH: SHORT_ONE_SHOT
PROMPT_VERSION: 001
FILENAME: SKM_ONESHOT_VOX_KeinSchlaf_AUTO_ANY_001.wav
STATUS: UNVALIDATED
```

### FL_STUDIO_QC

- [ ] Start am ersten Konsonanten geschnitten
- [ ] Fade-In/Fade-Out gegen Klicks geprüft
- [ ] Verständlichkeit der Phrase geprüft
- [ ] Keine ungewollten Doppelstimmen oder Backing Vocals
- [ ] Hallfahne geprüft, bei Bedarf gekürzt
- [ ] Keine Musik im Hintergrund des Samples
- [ ] Timing im Projekt gegen den Downbeat getestet
- [ ] Status bleibt UNVALIDATED bis zur ausdrücklichen Bestätigung
