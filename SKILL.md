---
name: calma
description: >
  Use this skill for EVERY request related to Calma — a nervous-system calming app
  with breathing exercises, body scan, and grounding, built in standalone HTML,
  deployed to GitHub Pages. Trigger on any mention of: "Calma", "Calma App",
  "Atemübung", "Atem-App", "Nervensystem", "Vagusnerv", "Body Scan", "Grounding",
  "Doppelseufzer", "Kohärenz-Atmung", "Box-Atmung", "Beruhigungs-App",
  "nicolehahn2890.github.io/Calma", or any request to add/fix/style features
  in the Calma app. Also trigger when the user uploads an HTML file related to this app.
  Never skip this skill for Calma work.
---

# Calma — Skill

## Wer ist die Nutzerin?

Nicole — kein Coding-Hintergrund, kein Terminal. Ausschliesslich Claude.ai (Browser/Mobile).
Deutsch, Du-Anrede. Deployment immer ueber GitHub Browser-Interface
(Stift-Symbol, Strg+A, Inhalt ersetzen, Commit).

---

## Was ist die App?

- Live-URL: https://nicolehahn2890.github.io/Calma/
- Repository: github.com/nicolehahn2890/Calma
- Technologie: Standalone HTML-Datei (kein Framework, kein Build-Schritt)
- Dateiname: index.html
- localStorage-Key: calma_v2 — NIEMALS umbenennen!
- Zweck: Beruhigung des Nervensystems durch evidenzbasierte Atem- und Achtsamkeits-Uebungen

---

## Design-System

Hintergrund: #1a1410 (warm-dunkel)
Surface: #2a201a
Cream (Text): #f4ead8
Cream-dim: #c9bfa9
Text-muted: #9a8e76
Primaerfarbe Terracotta: #c97b5b
Terracotta-soft: #a86349
Olive (Sekundaer): #8a956b
Olive-soft: #6b7556
Sand: #d4b896

Fonts:
- Cormorant Garamond (Display, kursiv fuer Akzente)
- DM Sans (Body)
Beide ueber Google Fonts CDN.

Aesthetik: warm-erdig, Spanien-Vibe (Andalusischer Innenhof), nervensystemfreundlich.
KEINE grellen Farben, keine harten Notifications, viel Weissraum.

---

## App-Architektur

### State-Objekt
```
state = {
  streak: 0,           // aktuelle Tage in Folge
  lastDate: null,      // ISO-Date "YYYY-MM-DD"
  totalSessions: 0,    // Gesamt-Sessions
  soundEnabled: false  // Ton an/aus
}
```

### Globale Variablen (let)
```
currentExercise, currentMode, isPlaying, breathTimeout, timerInterval,
elapsedSeconds, cycleCount, wakeLock, bodyscanIndex, bodyscanInterval,
bodyscanPlaying, groundingStep, audioCtx, droneNodes
```

### Wichtige Funktionen
```
showScreen(id)          Versteckt alle direkten #app-Kinder, zeigt nur den gewaehlten
goHome()                Zurueck zum Start, stoppt aktive Uebung, neues Greeting
goBackToMode()          Zurueck zur Mode-Liste (oder Home wenn currentMode null)
showMode(modeKey)       Mode-Detailseite mit Uebungsliste rendern
showRandomGreeting()    Setzt zufaelliges Zitat + Autor

startExercise(key)      Dispatcht je nach type (breath/bodyscan/grounding/static)
stopExercise()          Stoppt alles: Timeouts, Intervals, Drone, Wake Lock, Circle-Klassen
confirmStop()           Zeigt Modal wenn Uebung laeuft, sonst direkt zurueck

runBreathCycle(stepIdx) Rekursiver Atemzyklus mit setTimeout-Chain
toggleBodyscan()        Play/Pause Body Scan
nextGrounding()         Naechster Grounding-Schritt
completeExercise()      Aufraeumen, Streak hoch, Chime, Done-Screen

playTone(f,d,v)         Sinus-Doppelton mit Lowpass (warm)
startDrone(baseFreq)    Sehr leiser Hintergrund-Drone
toggleSound()           Aktiviert/deaktiviert Ton, speichert in state
```

---

## Drei Modi (nach SITUATION sortiert, NICHT nach Zeit)

### 1. quick — "Akut — gestresst, Herz rast"
Dauer: 1–3 Min. Fuer Stress-Peaks.
Uebungen: physiological-sigh, cold-face, box-breathing, 478-breathing

### 2. evening — "Müde & überdreht — komm runter"
Dauer: 5–10 Min. Fuer abends, vor dem Schlafen, Erschoepfung.
Uebungen: extended-exhale, vagus-humming, body-scan

### 3. routine — "Im Kopfkarussell — bin nicht da"
Dauer: 3–5 Min. Fuer Gedankenkreisen, Dissoziation.
Uebungen: grounding, coherence

WICHTIG: Modi sind NACH ZUSTAND sortiert, nicht nach Dauer. Wenn neue Uebungen
hinzukommen, der passenden Situation zuordnen, nicht nach Minuten gruppieren.

---

## Uebungstypen

### type: 'breath' — Atemuebungen mit atmendem Kreis
Pattern-Array mit Phasen:
```
{ phase, duration (ms), label, circle: 'expand'|'contract'|'hold', sound: 'inhale'|'exhale'|'hold'|null }
```
Plus: cycles (Anzahl Wiederholungen), droneFreq, optional circleClass

### type: 'bodyscan' — Geführte Körperreise
30 Sek pro Bereich, 10 Bereiche (Stirn → Ganzer Koerper).
Konstante: BODYSCAN_DURATION_PER_AREA = 30

### type: 'grounding' — 5-4-3-2-1 Sinneswahrnehmung
Manuelle Progression mit "Weiter"-Button.

### type: 'static' — Anleitung ohne Timer
Eigener Screen (z.B. Kalt-Reflex). Nutzer drueckt "Erledigt".

---

## Uebungen im Detail (alle 9, mit wissenschaftlichen Quellen)

### Akut-Modus
- physiological-sigh (Doppelseufzer): 2 Min, Stanford-Studie 2023 (Balban/Huberman/Spiegel,
  Cell Reports Medicine). Wirksamste Sofort-Technik. Pattern: 2.5s ein, 1.2s nach,
  6s aus. 12 Zyklen.
- cold-face (Kalt-Reflex): static. Tauchreflex (Diving Reflex). Richer et al. 2022.
  Anleitung mit 4 Karten, kein Timer.
- box-breathing (Box-Atmung): 3 Min, Navy SEALs. 4-4-4-4. 10 Zyklen.
- 478-breathing: 3 Min, Dr. Andrew Weil. 4 ein, 7 halten, 8 aus. 6 Zyklen.

### Müde-Modus
- extended-exhale (Lange Ausatmung): 6 Min. 4 ein, 8 aus. 25 Zyklen.
- vagus-humming (Summen): 4 Min. 3 ein, 8 aus mit Mmm-Summen. 18 Zyklen.
  Eigener Circle-Style (humming-circle, olive-Gradient).
- body-scan: 5 Min, 10 Bereiche x 30 Sek.

### Kopfkarussell-Modus
- grounding (5-4-3-2-1): 3-5 Min. Schritt-fuer-Schritt durch 5 Sinne.
- coherence (Kohaerenz-Atmung): 5 Min. 5.5 ein, 5.5 aus. 27 Zyklen.
  Laborde et al. 2022 Meta-Analyse (223 Studien) — am besten erforscht.

---

## Audio-System (Web Audio API)

Komplett ohne externe Sounddateien. Alles via OscillatorNodes.

### Toene
```
playInhaleTone()     → 396 Hz, 0.9s, 0.10 vol — Solfeggio-nah, Einatmen
playExhaleTone()     → 264 Hz, 1.4s, 0.10 vol — tiefer, Ausatmen
playHoldTone()       → 330 Hz, 0.5s, 0.06 vol — leise, Halten
playStartTone()      → 440 Hz, 1.0s, 0.08 vol — Bestaetigung Sound an
playCompletionChime  → Akkord 396 / 528 / 660 Hz, gestaffelt
```

### Drone
```
startDrone(baseFreq) → kontinuierlicher Hintergrund-Sinus, 2.5% Lautstaerke
                       baseFreq + baseFreq*1.5 (Quint), Lowpass 700Hz
                       Fade-in 2 Sek
stopDrone()          → Fade-out 0.5 Sek
```

### Wichtige Audio-Regeln
1. AudioContext wird LAZY initialisiert (erst beim ersten User-Click)
2. iOS Safari pausiert AudioContext bei visibilitychange — wird im Listener resumed
3. Ton-Toggle ist NUR auf Home-Screen (kein Toggle waehrend Uebung)
4. Niemals Binaural Beats hinzufuegen — brauchen Kopfhoerer, Studienlage mixed

---

## Zitate-System

15 echte Zitate mit Autor (nie erfundene!). Drei Traditionen:
- Stoa (5): Marc Aurel, Seneca
- Oestliche Weisheit (5): Laotse, Thich Nhat Hanh, Rumi
- Moderne (5): Rilke, Hesse, Tolle, Kabat-Zinn, Etty Hillesum

Format:
```
{ text: "Zitat-Text", author: "Autor-Name" }
```

Anzeige: Cormorant Garamond Italic + kleine Versalien-Autorenangabe mit "—".
Bei neuen Zitaten: NUR geprueft echte Zitate aus realen Quellen. Niemals
generische Wellness-Spruechen wie "Du musst nichts leisten".

---

## Modal-System

Eigenes Modal (kein native confirm()):
- HTML: #modalStop direkt unter #app
- CSS-Klasse "hidden" zum Verstecken
- Funktionen: confirmStop() / confirmStopYes() / confirmStopNo()

Modal hat keine ID-Konflikte mit showScreen(), weil showScreen alle direkten
#app-Kinder versteckt — d.h. Modal wird beim Screen-Wechsel automatisch zu.

---

## Streak-Logik

```
- Erste Session des Tages: lastDate = today
- Wenn lastDate war gestern: streak += 1
- Wenn lastDate war frueher: streak = 1
- Wenn lastDate ist heute: nichts (nur totalSessions++)
```

Wichtig: updateStreakDisplay() IMMER aufrufen, auch bei Same-Day-Sessions.

---

## WICHTIGE CODING-REGELN

1. localStorage-Key calma_v2 NIE umbenennen
2. Bei neuen Atemuebungen: pattern-Array mit korrektem Phasen-Format
3. Sound-Dateien NIEMALS einfuegen — nur Web Audio API (Sinus)
4. Zitate: nur geprueft echte mit Autor — niemals generische Spruechen erfinden
5. Modi sortieren nach SITUATION, nicht nach Dauer
6. Neue Uebungen: type-Feld pflegen (breath/bodyscan/grounding/static)
7. setTimeout/setInterval IMMER mit isPlaying/bodyscanPlaying-Check schuetzen
8. AudioContext-Resume bei visibilitychange nicht vergessen
9. Bei groesseren Aenderungen: am Ende nochmal alle onclick→Funktionen
   und ID→getElementById-Verbindungen pruefen
10. Sonderzeichen in JS-Strings vermeiden (Grad-Zeichen ° vermeiden)

---

## Bekannte Browser-Limitationen

- AudioContext braucht User-Gesture zum Start (iOS strikt)
- Wake Lock erst ab iOS 16.4 (Safari) — graceful fail mit try/catch
- Vibration nur auf echten Geraeten, nicht im Desktop-Browser
- Audio in Vorschau-iframes oft blockiert — funktioniert auf GitHub Pages

---

## Deployment

1. index.html herunterladen
2. github.com/nicolehahn2890/Calma > index.html > Stift-Symbol
3. Strg+A > Entf > Strg+V > Commit changes
4. ~2 Min warten > https://nicolehahn2890.github.io/Calma/
5. Auf iPhone: Safari > Teilen > Zum Home-Bildschirm

---

## Wenn Aenderungen gewuenscht werden

### Neue Atemuebung hinzufuegen
1. EXERCISES-Objekt erweitern mit key, name, symbol, meta, tagline, type, pattern, cycles, droneFreq
2. In passenden MODES.exercises Array eintragen (nach Situation sortieren!)
3. Symbol kurz halten (1 Zeichen, am besten Unicode wie ○ □ ◐ ~ ❄ ◇ ♪ ◈ ∽)

### Neue Quote hinzufuegen
1. Echte Quelle pruefen (nicht ChatGPT-erfunden!)
2. In GREETINGS-Array { text, author } einfuegen
3. Autor in einheitlichem Format (z.B. "Marc Aurel" nicht "Marcus Aurelius")

### Farben aendern
1. CSS-Variablen in :root anpassen
2. theme-color Meta-Tag im <head> nicht vergessen
3. Drone-Frequenzen passen zur warmen Aesthetik (98-110 Hz) — nur aendern
   wenn Aesthetik komplett geaendert wird

### Body Scan Bereiche aendern
1. BODYSCAN_AREAS-Array in JS bearbeiten
2. Gesamtdauer = length * 30 Sek = aktuell 5 Min

---

## Datei-Struktur (im Browser entwickelt)

Eine einzige Datei: index.html (~1850 Zeilen)
- HTML (~340 Zeilen): App-Container mit allen Screens + Modal
- CSS (~700 Zeilen): Im <style>-Block
- JavaScript (~810 Zeilen): Im <script>-Block

Keine externen Dateien ausser Google Fonts CDN.
