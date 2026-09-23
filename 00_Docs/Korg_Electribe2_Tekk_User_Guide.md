# Korg electribe 2 — Tekk-Guide für Einsteiger

Weitergebbares Handbuch, erzeugt mit der Skill `korg-electribe2-tekk-guide`.
Stand: erste Version, alle Gerätefunktions-Aussagen sind aktuell **Kategorie
C**, siehe Kategorie-Legende unten.

## Kategorie-Legende

- **Kategorie A** — verifiziert aus dem offiziellen Handbuch (mit Seitenangabe
  aus `05_Hardware_Records/Electribe/01_Manuals/electribe2_2s_OM_EFGSCJ3.pdf`)
- **Kategorie B** — praktische Tekk-Workflow-Empfehlung
- **Kategorie C** — am Gerät zu verifizieren, noch offen

## 1. Über dieses Handbuch

Dieses Handbuch begleitet dich beim Einstieg in deine **blaue Korg electribe
2** für Hard-/Rave-/Dark-/Psy-Tekk. Es ist für **Version 1** geschrieben:

- Gerät: blaue Korg electribe 2 — **Synth-Modell**, ausdrücklich **nicht**
  electribe sampler oder electribe 2S.
- Betrieb: vollständig **standalone**, Kopfhörer direkt am Gerät.
- **Nicht enthalten in V1:** MIDI-Mappings, MIDI-Noten/CCs/Ports/Kanäle,
  FL-Studio-Integration, Clock-Sync, Sampler-Funktionen.

Das offizielle Korg-Handbuch liegt im Projekt unter
`05_Hardware_Records/Electribe/01_Manuals/electribe2_2s_OM_EFGSCJ3.pdf` vor.
Es behandelt sowohl die electribe (Synth) als auch die electribe sampler in
einem Dokument — bislang wurde es noch nicht seitenweise ausgewertet, daher
sind alle Bedienschritte unten als Kategorie C markiert. Eine gezielte
Handbuch-Auswertung ist als Folge-Task geplant.

## 2. Dein Setup

- Blaue Korg electribe 2, direkt am Gerät mit Kopfhörer betrieben.
- Kein MIDI-Kabel, kein USB, keine Verbindung zu FL Studio in dieser Version.
- Stromversorgung gemäß Gerät anschließen, Kopfhörer in den dafür
  vorgesehenen Ausgang stecken.

## 3. Bedienoberfläche — Grundlagen

⚠️ Alle Punkte in diesem Kapitel sind **Kategorie C**, bis eine
Handbuch-Auswertung stattgefunden hat. Details siehe
`.claude/skills/korg-electribe2-tekk-guide/references/electribe-basics.md`.

- Sequencer / Pattern-Grundlagen: neues Pattern anlegen, Tempo einstellen,
  zwischen Patterns wechseln
- Part-Auswahl: welcher Part gerade bearbeitet wird
- Level: Lautstärke pro Part
- Filter: Cutoff und Resonance
- Master FX: Effekte auf das Gesamtsignal
- Motion Sequence: aufgezeichnete Parameter-Automation
- Write: Speichervorgang und dessen Bestätigungsanzeige

Bei Unklarheit: Foto vom Display oder von der Bedienoberfläche machen, statt
zu raten.

## 4. Dein erstes Pattern bei 165 BPM

Kategorie B (Workflow-Empfehlung). Konkrete Tastenwege pro Schritt: siehe
Kapitel 3 bzw. `references/electribe-basics.md` (Kategorie C).

1. Leeres Pattern wählen, Tempo auf **165 BPM** setzen.
2. Kick-Part programmieren — einfacher, durchgehender Grundpuls.
3. Bass-Part programmieren — passend zur Kick, z. B. Rolling- oder
   Sub-Charakter.
4. Percussion-Layer ergänzen — zusätzlicher Groove, ohne die Kick zuzudecken.
5. Rave-/Noise-Akzent hinzufügen — z. B. an einer Break- oder Fill-Stelle.
6. Variation erzeugen — genauer Mechanismus noch Kategorie C, am Gerät zu
   prüfen.
7. **Speichern (Write).** Siehe Kapitel 6 — vor jedem Verlassen des Patterns
   prüfen, ob gespeichert wurde.

## 5. Sounddesign-Grundlagen

Konzepte sind Kategorie B (allgemeines Synthesewissen), exakte Bedienung an
der electribe 2 ist Kategorie C.

- **Part** — eine einzelne Klangquelle im Pattern (z. B. Kick, Bass).
- **Level** — Lautstärke eines Parts; zu viele laute Parts gleichzeitig
  können zu Matschigkeit führen.
- **Filter Cutoff** — ab welcher Frequenz der Klang beschnitten wird;
  niedriger klingt tendenziell dumpfer, höher heller.
- **Resonance** — betont Frequenzen um den Cutoff-Punkt; zu hoch kann dünn
  oder instabil klingen.
- **Master FX** — Effekte auf das gesamte Pattern statt auf einen Part.
- **Motion Sequence** — aufgezeichnete Automation eines Parameters über die
  Zeit.

## 6. Speichern & Write-Schutz

- **Vor jedem Patternwechsel:** Hast du Write ausgeführt? Falls nicht, tu es
  jetzt — sonst gehen deine Änderungen verloren.
- **Vor dem Ausschalten:** Warte, bis ein laufender Write-Vorgang sicher
  abgeschlossen ist. Schalte das Gerät nicht während des Speicherns aus.
- Die genaue Anzeige für „Speichern läuft" vs. „Speichern abgeschlossen" ist
  Kategorie C — bitte am Gerät beobachten und ggf. notieren.

## 7. Fehlersuche

| Symptom | Verdacht (Kategorie B) | Prüfschritt (Kategorie C) |
|---|---|---|
| Kein Ton | Part stummgeschaltet, Level auf 0, Kopfhörer nicht eingesteckt, falscher Part ausgewählt | Am Gerät prüfen, wie Mute/Level pro Part angezeigt wird |
| Zu leise | Level zu niedrig eingestellt | Am Gerät prüfen, wo Gesamt-/Kopfhörerpegel eingestellt wird |
| Matschig / undefiniert | Zu viele Parts mit ähnlichem Frequenzbereich, Cutoff zu niedrig, Resonance zu hoch | Am Gerät Filter-Einstellungen pro Part vergleichen |
| Pattern nicht gespeichert | Write vor Wechsel/Ausschalten nicht abgeschlossen | Am Gerät prüfen, welche Anzeige Write-Status zeigt |
| Motion Sequence zu chaotisch | Zu viele Parameter gleichzeitig automatisiert | Am Gerät prüfen, wie eine einzelne Motion Sequence gezielt gelöscht/überschrieben wird |

## 8. Pattern-Blätter

Für jedes Pattern eine Kopie von
`.claude/skills/korg-electribe2-tekk-guide/templates/pattern-sheet-template.md`
ausfüllen (Tempo, Pattern-Slot, Level/Cutoff/Resonance/Motion pro Part,
Notizen).

## 9. Foto-/Screenshot-Checkliste

Für die spätere Handbuch-Auswertung und Kategorie-A-Aufwertung sind folgende
Fotos hilfreich:

- [ ] Display bei Tempo-Einstellung
- [ ] Part-Auswahl-Ansicht
- [ ] Filter/Resonance-Bereich
- [ ] Master-FX-Bereich
- [ ] Motion-Sequence-Aktivierung
- [ ] Write-Bestätigungsanzeige

Keine Fotos/Screenshots direkt in dieses Repository committen — nur die
Erkenntnisse daraus in Textform in dieses Handbuch bzw. in
`references/electribe-basics.md` übertragen.

## 10. Offene Punkte (Kategorie C)

Alle in diesem Handbuch als Kategorie C markierten Punkte, insbesondere:

- Exakte Bedienschritte für Sequencer, Part-Auswahl, Level, Filter, Master
  FX, Motion Sequence, Write
- Mechanismus für „Variation" in Kapitel 4, Schritt 6
- Anzeige für laufenden vs. abgeschlossenen Write-Vorgang
- Zuordnung jeder künftigen Handbuch-Stelle zu electribe (Synth) vs.
  electribe sampler, bevor sie als Kategorie A übernommen wird
