# M2 – Mixer-Template-Plan

## Plugin-Inventar (bestätigt)
- FL Studio 2026, Producer Edition v26.1.4, Signature Bundle, Apple Silicon / macOS
- Instrumente: 3xOSC, FLEX, FPC, Patcher
- FX: Maximus, Gross Beat, LFOTool, Fruity Parametric EQ 2, Fruity Multiband Compressor,
  Fruity Soft Clipper, Fruity Limiter, Fruity Blood Overdrive, Fruity Reeverb 2,
  Fruity Delay 3, Transient Processor, Fruity Peak Controller
- Nicht verfügbar: Harmor, Sytrus

## MCP & Tooling
- karl-andres/fl-studio-mcp, Port 8787, Toggle-App
- IAC-Treiber aktiv
- GitHub MCP: ThrivenArtConnect/SiCKaRiM-einfachfyn-FLStudio-Lab + ThrivenArtConnect/fl-studio-mcp-personal
- desktop-commander aktiv

## Repo-Stand (verifiziert, 2026-09-25)
- `00_Docs/ROUTING_LOG.md` existiert, bisher ohne PASS-Eintrag
- `02_Channel_Presets/Kick/` → 7 Dateien für 5 Sound-Konzepte:
  Kick_Sub_808.fst, Kick_Sub_808_v02.fst, Kick_Punch_165_A_Sine.fst,
  Kick_Punch_165_B_Click.fst, Kick_Distorted_Raw.fst, Kick_Tekk_Bounce.fst,
  Kick_Psy_Movement.fst
- `03_Mixer_Presets/Buses/` → 5 Kick-FX-States
- `02_Channel_Presets/Bass/`, `/Leads/`, `/Vocals/` → leer
- `04_FPC_Kits/Samples/` → leer
- `06_Projects/Preset_Lab_M1_Kicks/` → Pattern_1_2.wav, Backup-FLPs

## Channel-Rack (Planungsstand)

| Pos | Name | Plugin | Farbe | Mixer-Ziel |
|---|---|---|---|---|
| 1–5 | Kick-Presets (5 Konzepte, 7 Dateien¹) | 3xOSC | Rot | [KICK] Main Kick |
| 6 | Kick Top/Click | 3xOSC | Rot | [KICK] Top/Click |
| 7 | Sub Bass | 3xOSC | Lila | [BASS] Sub Bass |
| 8 | Mid Bass Distorted | FLEX + Blood Overdrive | Lila | [BASS] Mid Bass |
| 9–12 | Clap/Snare, Hats, Perc, Fills | FPC | Orange | [DRUM]… |
| 13 | Lead | FLEX | Blau | [SYNTH] Lead |
| 14 | Stabs | 3xOSC | Blau | [SYNTH] Stabs |
| 15 | Pads/Atmos | FLEX | Blau | [SYNTH] Pads |
| 16 | Vocal/Chops | Sampler+Edison | Gelb | [VOC] |
| 17 | Risers/FX | FLEX/Gross Beat | Cyan | [FX] |
| 18 | CS1x MIDI Out | MIDI Out | Teal | [HW] CS1x |
| 19 | Electribe MIDI Out | MIDI Out | Teal | [HW] Electribe |

¹ Punch_165 = 2 Layer-Dateien (A_Sine/B_Click), Sub_808 zusätzlich als v02-Variante.

## FX-Chains

| Bus | Chain |
|---|---|
| KICK | Transient Processor → EQ2 → Soft Clipper → Limiter |
| BASS | EQ2 → Multiband Comp → Soft Clipper |
| DRUMS | Transient Processor → EQ2 → Limiter |
| MUSIC | EQ2 → Gross Beat |
| VOCALS | EQ2 → Limiter |
| FX | Gross Beat + LFOTool |
| SEND SC | Peak Controller + LFOTool (Alt) |
| SEND Reverb | Reeverb 2 (Room/Hall) |
| SEND Delay | Delay 3 |
| SEND Parallel | Limiter (COMP aggressiv) |
| PREMASTER | Multiband Comp → EQ2 |
| MASTER | Maximus → Limiter |

Siehe `00_Docs/ROUTING_LOG.md` für PASS-Test-Einträge zu diesem Plan.
