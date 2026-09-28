# Wald-Coop: Tafel-Skillsystem und Fähigkeitenpool

## Überblick

Der Skillbaum ist die einzige Meta-Progression: Vor dem Run baut man ihn aus gefundenen Tafeln (Skillbaumfragmente), im Run lernt man seine Fähigkeiten mit Skillpunkten. Level und Items beginnen jeden Run bei null.

![Holztafel leer (links) und mit Bernstein gesockelt (rechts)](images/tafeln.webp)

*Links eine leere Holztafel mit 3 Naturfähigkeiten und leerem Sockel, rechts dieselbe Tafel mit gesockeltem Bernstein.*

- **Tafeln:** werden in Runs gefunden und bleiben dauerhaft. Jede Holztafel trägt 3 zufällige Naturfähigkeiten eines Themas und einen leeren Sockel. Ein gesockelter Stein schaltet eine vierte, magische Fähigkeit frei und bindet die Tafel dauerhaft an ihren Platz.
- **Raster:** Tafeln sind viereckig und liegen auf einem Quadratraster um den Klassenstein (Kern). Benachbart sind Tafeln, die sich an einer Kante berühren (oben, unten, links, rechts), Diagonalen zählen nicht. Der Ring ist die Entfernung vom Kern in Schritten über Nachbarn, also Ring 1 sind die vier Tafeln direkt am Kern.
- **Coop-Deckel:** Die Tafelanzahl des schwächsten Spielers begrenzt alle. Stärkere wählen einen zusammenhängenden Ausschnitt, der am Kern hängt. Der Kern zählt nicht mit.
- **Skillpunkte:** Erfahrung wird geteilt, beide steigen gleichzeitig auf. Pro Level-Up gibt es 1 Skillpunkt, bis zum Endboss etwa 15.
- **Erreichbarkeit:** Tafeln am Kern sind von Anfang an erreichbar. Weitere Tafeln werden erreichbar, sobald in einer angrenzenden Tafel eine Fähigkeit gelernt wurde. Innerhalb einer erreichbaren Tafel lernt man frei.
- **Starke Tafeln:** Holztafeln mit starker Fähigkeit aus Zone 3 oder tiefer dürfen nur ab Ring 2 liegen. Gesockelte Tafeln müssen zusätzlich im Mindestring ihres Steins liegen.
- **Im Run:** Lernen nur außerhalb von Begegnungen, kein Umverteilen.

## Startklassen

Jeder Klassenstein trägt 3 feste Fähigkeiten mit je 3 Rängen, also 9 Punkte. Er ist immer dabei, sichert die Identität der Klasse und fängt überzählige Punkte auf, wenn ein Spieler nur wenige Tafeln hat.

### Holzfäller

Nahkampf und Tank. Startet mit einer Axt und trägt doppelt so viel Holz für Barrikaden.

| Fähigkeit | Wirkung | Ränge | Hook |
| --- | --- | --- | --- |
| Kräftiger Hieb | +1 Nahkampfschaden pro Rang. Ab Rang 2 fällt ein Baum mit 2 statt 3 Hieben, ab Rang 3 mit einem | 3 | `stat` |
| Zähigkeit | +4 maximale HP pro Rang | 3 | `stat` |
| Spalthieb (aktiv) | Trifft das Ziel und das Feld dahinter, +2 Schaden pro Rang, Abklingzeit 10 Runden | 3 | `active` |

### Waldläuferin

Fernkampf und Späherin. Startet mit einem Bogen und 10 Pfeilen, die man wieder aufsammeln kann.

| Fähigkeit | Wirkung | Ränge | Hook |
| --- | --- | --- | --- |
| Scharfes Auge | +1 Sichtweite pro Rang. Ab Rang 3 sieht sie durch Unterholz | 3 | `stat` |
| Leichtfüßig | +10 % Ausweichen pro Rang | 3 | `stat` |
| Durchschuss (aktiv) | Pfeil trifft alle Gegner in einer Linie bis zum nächsten Baum, +2 Schaden pro Rang, Abklingzeit 12 Runden | 3 | `active` |

## Thema Eiche: Verteidigung und Holz

Eiche hält die Linie und nutzt Bäume als Deckung und Waffe.

| Fähigkeit | Wirkung | Ränge | Hook |
| --- | --- | --- | --- |
| Rindenhaut | +1 Rüstung pro Rang, doppelt, solange ein Baum an dich grenzt | 3 | `stat` |
| Wurzelstand | Wartest du eine Runde, erhältst du 3 Schild pro Rang und kannst bis zu deinem nächsten Zug nicht zurückgestoßen werden | 2 | `on_wait` |
| Gezielter Fall | Du bestimmst die Fallrichtung gefällter Bäume, die Fall-Linie macht 50 % mehr Schaden | 1 | `on_tree_felled` |
| Bollwerk | Barrikaden halten doppelt so viel aus. Rang 2: Bauen kostet keine Runde | 2 | `stat` |
| Herausforderung (aktiv) | Gegner im Umkreis von 3 Feldern greifen 3 Runden lang bevorzugt dich an, Abklingzeit 15 Runden | 1 | `active` |
| Schützender Ast (Coop) | Wird ein angrenzender Verbündeter getroffen, übernimmst du 25 % pro Rang des Schadens | 2 | `on_ally_hit` |
| Eichenherz (stark) | Einmal pro Etage überlebst du einen tödlichen Treffer mit 1 HP und bist 2 Runden unverwundbar | 1 | `on_lethal_damage` |

## Thema Moos: Heilung und Wissen

Moos hält die Gruppe am Leben und macht Unbekanntes lesbar.

| Fähigkeit | Wirkung | Ränge | Hook |
| --- | --- | --- | --- |
| Moosbett | Außerhalb von Begegnungen regenerierst du 50 % schneller pro Rang | 2 | `on_tick` |
| Pilzkunde | Der erste unbekannte Pilz pro Etage wird beim Aufheben erkannt. Rang 2: die ersten zwei | 2 | `on_item_pickup` |
| Baumflüsterer | Du erkennst getarnte Baumwesen in deinem Sichtfeld, und sie werden auch für deinen Mitspieler markiert | 1 | `on_fov_update` |
| Sporenwolke (aktiv) | Heilt dich und angrenzende Verbündete um 5 pro Rang, Abklingzeit 20 Runden | 3 | `active` |
| Heilende Hände (Coop) | Wiederbeleben dauert nur eine Runde, der Wiederbelebte steht mit 30 % statt 10 % HP auf | 1 | `on_revive` |
| Geteilte Mahlzeit (Coop) | Heilst du dich, erhält der nächste Verbündete im Umkreis von 5 Feldern 25 % pro Rang davon | 2 | `on_heal` |
| Wiedererblühen (stark) | Einmal pro Run: Wird ein Verbündeter niedergeschlagen, während du stehst, erhebt er sich nach 3 Runden selbst mit 50 % HP | 1 | `on_ally_downed` |

## Edelsteine

Ein Stein im Sockel einer Holztafel schaltet deren vierte, magische Fähigkeit frei. Je tiefer die Zone, desto stärker und dunkler die Magie.

| Stufe | Steine | Magie | Fundort | Mindestring |
| --- | --- | --- | --- | --- |
| Natur | Jade, Bernstein | Wachstum, Heilung, Bewahren, leichte Beschwörung | mittlere Zonen | 1 |
| Elementar | Topas | Feuer und Blitz | höhere Zonen | 2 |
| Dunkel | Rubin, Onyx | Blut, Nekromantie | letzte Ebenen | 3 |
| Licht | Diamant | starke Heilung, Auferstehung | letzte Ebenen | 3 |

- **Sockeln:** nur zwischen den Runs im Charakterbildschirm, mit klarer Bestätigung.
- **Bindung:** Eine gesockelte Tafel bleibt fest an ihrem Platz, lässt sich nicht entfernen oder tauschen und leuchtet in der Farbe ihres Steins.
- **Tauschen:** Lose Steine und ungesockelte Tafeln bleiben tauschbar.
- **Lernen im Run:** Die Magiefähigkeit ist lernbar, sobald 2 der 3 Holzfähigkeiten der Tafel gelernt sind. Magiefähigkeiten haben einen Rang.
- **Coop:** Mindestring und Deckel greifen ineinander. Hat der schwächste Spieler weniger als 3 Tafeln, bleiben dunkle Magie und Diamanten für alle unerreichbar.
- **Notausgang:** Ein sehr seltener Gegenstand vom Endboss zieht die Magie heraus. Der Stein wird zerstört, die Tafel wird frei.
- **Darstellung:** Bernstein honigbraun und trüb, Topas klar zitronengelb, damit man sie auf dem Handy auseinanderhält.

### Jade: Wachstum und Heilung

| Fähigkeit | Wirkung | Hook |
| --- | --- | --- |
| Rankenschlag (aktiv) | Wurzeln halten alle Gegner im Umkreis von 2 Feldern eine Runde fest, Abklingzeit 20 Runden | `active` |
| Schössling (aktiv) | Lässt auf einem leeren Feld sofort einen Baum wachsen, Abklingzeit 15 Runden | `active` |
| Keimkraft | Heilpilze und Sporenwolke heilen 50 % mehr | `on_heal` |
| Grüner Schutz (Coop) | Verbündete im Umkreis von 2 Feldern regenerieren in Begegnungen 1 HP pro Runde | `on_turn_start` |

### Bernstein: Bewahren und Erwecken

Bernstein ist versteinertes Baumharz und trägt die Erinnerung der Bäume. Er schließt Gegner ein und erweckt Bäume für kurze Zeit zu stationären Wächtern. Baumwächter bleiben Holz: Feuer verletzt sie.

| Fähigkeit | Wirkung | Hook |
| --- | --- | --- |
| Harzfalle (aktiv) | Schließt einen Gegner 3 Runden in Harz ein. Er kann nicht handeln, nimmt aber auch keinen Schaden. Abklingzeit 20 Runden | `active` |
| Harzhaut | Der erste Treffer jeder Begegnung wird vollständig abgefangen | `on_hit_taken` |
| Klebriges Harz (Coop) | Von dir getroffene Gegner sind 2 Runden verlangsamt, Verbündete treffen sie mit 15 % höherer Chance | `on_attack` |
| Baumwächter (aktiv) | Erweckt einen Baum in Sichtweite für 8 Runden. Er bewegt sich nicht und greift jede Runde einen Gegner im Umkreis von 2 Feldern mit 4 Schaden an. Abklingzeit 25 Runden | `active` |
| Letzter Fall | Endet ein Baumwächter, fällt der Baum auf den nächsten Gegner und trifft die ganze Fall-Linie | `on_summon_end` |

### Topas: Feuer und Blitz

Topas ist der Gewitterstein. Blitz ist stark gegen nasse Gegner, Feuer gegen trockene, und Blitze können den Wald entzünden. Jede Topas-Tafel macht ihren Träger zusätzlich halb feuerfest, damit Friendly Fire beherrschbar bleibt.

| Fähigkeit | Element | Wirkung | Hook |
| --- | --- | --- | --- |
| Flammenwand (aktiv) | Feuer | Setzt eine Reihe von 5 Feldern für 5 Runden in Brand, Abklingzeit 20 Runden | `active` |
| Hitzewelle (aktiv) | Feuer | Stößt alle angrenzenden Gegner ein Feld zurück und setzt sie in Brand, Abklingzeit 15 Runden | `active` |
| Kettenblitz (aktiv) | Blitz | Springt auf bis zu 3 Gegner, doppelter Schaden gegen nasse, Abklingzeit 15 Runden | `active` |
| Brandschneise | Feuer | Von dir gefällte Bäume fallen brennend und entzünden die ganze Fall-Linie | `on_tree_felled` |
| Gewitterzeichen | beide | Deine Angriffe markieren Gegner, der dritte Treffer entlädt einen Blitz mit 8 Schaden und entzündet das Feld | `on_attack` |
| Glut schüren (Coop) | Feuer | Brennende Gegner nehmen 25 % mehr Schaden von deinen Verbündeten | `on_damage` |
| Leiter (Coop) | Blitz | Deine Blitze springen auch über Verbündete weiter, ohne ihnen zu schaden, und verdoppeln so ihre Reichweite | `stat` |
| Waldbrandherz | Feuer | Dein Feuer breitet sich doppelt so schnell aus und verletzt keine Verbündeten | `on_fire_spread` |

### Rubin: Blutmagie

Dunkle Magie ist am stärksten, hat aber immer einen Preis.

| Fähigkeit | Wirkung | Hook |
| --- | --- | --- |
| Aderlass | Aktive Fähigkeiten kosten 10 % deiner maximalen HP statt Abklingzeit | `stat` |
| Blutdurst | 20 % des Schadens, den du verursachst, heilen dich | `on_damage` |
| Blutopfer (aktiv, Coop) | Opfere 30 % deiner HP, ein niedergeschlagener Verbündeter steht sofort auf | `active` |

### Onyx: Nekromantie (spätere Ausbaustufe)

Beschwörungen brauchen eigene KI, gehören einem Spieler und müssen synchronisiert werden. Onyx kommt deshalb erst nach dem Prototyp. Die stationären Baumwächter des Bernsteins sind ein guter Testlauf dafür, weil sie ohne Wegfindung auskommen.

| Fähigkeit | Wirkung | Hook |
| --- | --- | --- |
| Knochenruf (aktiv) | Ein getöteter Gegner kämpft 10 Runden lang auf deiner Seite, Abklingzeit 25 Runden | `active` |
| Todeshauch | Gegner unter 15 % HP sterben bei deinem Treffer sofort | `on_attack` |
| Seelenernte | Jeder Gegner, der im Umkreis von 3 Feldern stirbt, senkt deine Abklingzeiten um eine Runde | `on_kill` |

### Diamant: Licht und Auferstehung

Der Diamant ist das Gegenstück zur dunklen Magie: Er kostet nichts, ist dafür aber selten nutzbar, mit langen Abklingzeiten oder nur einmal pro Etage oder Run. Stärker als Wiederblühen aus dem Moos, weil er sofort wirkt.

| Fähigkeit | Wirkung | Hook |
| --- | --- | --- |
| Auferstehung (aktiv, Coop) | Holt einen niedergeschlagenen Verbündeten in Sichtweite sofort mit voller HP zurück, einmal pro Etage | `active` |
| Lichtquell (aktiv) | Heilt dich und alle Verbündeten in Sichtweite vollständig, Abklingzeit 40 Runden | `active` |
| Reinigung (aktiv) | Entfernt Gift, Brand und Verlangsamung von dir und Verbündeten im Umkreis von 3 Feldern und macht 2 Runden immun dagegen, Abklingzeit 20 Runden | `active` |
| Diamanthaut (Coop) | Fällt ein Verbündeter im Umkreis von 5 Feldern unter 25 % HP, erhält er einmal pro Begegnung einen Schild von 15 | `on_ally_hit` |
| Wiedergeburt | Einmal pro Run: Würdest du als letzter stehender Spieler niedergeschlagen, stehst du mit 50 % HP auf, alle Verbündeten mit 25 % | `on_lethal_damage` |

## Thema Dorn: Gift, Fallen und Kontrolle

Dorn kontrolliert das Feld und macht Schaden über Zeit.

| Fähigkeit | Wirkung | Ränge | Hook |
| --- | --- | --- | --- |
| Giftdorn | Angriffe vergiften: 1 Schaden pro Rang und Runde, 5 Runden lang | 3 | `on_attack` |
| Stachelpanzer | Wer dich im Nahkampf trifft, erleidet 2 Schaden pro Rang | 2 | `on_hit_taken` |
| Fallensteller | Du siehst versteckte Fallen im Umkreis von 3 Feldern. Rang 2: Du kannst Fallen aufnehmen und neu legen | 2 | `on_fov_update` |
| Dornenhecke (aktiv) | Pflanzt eine Hecke, die Gegner verlangsamt und vergiftet. 2 Ladungen pro Etage, Rang 2: 4 | 2 | `active` |
| Rankenfessel (aktiv) | Wurzelt einen Gegner in Sichtweite 3 Runden fest, Abklingzeit 20 Runden | 1 | `active` |
| Umschlingen (Coop) | Trifft ein Verbündeter einen von dir vergifteten Gegner, wird dieser eine Runde festgewurzelt, höchstens alle 5 Runden pro Gegner | 1 | `on_damage` |
| Dornenkrone (stark) | Gift stapelt sich unbegrenzt, ab 10 Stapeln platzt der Gegner und vergiftet alle Nachbarn | 1 | `on_status_applied` |

## Würfelregeln

Jede Holztafel zieht 3 verschiedene Fähigkeiten aus den 6 normalen ihres Themas und hat einen leeren Sockel. Jeder Stein trägt eine zufällige Fähigkeit aus dem Pool seines Typs. Das Ergebnis wird beim Fund gespeichert, nicht als Seed.

- Höchstens eine aktive Fähigkeit pro Tafel.
- Holztafeln aus Zone 3 oder tiefer können einen Platz gegen die starke Fähigkeit ihres Themas tauschen und dürfen nur ab Ring 2 liegen.
- Gefundene Tafeln sind bis zum Ende des Runs verwittert und unleserlich. Gespeichert werden sie trotzdem sofort beim Fund.
- Auf dem Handy gibt es 3 Schnelltasten, also höchstens 3 aktive Fähigkeiten pro Run, Kern- und Magiefähigkeiten eingeschlossen.

### Duplikate

Kommt eine Fähigkeit auf mehreren Tafeln im Ausschnitt vor, wirkt die zweite Kopie je nach Art unterschiedlich.

| Art der Fähigkeit | Wirkung der zweiten Kopie |
| --- | --- |
| Mit Rängen | Maximalrang steigt um 1 |
| Ohne Ränge | Alternativer Zugang: einmal lernbar, über die gerade erreichbare Tafel |
| Aktiv | Abklingzeit sinkt um 30 % |

## Coop-Kombos

Die stärksten Momente entstehen, wenn die Fähigkeit des einen Spielers die Aktion des anderen verstärkt.

| Kombo | Spieler A | Spieler B | Ergebnis |
| --- | --- | --- | --- |
| Köder und Hecke | Herausforderung | Dornenhecke vor A | Gegner laufen durch die Hecke zum Tank |
| Baumfalle | Herausforderung zieht Gegner in eine Reihe | Gezielter Fall | Der Baum trifft die ganze Reihe |
| Baum aus dem Nichts | Schössling (Jade) | Gezielter Fall | Ein frisch gewachsener Baum wird sofort auf die Gegner gefällt |
| Wächter aus dem Nichts | Schössling (Jade) | Baumwächter (Bernstein) | Ein frisch gewachsener Baum wird mitten im Kampf zum Wächter |
| Brand und Verstärkung | Glut schüren (Topas) | Hitzewelle (Topas) | B setzt Gegner in Brand und macht 25 % mehr Schaden gegen sie |
| Gift und Fessel | Giftdorn und Umschlingen | beliebige Treffer | Vergiftete Gegner werden festgewurzelt |
| Harz und Klinge | Klebriges Harz (Bernstein) | Nahkampf | B trifft verlangsamte Gegner zuverlässiger |
| Blutopfer | Blutopfer (Rubin) | liegt am Boden | A opfert HP, B steht sofort wieder auf |
| Licht und Blut | Aderlass (Rubin) | Lichtquell (Diamant) | A bezahlt Fähigkeiten mit HP, B füllt sie wieder auf |

Viele starke Kombos brauchen Steine bei beiden Spielern. Das macht es lohnend, sich vor dem Run abzusprechen oder lose Steine zu verschenken.

## Effektsystem

Der Pool braucht 20 Hooks. Die Coop-Fähigkeiten hängen an globalen Hooks, die auf Ereignisse aller Akteure reagieren, deshalb braucht das Effektsystem von Anfang an einen zentralen Ereignisbus.

| Hook | Auslöser | Reichweite | Beispiele |
| --- | --- | --- | --- |
| `stat` | kein Ereignis, dauerhafter Modifikator | eigener Charakter | Rindenhaut, Zähigkeit, Aderlass |
| `active` | Spieler löst die Fähigkeit aus | eigener Charakter | Spalthieb, Baumwächter, Auferstehung |
| `on_attack` | eigener Angriff | eigener Charakter | Giftdorn, Klebriges Harz, Gewitterzeichen |
| `on_hit_taken` | eigener Charakter wird getroffen | eigener Charakter | Stachelpanzer, Harzhaut |
| `on_kill` | Gegner stirbt in der Nähe | eigener Charakter | Seelenernte |
| `on_wait` | eigener Charakter wartet eine Runde | eigener Charakter | Wurzelstand |
| `on_tick` | globaler Tick außerhalb von Begegnungen | eigener Charakter | Moosbett |
| `on_turn_start` | neue Runde in einer Begegnung | eigener Charakter | Grüner Schutz |
| `on_tree_felled` | eigener Charakter fällt einen Baum | eigener Charakter | Gezielter Fall, Brandschneise |
| `on_summon_end` | eigene Beschwörung endet | eigener Charakter | Letzter Fall |
| `on_item_pickup` | eigener Charakter hebt etwas auf | eigener Charakter | Pilzkunde |
| `on_fov_update` | Sichtfeld wird neu berechnet | eigener Charakter | Baumflüsterer, Fallensteller |
| `on_heal` | eigener Charakter wird geheilt | eigener Charakter | Geteilte Mahlzeit, Keimkraft |
| `on_lethal_damage` | eigener Charakter würde sterben | eigener Charakter | Eichenherz, Wiedergeburt |
| `on_status_applied` | eigener Charakter setzt einen Zustand | eigener Charakter | Dornenkrone |
| `on_fire_spread` | Feuer breitet sich aus | Welt | Waldbrandherz |
| `on_damage` | beliebiger Akteur macht Schaden | alle Akteure | Glut schüren, Umschlingen, Blutdurst |
| `on_ally_hit` | Verbündeter wird getroffen | alle Akteure | Schützender Ast, Diamanthaut |
| `on_ally_downed` | Verbündeter wird niedergeschlagen | alle Akteure | Wiederblühen |
| `on_revive` | Wiederbelebung beginnt | alle Akteure | Heilende Hände |

## Offene Punkte

Alle Zahlen sind erste Schätzungen und müssen im Playtest geprüft werden.

- [ ] Skillpunkte pro Run festlegen, Annahme bisher etwa 15 bis zum Endboss
- [ ] Deckel im Playtest prüfen, eventuell lockern auf Minimum plus eine Tafel
- [ ] Drop-Raten für Tafeln und Steine pro Zone festlegen
- [ ] Seltenheit des Notausgangs festlegen, der Steine wieder entfernt
- [ ] Viertes Holzthema ergänzen, Vorschlag: Jagd für Beweglichkeit und Fernkampf
- [ ] Gestrichene Glut-Fähigkeiten (Glimmende Klinge, Feuerfest, Funkenflug) eventuell als weitere Topas-Fähigkeiten zurückholen
- [ ] Onyx nach dem Prototyp umsetzen, sobald Beschwörungen mit KI und Synchronisierung stehen
- [ ] Pool auf 10 bis 12 Fähigkeiten pro Holzthema ausbauen, damit Tafeln sich weniger ähneln
- [ ] Kern-Fähigkeiten für weitere Klassen entwerfen
