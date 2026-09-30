# Waldgeist – Projektplan

## Worum es geht

Wir bauen zuerst nur eines: **zwei Spieler gehen auf zwei Handys gemeinsam durch den Wald, und das macht Spaß.**

Alles andere kommt später. Das Tafel-Skillsystem, die Edelsteine, die tieferen Zonen und die Veröffentlichung im Store warten, bis dieser Kern funktioniert.

Das Spieldesign steht im [README](README.md) und in der [Skill-Doku](docs/skill-tree.md).

## Der Spielablauf im Coop

Ein Run läuft immer gleich ab. Diese Schleife ist das Herz des Spiels:

```
  ┌───────────────────────────────────────────────────────────┐
  │                                                           │
  ▼                                                           │
1. Erkunden  ─►  2. Kämpfen  ─►  3. Versorgen  ─►  4. Stärker werden  ─►  5. Tiefer gehen
```

1. **Erkunden:** Beide Spieler laufen durch eine Etage des Waldes. Jeder sieht auch, was der andere sieht. Laufen kostet keine Nahrung, man darf sich also ruhig aufteilen.
2. **Kämpfen:** Die Spieler treffen auf Banditen, Tiere und Fallen. Der Holzfäller hält die Gegner im Nahkampf auf, die Waldläuferin schießt aus der Distanz. Wer einen Baum fällt, kann ihn auf Gegner stürzen lassen. Aus Holz baut man Barrikaden, die Gegner aufhalten.
3. **Versorgen:** Jede Aktion (angreifen, hacken, bauen) kostet Nahrung. Nahrung bekommt man, indem man Tiere jagt und das Fleisch am Lagerfeuer kocht. In Hütten findet man Beute, manchmal aber auch Gegner.
4. **Stärker werden:** Beide Spieler bekommen dieselbe Erfahrung und steigen gleichzeitig auf. Pro Aufstieg gibt es einen Skillpunkt für eine neue Fähigkeit.
5. **Tiefer gehen:** Am Ausgang geht es gemeinsam zur nächsten Etage. Dort wird der Wald gefährlicher.

**Wenn es schiefgeht:** Fallen die Lebenspunkte eines Spielers auf null, stirbt er nicht sofort, sondern liegt am Boden. Der Partner kann ihn wiederbeleben. Erst wenn beide am Boden liegen, ist der Run vorbei.

### Warum das im Coop funktioniert

- **Unterschiedliche Rollen:** Einer steht vorne, die andere hinten. Keiner schafft es allein so gut.
- **Man braucht sich:** Nur der Partner kann einen wiederbeleben. Nahrung ist knapp und muss geteilt werden.
- **Gemeinsamer Fortschritt:** Weil beide gleichzeitig aufsteigen, fällt niemand zurück.

## Technik

Das Spiel wird mit der Engine **Godot 4** gebaut.

| Thema | Entscheidung | In einfachen Worten |
| --- | --- | --- |
| Programmiersprache | GDScript mit Typangaben | Die Standardsprache von Godot. Läuft problemlos auf Android und iOS |
| Coop-Verbindung | Ein Handy ist der **Host** | Nur der Host berechnet das Spiel. Das zweite Handy schickt nur, was sein Spieler tun will, und zeigt das Ergebnis an. So können die beiden Spielstände nie voneinander abweichen |
| Verbindungsart | Erst WLAN, Internet später | Im selben WLAN ist es am einfachsten. Spielen über das Internet kommt nach dem Prototyp |
| Züge | Beide wählen gleichzeitig | In einer Runde wählen beide Spieler ihren Zug, dann passiert alles zusammen. Niemand muss auf den anderen warten. Außerhalb von Kämpfen läuft man frei herum |
| Spiellogik getrennt von der Grafik | Eigener Ordner `core/` | Die Regeln des Spiels wissen nichts von Bildern oder Animationen. Dadurch lassen sie sich leicht testen und über das Netzwerk abgleichen |
| Karte | Raster aus Kacheln mit 16×16 Pixeln | Pixel-Art wie in Shattered Pixel Dungeon |
| Spielwerte | In Datendateien (`.tres`) | Schaden, Lebenspunkte usw. lassen sich ändern, ohne Code anzufassen |
| Tests | GUT oder gdUnit4 | Automatische Tests prüfen, ob die Spielregeln stimmen |
| Automatischer Build | GitHub Actions | Bei jedem Push entsteht automatisch eine installierbare Android-App (APK) |

### Ordnerstruktur

```
waldgeist/
├── autoload/   Dinge, die immer da sind: Ereignisse, Netzwerk
├── core/       Spielregeln: Karte, Figuren, Runden, Kampf
├── data/       Spielwerte: Gegner, Fähigkeiten
├── scenes/     Bildschirme: Spielfeld, Anzeige, Lobby
├── net/        Verbindung zwischen Host und zweitem Handy
├── assets/     Grafiken und Sounds
└── tests/      Automatische Tests
```

## Meilensteine

Coop ist von Anfang an dabei. Wir bauen nicht erst ein Einzelspieler-Spiel und fügen den Coop später hinzu, denn das nachträglich einzubauen ist sehr aufwendig und fehleranfällig.

Die Zeiten sind grobe Schätzungen.

### M0 – Grundgerüst (1 Woche)

- Godot-Projekt mit der Ordnerstruktur anlegen
- Automatischen Build einrichten
- Zwei Handys verbinden sich im WLAN

✅ **Fertig, wenn:** Zwei Handys sind verbunden und jedes zeigt die Figur des anderen an.

### M1 – Gemeinsam laufen (2–3 Wochen)

- Der Wald wird zufällig erzeugt: Bäume, Lichtungen und ein Ausgang
- Zwei Spielfiguren, die man per Tippen bewegt
- Rundenablauf mit gleichzeitigen Zügen
- Bäume versperren die Sicht, beide Spieler teilen ihr Sichtfeld
- Am Ausgang geht es gemeinsam zur nächsten Etage

✅ **Fertig, wenn:** Zwei Spieler laufen zusammen durch drei Etagen, und auf beiden Handys ist immer genau dasselbe zu sehen.

### M2 – Der komplette Spielablauf (4–6 Wochen)

- Nahkampf und Fernkampf. Pfeile bleiben liegen und können wieder aufgesammelt werden
- Gegner der ersten Zone: Banditen, zwei bis drei Tierarten, Fallen
- Bäume fällen: Der Baum fällt in eine Richtung und verletzt alle Gegner in dieser Linie. Dabei gibt es Holz
- Barrikaden aus Holz bauen
- Nahrung: Aktionen kosten Nahrung. Wer hungert, verliert stattdessen Lebenspunkte
- Tiere jagen und am Lagerfeuer kochen
- Hütten mit Beute oder Gegnern
- Am Boden liegen und wiederbeleben
- Gemeinsame Erfahrung und gleichzeitiger Aufstieg

✅ **Fertig, wenn:** Zwei Spieler können einen ganzen Run durch die erste Zone spielen, vom Start bis zum Ende oder bis zur Niederlage.

### M3 – Klassen und Spaß-Test (3–4 Wochen)

- Holzfäller und Waldläuferin bekommen ihre drei festen Klassenfähigkeiten
- Eine kleine, feste Auswahl an Fähigkeiten, die man mit Skillpunkten lernt. Noch ohne Tafeln
- Darunter zwei bis drei Coop-Fähigkeiten, zum Beispiel:
  - *Schützender Ast:* Man fängt einen Teil des Schadens ab, den der Partner bekommt
  - *Heilende Hände:* Man belebt den Partner schneller wieder
- Mindestens eine Kombination, bei der beide zusammenarbeiten müssen. Beispiel: Der Holzfäller lockt Gegner in eine Reihe, die Waldläuferin fällt einen Baum auf genau diese Reihe
- Mehrere Paare spielen das Spiel zur Probe, danach wird nachgebessert

✅ **Fertig, wenn:** Die Testpaare nach einem Run von sich aus noch eine Runde spielen wollen. **Das ist das wichtigste Ziel des ganzen Plans.**

## Später (erst wenn M3 überzeugt)

In ungefähr dieser Reihenfolge:

1. Tafel-Skillsystem: Der Skillbaum wächst über viele Runs hinweg
2. Erste Edelsteine: Jade und Bernstein
3. Tiefere Zonen mit stärkerer Magie (Topas, Saphir, Rubin, Diamant) und ein Endboss
4. Spielen über das Internet, Tutorial, zwei Schwierigkeitsgrade
5. iOS-Version und Veröffentlichung in den App-Stores

**Nicht geplant:** Onyx (Nekromantie), mehr als zwei Spieler, weitere Klassen.

## Größte Risiken

| Was schiefgehen kann | Was wir dagegen tun |
| --- | --- |
| Auf den beiden Handys ist nicht mehr dasselbe zu sehen | Nur der Host berechnet das Spiel. Coop ist ab M1 dabei, damit Fehler früh auffallen |
| Zu zweit fühlen sich die Runden langsam an | Beide ziehen gleichzeitig. Außerhalb von Kämpfen läuft man frei |
| Das Spiel macht keinen Spaß | Ab M2 regelmäßig zur Probe spielen. Lieber Bestehendes verbessern als Neues hinzufügen |

## Offene Fragen

Diese Fragen klären wir beim Probespielen in M2 und M3:

- **Nahrung:** Wie knapp muss sie sein, damit sie spannend ist, aber nicht nervt?
- **Abstand:** Dürfen sich die Spieler beliebig weit voneinander entfernen?
- **Länge:** Wie lange soll ein Run durch die erste Zone dauern? Vorschlag: 15 bis 20 Minuten.
- **Wiederbeleben:** Wie oft darf man wiederbelebt werden, bevor es zu leicht wird?

## Arbeitsweise

- Wir arbeiten direkt auf `master`. Der Stand dort muss immer lauffähig sein.
- Bei jedem Push baut GitHub automatisch die App und führt die Tests aus.
- Nach jedem Meilenstein spielen wir zusammen und passen den Plan an.
