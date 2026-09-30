# Waldgeist – Projektplan

Dieser Plan führt vom Game-Design im [README](README.md) und in der [Skill-Doku](docs/skill-tree.md) zu einem spielbaren Coop-Prototyp auf dem Handy und danach zur ersten veröffentlichbaren Version. Er ist in Meilensteine gegliedert, die jeweils mit etwas Spielbarem enden. Zeitangaben sind grobe Schätzungen für ein kleines Team (1–2 Personen, Teilzeit) und werden nach jedem Meilenstein neu bewertet.

## Ziele

1. **Prototyp:** Zwei Spieler laufen gemeinsam durch die ersten zwei Waldzonen, kämpfen rundenbasiert, fällen Bäume, bauen Barrikaden und lernen Fähigkeiten aus einem festen Skillbaum.
2. **Vertical Slice:** Das Tafel-Skillsystem als Meta-Progression funktioniert über mehrere Runs, inklusive Sockeln von Jade und Bernstein.
3. **Version 1.0:** Alle vier Magiestufen, fünf Zonen mit Endboss, beide Startklassen, vier Holzthemen (Eiche, Moos, Dorn, Lagerfeuer), zwei Schwierigkeitsgrade, stabile Online-Coop-Partien auf Android und iOS.

## Nicht-Ziele für 1.0

- Onyx und Beschwörungen mit Wegfindung (laut Skill-Doku bewusst nach dem Prototyp)
- Mehr als zwei Spieler
- Monetarisierung, Ranglisten, Accounts mit Cloud-Speicher
- Weitere Klassen über Holzfäller und Waldläuferin hinaus

## Engine: Godot 4 (entschieden)

Waldgeist wird mit **Godot 4** umgesetzt. Daraus ergeben sich folgende technische Festlegungen:

| Bereich | Festlegung | Begründung |
| --- | --- | --- |
| Version | Aktuelle stabile Godot-4.x-Version, im Repo festgehalten (`project.godot`, CI-Image) | Alle Beteiligten und die CI bauen mit derselben Version |
| Sprache | GDScript mit statischen Typen | Beste Unterstützung beim Export für Android und iOS; C# ist auf Mobilgeräten weniger ausgereift |
| Karte | `TileMapLayer` mit 16×16-Kacheln | Eingebautes Rendering, Kollision und Navigation für Rasterkarten |
| Spiellogik | Reine GDScript-Klassen (`RefCounted`) ohne Abhängigkeit von Nodes | Simulation lässt sich ohne Szene testen und im Coop zwischen Host und Client synchronisieren |
| Fähigkeiten und Gegner | Eigene `Resource`-Typen (`.tres`), Hook-Logik als kleine Skripte | Werte im Editor pflegbar, Balancing ohne Codeänderung |
| Ereignisbus | Autoload-Singleton mit Signalen für alle 19 Hooks | Globale Coop-Hooks erreichen jeden Akteur |
| Coop im LAN | `ENetMultiplayerPeer` und `@rpc`-Aufrufe, Host-autoritativ | Im High-Level-Multiplayer von Godot enthalten |
| Coop im Internet | `WebSocketMultiplayerPeer` über einen kleinen Relay-Server, alternativ `WebRTCMultiplayerPeer` mit Signalisierung | Umgeht NAT-Probleme zwischen Handys |
| Speicherstand | `ConfigFile` oder JSON in `user://`, mit Versionsnummer | Plattformunabhängig, migrierbar |
| Tests | GUT oder gdUnit4, headless in der CI | Hooks und Fähigkeiten automatisiert prüfen |
| CI | GitHub Actions mit Godot-Headless-Image, Export-Vorlagen für Android | APK bei jedem Push |
| Touch-Eingabe | `InputEventScreenTouch` und `InputEventScreenDrag`, Anzeige skaliert per `stretch_mode = canvas_items` | Einheitlich auf verschiedenen Bildschirmgrößen |

### Weitere Grundsatzentscheidungen (Meilenstein 0)

| Entscheidung | Optionen | Empfehlung | Begründung |
| --- | --- | --- | --- |
| Coop-Modell | Host-autoritativ, Lockstep, dedizierter Server | Host-autoritativ (ein Handy ist Host) | Rundenbasiert und nur 2 Spieler: wenig Bandbreite, keine Serverkosten, Zufall liegt nur beim Host |
| Verbindung | LAN/WLAN, Relay-Server, Bluetooth | Erst LAN, dann Relay für Internet-Partien | LAN reicht zum Testen, Relay löst NAT-Probleme für 1.0 |
| Rundensystem | Strikt abwechselnd, simultane Züge, Zeitbudget | Simultane Planung, gemeinsame Auflösung pro Runde | Kein Warten auf den Partner, passt zu „Laufen kostet kaum Nahrung“ |
| Grafikstil | Pixel-Art 16×16, 32×32 | 16×16 wie SPD | Schnell zu produzieren, gut lesbar auf dem Handy |

### Projektstruktur

```
waldgeist/
├── project.godot
├── autoload/        # EventBus, GameState, Net
├── core/            # reine Spiellogik: Karte, Akteure, Runden, Effekte
├── data/            # .tres-Ressourcen: Fähigkeiten, Tafeln, Steine, Gegner
├── scenes/          # Spielfeld, HUD, Charakterbildschirm, Lobby
├── net/             # Host- und Client-Logik, Nachrichtenformat
├── assets/          # Sprites, Kacheln, Audio, Schriften
└── tests/           # GUT- bzw. gdUnit4-Tests
```

**Ergebnis M0:** Godot-Projekt mit obiger Struktur, EventBus-Autoload, Test-Framework eingerichtet, CI baut ein Android-APK, das leere Projekt läuft auf einem Testgerät.

## Meilensteine

### M1 – Solo-Kern (ca. 4–6 Wochen)

Rundenbasiertes Roguelike für einen Spieler, noch ohne Skills und ohne Netzwerk.

- Hex- oder Quadratraster für die Karte festlegen (Skillbaum ist Hex, Karte darf Quadrat bleiben)
- Prozedurale Waldgenerierung: Lichtungen, Pfade, Bäume, Unterholz, Ausgang zur nächsten Etage
- Sichtfeld (FOV) mit Bäumen und Unterholz als Sichtblocker
- Bewegung, Nahkampf, Fernkampf mit aufsammelbaren Pfeilen
- Nahrungssystem: Aktionen kosten Nahrung, Laufen nicht; Hunger zieht HP ab
- Bäume fällen (3 Hiebe), Fall-Linie mit Schaden, Holz sammeln
- Barrikaden und kleine Türme bauen (Sichtweite +)
- Tiere jagen und am Lagerfeuer Essen kochen
- Hütten im Wald, die man betreten kann (Beute oder Gegner)
- Erste Gegner der Zone 1: Banditen, 2–3 Tiere, Fallen
- Touch-Steuerung: Tippen zum Laufen, Wischen/Halten für Aktionen

**Abnahme:** Ein Run durch 3 Etagen von Zone 1 ist auf dem Handy spielbar und endet mit Tod oder Ausgang.

### M2 – Effektsystem und Startklassen (ca. 3–4 Wochen)

- Zentraler Ereignisbus mit allen 19 Hooks aus der Skill-Doku (von Anfang an global, wegen Coop-Hooks)
- Stat-Modifikatoren, aktive Fähigkeiten mit Abklingzeit, Zustände (Gift, Brand, Verlangsamung, Festwurzeln, Schild)
- Fähigkeiten als `Resource`-Dateien definiert, Hook-Logik als kleine GDScript-Skripte
- Klassensteine Holzfäller und Waldläuferin mit je 3 Fähigkeiten à 1 Rang
- Erfahrung, Level-Up, 1 Skillpunkt pro Level, Lernen nur außerhalb von Begegnungen
- 3 Schnelltasten für aktive Fähigkeiten
- Unit-Tests für jeden Hook und jede Fähigkeit des Kerns

**Abnahme:** Beide Klassen spielen sich spürbar unterschiedlich; alle Hooks sind durch Tests abgedeckt.

### M3 – Coop (ca. 5–7 Wochen, größtes Risiko)

- Spielzustand (`core/`) strikt von Nodes und Rendering trennen (Voraussetzung für Synchronisierung)
- Host-autoritative Simulation: Clients senden Absichten, Host löst auf und verteilt Ergebnisse
- Lobby über LAN mit `ENetMultiplayerPeer`, Beitreten per Code oder QR
- Geteilte Erfahrung, gleichzeitiges Level-Up
- Niederschlagen und Wiederbeleben statt sofortigem Tod
- Coop-Hooks aktiv: `on_damage`, `on_ally_hit`, `on_ally_downed`, `on_revive`
- Sichtfeld-Teilen (Markierungen wie Baumflüsterer)
- Umgang mit Verbindungsabbruch: Pause, Wiederverbinden, Host-Wechsel oder sauberes Beenden
- Determinismus-Tests: gleiche Eingaben ergeben beim Host gleiche Ergebnisse, Replays zur Fehlersuche

**Abnahme:** Zwei Handys spielen einen kompletten Run durch Zone 1 ohne Desync; Abbruch und Wiederverbinden funktioniert.

### M4 – Prototyp-Zone 2 und Holzthemen (ca. 4–5 Wochen)

- Zone 2 mit erster Naturmagie bei Gegnern
- Themen Eiche, Moos, Dorn mit allen in der Skill-Doku beschriebenen Fähigkeiten
- Lagerfeuer-Thema, sobald sein Fähigkeitenpool festgelegt ist
- Fester Test-Skillbaum (ohne Tafeln), um die Fähigkeiten zu balancieren
- Coop-Kombos aus der Skill-Doku gezielt testen (Köder und Hecke, Baumfalle, Gift und Fessel)

**Abnahme = Prototyp:** Interner Playtest mit mindestens 5 Paaren, Feedbackbogen ausgewertet.

### M5 – Tafel-Skillsystem als Meta-Progression (ca. 5–6 Wochen)

- Speicherstand für dauerhafte Tafeln und Steine (lokal, versioniert, migrierbar)
- Tafel-Würfelregeln: 3 aus 6 Fähigkeiten, höchstens eine aktive, starke Fähigkeit ab Zone 3
- Verwitterte Tafeln im Run, Auflösung am Run-Ende
- Charakterbildschirm: Hex-Raster um den Klassenstein, Ringe, Drag-and-drop, Platzierungsregeln
- Erreichbarkeit im Run: Ausgänge und Verbindungen zwischen Tafeln, Pfade innerhalb einer Tafel
- Kaputte Tafeln (weniger Fähigkeiten oder fehlender Sockel)
- Coop-Deckel: Ausschnitt-Auswahl für stärkere Spieler, zusammenhängend am Kern
- Duplikat-Regeln (Rang +1, alternativer Zugang, Abklingzeit −30 %)
- Abgleich der Skillbäume beider Spieler in der Lobby

**Abnahme:** Drei aufeinanderfolgende Runs bauen einen Baum sichtbar auf; der Deckel greift korrekt bei ungleichen Spielern.

### M6 – Edelsteine Natur (ca. 3–4 Wochen)

- Sockeln mit Bestätigung, Bindung, Leuchtfarbe
- Jade und Bernstein mit allen Fähigkeiten
- Baumwächter als stationäre Beschwörung (Testlauf für spätere Onyx-KI)
- Lernbedingung: ein Pfad von einer gelernten Fähigkeit der Tafel zur Magiefähigkeit
- Grenzen der Natur-Magie einhalten (Rang 1, ein Ziel, Zustände höchstens 2 Runden)
- Mindestring-Regeln

**Abnahme = Vertical Slice:** Externer Playtest (geschlossene Beta) auf Android.

### M7 – Elementar, Licht und Dunkel, Zonen 3–5 (ca. 8–10 Wochen)

- Topas: Feuer- und Blitzsystem, Nässe, Waldbrand-Ausbreitung, halbe Feuerfestigkeit
- Saphir: Wasser und Eis, Nässe als Verstärker für Topas-Blitze, Löschen gegen Friendly Fire
- Rubin und Diamant
- Zonen 3–5 mit eigenen Gegnern und Umgebungen
- Endboss inklusive seltenem Notausgang-Gegenstand
- Coop-Regel: unter 3 Tafeln keine dunkle Magie und keine Diamanten

### M8 – Release-Vorbereitung (ca. 6–8 Wochen)

- Internet-Coop über Relay-Server (`WebSocketMultiplayerPeer` oder WebRTC), Einladungslinks
- Tutorial und erste Etage als geführter Einstieg
- Zwei Schwierigkeitsgrade, vor dem Run wählbar
- Audio, Effekte, Barrierefreiheit (Schriftgröße, Farbunterscheidung der Steine)
- Leistungstests auf schwachen Geräten, Akkuverbrauch
- iOS-Build, Store-Einträge, Datenschutzerklärung
- Absturzberichte und anonyme Balancing-Telemetrie (opt-in)

**Abnahme = 1.0:** Veröffentlichung in Google Play und App Store.

## Zeitplan (grob)

| Meilenstein | Dauer | Kumuliert |
| --- | --- | --- |
| M0 Godot-Setup und Entscheidungen | 1–2 Wochen | ~2 Wochen |
| M1 Solo-Kern | 4–6 Wochen | ~8 Wochen |
| M2 Effektsystem | 3–4 Wochen | ~12 Wochen |
| M3 Coop | 5–7 Wochen | ~19 Wochen |
| M4 Prototyp | 4–5 Wochen | ~24 Wochen |
| M5 Tafelsystem | 5–6 Wochen | ~30 Wochen |
| M6 Natursteine | 3–4 Wochen | ~34 Wochen |
| M7 Magie und Zonen | 8–10 Wochen | ~44 Wochen |
| M8 Release | 6–8 Wochen | ~52 Wochen |

## Risiken

| Risiko | Auswirkung | Gegenmaßnahme |
| --- | --- | --- |
| Desync im Coop | Unspielbare Partien | Host-autoritativ, Zustand strikt von Darstellung trennen, Replays, Coop früh (M3) statt spät |
| Rundenablauf zu zweit fühlt sich zäh an | Spielspaß leidet | Simultane Züge früh prototypen, Laufen außerhalb von Begegnungen frei |
| Tafelsystem zu komplex auf kleinem Bildschirm | Spieler verstehen Meta-Progression nicht | Papierprototyp und UI-Mockups vor M5, Tutorial-Tafel |
| Balancing bei ~50 Fähigkeiten | Dominante Builds, tote Fähigkeiten | Datengetriebene Werte, Playtest-Telemetrie, Balancing-Tabellen |
| Waldbrand-Simulation zu teuer | Ruckeln auf alten Handys | Zellbasierte Ausbreitung pro Runde, Obergrenze aktiver Brandfelder |
| Umfang wächst (Onyx, weitere Klassen, Jagd als 5. Thema) | Release verschiebt sich | Nicht-Ziele einhalten, Ideen in Backlog nach 1.0 |

## Offene Designpunkte und wann sie entschieden werden

| Offener Punkt (aus der Skill-Doku) | Entscheiden in |
| --- | --- |
| Skillpunkte pro Run (~15) | M4-Playtest |
| Deckel lockern auf Minimum + 1 | M5-Playtest |
| Drop-Raten für Tafeln und Steine | M5 (Tafeln), M6/M7 (Steine) |
| Seltenheit des Notausgangs | M7 |
| Fähigkeitenpool für Lagerfeuer | vor M4 |
| Einstieg, Ausgänge und kaputte Tafeln | vor M5 (Papierprototyp) |
| Tafelbilder pro Thema (leer und gesockelt) | parallel zu M5 |
| Natur-Magie und Baumwächter schwach genug? | M6-Playtest |
| Saphir gegen Friendly Fire von Topas | M7-Playtest |
| Jagd als fünftes Thema | nach 1.0 oder M7, falls Zeit bleibt |
| Gestrichene Glut-Fähigkeiten zurückholen | M7 beim Topas-Balancing |
| Onyx | nach 1.0 |
| Pool auf 10–12 pro Thema ausbauen | fortlaufend ab M5 |
| Weitere Klassen | nach 1.0 |

## Arbeitsweise

- Jeder Meilenstein bekommt ein GitHub-Milestone mit Issues pro Aufgabe.
- Hauptzweig bleibt jederzeit baubar; Arbeit in Feature-Branches mit Pull Requests.
- CI baut bei jedem Push mit Godot headless ein Android-APK und führt die Tests aus.
- Nach jedem Meilenstein: kurzer Playtest, Rückblick, Plan anpassen.
- Alle Balancing-Zahlen stehen in Datendateien, nicht im Code.
