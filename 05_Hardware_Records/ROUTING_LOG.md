# ROUTING_LOG — SiCKaRiM Studio Setup

> Repo: SiCKaRiM-einfachfyn-FLStudio-Lab  
> Ordner: 05_Hardware_Records  
> Stand: 22.08.2026  

---

## M0 — Planung und Sicherheitscheck

- **Datum:** 2026-08-22
- **Status:** ✅ PASS
- **Kernstil:** Hybrid (Bouncy Tekk + Hard Techno + Spoken Word)
- **BPM:** 165
- **Key:** D Minor (Default)
- **Vocals:** DE/EN/zweisprachig + Spoken Word + instrumental
- **Referenztracks:** keine — eigener Stil
- **Mix-Ziel:** Club + DJ-Set + Kopfhörer
- **Clock Master:** FL Studio
- **VERIFY offen:** V1 (Kopfhörer), V2 (DDJ rekordbox), V3 (electribe Kabel)
- **Bootleg-Projekt:** Ben Dust "Homeless" — separates Lernprojekt
- **Netzteil-Warnungen:** CS1x=DC, DX-50=AC — dokumentiert
- **Adapter-Engpass:** QinHeng CH345 — 1 Port-Paar, CS1x exklusiv, MPD24 über USB

---

## Verifizierte Geräteinventur — Stand 22.08.2026

### Audio-Signalwege

| Gerät | Kabel | Von → Nach | Status | Notiz |
|---|---|---|---|---|
| M-Track Duo | USB-B → USB-A | M-Track → Mac Mini M4 | ✅ | Audio Interface, Bus-Powered |
| CS1x Audio | K13: 2× 6,3mm TS → Cinch RCA | CS1x OUT L+R → M-Track Input 1+2 | ✅ NEU | LINE-Schalter ✅ · 48V AUS ✅ · beschafft 22.08.2026 |
| M-Track Main Out | 6,3mm TRS → Cinch RCA | M-Track → DX-50 BT | ✅ | Nur Abhöre — NICHT im FL-Routing |
| DX-50 → Boxen | DX-50 Master → Monitoring-Kette | Abhöre | ✅ | DX-50 = 12V AC! |
| Kopfhörer | M-Track Duo Kopfhörerausgang | Abhöre | ⚠️ VERIFY | Nicht bestätigt ob aktiv genutzt |

### MIDI-Signalwege

| Gerät | Kabel | Von → Nach | Status | Notiz |
|---|---|---|---|---|
| MPD24 (primär) | USB-B → USB-A | MPD24 → Mac Mini M4 | ✅ | Empfohlener Weg, Class-Compliant |
| MPD24 (Backup) | 2× MIDI DIN 5-pol (KLANG High Quali) | MPD24 DIN → QinHeng Adapter | ✅ vorhanden | ⚠️ NICHT gleichzeitig mit CS1x nutzbar! |
| CS1x MIDI | K14: USB-MIDI (feste DIN-Stecker) | CS1x OUT → Adapter IN / Adapter OUT → CS1x IN | ✅ | QinHeng CH345, nur 1 Port-Paar |
| DDJ-FLX10 | USB-C → USB-C | DDJ-FLX10 → Mac Mini M4 | ✅ Kabel da | ⚠️ VERIFY rekordbox |

### Adapter-Engpass (kritisch)

```
QinHeng CH345 USB-MIDI Adapter (USB2.0-MIDI)
USB-ID: 0x1a86 / 0x752d | Standort-ID: 0x02124000 | 12 Mb/s
├── 1× MIDI IN  ← CS1x MIDI OUT  ✅ (eingesteckt, aktiv)
├── 1× MIDI OUT → CS1x MIDI IN   ✅ (eingesteckt, aktiv)
└── NUR 1 PORT-PAAR!
    → MPD24 DIN und CS1x NICHT gleichzeitig möglich!
    → MPD24 primär über USB-B (kein Konflikt)
    → DIN-Kabel (KLANG) = Backup
```

### Geräte ohne Verbindung (ausstehend)

| Gerät | Audio | MIDI | USB | Status |
|---|---|---|---|---|
| Korg electribe 2 (blau) | ⚠️ kein Kabel | ⚠️ kein Kabel vor Ort | ⚠️ VERIFY | Nicht verbunden — nächste Session klären (M9) |

### Netzteile — Sicherheitszone

| Gerät | Netzteil | Typ | Warnung |
|---|---|---|---|
| Yamaha CS1x | PA-3B | **12V DC** | ⚡ NIEMALS AC-Netzteil! |
| Pronomic DX-50 BT | Mitgeliefert | **12V AC** | ⚡ NIEMALS DC-Netzteil! |
| M-Track Duo | keins | USB Bus-Powered | — |
| MPD24 | keins | USB Bus-Powered | — |

### Clock-Master-Festlegung

| Rolle | Gerät | Begründung |
|---|---|---|
| **Clock Master** | **FL Studio** | DAW gibt Tempo vor, alle Hardware folgt |
| Clock Slave | Korg electribe 2 | Erst nach Verbindung in M9 aktivieren |
| MIDI-gesteuert | Yamaha CS1x | Empfängt Noten/CC von FL Studio, keine Clock-Rolle |
| Unabhängig | MPD24 | Sendet nur MIDI Notes/CC, keine Clock-Rolle |
| Unabhängig | DDJ-FLX10 | rekordbox/Serato — eigene Clock, kein FL-Sync |

---

## VERIFY-Liste — offen

| # | Was | Wie klären | Priorität | Meilenstein |
|---|---|---|---|---|
| V1 | Kopfhörer an M-Track Duo — aktiv genutzt? | Kopfhörer einstecken, FL-Signal testen | Niedrig | M2 |
| V2 | DDJ-FLX10 rekordbox — eingerichtet? | rekordbox öffnen, DDJ verbinden, prüfen | Niedrig | M10 |
| V3 | electribe 2 — Kabel, USB-MIDI, Audio | Nächste Session: Micro-USB + Audio-Kabel | Niedrig | M9 |

---

## Referenzdokumente

- `SiCKaRiM_Studio_App_v2.0_FLStudio.html` — Studio App (Perplexity Space + Projektordner)
- `Yamaha_CS1x_Integration_LLM-Referenz_v2.md` — CS1x MIDI/Audio Specs
- `SiCKaRiM_Studio_Handbuch_v2_CS1x.md` — vollständiges Studio Handbuch
## Session: M1 – Kick Channel Presets (5/5 abgeschlossen)

---

### ✅ M1 (1/5): Kick_Sub_808

- Engine: 3xOSC, OSC1 Sine, GROB -5 / FEIN +35 (Note C3/36 → 50 Hz), OSC2/OSC3 MIX 0
- Amp-Envelope: ATT 0 / HOLD 0 / DEC 0.4s / SUS 0 / REL 0.05
- Pitch-Envelope: ATT 0 / DEC 0.15s / AMT ~40% (empirisch nach Gehör kalibriert – kein dokumentierter %→Halbton-Faktor in FL vorhanden)
- FX-Kette Insert 1: Fruity Parametric EQ 2 (Band 1, Low Pass, 80 Hz, Breite 61%) → Fruity Soft Clipper
- Routing: Insert 1, Name "Kick_Sub_808", Farbe FF3333
- Gespeichert: 02_Channel_Presets/Kick/Kick_Sub_808.fst
- Getestet: Solo-Loop, 165 BPM, kein Clipping

---

### ✅ Kick_Sub_808_v02 – Optimierung nach Hörtest

- Klangproblem vorher: Mischung aus "zu dumpf" + "zu lang" – nicht schlagartig genug
- Geänderte Parameter:
  - Volume-Envelope DEC: 0.4s → 0.207s (kürzerer Ausklang, mehr Schlag-Charakter)
  - Fruity Parametric EQ 2, Band 1 (LPF): 80 Hz → 110 Hz (mehr Attack-Klarheit)
- Unveränderte Kernparameter: OSC (Sine 50 Hz), Pitch-Envelope (AMT 40%, DEC 0.15s), Soft Clipper, Detune/Phase/HQ (alle 0/aus)
- Test: Solo-Loop bei 165 BPM, schlagartig + klar, kein Clipping
- Gespeichert: 02_Channel_Presets/Kick/Kick_Sub_808_v02.fst (Original Kick_Sub_808.fst bleibt erhalten)

---

### ✅ M1 (2/5): Kick_Punch_165

- Architektur: 2 separate 3xOSC-Channels auf gemeinsamem Insert 2
  - Layer A (Sine-Body): OSC1 Sine, GROB -1 / FEIN -49 (60 Hz), Amp-Envelope ATT 0 / DEC 0.2s / SUS 0, DEC-Spannung 40%, HQ an
    (Kratzen am Ende war reines Live-Preview-Artefakt, im Render verschwunden – unproblematisch)
  - Layer B (Noise-Click): OSC1 Noise, Amp-Envelope ATT 0 / DEC 0.03s / SUS 0, Filter BP, MOD X 0.547 (~55%), MOD Y 0.625 (~63%), VOL ~60%
- FX-Kette Insert 2: Fruity Parametric EQ 2 (Band 2, Peaking, 3000 Hz, +4 dB)
- Gespeichert:
  - 02_Channel_Presets/Kick/Kick_Punch_165_A_Sine.fst
  - 02_Channel_Presets/Kick/Kick_Punch_165_B_Click.fst
  - 03_Mixer_Presets/Buses/Kick_Punch_EQ3k.fst
- Getestet: Solo A, Solo B, Diagnose-Workflow (Reset + Einzelschritte)

---

### ✅ M1 (3/5): Kick_Distorted_Raw

- Engine: 3xOSC, OSC1 Sine GROB -3 / FEIN 0 (55 Hz) + OSC2 Square GROB 7 / FEIN +30 (~99,7 Hz), MIX 50%, OSC3 stumm
- Amp-Envelope: ATT 0 / HOLD 0 / DEC 0.15s / SUS 0 / REL 0.03
- FX-Kette Insert 3: Fruity Fast Dist (Type B, Threshold 70%, Pre 82%, Post 89%) → Fruity Parametric EQ 2 (Band 2: 80 Hz +5dB, Band 3: 400 Hz -6dB)
- Gespeichert:
  - 02_Channel_Presets/Kick/Kick_Distorted_Raw.fst
  - 03_Mixer_Presets/Buses/Kick_DistRaw_FX.fst
- Getestet: Solo-Loop, kein Clipping, unterscheidbar von Sub_808 und Punch_165

---

### ✅ M1 (4/5): Kick_Tekk_Bounce

- Engine: 3xOSC, OSC1 Triangle GROB 0 / FEIN -11 (65 Hz), OSC2/OSC3 stumm
- Amp-Envelope: Standard (kurz)
- Pitch-Envelope: ATT 0.005s / HOLD 0 / DEC 0.08s / SUS 0 / REL 0.03, AMT 58% (positiv, Bounce-Charakter)
- FX-Kette Insert 4: Fruity Blood Overdrive (Farbe 0.40, Pre Amp 0.25, x100 aus, Post Gain 0.0)
- Sidechain-Trigger: zurückgestellt, wird bei M2 (Bass Channel) verdrahtet
- Fix während Session: Insert 3 stand versehentlich auf Solo im Mixer, dadurch Master stumm bei Kanal 1/2/4 solo – behoben
- Gespeichert:
  - 02_Channel_Presets/Kick/Kick_Tekk_Bounce.fst
  - 03_Mixer_Presets/Buses/Kick_TekkBounce_FX.fst
- Getestet: Solo-Loop nach Mixer-Solo-Fix, kein Clipping

---

### ✅ M1 (5/5): Kick_Psy_Movement — MILESTONE M1 KOMPLETT

- Engine: 3xOSC, OSC1 Sine GROB -2 / FEIN -8 (58 Hz), OSC2/OSC3 stumm
- Amp-Envelope: ATT 0 / HOLD 0 / DEC 0.25s / SUS 0 / REL 0.05
- Filter: SVF LP, LFO Sine-Shape, Rate 1/4 Schlag = 1/16-Note (Tempo-Sync), AMT im LFO-Panel + MOD X ~50% (nach Gehör kalibriert, kein dokumentierter %→Hz-Faktor)
- FX-Kette Insert 5:
  - Fruity Delay 3 (Tempo Sync, 1/16 Dotted, Feedback 25%, Wet 15% / Dry 100%, Ping-Pong/Stereo)
  - Fruity Reeverb 2 ("Drum Room"-Preset als Basis, Dec 1.2s, Wet 10% / Dry 100%, Low Cut 100Hz)
- Gespeichert:
  - 02_Channel_Presets/Kick/Kick_Psy_Movement.fst
  - 03_Mixer_Presets/Buses/Kick_PsyMovement_FX.fst
- Getestet: Solo-Loop, LFO-Timing per Piano-Roll-Snap (1/4 Schlag) + Zähl-Methode verifiziert

---

## MILESTONE M1 STATUS: 5/5 Kick-Presets fertig

- [x] Kick_Sub_808 (+ v02 Optimierung)
- [x] Kick_Punch_165
- [x] Kick_Distorted_Raw
- [x] Kick_Tekk_Bounce
- [x] Kick_Psy_Movement

**Nächste:** M2 (Bass Channel Presets) — hier auch der offene Sidechain-Trigger von Kick_Tekk_Bounce verdrahten.
