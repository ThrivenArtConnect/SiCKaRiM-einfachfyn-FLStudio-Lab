# Tool-Entscheidungs-Guide

> Vor jedem Schritt diese Tabelle prüfen. Sie bestimmt, welches Tool du nutzt.

## Entscheidungs-Matrix

| Aufgabe | Tool | Warum |
|---|---|---|
| Routing testen, MIDI debuggen, Hardware prüfen, interaktive Fragen | **Claude Chat** | Screenshot-fähig, iterativer Test-Dialog, wartet auf PASS/FAIL |
| Dateien erstellen, Ordner anlegen, Markdown schreiben, Repo-Struktur aufbauen | **Claude Code** (Terminal) | Direkte Dateioperationen, Git-Push, Shell-Befehle |
| Versionierung, Milestone-Tracking, kollaboratives Editieren, PRs | **GitHub** (privat) | Versioniert, für beide Producer zugänglich |
| Spontane Idee, Session-Notiz, Soundkonzept, Lyric-Skizze | **Apple Notes / Craft** | Schnell, offline, mobil |
| Notizen strukturiert ins Repo überführen | **Claude Code** → GitHub-Push | Aus Notiz wird versioniertes Markdown |
| Recherche, Handbuch-Analyse, Prompt-Erstellung | **Perplexity Space** | Quellenbasiert, zitiert, kein Spekulieren |
| Live-Implementierung, FL-Klick-Pfade, MIDI-Plan, Preset-Startwerte | **Claude Chat** (Implementierung) | Nach Perplexity-Recherche ausführen |

## Automatisierungs-Workflow

```
Idee / Session-Notiz
       ↓
  Apple Notes / Craft (mobil / spontan)
       ↓
  Claude Code: Notiz → strukturiertes Markdown
       ↓
  Git Push → GitHub Repo (main oder dev-Branch)
       ↓
  Claude Chat: Perplexity-Ergebnis implementieren
       ↓
  Test → PASS → Merge in main
       ↓
  Milestone als DONE markieren (MILESTONES.md)
```

## Wann welchen Claude-Modus?

| Situation | Modus |
|---|---|
| Screenshot zeigen, MIDI debuggen, Live-Routing | Claude Chat |
| Dateien schreiben, Git-Commits, Ordner, Scripts | Claude Code |
| Recherche, Handbücher, neue Geräte | Perplexity Space |

## Commit-Konventionen

```
feat: neues Feature / Template
fix:  Routing-Fehler behoben
test: Meilenstein-Test dokumentiert
docs: Dokumentation aktualisiert
chore: Struktur, Umbenennung, Cleanup
preset: neuer Channel-/Mixer-State
midi: MIDI-Mapping aktualisiert
hw: Hardware-Setup-Notiz
```

## Branch-Strategie

```
main          → stabiles Template, nur nach PASS-Tests mergen
dev           → aktiver Bau
milestone/M0  → Planung + Richtung
milestone/M1  → FL Studio Base
milestone/M2  → M-Track Duo
milestone/M3  → Input 1 Aufnahme
milestone/M4  → Input 2 + Stereo
milestone/M5  → MPD24 MIDI
milestone/M6  → FPC Drum Bank
milestone/M7  → CS1x MIDI
milestone/M8  → CS1x Audio Return
milestone/M9  → electribe
milestone/M10 → DDJ-FLX10
milestone/M11 → Busse + Sends + Sidechain
milestone/M12 → Resampling + Template Release
```
