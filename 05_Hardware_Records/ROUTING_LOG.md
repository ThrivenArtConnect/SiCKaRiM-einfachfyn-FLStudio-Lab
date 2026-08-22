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
