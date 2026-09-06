# Fehlersuche

Format je Fall: Symptom → möglicher Verdacht (Kategorie B) → Prüfschritt
(überwiegend Kategorie C, da geräteabhängig).

## Kein Ton

- Verdacht (B): Part stummgeschaltet, Level auf 0, Kopfhörer nicht richtig
  eingesteckt, falscher Part ausgewählt.
- Prüfschritt (C): ⚠️ am Gerät zu verifizieren, wie Mute/Level pro Part
  angezeigt und geprüft werden.

## Zu leise

- Verdacht (B): Level einzelner oder aller Parts zu niedrig eingestellt.
- Prüfschritt (C): ⚠️ am Gerät zu verifizieren, wo der Gesamt- bzw.
  Kopfhörerpegel eingestellt wird.

## Matschig / undefiniert

- Verdacht (B): zu viele Parts mit ähnlichem Frequenzbereich und hohem
  Level gleichzeitig, Cutoff zu niedrig, Resonance zu hoch.
- Prüfschritt (C): ⚠️ am Gerät zu verifizieren, wie Filter-Einstellungen pro
  Part angezeigt und verglichen werden können.

## Pattern nicht gespeichert

- Verdacht (B): Write-Vorgang wurde vor dem Patternwechsel/Ausschalten nicht
  abgeschlossen.
- Prüfschritt (C): ⚠️ am Gerät zu verifizieren, welche Anzeige einen
  laufenden bzw. abgeschlossenen Write-Vorgang signalisiert. Siehe auch
  `SKILL.md` Abschnitt 6 (Write-Schutz).

## Motion Sequence zu chaotisch

- Verdacht (B): zu viele Parameter gleichzeitig automatisiert, Aufnahme
  ungewollt über mehrere Takte statt einem klaren Abschnitt.
- Prüfschritt (C): ⚠️ am Gerät zu verifizieren, wie eine einzelne Motion
  Sequence gezielt gelöscht oder überschrieben wird.
