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
- Aktuelle Version: Calma 2.2 (bilingual ES/DE) — Design-System "Luminous (Perla)"
- localStorage-Keys:
  - calma_v3 (Settings, Streak, Sound)
  - calma_diary_v3 (Tagebuch-Eintraege)
- Zweck: Umfassendes Werkzeug fuer mentale Gesundheit und Nervensystem-Beruhigung

WICHTIG: localStorage-Keys NIE umbenennen — sonst sind alle gespeicherten
Tagebuch-Eintraege weg.

---

## Design-System — "Luminous (Perla)"

Aesthetik: luminoeses, irisierendes Pastell. Heller traumhafter Hintergrund,
Frosted-Glass-Karten, echte Perlmutt/Marmor-Textur, transparente farbige
Glaskugel-Icons, perlmutt-Logo und feiner Diamant-Glitzer.

WICHTIG: Die CSS-Variablen-NAMEN sind dieselben geblieben wie im alten warm-dunklen
Theme (--bg, --surface, --cream, --terracotta, --olive ...), nur die WERTE wurden
auf Perla gemappt. So funktioniert der Rest des CSS unveraendert weiter. Nicht die
Namen umbenennen — nur Werte anpassen.

Palette (Perla):
- --bg: #ece2f8 (heller Lavendel) / --bg-soft: #f5eefb
- --surface: rgba(255,255,255,0.40) (Frosted Glass) / --surface-2: rgba(255,255,255,0.60)
- --cream (Text): #3d2c52 (tiefes Pflaume) / --cream-dim: #5a4775 / --text-muted: #7c6699
- --terracotta (Hauptakzent): #b78fe6 (Lavendel) / --olive (2. Akzent): #7fd6b8 (Mint)
- weitere: --sky #8fcdf0, --sand #ffd49a (Pfirsich), --plum #c4a9f0 (Flieder), --rose #ffb3d9

Zusatz-Tokens (neu): --lux-accent (#b78fe6), --lux-accent-2 (#7fd6b8),
--lux-bg-1 (#eccdf0), --lux-bg-2 (#c4ece2), --lux-glass-brd (rgba(255,255,255,0.95)),
--holo (irisierender Regenbogen-Gradient fuer Glas-Kanten).

Saeulen-Akzent = Glaskugel-Farbe pro Saeule (CSS-Var + JS-Map PILLAR_BEAD):
- Atmen: #ff9ec4 (pink)
- Ankommen: #8fe3c4 (mint)
- Gedanken: #8fcdf0 (sky)
- Dankbarkeit: #ffd49a (peach)
- Staerkung: #c4a9f0 (lilac)
- Mitgefuehl: #ffb3d9 (rose)

Fonts:
- Cormorant Garamond (Display, kursiv fuer Akzente, Zitate, Ueberschriften)
- DM Sans (Body; Overlines 600, uppercase, letter-spacing 0.22em)
Beide ueber Google Fonts CDN.

Signatur-Techniken (alle in index.html portiert, vanilla CSS/JS):
1. Seite + Marmor: heller Verlauf (zwei Radial-Glows) + pearl-marble.png als
   body::before mit mix-blend-mode:multiply; body::after = Header-Vignette.
2. Frosted-Glass-Karten: .pillar-card/.exercise-card/.modal etc. mit backdrop-filter,
   weisser Hairline, irisierender Kante (::before mit --holo, mask-composite) und
   sanftem farbigem Schatten.
3. Glaskugel-Icons (Signatur!): runde, glaenzende farbige Glas-Sphaeren mit
   Specular-Highlight, dunklem Rand, Glanz und Funkel-Punkt. Eingesetzt fuer
   Home-Grid (40px), Pillar-Header (46px, #pillarHeadBall), Uebungs-Zeilen
   (40px mit Unicode-Glyph) und Logo-Mark (52px, Lavendel #b9a0ef).
   Farbe via CSS-Var --ball / --list-ball (in showPillar gesetzt).
4. Diamant-Glitzer: .glitter-Layer wird per generateGlitter() einmalig beim Laden
   erzeugt (~200 funkelnde Punkte, ~34% 4-Punkt-Diamanten + 9 Seifenblasen).
   prefers-reduced-motion deaktiviert die Animation.
5. Logo: assets/calma-logo.png (perlmutt Wortmarke) als Header-Wortmarke.
6. Atem-Orb: durchscheinende Seifenblase, eingefaerbt pro Saeule via --pc
   (in startBreathExercise gesetzt).

KEINE bunten Emojis, alle Icons sind handgezeichnete SVGs in einer einzigen
Linienstaerke — in den Glaskugeln werden sie weiss dargestellt.

---

## App-Architektur (Calma 2.2 — bilingual)

### Sprach-Architektur (NEU in 2.2)
Die App ist bilingual ES/DE:
- **Spanisch ist Hauptsprache** (gross, Standard-Stil)
- **Deutsch ist Echo darunter** (klein, kursiv, gedaempft, ~0.65em)
- **Tonalitaet**: kastilisches Spanisch mit "tu", warm aber direkt
- **UI-Buttons NUR Spanisch** (Volver, Guardar, Continuar, Salir, Hecho, Eliminar, Mantener, Sonido on/off, Diario)
- **Sound-Optionen**: Silencio, Drone, Lluvia
- Tagebuch-Eintraege werden in der Sprache gespeichert, in der Nicole schreibt

### Bilingual-Datenstruktur
Alle uebersetzbaren Texte sind `{ es: "...", de: "..." }`-Objekte:
```javascript
name: { es: 'Suspiro fisiologico', de: 'Doppelseufzer' }
tagline: { es: '...', de: '...' }
prompts: [{ q: { es, de }, placeholder: { es, de } }]
```

### Bilingual Helper-Funktionen
```
bi(obj)    -> HTML-String mit beiden Sprachen (Spanisch gross, Deutsch klein darunter)
biES(obj)  -> Plain-Text Spanisch (fuer textarea placeholder, etc.)
biDE(obj)  -> Plain-Text Deutsch
```

### CSS-Klassen
- `.bi` — Container fuer bilingualen Text
- `.bi-de` — Deutscher Text klein, kursiv, gedaempft (0.65em opacity 0.65)
- `.bi-inline .bi-de` — Inline-Variante (selten genutzt)

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
- stroke="currentColor", stroke-width="1.4-1.5"
- fill="none" (ausser kleine Akzente)
- sitzen in der Glaskugel und werden weiss dargestellt
  (color: rgba(255,255,255,0.96), drop-shadow fuer Tiefe)
- die Pfade liegen doppelt vor: im Home-Grid-HTML und als JS-Map PILLAR_ICON_SVG
  (fuer den Pillar-Header) — bei Aenderung an einem Icon BEIDE Stellen pflegen

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

## iOS-Audio-Spezifika (WICHTIG!)

PROBLEM: Bei aktivem Stumm-Schalter blockiert iOS Safari standardmaessig
ALLE Web Audio API Toene, auch wenn die Lautstaerke voll aufgedreht ist.

LOESUNG IN CALMA: iOS-Audio-Unmute-Trick eingebaut.

### Wie der Trick funktioniert
1. iOS hat zwei Audio-Channels: "Ringer Channel" (vom Stumm-Schalter blockiert)
   und "Media Channel" (immer aktiv)
2. Web Audio API laeuft normalerweise auf Ringer Channel
3. HTML5 <audio>-Elemente laufen auf Media Channel
4. Wenn ein <audio>-Element parallel zur Web Audio laeuft, "verschiebt" iOS
   alle Audio-Aktivitaet auf den Media Channel — auch die Web Audio Toene
5. Wir embedden eine ~0.5s stumme MP3 als Base64-Data-URI direkt im HTML
   (kein externer Datei-Download noetig)
6. Beim Aktivieren des Sounds wird ein <audio>-Element mit dieser MP3 in Loop gestartet
7. Web Audio Toene kommen jetzt durch, auch bei aktivem Stumm-Schalter

### Code-Komponenten
- SILENT_MP3_DATA_URI: ~3.4 KB Base64 stumme MP3 (~0.5s, geloopt)
- activateIOSAudioUnmute(): erstellt versteckten <audio>-Tag und startet Loop
- deactivateIOSAudioUnmute(): stoppt und entfernt das Element
- silentAudioElement: globale Referenz auf das Audio-Element
- unmuteActivated: flag, true wenn aktiv

### Aufgerufen wird
- toggleSound() bei Sound-Aktivierung
- Globaler click-Listener als Fallback (falls erster Versuch blockiert)
- visibilitychange-Listener: Audio-Element neu starten nach Hintergrund

### Hinweis fuer User
Der Trick funktioniert ohne dass der User etwas tun muss.
Beim ersten Tippen auf "Ton an" startet alles automatisch, auch bei aktivem Stumm-Schalter.

### Stumme MP3 generieren (falls Re-Generation noetig)
```bash
ffmpeg -y -f lavfi -i anullsrc=r=22050:cl=mono -t 0.5 -c:a libmp3lame -b:a 32k silent.mp3
base64 silent.mp3 | tr -d '\n'
```
WICHTIG: NUR loopfaehige stumme MP3s funktionieren. Keine WAV oder OGG verwenden,
weil iOS bei Loop-Playback haengen bleibt.

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
15. iOS-Audio-Unmute (SILENT_MP3_DATA_URI) NIEMALS entfernen — sonst kein Ton bei
    aktivem Stumm-Schalter auf iPhone
16. Alle uebersetzbaren Texte als `{ es, de }`-Objekt — niemals nur ein String
17. UI-Buttons NUR auf Spanisch (Volver, Guardar, Continuar, Salir, Hecho, Eliminar)
18. bi() Helper fuer Anzeige verwenden — niemals .es oder .de direkt in textContent
19. textarea-Placeholder NUR auf Spanisch (biES verwenden)
20. Tagebuch-Eintraege haben backward-compat fuer alte String-Werte (typeof check)

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
1. CSS-Variablen in :root anpassen (NAMEN behalten, nur Werte)
2. Saeulen-Glaskugel-Farben: --p-* in :root UND die JS-Map PILLAR_BEAD anpassen
3. theme-color Meta-Tag im <head> (#ece2f8) + site.webmanifest (background/theme_color)
4. Der grosse "LUMINOUS (PERLA)"-Block am Ende des <style> ueberschreibt die
   Basis-Regeln per Source-Order — Anpassungen dort vornehmen

### Tagebuch-Felder erweitern
WICHTIG: alte Eintraege haben das neue Feld nicht. Defensive Defaults setzen.

---

## Datei-Struktur

Haupt-Datei: index.html (~3340 Zeilen, Calma 2.2 bilingual, Luminous-Reskin)
- HTML (~600 Zeilen): App-Container mit allen Screens + 2 Modals + bilinguale Texte
- CSS (~1300 Zeilen): Im <style>-Block (inkl. grossem LUMINOUS-Override-Block)
- JavaScript (~1450 Zeilen): Im <script>-Block

Design-Assets (NEU, im Ordner assets/ — von index.html via ./assets/... referenziert):
- assets/calma-logo.png — perlmutt "Calma"-Wortmarke (Header-Logo)
- assets/textures/pearl-marble.png — Marmor/Perlmutt-Seitentextur (Pflicht)
- assets/textures/pearls.png — optionale 2. Perlmutt-Textur

Zusatz-Dateien im Repo-Root (fuer Home-Bildschirm-Icon, siehe unten):
- apple-touch-icon.png (180x180) — iPhone Home-Bildschirm
- icon-192.png, icon-512.png — Android / PWA / Manifest
- site.webmanifest — PWA-Manifest

Code laeuft komplett in index.html. Keine externen Skripte/Styles ausser
Google Fonts CDN. WICHTIG: Seit dem Luminous-Reskin braucht die App zusaetzlich
den Ordner assets/ (Logo + Marmor-Textur). Fehlt er, fehlen Logo und Textur.
Die PNG/Manifest-Dateien im Root sind nur das Home-Bildschirm-Icon.

---

## Home-Bildschirm-Icon (App-Icon)

Damit auf dem iPhone-Home-Bildschirm ein richtiges Icon erscheint (statt einem
Safari-Screenshot der Seite), liegen im Repo-Root Icon-Dateien plus Manifest.

### Eingebunden im <head> von index.html
```html
<meta name="apple-mobile-web-app-title" content="Calma">
<link rel="apple-touch-icon" href="apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="192x192" href="icon-192.png">
<link rel="icon" type="image/png" sizes="512x512" href="icon-512.png">
<link rel="manifest" href="site.webmanifest">
```
`apple-mobile-web-app-title` = Label unter dem Icon auf dem Home-Bildschirm ("Calma").

### Icon-Design (Luminous/Perla — passt zum neuen App-Look)
- Irisierendes Pastell-Feld (Diagonal-Verlauf pink -> flieder -> sky -> mint)
  mit sanftem weissem Glow oben — wie der App-Hintergrund
- Motiv: zentrale Lavendel-Glaskugel (#b9a0ef) mit Specular-Highlight und dunklem
  Rand, darin die weissen Atem-Ringe (2 konzentrische Kreise + Punkt) + Funkel-Punkt
- Vollflaechig quadratisch, deckend (kein Transparenz-Rand) — iOS rundet die Ecken
  selbst; wichtige Elemente in der zentralen ~80%-Safe-Zone (purpose "any maskable")

### Icon neu generieren (wenn Aenderung gewuenscht)
Erzeugt wird das Icon per Python/Pillow-Skript (Supersampling 4x, runterskaliert
mit LANCZOS; Verlauf via 2x2-Eck-Gradient hochskaliert, Glaskugel aus radialen
Highlights/Schatten + Gauss-Blur). Drei PNGs entstehen: apple-touch-icon.png (180),
icon-192.png, icon-512.png. Bei Design-Aenderung: Skript anpassen, alle drei
PNGs neu erzeugen, committen.

### WICHTIG fuer Deployment
- Die PNG-Dateien + site.webmanifest sind EIGENE Dateien im Repo (nicht in index.html).
- Nicoles ueblicher Workflow ersetzt nur index.html — die Icon-Dateien bleiben
  dabei unberuehrt erhalten, das ist OK.
- Ein neues/geaendertes Icon erfordert das Hochladen der PNG-Dateien ueber das
  GitHub-Browser-Interface (Add file > Upload files) — Stift-Symbol funktioniert
  nur fuer Text-Dateien, nicht fuer Bilder.
- relative Pfade (apple-touch-icon.png) loesen korrekt zu
  https://nicolehahn2890.github.io/Calma/apple-touch-icon.png auf.

### Icon auf iPhone installieren (fuer Nicole)
Safari oeffnen > https://nicolehahn2890.github.io/Calma/ > Teilen-Symbol >
"Zum Home-Bildschirm". Das Calma-Icon und der Name "Calma" erscheinen automatisch.
