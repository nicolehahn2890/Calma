---
name: calma
description: >
  Use this skill for EVERY request related to Calma — a comprehensive nervous-system
  and mental health toolkit app with 6 pillars: breathing, grounding, thoughts (CBT),
  gratitude, strengths, and self-compassion. Built in standalone HTML, deployed to
  GitHub Pages. Trigger on any mention of: "Calma", "Calma App", "Atemübung",
  "Atem-App", "Nervensystem", "Vagusnerv", "Body Scan", "Grounding", "Doppelseufzer",
  "Kohärenz-Atmung", "Box-Atmung", "Beruhigungs-App", "Tagebuch-App", "Gedanken-Protokoll",
  "Dankbarkeits-Übung", "Selbstmitgefühl", "Self-Compassion", "Affirmation",
  "Werte-Reflexion", "Stärken", "Mental-Health-App", "nicolehahn2890.github.io/Calma",
  or any request to add/fix/style features in the Calma app. Also trigger when the user
  uploads an HTML file related to this app.
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
- Aktuelle Version: Calma 2.1
- localStorage-Keys:
  - calma_v3 (Settings, Streak, Sound)
  - calma_diary_v3 (Tagebuch-Eintraege)
- Zweck: Umfassendes Werkzeug fuer mentale Gesundheit und Nervensystem-Beruhigung

WICHTIG: localStorage-Keys NIE umbenennen — sonst sind alle gespeicherten
Tagebuch-Eintraege weg.

---

## Design-System

Hintergrund: #1a1410 (warm-dunkel)
Surface: #2a201a
Surface-2: #322620
Cream (Text): #f4ead8
Cream-dim: #c9bfa9
Text-muted: #9a8e76

Saeulen-Akzentfarben (jeweils oberer Rand der Kachel):
- Atmen: #c97b5b (terracotta)
- Ankommen: #8a956b (olive)
- Gedanken: #7a9bb0 (sky)
- Dankbarkeit: #d4b896 (sand)
- Staerkung: #8a7a9c (plum)
- Mitgefuehl: #b88a8a (rose)

Fonts:
- Cormorant Garamond (Display, kursiv fuer Akzente, Zitate, Ueberschriften)
- DM Sans (Body)
Beide ueber Google Fonts CDN.

Aesthetik: warm-erdig, Spanien-Vibe (Andalusischer Innenhof).
KEINE bunten Emojis, alle Icons sind handgezeichnete SVGs in einer einzigen Linienstaerke.

---

## App-Architektur (Calma 2.1)

### State-Objekte

```
state = {
  streak: 0,           // aktuelle Tage in Folge
  lastDate: null,      // ISO-Date "YYYY-MM-DD"
  totalSessions: 0,    // Gesamt-Sessions
  soundEnabled: false, // Ton an/aus
  soundType: 'drone'   // 'silence' | 'drone' | 'rain'
}

diary = []  // Array von Tagebuch-Eintraegen, neueste zuerst
```

### Tagebuch-Eintrag-Struktur
```
{
  id: "<timestamp>_<random>",
  date: "<ISO-String>",
  pillar: "gedanken" | "dankbarkeit" | "staerkung" | "mitgefuehl",
  pillarLabel: "Gedanken",
  exerciseKey: "thought-record",
  exerciseName: "Gedanken-Protokoll",
  prompts: [
    { q: "Frage", a: "Antwort" }
  ]
}
```

### Globale Variablen (let)
```
currentExercise, currentPillar, isPlaying, breathTimeout, timerInterval,
elapsedSeconds, cycleCount, wakeLock, bodyscanIndex, bodyscanInterval,
bodyscanPlaying, groundingStep, audioCtx, droneNodes, diaryFilter,
pendingDeleteId, completionChimeTimeouts
```

### Wichtige Funktionen
```
showScreen(id)           Versteckt alle direkten #app-Kinder ausser Modal-Overlays
goHome()                 Zurueck zum Start, stoppt Uebung, neues Greeting
goBackToPillar()         Zurueck zur Saeulen-Liste
showPillar(key)          Saeule mit Uebungsliste rendern
showRandomGreeting()     Setzt zufaelliges Zitat + Autor

startExercise(key)       Dispatcht je nach type
stopExercise()           Aufraeumen: Timer, Drone, Wake Lock, Chime, Circle
confirmStop()            Modal wenn Uebung laeuft
confirmStopJournal()     Modal nur wenn schon geschrieben wurde

runBreathCycle(stepIdx)  Rekursiver Atemzyklus
toggleBodyscan()         Body Scan Play/Pause
nextGrounding()          Grounding-Schritt weiter
startJournal()           Schreibuebung initialisieren
saveJournal()            Eintrag in diary speichern, completeExercise

showDiary()              Tagebuch oeffnen
renderDiary()            Filter + Liste neu rendern
askDeleteEntry(id)       Loesch-Modal anzeigen
confirmDeleteYes/No      Loesch-Modal handhaben

playTone(f,d,v)          Sinus-Doppelton mit Lowpass
playInhaleTone           396 Hz, 0.9s
playExhaleTone           264 Hz, 1.4s
playHoldTone             330 Hz, 0.5s
playCompletionChime      Akkord 396/528/660 mit Cancel-Schutz
cancelCompletionChime    Bricht laufende Chime ab
startDrone(freq)         Mehrlagiger Drone ODER Regen je nach soundType
startRainSound           Pink-Noise mit LFO
stopDrone                Fade-out 0.8s, dann Stop
toggleSound              Sound ein/aus, zeigt/versteckt Sound-Picker
selectSound(type)        Wechsel zwischen Stille/Drone/Regen
updateSoundUI            Update Toggle-Button + Picker
updateSoundPickerUI      Markiert aktive Option, blendet Picker ein/aus
```

---

## Die 6 Saeulen (PILLARS)

### 1. atmen — Atmen
Vagusnerv aktivieren, Stress senken
- physiological-sigh (Doppelseufzer, Stanford 2023)
- box-breathing (Box-Atmung, Navy SEALs)
- 478-breathing (4-7-8 Atmung)
- extended-exhale (Lange Ausatmung)
- vagus-humming (Summen, Vagusnerv-Stimulation)
- coherence (Kohaerenz-Atmung, Meta-Analyse)

### 2. ankommen — Ankommen
Im Moment landen, Sinne nutzen
- grounding (5-4-3-2-1)
- body-scan (10 Bereiche x 30 Sek)
- cold-face (Kalt-Reflex, Tauchreflex)

### 3. gedanken — Gedanken
Muster erkennen, sanft umdeuten — CBT-Klassiker
- thought-record (ABC-Modell, 6 Prompts)
- cognitive-reframe (Schnelle Variante, 4 Prompts)

### 4. dankbarkeit — Dankbarkeit
Sehen, was schon da ist
- three-good-things (Drei gute Dinge)
- gratitude-letter (Wertschaetzungs-Brief)
- small-joys (Kleine Freuden)

### 5. staerkung — Staerkung
Werte und innere Ressourcen
- values-reflection (Was mir wichtig ist)
- strengths-inventory (Meine Staerken)
- future-self (Mein Zukunfts-Ich)

### 6. mitgefuehl — Mitgefuehl
Freundlich mit dir selbst sein (Kristin Neff)
- self-compassion-break (Mitgefuehl-Pause, 3 Schritte)
- inner-friend (Innerer Freund, Brief)

---

## Saeulen-Icons (alles SVG, KEINE bunten Emojis!)

- Atmen: konzentrische Kreise (3 Kreise + Punkt)
- Ankommen: stilisiertes Auge (Augenform + Iris)
- Gedanken: Spirale von aussen nach innen (Pfad mit Bezier-Kurven)
- Dankbarkeit: Pflanze mit zwei Blaettern und Stiel
- Staerkung: Stern (5 Punkte)
- Mitgefuehl: Herz (klassische Form)

Alle Icons:
- viewBox="0 0 24 24"
- stroke="currentColor", stroke-width="1.4"
- fill="none" (ausser kleine Akzente)
- 24x24px, mit opacity 0.85

Tagebuch-Indikator (kleines Buch unten in Kachel):
- Nur fuer Saeulen mit Schreib-Uebungen: gedanken, dankbarkeit, staerkung, mitgefuehl
- 11px SVG mit "Tagebuch"-Label

---

## Uebungstypen

### type: 'breath' — Atemuebungen
Pattern-Array mit Phasen, atmender Kreis, optional droneFreq

### type: 'bodyscan' — Body Scan
30 Sek pro Bereich, 10 Bereiche

### type: 'grounding' — 5-4-3-2-1
Manuelle Progression mit Weiter-Button

### type: 'static' — Anleitung ohne Timer
Eigener Screen (z.B. screen-coldface), Erledigt-Button

### type: 'journal' — Schreibuebung (NEU in 2.0!)
Mit intro + prompts-Array `[{ q, placeholder }]`
Texteingabe in Textareas, Speichern in diary

---

## Audio-System (Calma 2.1)

KEINE externen Sounddateien. Alles via Web Audio API.

### Sound-Typen (Auswahl auf Home-Screen)
- silence: keine Hintergrundklaenge, nur Atemphasen-Toene
- drone: 4-Layer-Drone mit zwei LFOs (Bass + Fundamental + Quint + Triangle-Pad)
- rain: Pink-Noise mit LFO-Filter-Sweep ("Wind"-Effekt)

### Drone-Aufbau (multi-layered)
```
Layer 1: Bass (baseFreq * 0.5)            gain: 0.6
Layer 2: Fundamental (baseFreq)            gain: 0.4
Layer 3: Quint (baseFreq * 1.5 + 0.3)      gain: 0.25 (leicht detuned)
Layer 4: Triangle-Pad (baseFreq * 2 - 0.5) gain: 0.08

Lowpass-Filter @ 800 Hz, Q 0.7
LFO 1 @ 0.08 Hz auf Master-Gain (Wabern, ~12s Zyklus)
LFO 2 @ 0.05 Hz auf Filter-Frequenz (subtle Klangwechsel)
Master-Gain: 0 → 0.04 in 3s
```

### Atemphasen-Toene
- Inhale: 396 Hz, 0.9s, 0.10 vol
- Exhale: 264 Hz, 1.4s, 0.10 vol
- Hold: 330 Hz, 0.5s, 0.06 vol

### Wichtige Audio-Regeln
1. AudioContext wird LAZY initialisiert (erst beim ersten User-Click)
2. iOS Safari pausiert AudioContext bei visibilitychange — wird im Listener resumed
3. iOS Safari blockiert Web Audio bei aktivem Stumm-Schalter — siehe iOS-Hinweise unten
4. Toene sind KEIN UI-Element waehrend Uebung — Toggle nur auf Home
5. Niemals Binaural Beats — brauchen Kopfhoerer, Studienlage mixed
6. completionChimeTimeouts speichert verzoegerte Toene, damit cancelCompletionChime sie stoppen kann

---

## iOS-Audio-Spezifika

PROBLEM: Bei aktivem Stumm-Schalter (oranger Schalter an der iPhone-Seite) blockiert
iOS Safari ALLE Web Audio API Toene, auch wenn die Lautstaerke voll aufgedreht ist.
Das ist ein bekannter iOS-Bug, kein App-Fehler.

LOESUNG fuer User: Stumm-Schalter ausschalten (orange weg), dann funktioniert alles.

KEIN HACK in der App noetig — der User muss einmal den Schalter umlegen.

---

## Zitate-System

25 echte Zitate mit Autor (NIE erfunden!). Vier Quellen-Cluster:

### Stoa & Antike (6)
Marc Aurel (4x), Seneca, Cicero, Sokrates

### Oestliche Weisheit (7)
Laotse (3x), Thich Nhat Hanh (3x), Rumi, Buddha (2x)

### Moderne (12)
Rilke (2x), Hesse, Tolle, Kabat-Zinn, Etty Hillesum, Kristin Neff, Emerson,
Viktor Frankl, Nietzsche

Format:
```
{ text: "Zitat-Text", author: "Autor-Name" }
```

WICHTIG: Bei neuen Zitaten IMMER Quelle pruefen (Wikiquote, Original-Werke).
Keine generischen Wellness-Spruechen wie "Du musst nichts leisten".

---

## Tagebuch-System

### Speicherung
localStorage-Key: calma_diary_v3
Eintraege werden mit `unshift` hinzugefuegt (neueste zuerst)
Nur Eintraege mit MIND. EINER nicht-leeren Antwort werden gespeichert

### Anzeige (Tagebuch-Tab)
- Filter-Pills oben: "Alle" + jede Saeule die Eintraege hat
- Eintraege als Cards mit:
  - Saeulen-Label oben (klein, terracotta)
  - Datum (Heute/Gestern/Datum)
  - Uebungs-Name
  - Q&A pro Prompt (nur die mit Antwort)
  - Loeschen-Button (mit Modal-Bestaetigung)

### XSS-Schutz
escapeHtml() wird auf alle User-Eingaben angewandt
\\n wird zu <br> beim Anzeigen umgewandelt

---

## Modal-System

Zwei Modals:
- modalStop: "Uebung beenden?" — bei confirmStop / confirmStopJournal
- modalDelete: "Eintrag loeschen?" — bei askDeleteEntry

WICHTIG: showScreen() versteckt alle direkten #app-Kinder ausser modal-overlay-Klassen.
Modals bleiben dadurch beim Screen-Wechsel automatisch gut sichtbar bzw. werden
unabhaengig per CSS-Klasse hidden gehandhabt.

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

1. localStorage-Keys (calma_v3, calma_diary_v3) NIE umbenennen
2. Bei neuen Atemuebungen: pattern-Array mit korrektem Phasen-Format
3. Sound-Dateien NIEMALS einfuegen — nur Web Audio API
4. Zitate: nur geprueft echte mit Autor — niemals generische Spruechen erfinden
5. Saeulen sortieren nach THEMA (nicht nach Dauer oder Schwierigkeit)
6. Neue Uebungen: type-Feld pflegen (breath/bodyscan/grounding/static/journal)
7. setTimeout/setInterval IMMER mit isPlaying/bodyscanPlaying-Check schuetzen
8. AudioContext-Resume bei visibilitychange nicht vergessen
9. Bei groesseren Aenderungen: am Ende alle onclick→Funktionen
   und ID→getElementById-Verbindungen pruefen
10. Sonderzeichen in JS-Strings vermeiden (Grad-Zeichen vermeiden — "Grad" ausschreiben)
11. Schreibstil: konkret, nicht floskelhaft. Kurze Saetze. Keine Wellness-Phrasen wie
    "Liebevoll", "Spuer mal", "Mit Achtsamkeit". Stattdessen: konkrete Fragen.
12. Icons: NUR SVG mit currentColor + stroke-width 1.4. KEINE bunten Emojis.
13. Bei Schreibuebungen: HTML-Escape via escapeHtml(), \\n zu <br> beim Anzeigen
14. completionChimeTimeouts canceln in stopExercise

---

## Bekannte Browser-Limitationen

- AudioContext braucht User-Gesture zum Start (iOS strikt)
- Wake Lock erst ab iOS 16.4 (Safari) — graceful fail mit try/catch
- Vibration nur auf echten Geraeten, nicht im Desktop-Browser
- Web Audio Stumm-Schalter: iOS blockiert bei aktivem Stumm-Schalter
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
1. EXERCISES-Objekt erweitern
2. In atmen-Saeule eintragen (PILLARS.atmen.exercises)
3. Symbol kurz halten (1 Zeichen, Unicode wie ○ □ ◐ ~ ❄)

### Neue Schreibuebung hinzufuegen
1. EXERCISES-Objekt erweitern mit type: 'journal'
2. intro + prompts-Array definieren
3. In passender Saeule eintragen
4. Falls neue Saeule: PILLARS-Objekt + HTML-Pillar-Card + CSS-Akzent + SVG-Icon

### Neue Quote hinzufuegen
1. Echte Quelle pruefen (Wikiquote, Original)
2. In GREETINGS-Array { text, author } einfuegen
3. Autor in einheitlichem Format

### Neuer Sound-Typ
1. soundType-Auswahl im Picker erweitern
2. Logik in startDrone() Switch-Block hinzufuegen
3. Im saveState/loadState beruecksichtigt? (ist generisch, sollte funktionieren)

### Farben aendern
1. CSS-Variablen in :root anpassen
2. theme-color Meta-Tag im <head>
3. Drone-Frequenzen passen zur warmen Aesthetik (98-110 Hz)

### Tagebuch-Felder erweitern
WICHTIG: alte Eintraege haben das neue Feld nicht. Defensive Defaults setzen.

---

## Datei-Struktur

Eine einzige Datei: index.html (~2700 Zeilen, Calma 2.1)
- HTML (~530 Zeilen): App-Container mit allen Screens + 2 Modals
- CSS (~890 Zeilen): Im <style>-Block
- JavaScript (~1280 Zeilen): Im <script>-Block

Keine externen Dateien ausser Google Fonts CDN.
