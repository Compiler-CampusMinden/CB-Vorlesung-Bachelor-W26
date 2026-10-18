# IFM 3.1: Compilerbau (Winter 2026/27)

<a id="id-da39a3ee5e6b4b0d3255bfef95601890afd80709"></a>

## Syllabus

### Syllabus

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/admin/images/architektur_cb_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/admin/images/architektur_cb.png" width="80%" /></picture></p>

#### Kursbeschreibung

Der Compiler ist das wichtigste Werkzeug in der Informatik. In der Königsdisziplin der Informatik schließt sich der Kreis, hier kommen die unterschiedlichen Algorithmen und Datenstrukturen und Programmiersprachenkonzepte zur Anwendung.

In diesem Modul geht es um ein grundlegendes Verständnis für die wichtigsten Konzepte im Compilerbau. Wir schauen uns dazu relevante aktuelle Tools und Frameworks an und setzen diese bei der Erstellung eines kleinen Compiler-Frontends für C++ ein.

#### Überblick Modulinhalte

1.  Lexikalische Analyse: Scanner/Lexer
    -   Reguläre Sprachen
    -   Generierung mit ANTLR
2.  Syntaxanalyse: Parser
    -   Kontextfreie Grammatiken (CFG)
    -   LL-Parser (Recursive-Descent-Parser)
    -   Generierung mit ANTLR
3.  Semantische Analyse: Symboltabellen, Name-Resolving, Type-Checking
    -   Namen und Scopes
    -   Typen, Klassen, Polymorphie
4.  Interpreter: AST-Traversierung
5.  C++ als zu verarbeitende Programmiersprache

#### Team

-   [BC George](https://www.hsbi.de/minden/ueber-uns/personenverzeichnis/birgit-christina-george)
-   [Carsten Gips](https://www.hsbi.de/minden/ueber-uns/personenverzeichnis/carsten-gips) (Sprechstunde nach Vereinbarung)
-   Alesia Herbertz, Vivien Traue, Jonathan Hauer (Tutor:innen)

#### Kursformat

| Vorlesung (2 SWS)            | Praktikum (2 SWS)                        |
|:-----------------------------|:-----------------------------------------|
| Mo, 15:45 - 17:15 Uhr (Zoom) | G1: Mi, 11:30 - 13:00 Uhr (Zoom)         |
|                              | G2: Mi, 14:00 - 15:30 Uhr (Zoom)         |
|                              | G3: Mi, 11:30 - 13:00 Uhr (Präsenz D327) |
|                              | G4: Mi, 14:00 - 15:30 Uhr (Präsenz D327) |

Durchführung der Vorlesung als *Flipped Classroom* (Carsten) bzw. als *reguläre Vorlesung* (BC). **Zugangsdaten Zoom siehe [ILIAS](https://www.hsbi.de/elearning/goto.php/crs/1702066)**.

#### Fahrplan

| Monat | Woche vom | Vorlesung (Mo) | Praktikum (Mi) | Edmonton/Minden-Meetings |
|:----|:----|:----------------------------------|:----------|:-----------------|
| Oktober | 12.10 | Orga \|\| [Überblick](#id-1df70a478502c95d7f7d1fa78f328a16640b3907) \| [Sprachen](#id-55cceb8d95eb4d3a67a6a18eb0e778b5695106a3) \| [Anwendungen](#id-315c4a6ad46ecd3cdd06bff39540497383b62904) | \- |  |
|  | 19.10. | [Reguläre Sprachen 1](#id-cb0be27b07154b726b38212279070d854085a4f8) | [B01](#id-6f673c2e093cdfc53b1f78baef11fd06cc8aa415) |  |
|  | 26.10. | [Reguläre Sprachen 2](#id-e6527ca4572a4752c431b91119c87d78fa032789) \|\| [CFG](#id-7c0e930dad216729eeb1545678306ab9f0d6a57b) | [B02](#id-0db349230022c35e045dc3b052a4faea50fe5f40) |  |
| November | 02.11. | [LL-Parser (Theorie)](#id-9aa1932181298bc40b56bb55a0bf53edf1c3aa88) | \- | **Di, 03.11., 17:00 - 18:00 Uhr (online): ANTLR + Live-Coding** |
|  | 09.11. | L-Int (Teil 1): [Intro](#id-92efa03f813e51c04582d54092121544fac6d2f6) \| [Grammatik](#id-5b9897e3373f2ab9cb2db150c803f2e2f79b5bc5) \| [Interpreter](#id-d8a55d89d69aa672b84f607d0f4d791de1cb29a3) | **Station 1** |  |
|  | 16.11. | L-Int (Teil 2): [Lexer](#id-ace9fbbfab20407167957763e345b4cb8130dc1b) \| [RD-Parser](#id-ebe1e096694731fb5532fdd9572b900a699e7eaf) | [B03: C-Int](#id-c6885788e4bb28c92d66417730e18408254766a3) |  |
|  | 23.11. | [L-Expr](#id-99cb41fc092ae89529d2cfd0281e8535a15dbfbf) | C-Expr |  |
| Dezember | 30.11. | [L-Var](#id-f8afd3e3cb9ba1df1e2b5422b50bc32aaa3350c7) | **Station 2** | **Mo, 30.11., 17:00 - 18:00 Uhr (online): Minden Presentations** |
|  | 07.12. | [L-If](#id-51bc1e52157fd6e296d145056356183c5f0620e5) | C-Var, C-If | **Mo, 07.12., 17:00 - 18:00 Uhr (online): Edmonton Presentations** |
|  | 14.12. | [L-Fun](#id-d5e1d626434b082efe6661d1c3dbae47343aaa38) | C-Fun |  |
|  | *21.12.* | **Weihnachtspause** | \- |  |
|  | *28.12.* | **Weihnachtspause** | \- |  |
| Januar | 04.01. | [L-Class](#id-883bc38559a6155ddb7626d1df4fa119f5b21780) | **Station 3** |  |
|  | 11.01. | [L-Inherit](#id-19c98c0c74958c63b88445f7d53a44f314a603cd) | C-Class, C-Inherit |  |
|  | 18.01. | [L-Self](#id-6d845692d09d49d96e2c03493d4e812a0d48d712) | C-Self, Snake |  |
|  | 25.01. | Rückblick | **Projektvorstellung** (Video) |  |

#### Prüfungsform, Note und Credits

**Parcoursprüfung plus Studienleistung (Portfolio)**, 5 ECTS

##### **Studienleistung**: "Portfolio"

Die Studienleistung ist eine unbenotete Leistung und setzt sich aus mehreren Komponenten zusammen:

1.  Pro Person: Teilnahme an **mind. zwei Edmonton/Minden-Terminen** mit aktiver Beteiligung und Abgabe eines ausreichenden **Post Mortems** (pro Meeting, je Person)
    -   Termin 1: Dienstag, 03.11., 17:00 - 18:00 Uhr (online)
    -   Termin 2: Montag, 30.11., 17:00 - 18:00 Uhr (online)
    -   Termin 3: Montag, 07.12., 17:00 - 18:00 Uhr (online)
    -   Abgabe der Post Mortems (s.u.) zu den Edmonton-Meetings jeweils bis Montag 09:00 Uhr in der Folgewoche im [ILIAS](https://www.hsbi.de/elearning/goto.php/exc/1737956)
2.  Pro Team: **Abschluss-Video-Vortrag** zum erfolgreich bearbeiteten **Snake-Mini-Projekt** (letztes Blatt) am Semesterende
    -   Termin: Mittwoch, 27.01., in Praktikumszeit (*Slots werden noch bekannt gegeben*)
    -   Video: 10 Minuten Dauer (pro Team)
    -   Anschließend kurzes Q&A (Fragen zum Snake-Mini-Projekt, pro Team)
    -   Vorführung des Videos und die Q&A findet pro Team statt, Anwesenheit erforderlich
    -   Abgabe des Videos und der Lösung zum Snake-Mini-Projekt bis Montag, 25.01., 09:00 Uhr im [ILIAS](https://www.hsbi.de/elearning/goto.php/exc/1737956)
    -   Teilleistung jeder Person im Team muss erkennbar sein

##### **Gesamtnote**: Parcoursprüfung

Die Modul-Note ergibt sich aus der Leistung in der Parcoursprüfung.

Sie können die Prüfung in der ersten oder in der zweiten Prüfungsphase ablegen. Die Stationen der Parcoursprüfung sind je nach Prüfungsphase unterschiedlich gestaltet und in sich geschlossen (kein Übertrag):

-   **Prüfungsphase I**: **Vier Stationen** (digitale E-Assessments im B40 mit je 30 Minuten Dauer), **beste drei Ergebnisse ergeben die Note**
    -   Station 1: Mittwoch, 11.11., in Praktikumszeit (*Slots werden noch bekannt gegeben*)
    -   Station 2: Mittwoch, 02.12., in Praktikumszeit (*Slots werden noch bekannt gegeben*)
    -   Station 3: Mittwoch, 06.01., in Praktikumszeit (*Slots werden noch bekannt gegeben*)
    -   Station 4: Im ersten Prüfungszeitraum (*Termin wird vom Prüfungsamt bekanntgegeben*)
-   **Prüfungsphase II**: **Digitale Klausur** im B40, Dauer 120 Minuten (*Termin wird vom Prüfungsamt bekanntgegeben*), **Klausurergebnis bestimmt die Note**

##### Hinweise

-   Die Parcoursprüfung und die Studienleistung sind zwei unabhängige Prüfungsleistungen und werden separat im CAT angemeldet. Die Studienleistung schaltet die Credits für das Modul frei, die Parcoursprüfung ergibt die Modulnote. Die Studienleistung ist **keine** Vorleistung für die Parcoursprüfung.
-   Die Bearbeitung der Aufgaben erfolgt im Team.
-   Ein Team umfasst 3 Personen.
-   Im Praktikum beginnen wir gemeinsam mit der Bearbeitung der Übungsblätter, diskutieren über Lösungsansätze und erarbeiten Abnahmekriterien. Die Lösung soll anschließend teamweise fertiggestellt werden und kann auf Wunsch im nächsten Praktikum von Ihnen vorgestellt werden.
-   Wir bauen schrittweise über die Übungsblätter hinweg einen Interpreter für einen Mini-C++-Dialekt auf. Sie benötigen diese schrittweise erarbeiteten Bausteine für das erfolgreiche Bearbeiten des Snake-Mini-Projekts (letztes Blatt) und damit das Bestehen der Studienleistung.
-   "Erfolgreiche Bearbeitung" umfasst die Bearbeitung aller Aufgaben im Zusammenhang des Snake-Mini-Projekts. Die intensive Beschäftigung mit den Aufgaben muss erkennbar sein.
-   Die Teilnahme am Praktikum ist freiwillig, wird aber deutlich empfohlen.
-   Eine Bewertung einzelner Übungsblätter findet nicht statt.
-   Die Post Mortems sind pro Edmonton-Meeting und individuell zu erstellen und abzugeben.
-   Das Video zum Snake-Mini-Projekt ist pro Team zu erstellen und einmal abzugeben unter Angabe der Teammitglieder.
-   "Aktive Beteiligung" umfasst Anwesenheit und sachbezogene Beiträge; Anwesenheit/Beteiligung werden dokumentiert.

<!-- -->

-   **Post Mortem**: Jede Person beschreibt individuell(!) die Teilnahme an den Edmonton/Minden-Meetings zurückblickend mit mind. 150 bis max. 400 Wörtern (Nutzlast! Überschriften und Links zählen nicht mit). Gehen Sie dabei aussagekräftig und nachvollziehbar auf folgende Punkte ein:

    1.  **Zusammenfassung**: Was wurde auf dem Meeting besprochen?
    2.  **Details**: Kurze Beschreibung besonders interessanter Aspekte.
    3.  **Reflexion**: Was war der schwierigste Teil? Wie haben Sie dieses Problem gelöst?
    4.  **Reflexion**: Was haben Sie gelernt oder (besser) verstanden?

    Die Post Mortems geben Sie bitte pro Person bis spätestens zur jeweiligen Deadline im [ILIAS](https://www.hsbi.de/elearning/goto.php/exc/1737956) ab.

#### Materialien

1.  ["**Crafting Interpreters**"](https://github.com/munificent/craftinginterpreters). Nystrom, R., Genever Benning, 2021. ISBN [978-0-9905829-3-9](https://fhb-bielefeld.digibib.net/openurl?isbn=978-0-9905829-3-9). [Online](https://www.craftinginterpreters.com/).
2.  ["**Introduction to Compiler Design**"](https://doi.org/10.1007/978-3-031-46460-7). Mogensen, T.A., Springer International, 2024. ISBN [978-3-031-46460-7](https://fhb-bielefeld.digibib.net/openurl?isbn=978-3-031-46460-7).

#### Förderungen und Kooperationen

##### Kooperation mit University of Alberta, Edmonton (Kanada)

Über das Projekt ["We CAN virtuOWL"](https://www.uni-bielefeld.de/international/profil/netzwerk/alberta-owl/we-can-virtuowl/) der Fachhochschule Bielefeld ist im Frühjahr 2021 eine Kooperation mit der [University of Alberta](https://www.hsbi.de/en/international-office/alberta-owl-cooperation) (Edmonton/Alberta, Kanada) im Modul "Compilerbau" gestartet.

Wir freuen uns, auch in diesem Semester wieder drei gemeinsame Sitzungen für beide Hochschulen anbieten zu können. (Diese Termine werden in englischer Sprache durchgeführt.)

------------------------------------------------------------------------

#### LICENSE

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, [this work](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor) by [BC George](https://github.com/bcg7), [Carsten Gips](https://github.com/cagix) and [contributors](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/graphs/contributors) is licensed under [CC BY-SA 4.0](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/blob/master/LICENSE.md). See the [credits](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/blob/master/CREDITS.md) for a detailed list of contributing projects.

<a id="id-af09e2fcaf4589921086150d991647b7b52abd03"></a>

## Vorlesungsunterlagen

<a id="id-352b1e776856b1ada7109fc42e5a89b3d7f98605"></a>

### Überblick

Was ist ein Compiler? Welche Bausteine lassen sich identifizieren, welche Aufgaben haben diese?

<a id="id-1df70a478502c95d7f7d1fa78f328a16640b3907"></a>

#### Struktur eines Compilers

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Compiler übersetzen (formalen) Text in ein anderes Format.
>
> Typischerweise kann man diesen Prozess in verschiedene Stufen/Phasen einteilen. Dabei verarbeitet jede Phase den Output der vorangegangenen Phase und erzeugt ein (kompakteres) Ergebnis, welches an die nächste Phase weitergereicht wird. Dabei nimmt die Abstraktion von Stufe zu Stufe zu: Der ursprüngliche Input ist ein Strom von Zeichen, daraus wird ein Strom von Wörtern (Token), daraus ein Baum (Parse Tree), Zwischencode (IC), ...
>
> <p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/architektur_cb_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/architektur_cb.png"  /></picture></p>
>
> Die gezeigten Phasen werden traditionell unterschieden. Je nach Aufgabe können verschiedene Stufen zusammengefasst werden oder sogar gar nicht auftreten.
>
> </details>

> [!TIP]
>
> <details open>
> <summary><strong>🎦 Videos</strong></summary>
>
> Vorlesung \[[YT](https://youtu.be/Fubh-W3uN48)\], \[[HSBI](https://www.hsbi.de/medienportal/album/video/cb-struktur-eines-compilers/457e730cef493e2ecc643113be3c4f91/253)\]
>
> </details>

##### Sprachen verstehen, Texte transformieren

> The cat runs quickly.

=\> Struktur? Bedeutung?

Wir können hier (mit steigender Abstraktionsstufe) unterscheiden:

-   Sequenz von Zeichen

-   Wörter: Zeichenketten mit bestimmten Buchstaben, getrennt durch bestimmte andere Zeichen; Wörter könnten im Wörterbuch nachgeschlagen werden

-   Sätze: Anordnung von Wörtern nach einer bestimmten Grammatik, Grenze: Satzzeichen

    Hier (vereinfacht): Ein Satz besteht aus Subjekt und Prädikat. Das Subjekt besteht aus einem oder keinen Artikel und einem Substantiv. Das Prädikat besteht aus einem Verb und einem oder keinem Adverb.

-   Sprache: Die Menge der in einer Grammatik erlaubten Sätze

##### Compiler: Big Picture

<p align="center"><img src="https://github.com/munificent/craftinginterpreters/blob/master/site/image/a-map-of-the-territory/mountain.png?raw=true" width="80%" /></p>

Quelle: [A Map of the Territory (mountain.png)](https://github.com/munificent/craftinginterpreters/blob/master/site/image/a-map-of-the-territory/mountain.png) by [Bob Nystrom](https://github.com/munificent) on Github.com ([MIT](https://github.com/munificent/craftinginterpreters/blob/master/LICENSE))

**Begriffe und Phasen**

Die obige Bergsteige-Metapher kann man in ein nüchternes Ablaufdiagramm mit verschiedenen Stufen und den zwischen den Stufen ausgetauschten Artefakten übersetzen:

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/architektur_cb_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/architektur_cb.png" width="70%" /></picture></p>

###### Frontend, Analyse

Die ersten Stufen eines Compilers, die mit der **Analyse** des Inputs beschäftigt sind. Dies sind in der Regel der Scanner, der Parser und die semantische Analyse.

-   Scanner, Lexer, Tokenizer, Lexikalische Analyse

    Zerteilt den Zeichenstrom in eine Folge von Wörtern. Mit regulären Ausdrücken kann definiert werden, was Klassen gültiger Wörter ("Token") sind. Ein Token hat i.d.R. einen Namen und einen Wert.

-   Parser, Syntaxanalyse

    Der Parser erhält als Eingabe die Folge der Token und versucht mit Hilfe einer Grammatik zu bestimmen, ob es sich bei der Tokensequenz um gültige Sätze im Sinne der Grammatik handelt. Hier gibt es viele Algorithmen, die im Wesentlichen in die Klassen "top-down" und "bottom-up" fallen.

-   Semantische Analyse, Kontexthandling

    In den vorigen Stufen wurde eher lokal gearbeitet. Hier wird über den gesamten Baum und die Symboltabelle hinweg geprüft, ob beispielsweise Typen korrekt verwendet wurden, in welchen Scope ein Name gehört etc. Mit diesen Informationen wird der AST angereichert.

-   Symboltabellen

    Datenstrukturen, um Namen, Werte, Scopes und weitere Informationen zu speichern. Die Symboltabellen werden vor allem beim Parsen befüllt und bei der semantischen Analyse gelesen, aber auch der Lexer benötigt u.U. diese Informationen.

###### Backend, Synthese

Die hinteren Stufen eines Compilers, die mit der **Synthese** der Ausgabe beschäftigt sind. Dies sind in der Regel verschiedene Optimierungen und letztlich die Code-Generierung

-   Codegenerierung

    Erzeugung des Zielprogramms aus der (optimierten) Zwischendarstellung. Dies ist oft Maschinencode, kann aber auch C-Code oder eine andere Ziel-Sprache sein.

-   Optimierung

    Diverse Maßnahmen, um den resultierenden Code kleiner und/oder schneller zu gestalten.

-   Symboltabellen

    Datenstrukturen, um Namen, Werte, Scopes und weitere Informationen zu speichern. Die Symboltabellen werden vor allem beim Parsen befüllt und bei der semantischen Analyse gelesen, aber auch der Lexer benötigt u.U. diese Informationen.

###### Weitere Begriffe

-   Parse Tree, Concrete Syntax Tree

    Repräsentiert die Struktur eines Satzes, wobei jeder Knoten dem Namen einer Regel der Grammatik entspricht. Die Blätter bestehen aus den Token samt ihren Werten.

-   AST, (Abstract) Syntax Tree

    Vereinfachte Form des Parse Tree, wobei der Bezug auf die Element der Grammatik (mehr oder weniger) weggelassen wird.

-   Annotierter AST

    Anmerkungen am AST, die für spätere Verarbeitungsstufen interessant sein könnten: Typ-Informationen, Optimierungsinformationen, ...

-   Zwischen-Code, IC

    Zwischensprache, die abstrakter ist als die dem AST zugrunde liegenden Konstrukte der Ausgangssprache. Beispielsweise könnten `while`-Schleifen durch entsprechende Label und Sprünge ersetzt werden. Wie genau dieser Zwischen-Code aussieht, muss der Compilerdesigner entscheiden. Oft findet man den Assembler-ähnlichen "3-Adressen-Code".

-   Sprache

    Eine Sprache ist eine Menge gültiger Sätze. Die Sätze werden aus Wörtern gebildet, diese wiederum aus Zeichenfolgen.

-   Grammatik

    Eine Grammatik beschreibt formal die Syntaxregeln für eine Sprache. Jede Regel in der Grammatik beschreibt dabei die Struktur eines Satzes oder einer Phrase.

##### Lexikalische Analyse: Wörter ("*Token*") erkennen

Die lexikalische Analyse (auch *Scanner* oder *Lexer* oder *Tokenizer* genannt) zerteilt den Zeichenstrom in eine Folge von Wörtern ("*Token*"). Die geschieht i.d.R. mit Hilfe von *regulären Ausdrücken*.

Dabei müssen unsinnige/nicht erlaubte Wörter erkannt werden.

Überflüssige Zeichen (etwa Leerzeichen) werden i.d.R. entfernt.

    sp = 100;

    <ID, sp>, <OP, =>, <INT, 100>, <SEM>

*Anmerkung*: In der obigen Darstellung werden die Werte der Token ("*Lexeme*") zusammen mit den Token "gespeichert". Alternativ können die Werte der Token auch direkt in der Symboltabelle gespeichert werden und in den Token nur der Verweis auf den jeweiligen Eintrag in der Tabelle.

##### Syntaxanalyse: Sätze erkennen

In der Syntaxanalyse (auch *Parser* genannt) wird die Tokensequenz in gültige Sätze unterteilt. Dazu werden in der Regel *kontextfreie Grammatiken* und unterschiedliche Parsing-Methoden (*top-down*, *bottom-up*) genutzt.

Dabei müssen nicht erlaubte Sätze erkannt werden.

    <ID, sp>, <OP, =>, <INT, 100>, <SEM>

``` lex
statement : assign SEM ;
assign : ID OP INT ;
```

                       statement                  =
                       /       \                 / \
                   assign      SEM             sp  100
                 /   |   \      |
               ID    OP  INT    ;
               |     |    |
               sp    =   100

Mit Hilfe der Produktionsregeln der Grammatik wird versucht, die Tokensequenz zu erzeugen. Wenn dies gelingt, ist der Satz (also die Tokensequenz) ein gültiger Satz im Sinne der Grammatik. Dabei sind die Token aus der lexikalischen Analyse die hier betrachteten Wörter!

Dabei entsteht ein sogenannter *Parse-Tree* (oder auch "*Syntax Tree*"; in der obigen Darstellung der linke Baum). In diesen Bäumen spiegeln sich die Regeln der Grammatik wider, d.h. zu einem Satz kann es durchaus verschiedene Parse-Trees geben.

Beim *AST* ("*Abstract Syntax Tree*") werden die Knoten um alle später nicht mehr benötigten Informationen bereinigt (in der obigen Darstellung der rechte Baum).

*Anmerkung*: Die Begriffe werden oft nicht eindeutig verwendet. Je nach Anwendung ist das Ergebnis des Parsers ein AST oder ein Parse-Tree.

*Anmerkung*: Man könnte statt `OP` auch etwa ein `ASSIGN` nutzen und müsste dann das "`=`" nicht extra als Inhalt speichern, d.h. man würde die Information im Token-Typ kodieren.

##### Vorschau: Parser implementieren

``` lex
stat : assign | ifstat | ... ;
assign : ID '=' expr ';' ;
```

``` java
void stat() {
    switch (<<current token>>) {
        case ID : assign(); break;
        case IF : ifstat(); break;
        ...
        default : <<raise exception>>
    }
}
void assign() {
    match(ID);
    match('=');
    expr();
    match(';');
}
```

Der gezeigte Parser ist ein sogenannter "LL(1)"-Parser und geht von oben nach unten vor, d.h. ist ein Top-Down-Parser.

Nach dem Betrachten des aktuellen Tokens wird entschieden, welche Alternative vorliegt und in die jeweilige Methode gesprungen.

Die `match()`-Methode entspricht dabei dem Erzeugen von Blättern, d.h. hier werden letztlich die Token der Grammatik erkannt.

##### Semantische Analyse: Bedeutung erkennen

In der semantischen Analyse (auch *Context Handling* genannt) wird der AST zusammen mit der Symboltabelle geprüft. Dabei spielen Probleme wie Scopes, Namen und Typen eine wichtige Rolle.

Die semantische Analyse ist direkt vom Programmierparadigma der zu übersetzenden Sprache abhängig, d.h. müssen wir beispielsweise das Konzept von Klassen verstehen?

Als Ergebnis dieser Phase entsteht typischerweise ein *annotierter AST*.

``` c
{
    int x = 42;
    {
        int x = 7;
        x += 3;    // ???
    }
}
```

                                                  = {type: real, loc: tmp1}
    sp = 100;                                    / \
                                                /   \
                                              sp     inttofloat
                                      {type: real,       |
                                       loc: var b}      100

##### Zwischencode generieren

Aus dem annotierten AST wird in der Regel ein Zwischencode ("*Intermediate Code*", auch "IC") generiert. oft findet man hier den Assembler-ähnlichen "3-Adressen-Code", in manchen Compilern wird als IC aber auch der AST selbst genutzt.

                     = {type: real, loc: tmp1}
                    / \
                   /   \
                 sp     inttofloat
         {type: real,       |
          loc: var b}      100

=\> `t1 = inttofloat(100)`

##### Code optimieren

An dieser Stelle verlassen wir das Compiler-Frontend und begeben uns in das sogenannte *Backend*. Die Optimierung des Codes kann sehr unterschiedlich ausfallen, beispielsweise kann man den Zwischencode selbst optimieren, dann nach sogenanntem "Targetcode" übersetzen und diesen weiter optimieren, bevor das Ergebnis im letzten Schritt in Maschinencode übersetzt wird.

Die Optimierungsphase ist sehr stark abhängig von der Zielhardware. Hier kommen fortgeschrittene Mengen- und Graphalgorithmen zur Anwendung. Die Optimierung stellt den wichtigsten Teil aktueller Compiler dar.

Aus zeitlichen und didaktischen Gründen werden wir in dieser Veranstaltung den Fokus auf die Frontend-Phasen legen und die Optimierung nur grob streifen.

`t1 = inttofloat(100)` =\> `t1 = 100.0`

`x = y*0;` =\> `x = 0;`

##### Code generieren

-   Maschinencode:

    ``` gnuassembler
    STD  t1, 100.0
    ```

<!-- -->

-   Andere Sprache:
    -   Bytecode
    -   C
    -   ...

##### Probleme

    5*4+3

**AST**?

Problem: Vorrang von Operatoren

-   Variante 1: `+(*(5, 4), 3)`
-   Variante 2: `*(5, +(4, 3))`

``` lex
stat : expr ';'
     | ID '(' ')' ';'
     ;
expr : ID '(' ')'
     | INT
     ;
```

##### Wrap-Up

-   Compiler übersetzen Text in ein anderes Format

<!-- -->

-   Typische Phasen:
    1.  Lexikalische Analyse
    2.  Syntaxanalyse
    3.  Semantische Analyse
    4.  Generierung von Zwischencode
    5.  Optimierung des (Zwischen-) Codes
    6.  Codegenerierung

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023)): Kapitel 1 Introduction
> -   Grune u. a. ([2012](#ref-Grune2012)): Kapitel 1 Introduction
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann die Struktur eines Compilers und die verschiedenen Phasen und deren Aufgaben erklären
>
> </details>

<a id="id-55cceb8d95eb4d3a67a6a18eb0e778b5695106a3"></a>

#### Bandbreite der Programmiersprachen

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Am Beispiel des Abzählreims "99 Bottles of Beer" werden (ganz kurz) verschiedene Programmiersprachen betrachtet. Jede der Sprachen hat ihr eigenes Sprachkonzept (Programmierparadigma) und auch ein eigenes Typ-System sowie ihre eigene Strategie zur Speicherverwaltung und Abarbeitung.
>
> Auch wenn die Darstellung längst nicht vollständig ist, macht sie doch deutlich, dass Compiler teilweise sehr unterschiedliche Konzepte "verstehen" müssen.
>
> </details>

> [!TIP]
>
> <details open>
> <summary><strong>🎦 Videos</strong></summary>
>
> Vorlesung \[[YT](https://youtu.be/cluS9modzDk)\], \[[HSBI](https://www.hsbi.de/medienportal/album/video/cb-bandbreite-der-programmiersprachen/b355504c34965b385cd2348611418e55/253)\]
>
> </details>

##### 99 Bottles of Beer

> 99 bottles of beer on the wall, 99 bottles of beer. Take one down and pass it around, 98 bottles of beer on the wall.
>
> 98 bottles of beer on the wall, 98 bottles of beer. Take one down and pass it around, 97 bottles of beer on the wall.
>
> \[...\]
>
> 2 bottles of beer on the wall, 2 bottles of beer. Take one down and pass it around, 1 bottle of beer on the wall.
>
> 1 bottle of beer on the wall, 1 bottle of beer. Take one down and pass it around, no more bottles of beer on the wall.
>
> No more bottles of beer on the wall, no more bottles of beer. Go to the store and buy some more, 99 bottles of beer on the wall.

Quelle: Abzählreim "99 Bottles of Beer" nach ["Lyrics of the song 99 Bottles of Beer"](https://www.99-bottles-of-beer.net/lyrics.html) on 99-bottles-of-beer.net

##### Imperativ, Hardwarenah: C

``` {.c size="footnotesize"}
 #define MAXBEER (99)
 void chug(int beers);
 main() {
    register beers;
    for(beers = MAXBEER; beers; chug(beers--))  puts("");
    puts("\nTime to buy more beer!\n");
 }
 void chug(register beers) {
    char howmany[8], *s;
    s = beers != 1 ? "s" : "";
    printf("%d bottle%s of beer on the wall,\n", beers, s);
    printf("%d bottle%s of beeeeer . . . ,\n", beers, s);
    printf("Take one down, pass it around,\n");
    if(--beers) sprintf(howmany, "%d", beers); else strcpy(howmany, "No more");
    s = beers != 1 ? "s" : "";
    printf("%s bottle%s of beer on the wall.\n", howmany, s);
 }
```

Quelle: ["Language C"](https://www.99-bottles-of-beer.net/language-c-116.html) by Bill Wein on 99-bottles-of-beer.net

-   Imperativ

-   Procedural

-   Statisches Typsystem

-   Resourcenschonend, aber "unsicher": Programmierer muss wissen, was er tut

-   Relativ hardwarenah

-   Einsatz: Betriebssysteme, Systemprogrammierung

##### Imperativ, Objektorientiert: Java

``` java
class bottles {
    public static void main(String args[]) {
        String s = "s";
        for (int beers=99; beers>-1;) {
            System.out.print(beers + " bottle" + s + " of beer on the wall, ");
            System.out.println(beers + " bottle" + s + " of beer, ");
            if (beers==0) {
                System.out.print("Go to the store, buy some more, ");
                System.out.println("99 bottles of beer on the wall.\n");
                System.exit(0);
            } else
                System.out.print("Take one down, pass it around, ");
            s = (--beers == 1)?"":"s";
            System.out.println(beers + " bottle" + s + " of beer on the wall.\n");
        }
    }
}
```

Quelle: ["Language Java"](https://www.99-bottles-of-beer.net/language-java-4.html) by Sean Russell on 99-bottles-of-beer.net

-   Imperativ

-   Objektorientiert

-   Multi-Threading

-   Basiert auf C/C++

-   Statisches Typsystem

-   Automatische Garbage Collection

-   "Sichere" Architektur: Laufzeitumgebung fängt viele Probleme ab

-   Architekturneutral: Nutzt Bytecode und eine JVM

-   Einsatz: High-Level All-Purpose Language

##### Logisch: Prolog

``` prolog
bottles :-
    bottles(99).

bottles(1) :-
    write('1 bottle of beer on the wall, 1 bottle of beer,'), nl,
    write('Take one down, and pass it around,'), nl,
    write('Now they are all gone.'), nl,!.
bottles(X) :-
    write(X), write(' bottles of beer on the wall,'), nl,
    write(X), write(' bottles of beer,'), nl,
    write('Take one down and pass it around,'), nl,
    NX is X - 1,
    write(NX), write(' bottles of beer on the wall.'), nl, nl,
    bottles(NX).
```

Quelle: ["Language Prolog"](https://www.99-bottles-of-beer.net/language-prolog-965.html) by M@ on 99-bottles-of-beer.net

-   Deklarativ

-   Logisch: Definition von Fakten und Regeln; eingebautes Beweissystem

-   Einsatz: Theorem-Beweisen, Natural Language Programming (NLP), Expertensysteme, ...

##### Funktional: Haskell

``` haskell
bottles 0 = "no more bottles"
bottles 1 = "1 bottle"
bottles n = show n ++ " bottles"

verse 0   = "No more bottles of beer on the wall, no more bottles of beer.\n"
         ++ "Go to the store and buy some more, 99 bottles of beer on the wall."

verse n   = bottles n ++ " of beer on the wall, " ++ bottles n ++ " of beer.\n"
         ++ "Take one down and pass it around, " ++ bottles (n-1)
                                                 ++ " of beer on the wall.\n"

main      = mapM (putStrLn . verse) [99,98..0]
```

Quelle: ["Language Haskell"](https://www.99-bottles-of-beer.net/language-haskell-1070.html) by Iavor on 99-bottles-of-beer.net

-   Deklarativ

-   Funktional

-   Lazy, pure

-   Statisches Typsystem

-   Typinferenz

-   Algebraische Datentypen, Patternmatching

-   Einsatz: Compiler, DSL, Forschung

##### Brainfuck

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/screenshot_brainfuck_99bottles_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/screenshot_brainfuck_99bottles.png" width="15%" /></picture></p>

Quelle: Screenshot of ["Language Brainfuck"](https://99-bottles-of-beer.net/language-brainfuck-2542.html) by Michal Wojciech Tarnowski on 99-bottles-of-beer.net

-   Imperativ

-   Feldbasiert (analog zum Band der Turingmaschine)

-   8 Befehle: Zeiger und Zellen inkrementieren/dekrementieren, Aus- und Eingabe, Sprungbefehle

##### Programmiersprache Lox

    fun fib(x) {
        if (x == 0) {
            return 0;
        } else {
            if (x == 1) {
                return 1;
            } else {
                fib(x - 1) + fib(x - 2);
            }
        }
    }

    var wuppie = fib;
    wuppie(4);

-   Die Sprache "Lox" finden Sie hier: [craftinginterpreters.com/the-lox-language.html](https://www.craftinginterpreters.com/the-lox-language.html)

-   C-ähnliche Syntax

-   Imperativ, objektorientiert, Funktionen als *First Class Citizens*, Closures

-   Dynamisch typisiert

-   Garbage Collector

-   Statements und Expressions

-   (Kleine) Standardbibliothek eingebaut

Die Sprache ähnelt stark anderen modernen Sprachen und ist gut geeignet, um an ihrem Beispiel Themen wie Scanner/Parser/AST, Interpreter, Object Code und VM zu studieren :)

##### Wrap-Up

-   Compiler übersetzen formalen Text in ein anderes Format

<!-- -->

-   Berücksichtigung von unterschiedlichen
    -   Sprachkonzepten (Programmierparadigmen)
    -   Typ-Systemen
    -   Speicherverwaltungsstrategien
    -   Abarbeitungsstrategien

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023)): Kapitel 1 Introduction
> -   Grune u. a. ([2012](#ref-Grune2012)): Kapitel 1 Introduction
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne verschiedene Beispiele für verschiedene Programmiersprachen und Paradigmen
>
> </details>

<a id="id-315c4a6ad46ecd3cdd06bff39540497383b62904"></a>

#### Anwendungen

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Es gibt verschiedene Anwendungsmöglichkeiten für Compiler. Je nach Bedarf wird dabei die komplette Toolchain durchlaufen oder es werden Stufen ausgelassen. Häufig genutzte Varianten sind dabei:
>
> -   "Echte" Compiler: Übersetzen Sourcecode nach ausführbarem Maschinencode
> -   Interpreter: Interaktive Ausführung von Sourcecode
> -   Virtuelle Maschinen als Zwischending zwischen Compiler und Interpreter
> -   Transpiler: Übersetzen formalen Text nach formalem Text
> -   Analysetools: Parsen den Sourcecode, werten die Strukturen aus
>
> </details>

> [!TIP]
>
> <details open>
> <summary><strong>🎦 Videos</strong></summary>
>
> Vorlesung \[[YT](https://youtu.be/JJ2H3fZnbTc)\], \[[HSBI](https://www.hsbi.de/medienportal/album/video/cb-anwendungen-compiler/d4e98c05a03cb5cfbbe5e1fadf6c67cc/253)\]
>
> </details>

##### Anwendung: Compiler

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/compiler_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/compiler.png"  /></picture></p>

Wie oben diskutiert: Der Sourcecode durchläuft alle Phasen des Compilers, am Ende fällt ein ausführbares Programm heraus. Dieses kann man starten und ggf. mit Inputdaten versehen und erhält den entsprechenden Output. Das erzeugte Programm läuft i.d.R. nur auf einer bestimmten Plattform.

Beispiele: gcc, clang, ...

##### Anwendung: Interpreter

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/interpreter_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/interpreter.png"  /></picture></p>

Beim Interpreter durchläuft der Sourcecode nur das Frontend, also die Analyse. Es wird kein Code erzeugt, stattdessen führt der Interpreter die Anweisungen im AST bzw. IC aus. Dazu muss der Interpreter mit den Eingabedaten beschickt werden. Typischerweise hat man hier eine "Read-Eval-Print-Loop" (*REPL*).

Beispiele: Python

##### Anwendung: Virtuelle Maschinen

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/virtualmachine_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/virtualmachine.png"  /></picture></p>

Hier liegt eine Art Mischform aus Compiler und Interpreter vor: Der Compiler übersetzt den Quellcode in ein maschinenunabhängiges Zwischenformat ("Byte-Code"). Dieser wird von der virtuellen Maschine ("VM") gelesen und ausgeführt. Die VM kann also als Interpreter für Byte-Code betrachtet werden.

Beispiel: Java mit seiner JVM

##### Anwendung: C-Toolchain

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/c-toolchain_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/c-toolchain.png" width="70%" /></picture></p>

Erinnern Sie sich an die LV "Systemprogrammierung" im dritten Semester :-)

Auch wenn es so aussieht, als würde der C-Compiler aus dem Quelltext direkt das ausführbare Programm erzeugen, finden hier dennoch verschiedene Stufen statt. Zuerst läuft ein Präprozessor über den Quelltext und ersetzt alle `#include` und `#define` etc., danach arbeitet der C-Compiler, dessen Ausgabe wiederum durch einen Assembler zu ausführbarem Maschinencode transformiert wird.

Beispiele: gcc, clang, ...

##### Anwendung: C++-Compiler

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/cpp-toolchain_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/cpp-toolchain.png" width="60%" /></picture></p>

C++ hat meist keinen eigenen (vollständigen) Compiler :-)

In der Regel werden die C++-Konstrukte durch `cfront` nach C übersetzt, so dass man anschließend auf die etablierten Tools zurückgreifen kann.

Dieses Vorgehen werden Sie relativ häufig finden. Vielleicht sogar in Ihrem Projekt ...

Beispiel: g++

##### Anwendung: Bugfinder

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/findbugs_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/00-intro/images/findbugs.png" width="60%" /></picture></p>

Tools wie FindBugs analysieren den (Java-) Quellcode und suchen nach bekannten Fehlermustern. Dazu benötigen sie nur den Analyse-Teil eines Compilers!

Auf dem AST kann dann nach vorab definierten Fehlermustern gesucht werden (Stichwort "Graphmatching"). Dazu fällt die semantische Analyse entsprechend umfangreicher aus als normal.

Zusätzlich wird noch eine Reporting-Komponente benötigt, da die normalen durch die Analysekette erzeugten Fehlermeldungen nicht helfen (bzw. sofern der Quellcode wohlgeformter Code ist, würden ja keine Fehlermeldungen durch die Analyseeinheit generiert).

Beispiele: SpotBugs, Checkstyle, ESLint, ...

##### Anwendung: Pandoc

[Pandoc](https://pandoc.org/) ist ein universeller und modular aufgebauter Textkonverter, der mit Hilfe verschiedener *Reader* unterschiedliche Textformate einlesen und in ein Zwischenformat (hier JSON) transformieren kann. Über verschiedene *Writer* können aus dem Zwischenformat dann Dokumente in den gewünschten Zielformaten erzeugt werden.

Die Reader entsprechen der Analyse-Phase und die Writer der Synthese-Phase eines Compilers. Anstelle eines ausführbaren Programms (Maschinencode) wird ein anderes Textformat erstellt/ausgegeben.

Beispielsweise wird aus diesem Markdown-Schnipsel ...

    Dies ist ein Satz mit
    *  einem Stichpunkt, und
    *  einem zweiten Stichpunkt.

... dieses Zwischenformat erzeugt, ...

``` json
{"blocks":[{"t":"Para","c":[{"t":"Str","c":"Dies"},{"t":"Space"},
           {"t":"Str","c":"ist"},{"t":"Space"},{"t":"Str","c":"ein"},
           {"t":"Space"},{"t":"Str","c":"Satz"},{"t":"Space"},
           {"t":"Str","c":"mit"}]},
           {"t":"BulletList","c":[[{"t":"Plain","c":[{"t":"Str","c":"einem"},{"t":"Space"},{"t":"Str","c":"Stichpunkt,"},{"t":"Space"},{"t":"Str","c":"und"}]}],[{"t":"Plain","c":[{"t":"Str","c":"einem"},{"t":"Space"},{"t":"Str","c":"zweiten"},{"t":"Space"},{"t":"Str","c":"Stichpunkt."}]}]]}],"pandoc-api-version":[1,17,0,4],"meta":{}}
```

... und daraus schließlich dieser TeX-Code.

``` latex
Dies ist ein Satz mit
\begin{itemize}
\tightlist
\item einem Stichpunkt, und
\item einem zweiten Stichpunkt.
\end{itemize}
```

Im Prinzip ist Pandoc damit ein Beispiel für Compiler, die aus einem formalen Text nicht ein ausführbares Programm erzeugen (Maschinencode), sondern einen anderen formalen Text. Dieser werden häufig auch "Transpiler" genannt.

Weitere Beispiele:

-   Lexer-/Parser-Generatoren: ANTLR, Flex, Bison, ...: formale Grammatik nach Sourcecode
-   CoffeeScript: CoffeeScript (eine Art "JavaScript light") nach JavaScript
-   Emscripten: C/C++ nach LLVM nach WebAssembly (tatsächlich kann LLVM-IR auch direkt als Input verwendet werden)
-   Fitnesse: Word/Wiki nach ausführbare Unit-Tests

##### Was bringt mir das?

<div data-align="center">

**Beschäftigung mit dem schönsten Thema in der Informatik ;-)**

</div>

###### Auswahl einiger Gründe für den Besuch des Moduls "Compilerbau"

-   Erstellung eigener kleiner Interpreter/Compiler
    -   Einlesen von komplexen Daten
    -   DSL als Brücke zwischen Stakeholdern
    -   DSL zum schnelleren Programmieren (denken Sie etwa an [CoffeeScript](http://coffeescript.org/) ...)
-   Wie funktionieren FindBugs, Lint und ähnliche Tools?
    -   Statische Codeanalyse: Dead code elimination
-   Language-theoretic Security: [LangSec](http://langsec.org/)
-   Verständnis für bestimmte Sprachkonstrukte und -konzepte (etwa `virtual` in C++)
-   Vertiefung durch Besuch "echter" Compilerbau-Veranstaltungen an Uni möglich :-)
-   Wie funktioniert:
    -   ein Python-Interpreter?
    -   das Syntaxhighlighting in einem Editor oder in Doxygen?
    -   ein Hardwarecompiler (etwa VHDL)?
    -   ein Text-Formatierer (TeX, LaTeX, ...)?
    -   CoffeeScript oder Emscripten?
-   Wie kann man einen eigenen Compiler/Interpreter basteln, etwa für
    -   MiniJava (mit C-Backend)
    -   Brainfuck
    -   Übersetzung von JSON nach XML
-   Um eine profundes Kenntnis von Programmiersprachen zu erlangen, ist eine Beschäftigung mit ihrer Implementierung unerlässlich.
-   Viele Grundtechniken der Informatik und elementare Datenstrukturen wie Keller, Listen, Abbildungen, Bäume, Graphen, Automaten etc. finden im Compilerbau Anwendung. Dadurch schließt sich in gewisser Weise der Kreis in der Informatikausbildung ...
-   Aufgrund seiner Reife gibt es hervorragende Beispiele von formaler Spezifikation im Compilerbau.
-   Mit dem Gebiet der formalen Sprachen berührt der Compilerbau interessante Aspekte moderner Linguistik. Damit ergibt sich letztlich eine Verbindung zur KI ...
-   Die Unterscheidung von Syntax und Semantik ist eine grundlegende Technik in fast allen formalen Systeme.

###### Parser-Generatoren (Auswahl)

Diese Tools könnte man beispielsweise nutzen, um seine eigene Sprache zu basteln.

-   ANTLR (ANother Tool for Language Recognition) is a powerful parser generator for reading, processing, executing, or translating structured text or binary files: [github.com/antlr/antlr4](https://github.com/antlr/antlr4)
-   Grammars written for ANTLR v4; expectation that the grammars are free of actions: [github.com/antlr/grammars-v4](https://github.com/antlr/grammars-v4)
-   An incremental parsing system for programmings tools: [github.com/tree-sitter/tree-sitter](https://github.com/tree-sitter/tree-sitter)
-   Flex, the Fast Lexical Analyzer - scanner generator for lexing in C and C++: [github.com/westes/flex](https://github.com/westes/flex)
-   Bison is a general-purpose parser generator that converts an annotated context-free grammar into a deterministic LR or generalized LR (GLR) parser employing LALR(1) parser tables: [gnu.org/software/bison](https://www.gnu.org/software/bison/)
-   Parser combinators for binary formats, in C: [github.com/UpstandingHackers/hammer](https://github.com/UpstandingHackers/hammer)
-   Eclipse Xtext is a language development framework: [github.com/eclipse/xtext](https://github.com/eclipse/xtext)

###### Statische Analyse, Type-Checking und Linter

Als Startpunkt für eigene Ideen. Oder Verbessern/Erweitern der Projekte ...

-   Pluggable type-checking for Java: [github.com/typetools/checker-framework](https://github.com/typetools/checker-framework)
-   SpotBugs is FindBugs' successor. A tool for static analysis to look for bugs in Java code: [github.com/spotbugs/spotbugs](https://github.com/spotbugs/spotbugs)
-   An extensible cross-language static code analyzer: [github.com/pmd/pmd](https://github.com/pmd/pmd)
-   Checkstyle is a development tool to help programmers write Java code that adheres to a coding standard: [github.com/checkstyle/checkstyle](https://github.com/checkstyle/checkstyle)
-   JaCoCo - Java Code Coverage Library: [github.com/jacoco/jacoco](https://github.com/jacoco/jacoco)
-   Sanitizers: memory error detector: [github.com/google/sanitizers](https://github.com/google/sanitizers)
-   JSHint is a tool that helps to detect errors and potential problems in your JavaScript code: [github.com/jshint/jshint](https://github.com/jshint/jshint)
-   Haskell source code suggestions: [github.com/ndmitchell/hlint](https://github.com/ndmitchell/hlint)
-   Syntax checking hacks for vim: [github.com/vim-syntastic/syntastic](https://github.com/vim-syntastic/syntastic)

###### DSL (Domain Specific Language)

-   NVIDIA Material Definition Language SDK: [github.com/NVIDIA/MDL-SDK](https://github.com/NVIDIA/MDL-SDK)
-   FitNesse -- The Acceptance Test Wiki: [github.com/unclebob/fitnesse](https://github.com/unclebob/fitnesse)

Hier noch ein Framework, welches auf das Erstellen von DSL spezialisiert ist:

-   Eclipse Xtext is a language development framework: [github.com/eclipse/xtext](https://github.com/eclipse/xtext)

###### Konverter von X nach Y

-   Emscripten: An LLVM-to-JavaScript Compiler: [github.com/kripken/emscripten](https://github.com/kripken/emscripten)
-   "Unfancy JavaScript": [github.com/jashkenas/coffeescript](https://github.com/jashkenas/coffeescript)
-   Universal markup converter: [github.com/jgm/pandoc](https://github.com/jgm/pandoc)
-   Übersetzung von JSON nach XML

###### Odds and Ends

-   How to write your own compiler: [staff.polito.it/silvano.rivoira/HowToWriteYourOwnCompiler.htm](http://staff.polito.it/silvano.rivoira/HowToWriteYourOwnCompiler.htm)
-   Building a modern functional compiler from first principles: [github.com/sdiehl/write-you-a-haskell](https://github.com/sdiehl/write-you-a-haskell)
-   Language-theoretic Security: [LangSec](http://langsec.org/)
-   Generierung von automatisierten Tests mit [Esprima](http://esprima.org/): [heise.de/-4129726](https://www.heise.de/developer/artikel/Generierung-von-automatisierten-Tests-mit-Esprima-4129726.html?view=print)
-   Eigener kleiner Compiler/Interpreter, etwa für
    -   MiniJava mit C-Backend oder sogar [LLVM](http://llvm.org/)-Backend
    -   Brainfuck

###### Als weitere Anregung: Themen der Mini-Projekte im W17

-   Java2UMLet
-   JavaDoc-to-Markdown
-   Validierung und Übersetzung von Google Protocol Buffers v3 nach JSON
-   svg2tikz
-   SwaggerLang -- Schreiben wie im Tagebuch
-   Markdown zu LaTeX
-   JavaDocToLaTeX
-   MySQL2REDIS-Parser

##### Wrap-Up

-   Compiler übersetzen formalen Text in ein anderes Format

<!-- -->

-   Nicht alle Stufen kommen immer vor =\> unterschiedliche Anwendungen
    -   "Echte" Compiler: Sourcecode nach Maschinencode
    -   Interpreter: Interaktive Ausführung
    -   Virtuelle Maschinen als Zwischending zwischen Compiler und Interpreter
    -   Transpiler: formaler Text nach formalem Text
    -   Analysetools: Parsen den Sourcecode, werten die Strukturen aus

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023)): Kapitel 1 Introduction
> -   Grune u. a. ([2012](#ref-Grune2012)): Kapitel 1 Introduction
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne verschiedene Anwendungen für Compiler durch Einsatz bestimmter Stufen der Compiler-Pipeline
>
> </details>

<a id="id-523b0fd61bf6b4cfea500aa11ae765e78dcb00cc"></a>

### Reguläre Sprachen, kontextfreie Grammatiken und Sprachen, lexikalische und syntaktische Analyse

In der lexikalischen Analyse soll ein Lexer (auch "Scanner") den Zeichenstrom in eine Folge von Token zerlegen. Zur Spezifikation der Token werden in der Regel reguläre Ausdrücke verwendet.

In der syntaktischen Analyse arbeitet ein Parser mit dem Tokenstrom, der vom Lexer kommt. Mit Hilfe einer Grammatik wird geprüft, ob gültige Sätze im Sinne der Sprache/Grammatik gebildet wurden. Der Parser erzeugt dabei den Parse-Tree. Man kann verschiedene Parser unterscheiden, beispielsweise die LL- und die LR-Parser.

<a id="id-cb0be27b07154b726b38212279070d854085a4f8"></a>

#### Reguläre Sprachen, Ausdrucksstärke (Teil 1)

##### Motivation

###### Was muss ein Compiler wohl als erstes tun?

Hier entsteht ein Tafelbild.

###### Themen für heute

-   Lexer
-   Endliche Automaten
-   Reguläre Sprachen

##### Endliche Automaten

###### Alphabete

**Def.:** Ein **Alphabet** $\Sigma$ ist eine endliche, nicht-leere Menge von Symbolen. Die Symbole eines Alphabets heißen *Buchstaben*.

**Def.:** Ein **Wort** $w$ *über einem Alphabet* $\Sigma$ ist eine endliche Folge von Symbolen aus $\Sigma$. $\epsilon$ ist das leere Wort. Die *Länge* $\vert w \vert$ eines Wortes $w$ ist die Anzahl von Buchstaben, die es enthält (Kardinalität).

**Def.:** Eine **Sprache** $L$ *über einem Alphabet* $\Sigma$ ist eine Menge von Wörtern über diesem Alphabet. Sprachen können endlich oder unendlich viele Wörter enthalten.

###### State machine

Hier entsteht ein Tafelbild.

###### Deterministische endliche Automaten

Bestimmte State machines:

-   Eingaben bestimmen Zustandsübergänge

-   Zustandsübergänge sind eindeutig

-   Es gibt Anfang(szustand) und End(zuständ)e

###### Wie definieren wir das formal?

Hier entsteht ein Tafelbild.

###### Def.: Deterministischer endlicher Automat

**Def.:** Ein **deterministischer endlicher Automat** (DFA) ist ein 5-Tupel $A = (Q, \Sigma, \delta, q_0, F)$ mit

-   $Q$ : endliche Menge von **Zuständen**

-   $\Sigma$ : Alphabet von **Eingabesymbolen**

-   $\delta$ : die (eventuell partielle) **Übergangsfunktion** $(Q \times \Sigma) \rightarrow Q$, $\delta$ kann partiell sein

-   $q_0 \in Q$ : der **Startzustand**

-   $F \subseteq Q$ : die Menge der **Endzustände**

###### Beispiel

Hier entsteht ein Tafelbild.

###### Eingabewörter statt Buchstaben

**Def.:** Wir definieren $\delta^{\ast}: (Q \times \Sigma^{\ast}) \rightarrow Q$: induktiv wie folgt:

-   Basis: $\delta^{\ast}(q, \epsilon) = q\ \forall q \in Q$

-   Induktion: $\delta^{\ast}(q, a_1, \ldots, a_n) = \delta(\delta^{\ast}(q, a_1, \ldots , a_{n-1}), a_n)$

**Def.:** Ein DFA akzeptiert ein Wort $w \in \Sigma^{\ast}$ genau dann, wenn $\delta^{\ast}(q_0, w) \in F.$

###### Beispiel

Hier entsteht ein Tafelbild.

###### Nichtdeterministische endliche Automaten

Hier entsteht ein Tafelbild.

###### Def.: Nichtdeterministischer Automat

**Def.:** Ein **nichtdeterministischer endlicher Automat** (NFA) ist ein 5-Tupel $A = (Q, \Sigma, \delta, q_0, F)$ mit

-   $Q$ : endliche Menge von **Zuständen**

-   $\Sigma$ : Alphabet von **Eingabesymbolen**

-   $\delta$ : die (eventuell partielle) **Übergangsfunktion** $(Q \times \Sigma) \rightarrow Q$

-   $q_0 \in Q$ : der **Startzustand**

-   $F \subseteq Q$ : die Menge der **Endzustände**

###### Akzeptierte Sprachen

**Def.:** Sei A ein DFA oder ein NFA. Dann ist **L(A)** die von A akzeptierte Sprache, d. h.

$L(A) = \lbrace \text{Wörter}\ w\ |\ \delta^*(q_0, w) \in F \rbrace$

###### Wozu NFAs im Compilerbau?

Pattern Matching (Erkennung von Schlüsselwörtern, Bezeichnern, ...) geht mit NFAs.

NFAs sind so nicht zu programmieren, aber:

**Satz:** Eine Sprache $L$ wird von einem NFA akzeptiert $\Leftrightarrow L$ wird von einem DFA akzeptiert.

D. h. es existieren Algorithmen zur

-   Umwandlung von NFAs in DFAS
-   Minimierung von DFAs

##### Reguläre Sprachen

###### Reguläre Ausdrücke definieren Sprachen

**Def.:** Induktive Definition von **regulären Ausdrücken** (regex) und der von ihnen repräsentierten Sprache **L**:

-   Basis:

    -   $\epsilon$ und $\emptyset$ sind reguläre Ausdrücke mit $L(\epsilon) =
          \lbrace \epsilon\rbrace$, $L(\emptyset)=\emptyset$
    -   Sei $a$ ein Symbol $\Rightarrow a$ ist ein regex mit $L(a) = \lbrace a\rbrace$

-   Induktion: Seien $E,\ F$ reguläre Ausdrücke. Dann gilt:

    -   $E+F$ ist ein regex und bezeichnet die Vereinigung $L(E + F) = L(E)\cup L(F)$
    -   $EF$ ist ein regex und bezeichnet die Konkatenation $L(EF) = L(E)L(F)$
    -   $E^{\ast}$ ist ein regex und bezeichnet die Kleene-Hülle $L(E^{\ast})=(L(E))^{\ast}$
    -   $(E)$ ist ein regex mit $L((E)) = L(E)$

Vorrangregeln der Operatoren für reguläre Ausdrücke: \*, Konkatenation, +

###### Beispiel

Hier entsteht ein Tafelbild.

###### Wichtige Identitäten

**Satz:** Sei $A$ ein DFA $\Rightarrow \exists$ regex $R$ mit $L(A) = L(R)$.

**Satz:** Sei $E$ ein regex $\Rightarrow \exists$ DFA $A$ mit $L(E) = L(A)$.

###### Formale Grammatiken

Hier entsteht ein Tafelbild.

###### Formale Definition formaler Grammatiken

**Def.:** Eine *formale Grammatik* ist ein 4-Tupel $G=(N,T,P,S)$ aus

-   $N$: endliche Menge von **Nichtterminalen**

-   $T$: endliche Menge von **Terminalen**, $N \cap T = \emptyset$

-   $S \in N$: **Startsymbol**

-   $P$: endliche Menge von **Produktionen** der Form

$\qquad X \rightarrow Y$ mit $X \in (N \cup T)^{\ast} N  (N \cup T)^{\ast}, Y \in (N \cup T)^{\ast}$

###### Ableitungen

**Def.:** Sei $G = (N, T, P, S)$ eine Grammatik, sei $\alpha A \beta$ eine Zeichenkette über $(N \cup T)^{\ast}$ und sei $A \rightarrow \gamma$ eine Produktion von $G$.

Wir schreiben: $\alpha A \beta \Rightarrow \alpha \gamma \beta$ ($\alpha A \beta$ leitet $\alpha \gamma \beta$ ab).

**Def.:** Wir definieren die Relation $\overset{\ast}{\Rightarrow}$ induktiv wie folgt:

-   Basis: $\forall \alpha \in (N \cup T)^{\ast} \alpha \overset{\ast}{\Rightarrow} \alpha$ (Jede Zeichenkette leitet sich selbst ab.)

-   Induktion: Wenn $\alpha \overset{\ast}{\Rightarrow} \beta$ und $\beta\Rightarrow \gamma$ dann $\alpha \overset{\ast}{\Rightarrow} \gamma$

**Def.:** Sei $G = (N, T ,P, S)$ eine formale Grammatik. Dann ist $L(G) = \lbrace \text{Wörter}\ w\ \text{über}\ T \mid S \overset{\ast}{\Rightarrow} w\rbrace$ die von $G$ erzeugte Sprache.

###### Beispiel

Hier entsteht ein Tafelbild.

###### Reguläre Grammatiken

**Def.:** Eine **reguläre (oder type-3-) Grammatik** ist eine formale Grammatik mit den folgenden Einschränkungen:

-   Alle Produktionen sind entweder von der Form

    -   $X \to aY$ mit $X \in N, a \in T, Y \in N$ (*rechtsreguläre* Grammatik) oder
    -   $X \to Ya$ mit $X \in N, a \in T, Y \in N$ (*linksreguläre* Grammatik)

-   $X\rightarrow\epsilon$ ist erlaubt

###### Beispiel

Hier entsteht ein Tafelbild.

###### Reguläre Sprachen

**Satz:** Die von endlichen Automaten akzeptiert Sprachklasse, die von regulären Ausdrücken beschriebene Sprachklasse und die von regulären Grammatiken erzeugte Sprachklasse sind identisch und heißen **reguläre Sprachen**.

##### Wrap-Up

###### Wrap-Up

-   Definition und Aufgaben von Lexern
-   DFAs und NFAs
-   Reguläre Ausdrücke
-   Reguläre Grammatiken
-   Zusammenhänge zwischen diesen Mechanismen und Lexern, bzw. Lexergeneratoren

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023)): Abschnitt 2.6 und Kapitel 3
> -   Torczon und Cooper ([2012](#ref-Torczon2012)): Kapitel 2
> -   Parr ([2014](#ref-Parr2014))
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne DFAs
> -   k1: Ich kenne reguläre Ausdrücke
> -   k1: Ich kenne reguläre Grammatiken
> -   k2: Ich kann die Zusammenhänge und Gesetzmäßigkeiten bzgl. der oben genannten Konstrukte an einem Beispiel erklären
> -   k3: Ich kann für eine Fragestellung DFAs, reguläre Ausdrücke, reguläre Grammatiken entwickeln
> -   k3: Ich kann einen DFA entwickeln, der alle Schlüsselwörter, Namen und weitere Symbole einer Programmiersprache akzeptiert
>
> </details>

<a id="id-e6527ca4572a4752c431b91119c87d78fa032789"></a>

#### Reguläre Sprachen, Ausdrucksstärke (Teil 2)

##### Wiederholung

###### Endliche Automaten, reguläre Ausdrücke, reguläre Grammatiken, reguläre Sprachen

-   Wie sind DFAs und NFAs definiert?
-   Was sind reguläre Ausdrücke?
-   Was sind formale und reguläre Grammatiken?
-   In welchem Zusammenhang stehen all diese Begriffe?

##### Motivation

###### Was haben reguläre Sprachen mit Compilern zu tun?

Hier entsteht ein Tafelbild.

###### Themen für heute

-   Reguläre Sprachen
-   Lexer
-   Grenzen regulärer Sprachen

###### Wozu reguläre Sprachen im Compilerbau?

Reguläre Ausdrücke

-   definieren Schlüsselwörter und alle weiteren Symbole einer Programmiersprache, z. B. den Aufbau von Gleitkommazahlen
-   werden (oft von einem Generator) in DFAs umgewandelt
-   sind die Basis des *Scanners* oder *Lexers*

##### Lexer

###### Ein Lexer ist mehr als ein DFA

Ein **Lexer**

-   kann aus regulären Ausdrücken automatisch generiert werden

-   wandelt mittels DFAs aus regulären Ausdrücken die Folge von Zeichen der Quelldatei in eine Folge von sog. Token um

-   bekommt als Input eine Liste von Paaren aus regulären Ausdrücken und Tokennamen, z. B. ("while", WHILE)

-   Kommentare und Strings müssen richtig erkannt werden. (Schachtelungen)

-   liefert Paare von Token und deren Werte, sofern benötigt, z. B. (WHILE, \_), oder (IDENTIFIER, "radius") oder (INTEGERZAHL, "334")

###### Wofür reichen reguläre Sprachen nicht?

Für z. B. alle Sprachen, in deren Wörtern Zeichen über eine Konstante hinaus gezählt werden müssen. Diese Sprachen lassen sich oft mit Variablen im Exponenten beschreiben, die unendlich viele Werte annehmen können.

-   $a^ib^{2*i}$ ist nicht regulär
-   $a^ib^{2*i}$ für $0 \leq i \leq 3$ ist regulär

<!-- -->

-   Wo finden sich die oben genannten Variablen bei einem DFA wieder?
-   Warum ist die erste Sprache oben nicht regulär, die zweite aber?

###### Wie geht es weiter?

Ein **Parser**

-   führt mit Hilfe des Tokenstreams vom Lexer die Syntaxanalyse durch

-   basiert auf einer sog. kontextfreien Grammatik, deren Terminale die Token sind

-   liefert die syntaktische Struktur in Form eines Ableitungsbaums (**syntax tree**, **parse tree**), bzw. einen **AST** (abstract syntax tree) ohne redundante Informationen im Ableitungsbaum (z. B. Semikolons)

-   liefert evtl. Fehlermeldungen

##### Wrap-Up

###### Wrap-Up

-   Definition und Aufgaben von Lexern
-   Zusammenhänge zwischen diesen Mechanismen und Lexern, bzw. Lexergeneratoren
-   Grenzen regulärer Sprachen

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023)): Abschnitt 2.6 und Kapitel 3
> -   Torczon und Cooper ([2012](#ref-Torczon2012)): Kapitel 2
> -   Parr ([2014](#ref-Parr2014))
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne die Aufgaben eines Lexers
> -   k1: Ich kenne die Zusammenhänge zwischen DFAs, regulären Ausdrücken, regulären Grammatiken und Lexern
> -   k2: Ich kann für eine Beispielsprache begründen, warum sie nicht mit einem der oben genannten Mechanismen beschrieben werden kann
>
> </details>

<a id="id-7c0e930dad216729eeb1545678306ab9f0d6a57b"></a>

#### CFG

##### Wiederholung

###### Endliche Automaten, reguläre Ausdrücke, reguläre Grammatiken, reguläre Sprachen

-   Was ist ein Lexer?
-   In welchem Zusammenhang stehen Lexer und reguläre Sprachen?
-   Was können Lexer nicht?

##### Motivation

###### Was brauchen wir jetzt?

###### Themen für heute

-   PDAs: mächtiger als DFAs, NFAs
-   kontextfreie Grammatiken und Sprachen: mächtiger als reguläre Grammatiken und Sprachen
-   DPDAs und deterministisch kontextfreie Grammatiken: die Grundlage der Syntaxanalyse im Compilerbau
-   Syntaxanalyse

###### Einordnung: Erweiterung der Automatenklasse DFA, um komplexere Sprachen als die regulären akzeptieren zu können

Wir spendieren den DFAs einen möglichst einfachen, aber beliebig großen, Speicher, um zählen und matchen zu können. Wir suchen dabei konzeptionell die "kleinstmögliche" Erweiterung, die die akzeptierte Sprachklasse gegenüber DFAs vergrößert.

-   Der konzeptionell einfachste Speicher ist ein Stack. Wir haben keinen wahlfreien Zugriff auf die gespeicherten Werte.

-   Es soll eine deterministische und eine indeterministische Variante der neuen Automatenklasse geben.

-   In diesem Zusammenhang wird der Stack auch Keller genannt.

###### Kellerautomaten (Push-Down-Automata, PDAs)

**Def.:** Ein Kellerautomat (PDA) $P = (Q,\ \Sigma,\ \Gamma,\  \delta,\ q_0,\ \perp,\ F)$ ist ein Septupel mit:

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/Def_PDA_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/Def_PDA.png" width="60%" /></picture></p><p align="center">Definition eines PDAs</p>

Ein PDA ist per Definition nichtdeterministisch und kann spontane Zustandsübergänge durchführen.

###### Was kann man damit akzeptieren?

Strukturen mit paarweise zu matchenden Symbolen.

Bei jedem Zustandsübergang wird ein Zeichen (oder $\epsilon$) aus der Eingabe gelesen, ein Symbol von Keller genommen. Diese und das Eingabezeichen bestimmen den Folgezustand und eine Zeichenfolge, die auf den Stack gepackt wird. Dabei wird ein Symbol, (z. B. eines, das später mit einem Eingabesymbol zu matchen ist,) auf den Stack gepackt. Soll das automatisch vom Stack genommene Symbol auf dem Stack bleiben, muss es wieder gepusht werden.

###### Beispiel

Ein PDA für $L=\lbrace ww^{R}\mid w\in \lbrace a,b\rbrace^{\ast}\rbrace$:

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/pda2_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/pda2.png" width="45%" /></picture></p>

###### Noch ein Beispiel

Hier entsteht ein Tafelbild.

###### Deterministische PDAs

**Def.** Ein PDA $P = (Q, \Sigma, \Gamma, \delta, q_0, \perp, F)$ ist *deterministisch* $: \Leftrightarrow$

-   $\delta(q, a, X)$ hat höchstens ein Element für jedes $q \in Q, a \in\Sigma$ oder $(a = \epsilon$ und $X \in \Gamma)$.

-   Wenn $\delta (q, a, x)$ nicht leer ist für ein $a \in \Sigma$, dann muss $\delta (q, \epsilon, x)$ leer sein.

Deterministische PDAs werden auch *DPDAs* genannt.

###### Der kleine Unterschied

**Satz:** Die von DPDAs akzeptierten Sprachen sind eine echte Teilmenge der von PDAs akzeptierten Sprachen.

Die regulären Sprachen sind eine echte Teilmenge der von DPDAs akzeptierten Sprachen.

##### Kontextfreie Grammatiken und Sprachen

###### Kontextfreie Grammatiken

**Def.** Eine *kontextfreie (cf-)* Grammatik ist ein 4-Tupel $G = (N, T, P, S)$ mit $N, T, S$ wie in (formalen) Grammatiken und $P$ ist eine endliche Menge von Produktionen der Form:

$X \rightarrow Y$ mit $X \in N, Y \in {(N \cup T)}^{\ast}$.

$\Rightarrow, \overset{\ast}{\Rightarrow}$ sind definiert wie bei regulären Sprachen. Bei cf-Grammatiken nennt man die Ableitungsbäume oft *Parse trees*.

###### Beispiel

Hier entsteht ein Tafelbild.

###### Was ist hier los?

$S \rightarrow a \mid S\ +\  S\ |\  S \ast S$

Ableitungsbäume für $a + a \ast a$:

Hier entsteht ein Tafelbild.

###### Nicht jede kontextfreie Grammatik ist eindeutig

**Def.:** Gibt es in einer von einer kontextfreien Grammatik erzeugten Sprache ein Wort, für das mehr als ein Ableitungsbaum existiert, so heißt diese Grammatik *mehrdeutig*. Anderenfalls heißt sie *eindeutig*.

**Satz:** Es ist nicht entscheidbar, ob eine gegebene kontextfreie Grammatik eindeutig ist.

**Satz:** Es gibt kontextfreie Sprachen, für die keine eindeutige Grammatik existiert.

###### Kontextfreie Grammatiken und PDAs

**Satz:** Die kontextfreien Sprachen und die Sprachen, die von PDAs akzeptiert werden, sind dieselbe Sprachklasse.

**Satz:** Eine von einem DPDA akzeptierte Sprache hat eine eindeutige Grammatik.

Vorgehensweise im Compilerbau: Eine (cf) Grammatik für die gewünschte Sprache definieren und schauen, ob sich daraus ein DPDA generieren lässt (automatisch).

##### Syntaxanalyse

###### Syntax

Wir verstehen unter Syntax eine Menge von Regeln, die die Struktur von Daten (z. B. Programmen) bestimmen.

###### Ziele der Syntaxanalyse

-   Bestimmung der syntaktischen Struktur eines Programms

-   aussagekräftige Fehlermeldungen, wenn ein Eingabeprogramm syntaktisch nicht korrekt ist

-   Erstellung des AST (abstrakter Syntaxbaum): Der Parse Tree ohne Symbole, die nach der Syntaxanalyse inhaltlich irrelevant sind (z. B. Semikolons, manche Schlüsselwörter)

-   die Symboltabelle(n) mit Informationen bzgl. Bezeichner (Variable, Funktionen und Methoden, Klassen, benutzerdefinierte Typen, Parameter, ...), aber auch die Gültigkeitsbereiche

###### Was brauchen wir für die Syntaxanalyse von Programmen?

-   einen Grammatiktypen, aus dem sich manuell oder automatisiert ein Programm zur deterministischen Syntaxanalyse (= Parser) erstellen lässt

-   einen Algorithmus zum Parsen von Programmen mit Hilfe einer solchen Grammatik

##### Wrap-Up

###### Das sollen Sie mitnehmen

-   Die Struktur von gängigen Programmiersprachen lässt sich nicht mit regulären Ausdrücken beschreiben und damit nicht mit DFAs akzeptieren.
-   Das Automatenmodell der DFAs wird um einen endlosen Stack erweitert, das ergibt PDAs.
-   Kontextfreie Grammatiken (CFGs) erweitern die regulären Grammatiken.
-   PDAs akzeptieren kontextfreie Sprachen.
-   Deterministisch parsbare Sprachen haben eine eindeutige kontextfreie Grammatik, aber nicht für jede eindeutige kontextfreie Grammatik lässt sich ein deterministischer PDA finden.
-   Es ist nicht entscheidbar, ob eine gegebene kontextfreie Grammatik eindeutig ist.
-   Syntaxanalyse wird mit (möglichst deterministisch) kontextfreien Grammatiken durchgeführt.
-   In der Praxis werden aus kontextfreien Grammatiken Parser automatisch generiert.

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023))
> -   Hopcroft u. a. ([2003](#ref-hopcroft2003))
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne PDAs
> -   k1: Ich kenne deterministische PDAs
> -   k1: Ich kenne kontextfreie Grammatiken
> -   k1: Ich kenne deterministisch kontextfreie Grammatiken
> -   k2: Ich kann den Zusammenhang zwischen PDAs und kontextfreien Grammatiken an einem Beispiel erklären
> -   k2: Ich kann PDAs entwickeln
> -   k2: Ich kann kontextfreie Grammatiken entwickeln
>
> </details>

<a id="id-9aa1932181298bc40b56bb55a0bf53edf1c3aa88"></a>

#### LL-Parser

##### Wiederholung

###### PDAs und kontextfreie Grammatiken

-   Warum reichen uns DFAs nicht zum Matchen von Eingabezeichen?
-   Wie könnnen wir sie minimal erweitern?
-   Sind PDAs deterministisch?
-   Wie sind kontextfreie Grammatiken definiert?
-   Sind kontextfreie Grammatiken eindeutig?

##### Motivation

###### Was brauchen wir für die Syntaxanalyse von Programmen?

-   einen Grammatiktypen, aus dem sich manuell oder automatisiert ein Programm zur deterministischen Syntaxanalyse (=Parser) erstellen lässt
-   einen Algorithmus zum Parsen von Programmen mit Hilfe einer solchen Grammatik

###### Themen für heute

-   Automatische Generierung von Top-Down-Parsern aus LL-Grammatiken

##### Syntaxanalyse

###### Syntax

Wir verstehen unter Syntax eine Menge von Regeln, die die Struktur von Daten (z. B. Programmen) bestimmen.

Syntaxanalyse ist die Bestimmung, ob Eingabedaten einer vorgegebenen Syntax entsprechen.

Diese vorgegebene Syntax wird im Compilerbau mit einer kontextfreien Grammatik beschrieben und mit einem sogenannten **Parser** analysiert.

Wir beshäftigen uns heute mit LL-Parsing, mit dem man eine Teilmenge der eindeutigen kontextfreien Grammatiken syntaktich analysieren kann.

Der Ableitungsbaumwird von oben nach unten aufgebaut.

###### Ziele der Syntaxanalyse

-   aussagekräftige Fehlermeldungen, wenn ein Eingabeprogramm syntaktisch nicht korrekt ist
-   evtl. Fehlerkorrektur
-   Bestimmung der syntaktischen Struktur eines Programms
-   Erstellung des AST (abstrakter Syntaxbaum): Der Parse Tree ohne Symbole, die nach der Syntaxanalyse inhaltlich irrelevant sind (z. B. Semikolons, manche Schlüsselwörter)
-   die Symboltablelle(n) mit Informationen bzgl. Bezeichner (Variable, Funktionen und Methoden, Klassen, benutzerdefinierte Typen, Parameter, ...), aber auch die Gültigkeitsbereiche.

##### LL(k)-Grammatiken

###### First-Mengen

$S \rightarrow A \ \vert \ B \ \vert \ C$

Welche Produktion nehmen?

Wir brauchen die "terminalen k-Anfänge" von Ableitungen von Nichtterminalen, um eindeutig die nächste zu benutzende Produktion festzulegen. $k$ ist dabei die Anzahl der Vorschautoken.

**Def.:** Wir definieren $First$ - Mengen einer Grammatik wie folgt:

-   $a \in T^\ast, |a| \leq k: {First}_k (a) = \lbrace a\rbrace$
-   $a \in T^\ast, |a| > k: {First}_k (a) = \lbrace v \in T^\ast \mid a = vw, |v| = k\rbrace$
-   $\alpha \in (N \cup T)^\ast \backslash T^\ast: {First}_k (\alpha) = \lbrace v \in T^\ast \mid  \alpha \overset{\ast}{\Rightarrow} w,\text{mit}\ w \in T^\ast, First_k(w) = \lbrace v \rbrace \rbrace$

###### Linksableitungen

**Def.:** Bei einer kontextfreien Grammatik $G$ ist die *Linksableitung* von $\alpha \in (N \cup T)^{\ast}$ die Ableitung, die man erhält, wenn in jedem Schritt das am weitesten links stehende Nichtterminal in $\alpha$ abgeleitet wird.

Man schreibt $\alpha \overset{\ast}{\Rightarrow}_l \beta.$

###### LL(k)-Grammatiken

**Def.:** Eine kontextfreie Grammatik *G = (N, T, P, S)* ist genau dann eine *LL(k)*-Grammatik, wenn für alle Linksableitungen der Form:

$S \overset{\ast}{\Rightarrow}_l\ wA \gamma\ {\Rightarrow}_l\ w\alpha\gamma \overset{\ast}{\Rightarrow}_l wx$

und

$S \overset{\ast}{\Rightarrow}_l wA \gamma {\Rightarrow}_l w\beta\gamma \overset{\ast}{\Rightarrow}_l wy$

mit $(w, x, y \in T^\ast, \alpha, \beta, \gamma \in (N \cup T)^\ast, A \in N)$ und $First_k(x) = First_k(y)$ gilt:

$\alpha = \beta$

###### LL(1)-Grammatiken

Hier entsteht ein Tafelbild.

###### LL(k)-Sprachen

Die von *LL(k)*-Grammatiken erzeugten Sprachen sind eine echte Teilmenge der deterministisch parsbaren Sprachen.

Die von *LL(k)*-Grammatiken erzeugten Sprachen sind eine echte Teilmenge der von *LL(k+1)*-Grammatiken erzeugten Sprachen.

Für eine kontextfreie Grammatik *G* ist nicht entscheidbar, ob es eine *LL(1)* - Grammatik *G'* gibt mit $L(G) = L(G')$.

In der Praxis reichen $LL(1)$ - Grammatiken oft. Hier gibt es effiziente Parsergeneratoren (hier: ANTLR), deren Eingabe eine LL(k)- (meist LL(1)-) Grammatik ist, und die als Ausgabe den Quellcode eines (effizienten) tabellengesteuerten Parsers generieren.

###### Algorithmus: Konstruktion einer LL-Parsertabelle

**Eingabe:** Eine Grammatik G = (N, T, P, S)

**Ausgabe:** Eine Parsertabelle *P*

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/LL-Parsertabelle_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/LL-Parsertabelle.png" width="60%" /></picture></p><p align="center">Algorithmus zur Generierung einer LL-Parsertabelle</p>

Hier ist $\perp$ das Endezeichen des Inputs. Statt $First_1(\alpha)$ wird oft nur $First(\alpha)$ geschrieben.

###### LL-Parsertabellen

Hier entsteht ein Tafelbild.

###### LL-Parsertabellen

Rekursive Programmierung bedeutet, dass das Laufzeitsystem einen Stack benutzt. Diesen Stack kann man auch "selbst programmieren", d. h. einen PDA implementieren. Dabei wird ebenfalls die oben genannte Tabelle zur Bestimmung der nächsten anzuwendenden Produktion benutzt. Der Stack enthält die zu erwartenden Eingabezeichen, wenn immer eine Linksableitung gebildet wird. Diese Zeichen im Stack werden mit dem Input gematcht.

###### Algorithmus: Tabellengesteuertes LL-Parsen mit einem PDA

**Eingabe:** Eine Grammatik G = (N, T, P, S), eine Parsertabelle *P* mit "$w\perp$" als initialem Kellerinhalt

**Ausgabe:** Wenn $w \in L(G)$, eine Linksableitung von $w$, Fehler sonst

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/LL-Parser_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/01-theory/images/LL-Parser.png" width="49%" /></picture></p><p align="center">Algorithmus zum tabellengesteuerten LL-Parsen</p>

###### Ergebnisse der Syntaxanalyse

-   eventuelle Syntaxfehler mit Angabe der Fehlerart und des -Ortes
-   Fehlerkorrektur
-   Format für die Weiterverarbeitung:
    -   Ableitungsbaum oder Syntaxbaum oder Parse Tree
    -   abstrakter Syntaxbaum (AST): Der Parse Tree ohne Symbole, die nach der Syntaxanalyse inhaltlich irrelevant sind (z. B. ;, Klammern, manche Schlüsselwörter, $\ldots$)
-   Symboltabelle

##### Wrap-Up

###### Wrap-Up

-   Syntaxanalyse wird mit deterministisch kontextfreien Grammatiken durchgeführt.
-   Eine Teilmenge der dazu gehörigen Sprachen lässt sich top-down parsen.
-   Ein effizienter LL(k)-Parser realisiert einen DPDA und kann automatisch aus einer LL(k)-Grammatik generiert werden.
-   Der Parser liefert in der Regel einen abstrakten Syntaxbaum.

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> -   Aho u. a. ([2023](#ref-Aho2023))
> -   Hopcroft u. a. ([2003](#ref-hopcroft2003))
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne die Top-Down-Analyse
> -   k1: Ich kenne LL-Parser
> -   k2: Ich kann den algorithmischen Ablauf von LL-Parsern an einem Beispiel erklären
>
> </details>

<a id="id-1367160fc9ac7c83938c632f3adee2171857a427"></a>

### Sprache L-Int: Integer, Addition, Subtraktion

##### Teil 1:

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   AST, InterpreterLInt

##### Teil 2:

-   Wiederholung ANTLR, Visitor mit Rückgabe (zustandslos), ANTLR-Grammatik
-   Handgeschriebener Lexer, RD-Parser

<a id="id-92efa03f813e51c04582d54092121544fac6d2f6"></a>

#### L-Int: Die Sprache Lispy

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Lispy ist eine für diese Lehrveranstaltung frei erfundene Lisp-ähnliche Programmiersprache. Ihre Ausdrücke werden als S-Expressions in einer Klammer- und Präfixnotation geschrieben.
>
> L-Int ist dabei die Grundstufe von Lipsy und umfasst lediglich Integer-Literale sowie Addition und Subtraktion. Ein Integer-Literal ist bereits ein minimaler Ausdruck, während eine Liste wie `(+ 1 2)` einen Operator und seine Operanden enthält.
>
> Die Syntax legt fest, welche Ausdrücke gültig sind; die Semantik legt fest, welchen Wert ein gültiger Ausdruck ergibt.
>
> Mehrere Operanden können bei der Addition oder Subtraktion als "syntaktischer Zucker" für verschachtelte binäre Ausdrücke behandelt werden.
>
> Kommentare beginnen mit `;;` und reichen bis zum Ende der Zeile. L-Int bildet die Grundlage, um schrittweise Grammatik, Lexer, Parser, Syntaxbaum, AST und Interpreter zu entwickeln.
>
> </details>

##### Problem: Wie verstehen wir `1 + 2 - 3`?

-   Eingabe: Text wie `1 + 2 - 3`
-   Ziel: Ergebnis berechnen
-   Frage:
    -   Wie modellieren wir die Struktur solcher Ausdrücke?
    -   Wie bringen wir den Rechner dazu, "richtig" zu rechnen?

Starten wir mit einem bewusst einfachen Beispiel: `1 + 2 - 3`. Das kennen alle, aber wir tun jetzt so, als wüssten wir noch nicht, wie ein Rechner das versteht. Die beiden Kernfragen sind: Wie sieht die Struktur dieses Ausdrucks aus? Und wie können wir die Bedeutung so modellieren, dass wir sie programmatisch auswerten können?

Das wird uns durch die gesamte Veranstaltung begleiten: **Vom Text (Eingabe) über Struktur (Syntax) hin zur Bedeutung (Semantik)**.

> [!IMPORTANT]
>
> ###### Lehrkonzept
>
> Wir werden im Laufe der Veranstaltung **inkrementell** vorgehen und mit einer minimalen Sprache L-Int (nur Integer, Addition und Subtraktion) starten. Für diese Sprache werden wir die Syntax in Form von Grammatiken festlegen, Lexer und Parser bauen und aus einer Eingabe einen Syntax-Tree (Parse-Tree) und daraus einen abstrakten Syntax-Tree (AST) erzeugen und diesen schließlich mit einem Interpreter evaluieren. (Ein konkreter Syntaxbaum beziehungsweise Parse-Tree bildet die Struktur der konkreten Grammatik ab; der AST lässt syntaktische Details weg, die für die weitere Verarbeitung nicht benötigt werden.) In den Folgewochen erweitern wir L-Int schrittweise um weitere Konzepte. Wir leiten die Java-Implementierung einer Sprachstufe von der vorherigen ab und überschreiben die Methoden, deren Verhalten sich ändert. Mit `super.xyz()` rufen wir die Implementierung der Oberklasse auf, d.h. rufen so das Verhalten der Vorgängerversion ab ohne es erneut zu implementieren.
>
> Wir orientieren uns damit grob an dem inkrementellen Vorgehen in ([Siek 2023](#ref-Siek2023python)). Konzeptionell/didaktisch lassen wir uns bei der Modellierung der Bausteine in vielen Fällen ungefähr von der Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) inspirieren. Es werden sich also immer wieder Hinweise auf diese beiden Werke als Vertiefung finden.

##### S-Expressions als syntaktische Grundform

Ein Programm in L-Int besteht aus einem **Ausdruck** (*Expression*). Die Ausdrücke haben eine spezielle Form: Sie sind sogenannte [**S-Expressions**](https://en.wikipedia.org/wiki/S-expression).

Eine S-Expression ist:

-   ein Atom der Form `x`, oder
-   ein Ausdruck der Form `(. x y)` (mit `x` u. `y` S-Expressions, `.` eine Operation oder Funktion oder ein Keyword).

> [!TIP]
>
> ###### Variadische S-Expressions
>
> Die Anzahl der S-Expressions in einem Klammer-Ausdruck ist nicht näher definiert - `x` und `y` sind nur Beispiele. Es könnten auch mehr oder weniger S-Expressions nach der Operation/Funktion/Keyword auftauchen (vgl. nachfolgende Sprachdefinition).

> [!IMPORTANT]
>
> ###### Syntax vs. Semantik
>
> Die Syntax legt fest, welche Ausdrücke gültig sind; die Semantik legt fest, welchen Wert ein gültiger Ausdruck ergibt.
>
> Wir werden zur Beschreibung der Syntax verschiedene Grammatiken entwickeln. Die Auswertung wird über eine semantische Analyse und einen Interpreter erfolgen.

In der *Vorlesung* werden wir für unsere aufeinander aufbauenden Sprachfamilien **S-Expressions** nutzen in Anlehnung an Clojure und Lisp. Das vereinfacht das Parsing deutlich und lässt die Konzepte im Skript und den Folien deutlicher hervortreten. Im Praktikum werden wir diese Konzepte auf einen Subdialekt von C++ anwenden und über das Semester hinweg in modernem Java einen Interpreter für dieses Mini-C++ schreiben.

Lisp ist eine alte Sprachfamilie, die aus dem KI-Umfeld stammt, und ist eine Abkürzung für "*list processing*". In vielen Lisp-Dialekten werden Programme und viele Daten in einer gemeinsamen listenartigen Form ([S-Expressions](https://en.wikipedia.org/wiki/S-expression)) dargestellt ("Homoikonizität", "*code as data*") - einfach zu merken und gleichzeitig sehr mächtig.

Clojure ist ein moderner Lisp-Dialekt und läuft auf der Java-VM. Der Legende nach stammt der Name "Clojure" von "*closure*" (Programmierkonzept) sowie von "C" für C bzw. C#, "L" für Lisp und "J" für Java (Sprachen, bei denen Konzepte entlehnt wurden). (Quelle: [Hickey, Rich (2009-01-05). "meaning and pronunciation of Clojure"](https://groups.google.com/g/clojure/c/4uDxeOS8pwY/m/UHiYp7p1a3YJ))

Clojure und Lisp nutzen die "*prefix notation*": `(. x y z)`

<div data-align="center">

<p align="center"><img src="https://clojure.org/images/content/guides/learn/syntax/structure-and-semantics.png" width="85%" /></p>

Quelle: [Clojure.org: Learn Clojure](https://clojure.org/guides/learn/syntax#_structure_vs_semantics)

</div>

In grüner Beschriftung ist die Syntax markiert, also die Datenstruktur, die nach dem Einlesen des Codeschnipsels entsteht. Eine S-Expression ist also einfach eine Liste mit einem Symbol und hier im Beispiel zwei Zahlen.

In blauer Schrift ist die Semantik markiert, also das, was die Clojure Runtime dann ausführt. Dabei wird die Liste als Funktionsaufruf interpretiert, das Symbol zu einer Funktion aufgelöst und die beiden Zahlen werden als Argumente übergeben.

Die Verwendung von S-Expressions spielt direkt eine Rolle, warum Lisp und die modernen Varianten wie Clojure und Racket so beliebt sind.

S-Expressions werden in Clojure und Lisp oft auch "*form*" genannt.

> [!TIP]
>
> Wir ignorieren die Aspekte "Liste" und "Funktion" vorerst und stellen das für später zurück. Ein `(+ 1 2)` ist für uns einfach eine andere Schreibweise für `1 + 2`.

##### L-Int: Erste Sprachstufe

Die Sprache L-Int ist bewusst sehr eingeschränkt, damit wir schnell die Interpreter-Pipeline aufbauen können. Wir werden dann schrittweise weitere Sprachinkremente darauf aufbauen.

In L-Int haben wir:

-   Nichtnegative Ganzzahlen (z.B. `0`, `42`, `1337`)
-   Binäre Operatoren: `+`, `-`
-   Klammern: `(`, `)`
-   Zeilen-Kommentar: `;;`

Beispiele:

``` clojure
1                       ;; 1 (Integer)
(+ 1 2)                 ;; 1 + 2
(+ 1 2 3 4)             ;; 1 + 2 + 3 + 4
(- 10 (+ 3 5))          ;; 10 - (3 + 5)
(- (+ 1 2) (- 3 4))     ;; (1 + 2) - (3 + 4)
```

L-Int ist unsere minimalistische Basissprache für die folgenden Sitzungen. Sie enthält nur das Nötigste: Integer-Literale, Addition und Subtraktion, sowie Klammern. Das ist bewusst klein gehalten, damit wir uns auf die Architektur konzentrieren können: Grammatik, AST, Interpreter. Alles, was wir hier aufbauen, lässt sich (und wird) später systematisch erweitern.

Die einfachste Form sind Integer-Literale mit konkreten Werten.

In der listenartigen Form ist der erste Eintrag der Liste immer eine Operation (oder ein Funktionsname), danach kommen je nach Operation/Funktion (die Arität muss passen!) entsprechende Einträge, die als Parameter für die Operation oder Funktion zu verstehen sind.

Der Ausdruck `(+ 1 1)` ist eine S-Expression, gebildet aus den atomaren Ausdrücken `+` und `1`. Bei der Evaluierung wird das `+`-Symbol zu einer Additionsfunktion evaluiert, und die `1` steht für sich, also für eine Integerzahl mit dem Wert 1. Bei der Evaluierung der Liste wird die Additionsfunktion mit den beiden Integerzahlen als Argumente aufrufen.

Über `;;` wird ein Kommentar eingeleitet, der bis zum Ende der Zeile geht.

> [!TIP]
>
> ###### Syntactic Sugar
>
> Die Ausdrücke sind implizit von links nach rechts geklammert, d.h. der Ausdruck `(+ 1 2 3 4)` ist [*syntactic sugar*](https://en.wikipedia.org/wiki/Syntactic_sugar) für `(+ (+ (+ 1 2) 3) 4)`.

> [!TIP]
>
> ###### Statements vs. Expressions
>
> In vielen Sprachen unterscheidet man zwischen **Statements** und **Expressions** (Anweisungen und Ausdrücke). Dabei haben Statements üblicherweise keinen (Rückgabe-) Wert und verändern den Zustand des laufenden Programms (*state*), während Expressions immer einen Wert ergeben.
>
> In unserem L-Int ist zunächst alles eine Expression, d.h. Ausdrücke ergeben bei der Auswertung immer einen Wert. In späteren Sprachstufen werden wir komplexere Ausdrücke und auch Statements über die Listenform bilden.

##### Wrap-up

-   L-Int besteht aus S-Expressions
-   Formen:
    -   Integer-Literal als Atom
    -   S-Expressions mit `+` und `-` als Operatoren

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Einen schönen Überblick über das, was uns in diesem Semester erwartet, gibt Nystrom ([2021](#ref-Nystrom2021)) in Kapitel 2 "A Map of the Territory".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den Unterschied zwischen Syntax und Semantik an einem L-Int-Ausdruck erklären.
> -   k2: Ich kann erklären, wie Integer-Literale und Operatorlisten als S-Expressions dargestellt werden.
> -   k3: Ich kann einfache L-Int-Ausdrücke entsprechend der festgelegten Auswertungsregeln auswerten.
> -   k3: Ich kann einen mehrstelligen Additionsausdruck in eine äquivalente Folge binärer Ausdrücke umformen.
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Form und Bedeutung**
>
> Betrachten Sie den Ausdruck:
>
> ``` clojure
> (- 10 (+ 3 5))
> ```
>
> 1.  Beschreiben Sie seine Struktur in eigenen Worten: Welche Operation steht außen, und welche Operanden hat sie?
> 2.  Berechnen Sie den Wert.
> 3.  Erklären Sie, welche Ihrer Aussagen zur Syntax und welche zur Semantik gehören.
>
> **Mehrstellige Subtraktion als syntaktischer Zucker**
>
> Der Ausdruck
>
> ``` clojure
> (- 20 5 3)
> ```
>
> wird laut Skript von links nach rechts geklammert.
>
> 1.  Schreiben Sie ihn als verschachtelten Ausdruck mit jeweils zwei Operanden um.
> 2.  Berechnen Sie den Wert.
> 3.  Berechnen Sie zum Vergleich auch die rechts geklammerte Variante. Sind die Ergebnisse gleich?
>
> **Negative Zahl: Literal oder Ergebnis**
>
> Vergleichen Sie diese Schreibweisen:
>
> ``` clojure
> -3
> (- 0 3)
> (+ 0 (- 5 8))
> ```
>
> Welche davon sind nach der Beschreibung von L-Int als Eingabeausdrücke vorgesehen? Welche Werte ergeben die Ausdrücke, die Sie auswerten können? Erklären Sie den Unterschied zwischen einem **Integer-Literal** und einem **Auswertungsergebnis**.
>
> </details>

<a id="id-5b9897e3373f2ab9cb2db150c803f2e2f79b5bc5"></a>

#### L-Int: Konkrete und abstrakte Syntax

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Die **konkrete Grammatik** beschreibt, welche Texte gültige L-Int-Programme sind und welche Struktur ihr **Parse-Tree** hat. Ein L-Int-Ausdruck ist entweder ein Integer-Literal oder eine Addition beziehungsweise Subtraktion mit mindestens zwei Operanden.
>
> Die konkrete Syntax enthält Details wie Klammern und Hilfsregeln, die für die spätere Verarbeitung teilweise nicht mehr benötigt werden. Die **abstrakte Grammatik** beschreibt stattdessen die wesentlichen Ausdrucksformen als `IntLiteral`, `AddExpr` und `SubExpr`. Daraus entsteht ein **AST** (*abstract syntax tree*), in dem beispielsweise Klammern nicht als eigene Knoten vorkommen.
>
> Die AST-Knoten lassen sich in Java durch ein gemeinsames `Expr`-Interface und Records modellieren. Da der AST Addition und Subtraktion binär darstellt, muss festgelegt werden, wie Ausdrücke mit mehr als zwei Operanden in binäre Knoten übersetzt werden - eine einfache Form von "Desugaring".
>
> </details>

##### Erinnerung Lispy in der Sprachstufe L-Int

``` clojure
1                       ;; 1 (Integer)
(+ 1 2)                 ;; 1 + 2
(+ 1 2 3 4)             ;; 1 + 2 + 3 + 4
(- 10 (+ 3 5))          ;; 10 - (3 + 5)
(- (+ 1 2) (- 3 4))     ;; (1 + 2) - (3 + 4)
```

Lispy wird aus S-Expressions aufgebaut, die die folgende Form haben:

-   ein Atom der Form `x`, oder
-   ein Ausdruck der Form `(. x y)` (mit `x` u. `y` S-Expressions, `.` eine Operation oder Funktion oder ein Keyword; dabei dürfen beliebig viele S-Expressions als Operanden vorkommen - hier im Beispiel sind nur zwei angedeutet).

In der Sprachstufe L-Int haben wir als Atom nur Integer-Literale, und als Operatoren kennen wir nur `+` und `-`.

**Frage**: Wie können wir das formal definieren?

##### Konkrete Grammatik für L-Int

``` ebnf
eform ::= '(' '+' eform eform+ ')'
        | '(' '-' eform eform+ ')'
        | INT
INT   ::= [0-9]+

L_int ::= eform EOF
```

Diese konkrete Grammatik beschreibt die Struktur (Syntax) der Zeichenketten, die zulässige Ausdrücke in L-Int sind.

Ein Programm in L-Int besteht aus einer S-Expression `eform` ("e" wie "Expression" - diese `eform` sind Ausdrücke und evaluieren zu einem Wert), auf die ein `EOF`-Token folgt. Weitere Eingaben sind nicht zulässig.

Ein `eform` besteht aus drei Alternativen:

-   eine S-Expression mit einem `'+'` als Operator (Addition),
-   eine S-Expression mit einem `'-'` als Operator (Subtraktion),
-   ein Integer-Literal.

Das Integer-Literal ist ein Terminal-Symbol (Token) und wird über den regulären Ausdruck `[0-9]+` beschrieben. Es besteht aus mindestens einer oder beliebig vielen Ziffern. In der Definition von S-Expressions ist das ein "Atom".

> [!TIP]
>
> *Anmerkung*: Man kann hier zwischen der Tokenart (`INT`) und dem Lexem (konkrete Ziffernfolge, die im Lexer durch den regulären Ausdruck gematcht wird) unterscheiden. Das gilt auch für die "impliziten Token", die in der Grammatik direkt als Literal dargestellt werden, beispielsweise `'('`: Dies ist die Tokenart `LPAREN` mit dem Lexem "(".

Die beiden Ausdrücke für Addition und Subtraktion haben die Klammern und den Operator als Terminal-Symbol (Token), und bestehen dann aus mindestens zwei weiteren `eform`-Ausdrücken - das sind die Operanden. Über das `eform+` erlauben wir auch mehr als zwei `eform` als Operanden, das `+` ist hierbei ein Wiederholungsoperator der Grammatik (ein- oder mehrfach). Ein `*` würde "kein- oder mehrfach" bedeuten, das `?` entsprechend "ein- oder keinmal". Bitte nicht mit dem `'+'` verwechseln: Dies ist ein "implizites" Token, also ein Token, dessen Lexem hier statt der Tokenart verwendet wird.

Das nennt sich "konkrete Grammatik", weil wir hier ausdrücken, was gültige konkrete Programme sind.

> [!TIP]
>
> ###### Schreibweise konkrete Grammatik
>
> Wir orientieren uns an einer Mischung aus ANTLR-Grammatik und der Notation in ([Siek 2023](#ref-Siek2023python)).
>
> -   klein geschrieben: Nichtterminale (Produktionsregeln) der konkreten Syntax, etwa `eform`
> -   GROSS geschrieben: Terminal-Symbole (Token) des Lexers, etwa `INT` oder `EOF`
> -   in einfachen 'Anführungszeichen': unbenannte Terminale als konkrete Zeichenketten (implizite Token), etwa `'+'` oder `'('` - für diese gibt es ebenfalls Token im Lexer (`PLUS`, `LPAREN`)
> -   UpperCamelCase: kommt in der konkreten Syntax ausschließlich für die Einstiegsregel vor: `L_int`
> -   Wiederholungsoperatoren: beziehen sich auf den Ausdruck davor (ggf. klammern!)
>     -   `?`: "ein- oder keinmal"
>     -   `*`: "kein- oder mehrfach"
>     -   `+`: "ein- oder mehrfach"

> [!TIP]
>
> ###### Implizite Token
>
> Wir verwenden in der Grammatik an einigen Stellen aus Gründen der Lesbarkeit "implizite" Token. Das bedeutet, dass wir statt der Tokenart das dazugehörige Lexem angeben. Das kann man immer dann machen, wenn das Lexem fest ist: Für das Token `LPAREN` gibt es nur ein Lexem, nämlich "(".
>
> Beispiel: Wir haben die Form `'(' '+' eform eform+ ')'`. Statt der impliziten Token könnten wir hier auch wie üblich die Tokenart nutzen: `LPAREN PLUS eform eform+ RPAREN`.
>
> Bitte die impliziten Token nicht mit den Wiederholungsoperatoren der Grammatik verwechseln.

> [!IMPORTANT]
>
> Die Grammatik zeigt die Token und wie der Lexer `INT`, `'+'`, `'('` und `')'` erkennt: Für `INT` wurde ein regulärer Ausdruck definiert, der zum Erkennen von `INT` angewendet wird. Die "impliziten" Token wie `'('` werden direkt mit der angegebenen Zeichenfolge (hier "(") erkannt.
>
> Whitespace (Leerzeichen, Tabs, Zeilenumbrüche) werden vom Lexer ignoriert. Kommentare `;; ...` (bis zum Zeilenende) werden ebenfalls vom Lexer ignoriert. Dies muss man für Tools wie ANTLR explizit formulieren; in der Darstellung der konkreten Syntax in den Vorlesungsunterlagen sparen wir uns dies zugunsten einer besseren Lesbarkeit.
>
> `EOF` ist kein Zeichen in der Eingabe, es markiert lediglich das Ende der Eingabe und wird vom Lexer als End-of-Input-Token produziert.

##### Beispiel konkrete Syntax

Schauen wir uns an, wie der Ausdruck `(- (+ 1 2) 3)` als Parse-Tree aussieht.

Text: `(- (+ 1 2) 3)`

Parse-Tree:

                                  L_int
                                 /     \
                             eform      EOF
                        _____/ | \ \______________
                       /       |  \      \        \
                     "("      "-"  eform  eform   ")"
                                   /         |
                            / | | \ \        |
                          "(" "+" ... ")"   INT
                               ...           |
                                            "3"

Wir starten mit der Startregel `L_int`. Diese fordert ein `eform` und ein `EOF`, beide bilden die zweite Ebene. Das "(" gefolgt von einem "-" passt zur zweiten Alternative in der Regel `eform`, die beiden Token werden als Kinder des `eform`-Knotens eingetragen. Dann erwarten wir eine `eform eform+` und die schließende rechte Klammer ")". Der Teilausdruck "(+ 1 2)" passt auf `eform` (hier nur teilweise als Skizze dargestellt). Die "3" passt zu `eform+`, da hier mindestens eine oder beliebig viele `eform`-Ausdrücke erwartet werden und "3" zu der Alternative `INT` in `eform` passt.

> [!TIP]
>
> **Beobachtung**: Der Parse-Tree spiegelt die Strukturen der Grammatik wider und wird selbst bei unserer einfachen L-Int-Grammatik und der kleinen Eingabe bereits relativ komplex.

##### Konkrete Grammatik - Eigenschaften

-   Die Grammatik legt fest:
    -   Welche Eingabe-Strings gültige Programme sind
    -   Struktur des **Parse-Trees**

<!-- -->

-   Aber:
    -   Enthält "technische" Details:
        -   Klammern, `EOF`, ...
        -   Hilfsnonterminale (`eform`)
    -   Nicht optimal als interne Repräsentation für die Weiterverarbeitung

Die konkrete Grammatik ist gut, um zu entscheiden: Gehört ein String zur Sprache, ja oder nein? Und sie ermöglicht es uns, einen Recursive-Descent-Parser aus der Grammatik abzuleiten. Sie liefert für eine Eingabe eines syntaktisch korrekt formulierten Programms einen dazu passenden Parse-Tree, der strukturell entsprechend den Regeln in der Grammatik aufgebaut ist.

Aber sie enthält viele technische Hilfskonstrukte, z.B. `eform` oder explizite Klammern, die wir im weiteren Verlauf für die Semantik nicht zwingend brauchen. Für die weitere Verarbeitung ist es oft besser, eine aufgeräumtere und abstraktere Sicht auf das eingegebene Programm zu haben.

##### Konkrete vs. abstrakte Grammatik

-   **Konkrete Grammatik**:
    -   Wie der Code "auf dem Papier" aussieht
    -   Enthält Syntaxdetails (Klammern, Keywords, Vorrangregeln)

<!-- -->

-   **Abstrakte Grammatik**:
    -   Wie wir den Code **im Programm** modellieren
    -   Fokus auf Bedeutungsträger (z.B. `AddExpr`, `SubExpr`, `IntLiteral`)

Wir orientieren uns an der Darstellung in ([Siek 2023](#ref-Siek2023python)) und unterscheiden sehr bewusst zwischen **konkreter** und **abstrakter Grammatik**.

Die konkrete Grammatik beschreibt das, was der Parser sieht: der tatsächliche Text, inklusive aller Klammern und syntaktischen Konstrukte.

Die abstrakte Grammatik hingegen ist das Modell, das wir intern verwenden, um die Semantik zu definieren. Sie reduziert die Sprache auf die wesentlichen Knoten, die Bedeutung tragen.

##### Abstrakte Grammatik für L-Int

``` ebnf
Expr  = IntLiteral(Int)
      | AddExpr(Expr, Expr)
      | SubExpr(Expr, Expr)

L_int = Expr
```

-   Keine `eform`-Regel mehr
-   Klammern, `EOF`, ... werden **nicht** als Knoten repräsentiert

Hier ist die abstrakte Grammatik von L-Int. Sie ist sehr kompakt: Ein Ausdruck ist entweder eine Ganzzahl, eine Addition oder eine Subtraktion. Wir geben dabei auch die Typen der Parameter vor: Die Argumente von `AddExpr` und `SubExpr` sind ihrerseits wieder `Expr`, und das `IntLiteral` wird über einem Integerwert gebildet. Klammern kommen hier nicht mehr vor - sie beeinflussen nur, wie wir beim Parsen den Baum bauen, nicht den Typ der Knoten. Diese abstrakte Grammatik bildet direkt ab, welche Bestandteile der Struktur des Programms wir für die semantische Auswertung unterscheiden wollen.

Mit der abstrakten Grammatik beschreiben wir den *abstract syntax tree* (**AST**).

> [!TIP]
>
> ###### Schreibweise abstrakte Grammatik
>
> Die abstrakte Grammatik entspricht der Definition von algebraischen Datentypen (**ADT**). Da wir das in die Implementierungssprache Java übertragen, nutzen wir hier die Konventionen aus Java und schreiben alle Datentypen (spätere Interfaces und Records/Klassen) in UpperCamelCase.
>
> Mit `Int` ist ein Integer-Datentyp gemeint, d.h. wir könnten das in Java entweder als `int` oder als `Integer` schreiben.
>
> Wir werden später Listen benötigen. Diese werden mit `[]` ausgedrückt, d.h. `[Int]` würde eine Liste mit Integer-Werten bedeuten.

##### Beispiel abstrakte Syntax (AST)

Schauen wir uns an, wie der Ausdruck `(- (+ 1 2) 3)` als AST aussieht.

Text: `(- (+ 1 2) 3)`

AST:

                                    SubExpr
                                   /       \
                              AddExpr    IntLiteral
                              /     \         |
                       IntLiteral IntLiteral  3
                           |          |
                           1          2

Wir starten mit einer Subtraktion (`SubExpr`). Der linke Operand ist eine Addition (`AddExpr`). Alle Integer werden über `IntLiteral` repräsentiert.

> [!IMPORTANT]
>
> ###### Desugaring
>
> Wir haben für den AST nur *binäre* Knoten modelliert, während der Parse-Tree variadisch ist und mehr als zwei Operanden beispielsweise in der Addition erlaubt. Dies muss an einer geeigneten Stelle entsprechend umgeformt werden, beispielsweise noch direkt im Parser oder in einem nachgelagerten Lauf ("*Pass*") über den AST.
>
> Beispiel: Wenn wir den Ausdruck `(+ 1 2 3)` betrachten, würde der Parse-Tree hier einen `eform`-Knoten mit drei Operanden beinhalten. Dies können wir nicht direkt im AST repräsentieren. Dazu müssen wir beim Erzeugen des AST den Ausdruck geeignet klammern (typisch wäre hier eine Klammerung von links nach rechts): `(+ (+ 1 2) 3)` und dazu den passenden AST erzeugen:
>
> ``` java
> AddExpr(
>     AddExpr(
>         IntLiteral(1),
>         IntLiteral(2)
>     ),
>     IntLiteral(3)
> )
> ```
>
> `(+ 1 2 3)` ist also nur "**syntaktischer Zucker**" für `(+ (+ 1 2) 3)`. Die gezeigte Vereinfachung ist eine besonders einfache Form der Vereinfachung/Umformung (auch "**Desugaring**" genannt).

##### AST in Java: Definition

Die abstrakte Grammatik übersetzen wir jetzt direkt in Java-Klassen. Wir modellieren jedes Element der abstrakten Grammatik mit passenden Java-Typen:

``` java
public interface Expr {}

public record AddExpr(Expr left, Expr right) implements Expr {}
public record SubExpr(Expr left, Expr right) implements Expr {}
public record IntLiteral(int value) implements Expr {}
```

Die Regel `Expr` in der abstrakten Grammatik kann aus verschiedenen Alternativen bestehen. Wir modellieren `Expr` als Interface, von dem die Alternativen ableiten. Jede dieser Alternativen *ist* eine `Expr`.

Für die Alternativen brauchen wir Datentypen, die ihrerseits Felder speichern können: `AddExpr(Expr, Expr)` muss zwei `Expr` als Kind-Ausdrücke speichern. Jede dieser Formen wird als `record` definiert, was uns Konstruktor, Zugriffsmethoden und `toString` automatisch mitliefert. So bekommen wir eine sehr knappe, typsichere AST-Repräsentation, die gut mit Pattern Matching zusammenspielt. (Wir könnten hier auch normale Klassen nutzen, müssten aber den ganzen Boilerplate-Code selbst schreiben.)

Diese Struktur bildet unseren AST in L-Int und wird in späteren Sprachstufen erweitert.

Die Startregel `L_int = Expr` soll vor allem ausdrücken, dass alle L-Int-Programme aus *einem* `Expr`-Ausdruck bestehen. Diese wird deshalb nicht in den AST-Typen umgesetzt, sondern findet später im Parser und Interpreter Beachtung.

> [!TIP]
>
> ###### Sealed Interfaces
>
> Normalerweise würde man das gemeinsame Interface als `sealed` Interface modellieren:
>
> ``` java
> public sealed interface Expr permits AddExpr, SubExpr, IntLiteral {}
>
> public record AddExpr(Expr left, Expr right) implements Expr {}
> public record SubExpr(Expr left, Expr right) implements Expr {}
> public record IntLiteral(int value) implements Expr {}
> ```
>
> Das hat den Vorteil, dass wir später in den `switch`-Expressions keinen `default`-Zweig brauchen und der Compiler automatisch darauf achtet, dass wir immer alle Fälle abgedeckt haben.
>
> Da wir unsere Sprachstufen aber inkrementell aufeinander aufbauen, werden wir im Laufe der Zeit immer weitere AST-Knoten hinzufügen. Wenn unser gemeinsames Interface `Expr` nun `sealed` wäre, würden wir im alten Code entsprechende Fehler bekommen, weil wir dort ja noch nicht die neuen Fälle kennen (und auch nicht kennen wollen). D.h. wir müssten dann doch wieder mit `default` arbeiten - und damit ist `sealed` nur noch halb so nützlich (schützt nur noch vor unbeabsichtigten Ableitungen).

##### AST in Java: Beispielkonstruktion

Text: `(- (+ 1 2) 3)`

AST:

                                    SubExpr
                                   /       \
                              AddExpr    IntLiteral
                              /     \         |
                       IntLiteral IntLiteral  3
                           |          |
                           1          2

``` java
Expr e = new SubExpr(
    new AddExpr(
        new IntLiteral(1),
        new IntLiteral(2)
    ),
    new IntLiteral(3)
);
```

Hier konstruieren wir den AST für `(- (+ 1 2) 3)` von Hand in Java. Das ist wichtig, um die Struktur zu verinnerlichen. Später wird der Parser genau solche Bäume für uns erzeugen. Wir können hier gut sehen, wie der abstrakte Syntaxbaum eins zu eins im Code auftaucht. Um die Tests beispielsweise für den Interpreter vom Parser zu entkoppeln, dienen uns genau solche manuell erstellten AST als direkte Eingabe.

##### Wrap-up

-   Zwei Sichtweisen auf L-Int:
    -   Konkrete Grammatik: Syntax der Eingabe-Strings
    -   Abstrakte Grammatik: AST-Struktur

<!-- -->

-   Konkrete Grammatik:
    -   Regeln und Terminale
    -   Akzeptiert eine Eingabe
    -   Erzeugt dabei einen Parse-Tree
    -   Erlaubt das Ableiten eines Parsers
-   Abstrakte Grammatik:
    -   Modellierung als algebraische Datentypen
    -   Umsetzung in Java: Interfaces und Klassen/Records

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema konkrete vs. abstrakte Syntax findet sich eine schöne Darstellung in ([Siek 2023](#ref-Siek2023python)): Kapitel 1 "Preliminaries". In ([Nystrom 2021](#ref-Nystrom2021)) kann man zum Thema Grammatik und AST in Kapitel 5 "Representing Code" nachlesen.
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den Unterschied zwischen konkreter und abstrakter Syntax erklären.
> -   k2: Ich kann Parse-Tree und AST unterscheiden und erläutern, welche Syntaxdetails im AST weggelassen werden.
> -   k3: Ich kann für einen gegebenen L-Int-Ausdruck den passenden Parse-Tree beziehungsweise AST skizzieren.
> -   k3: Ich kann einen L-Int-AST mit den passenden Java-Records konstruieren.
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Was ist ein gültiger L-Int-Ausdruck?**
>
> Ordnen Sie die folgenden Eingaben in zwei Gruppen: nach der konkreten Grammatik gültig und nicht gültig. Begründen Sie Ihre Entscheidung jeweils mit Bezug auf die Grammatik.
>
> ``` clojure
> (+ 1 2)
> (+ 1)
> (- 1 2 3)
> (+ 1 (- 2 3))
> (+ 1 2) 3
> ```
>
> Zusatzfrage: Welche Rolle spielt `EOF` bei der Entscheidung?
>
> **Vom Parse-Tree zum AST**
>
> Betrachten Sie den Ausdruck:
>
> ``` clojure
> (- (+ 1 2 3) 4)
> ```
>
> 1.  Skizzieren Sie den **Parse-Tree**. Achten Sie darauf, welche Klammern, Operatoren und `eform`-Knoten in der konkreten Grammatik vorkommen.
> 2.  Erzeugen Sie den **AST** mit der in der Vorlesung gewählten links-nach-rechts-Klammerung für mehr als zwei Operanden.
> 3.  Markieren Sie, welche Teile der konkreten Syntax im AST nicht als eigene Knoten auftauchen.
>
> **Mehrere Operanden, binärer AST**
>
> Die konkrete Grammatik erlaubt den Ausdruck:
>
> ``` clojure
> (- 10 3 2)
> ```
>
> Der AST kann `SubExpr` aber nur mit zwei Operanden darstellen.
>
> 1.  Schreiben Sie die Klammerung auf, die sich aus der links-nach-rechts-Konvention ergibt.
> 2.  Erstellen Sie den passenden AST mit den Java-Records.
> 3.  Vergleichen Sie ihn mit der alternativen Klammerung von rechts nach links. Sind die beiden ASTs strukturell gleich?
>
> </details>

<a id="id-d8a55d89d69aa672b84f607d0f4d791de1cb29a3"></a>

#### L-Int: Von Ausdrücken zum Interpreter

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Ein AST-basierter Interpreter besteht oft aus einem "Dispatch": Man traversiert mit einer `eval()`-Funktion den AST und ruft je nach Knotentyp die passende Funktion auf. Dabei werden bei Ausdrücken (*Expressions*) Werte berechnet und zurückgegeben ("eval" von "evaluieren" - "auswerten"), d.h. hier hat man einen Rückgabewert und ein entsprechendes `return` im `switch`/`case`, während man bei Anweisungen (*Statements*) keinen Rückgabewert hat.
>
> Der Dispatch (Aufruf der zum Knotentyp passenden Funktion) kann wahlweise per Visitor-Pattern oder manueller Traversierung mit `switch`/`case` erfolgen.
>
> Der Wert von Literalen ergibt sich direkt durch die Übersetzung des jeweiligen Werts in den passenden Typ der Implementierungssprache. Bei komplexeren Ausdrücken interpretiert man zunächst die Teilausdrücke durch den rekursiven Aufruf von `eval()` für die jeweiligen AST-Kindknoten und berechnet daraus das gewünschte Ergebnis.
>
> </details>

##### Von AST zu Werten: Idee

-   Gegeben: Lexer/Parser haben einen AST aufgebaut
-   Gesucht: `eval(expr)` berechnet einen **Wert**

<!-- -->

-   Beispiele:
    -   `eval( IntLiteral(1) )` $\to$ `1`
    -   `eval( AddExpr(IntLiteral(1), IntLiteral(2)) )` $\to$ `3`

<!-- -->

-   Frage:
    -   Wie repräsentieren wir solche Werte sauber im Code?

Der nächste Schritt: Aus dem AST wollen wir konkrete Ergebnisse berechnen. Dazu definieren wir eine Funktion wie `eval()`, die einen `Expr`-Knoten nimmt und einen Wert zurückgibt. Bei L-Int sind das Integer-Werte, aber wir wollen die Architektur so wählen, dass wir später problemlos weitere Typen hinzufügen können. Daher abstrahieren wir Werte ebenfalls in einer kleinen Hierarchie.

##### Value-Hierarchie: Motivation

Erinnerung: Unsere konkrete Syntax für L-Int sah so aus:

``` ebnf
Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
L_int = Expr
```

-   L-Int: Nur ganze Zahlen
-   Später:
    -   Booleans
    -   Funktionen
    -   Objekte

$\to$ `Value` als gemeinsamer Basistyp für Laufzeitwerte

Aktuell könnten wir streng genommen einfach `int` als Rückgabetyp verwenden. Aber wir planen ja schon die nächsten Sprachstufen: Booleans, Funktionen, Objekte, Klassen. Deshalb lohnt es sich, von Anfang an eine `Value`-Abstraktion einzuführen. So müssen wir den Interpreter später nicht grundlegend umbauen, sondern können die Value-Hierarchie einfach erweitern.

##### Value-Hierarchie in Java

``` java
public interface Value {

    static int asInt(Value v) {
        if (v instanceof IntVal(int val)) return val;
        throw new RuntimeException("number expected");
    }
}


public record IntVal(int v) implements Value {}
```

-   Klar getrennt von `Expr` (Syntax vs. Laufzeitwerte)

Wir müssen die Typen der Zielsprache (hier Lispy) auf unsere Implementierungssprache mappen (hier Java). D.h. die in der Zielsprache verwendeten (primitiven) Typen müssen auf passende Typen der Sprache, in der der Interpreter selbst implementiert ist, abgebildet werden.

`Value` wird unser Basistyp für Laufzeitwerte, von dem die verschiedenen Typen für Werte ableiten werden. Aktuell betrachten wir nur Integer, die wir über den Subtyp `IntVal` modellieren. Später werden weitere Typen hinzukommen.

> [!IMPORTANT]
>
> **Wichtig**: `Expr` beschreibt die abstrakte Syntax, also die Struktur des Programms, und `Value` beschreibt die Ergebnisse der Auswertung zur Laufzeit. Diese Trennung hilft uns später auch bei Typprüfung und Fehlerbehandlung.

##### Tree-Walking-Interpreter: Grundidee

-   Funktion: `Value eval(Expr expr)`

<!-- -->

-   Idee:
    -   Rekursiv über den AST laufen
    -   Pro Knotentyp die passende Operation ausführen:
        -   Literal (Integer): Als Integer-Wert zurückgeben
        -   Ausdruck (Addition, Subtraktion): linke und rechte Seite zu Integer-Werten evaluieren, die Operation ausführen, Ergebnis als Integerwert zurückgeben

Der Tree-Walking-Interpreter ist im Kern eine rekursive Funktion über dem AST. Für jeden Knotentyp - Literal, Addition, Subtraktion - definieren wir, wie er auszuwerten ist. Das ist eine direkte Implementierung der operationalen Semantik unserer Sprache: Wir sagen dem Rechner Schritt für Schritt, wie die Auswertung funktioniert.

##### Interpreter für L-Int-Expressions in Java (Pattern Matching)

``` java
public class InterpreterLint {

    public Value eval(Expr expr) {
        return switch (expr) {
            case IntLiteral(var v) -> new IntVal(v);

            case AddExpr(var l, var r) -> {
                int a = Value.asInt(eval(l));
                int b = Value.asInt(eval(r));
                yield new IntVal(a + b);
            }

            default -> throw new RuntimeException("unexpected expression: " + expr);
        };
    }
}
```

Hier sehen wir die (teilweise) Implementierung unseres L-Int-Interpreters.

Die `eval()`-Methode bildet das Kernstück des (AST-traversierenden) Interpreters für Ausdrücke. Hier wird passend zum aktuellen AST-Knoten die passende Methode des Interpreters aufgerufen.

Wir nutzen Pattern Matching in einer `switch`-Expression über dem AST-Typ `Expr`. Wir könnten hier auch das Visitor-Pattern nutzen - wichtig ist nur, dass der Baum rekursiv traversiert wird und für jeden Knoten die richtige Aktion durchgeführt wird.

###### Literale auswerten

Das ist der einfachste Teil ... Die primitiven Typen der Zielsprache Lispy müssen als Datentyp der Interpreter-Programmiersprache Java ausgewertet werden.

In der abstrakten Grammatik hatten wir definiert:

``` ebnf
Expr  = IntLiteral(Int)
```

Im AST werden also Knoten vom Typ `IntLiteral` angelegt, um die Integer-Literale zu repräsentieren.

Für jedes `IntLiteral` erzeugen wir nun im L-Int-Interpreter ein `IntVal`.

###### Ausdrücke auswerten

Für Ausdrücke hatten wir in der abstrakten Grammatik für L-Int definiert:

``` ebnf
Expr  = AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
```

Wir bekommen im AST also Knoten vom Typ `AddExpr` für eine Addition bzw. vom Typ `SubExpr` für eine Subtraktion. Beide Knoten haben jeweils ein linkes und ein rechtes Kind vom Typ `Expr`.

Für `AddExpr` und `SubExpr` werten wir zunächst die beiden Operanden rekursiv aus und kombinieren die Ergebnisse passend. Das entspricht genau der mathematischen Intuition von Addition und Subtraktion, nur eben explizit im Code ausgedrückt.

**Wichtig**: Die Subtraktion (Auswertung von `SubExpr`) fehlt hier und dient als kleine Übung.

> [!IMPORTANT]
>
> ###### Fehlerbehandlung und Fehlermeldungen
>
> Die meisten möglichen Fehlerzustände sind bereits durch den Lexer und Parser (und in späteren Sprachstufen während der semantischen Analyse) abgefangen worden. Falls zur Laufzeit die Auswertung der beiden Summanden keine Zahl ergibt, würde eine Java-Exception geworfen, die der Aufrufer an geeigneter Stelle fangen und behandeln muss. Der Interpreter soll sich ja nicht mit einem Stack-Trace verabschieden, sondern soll eine Fehlermeldung präsentieren und danach normal weiter machen ...
>
> Die hier gezeigte Fehlerbehandlung ist nur sehr rudimentär, um nicht zu sehr von den Kernkonzepten für unsere Veranstaltung abzulenken. In der Praxis würde man hier deutlich komplexere Fehlerbehandlung machen, beispielsweise auf die Zeile und Spalte in der Eingabe hinweisen. Gute Fehlerbehandlung ist eine Wissenschaft für sich. Vergleichen Sie einmal die Fehler, die Python ausgibt, und die Meldungen beispielsweise von Rust.

> [!TIP]
>
> ###### REPL: Read-Eval-Print-Loop
>
> In den Beispielen in dieser Veranstaltung wird immer davon ausgegangen, dass ein komplettes Programm eingelesen, geparst, vorverarbeitet und dann interpretiert wird.
>
> Für einen interaktiven Interpreter würde man in einer Schleife die Eingaben lesen, parsen und vorverarbeiten und dann interpretieren. Dabei würden jeweils der AST und die Symboltabelle und das Environment *ergänzt*, damit die neuen Eingaben auf frühere verarbeitete Eingaben zurückgreifen können. Durch die Form der Schleife "Einlesen - Verarbeiten - Auswerten" hat sich auch der Name "*Read-Eval-Loop*" bzw. "*Read-Eval-Print-Loop*" (**REPL**) eingebürgert.

> [!TIP]
>
> ###### Exkurs Expressions (Ausdrücke) vs. Statements (Anweisungen)
>
> In Programmiersprachen unterscheiden wir häufig **Expressions** (*Ausdrücke*) und **Statements** (*Anweisungen*).
>
> Expressions sind dabei syntaktische Konstrukte einer Programmiersprache, die (in einem gegebenen Kontext) zu einem Wert **evaluiert** werden können. Typische Expressions sind beispielsweise Ausdrücke wie `(+ 2 3)` oder `(foo 42)` ... In manchen Sprachen sind beispielsweise auch Zuweisungen Expressions: `v = 42 + 7;` würde in C der Variablen `v` den Wert 49 zuweisen, dies ist gleichzeitig auch der Wert des gesamten Ausdrucks. Man könnte in C also Dinge formulieren wie `if (v = 42 + 7) ...` (wobei das Interpretieren eines Integers in einem bool'schen Kontext nochmal ein anderes Problem ist).
>
> Statements sind syntaktische Konstrukte in Programmiersprachen, die **ausgeführt** werden können und dabei in der Regel einen Zustand im Programm verändern, also einen Seiteneffekt haben. Die Ausführung eines Statements hat normalerweise keinen Wert an sich. Typische Beispiele sind Zuweisungen `v = 7`, Kontrollfluss `if (...) then {...} else {...}`, Schleifen `for x in foo: ...`, `switch/case`-Statements. (Es gibt aber auch Programmiersprachen, wo ein `if/then/else`-Konstrukt eine Expression ist, also bei der Ausführung einen Wert ergibt.) Expressions können in den meisten Programmiersprachen Teile von Statements bilden: In `v = 42 + 7` ist die gesamte Zuweisung eine Anweisung (Seiteneffekt: die Variable `v` hat danach einen anderen Zustand), und der Teil `42 + 7` ist ein Ausdruck, der ausgewertet werden kann und üblicherweise den Wert 49 ergibt (außer man beauftragt ein LLM mit der Auswertung). In C-ähnlichen Sprachen kann durch Hinzufügen eines Semikolons aus dem Ausdruck `42 +7` eine Anweisung gemacht werden ...
>
> Vergleiche auch ([Nystrom 2021](#ref-Nystrom2021)), Kapitel 6 "Parsing Expressions", Kapitel 7 "Evaluating Expressions" und Kapitel 8 "Statements and State", aber auch [Wikipedia: Expression](https://en.wikipedia.org/wiki/Expression_(computer_science)) und [Wikipedia: Statement](https://en.wikipedia.org/wiki/Statement_(computer_science)). Wir werden uns an späterer Stelle noch darum kümmern.

##### Interpreter in Aktion

Beispiel:

``` java
// (- (+ 1 2) 3)
Expr expr =
    new SubExpr(
        new AddExpr(new IntLiteral(1), new IntLiteral(2)),
        new IntLiteral(3)
    );

var interp = new InterpreterLint();
Value result = interp.eval(expr);
// result: IntVal(0)
```

Wenn wir den eben gezeigten Ausdruck `(1 + 2) - 3` mit unserem Interpreter auswerten, erhalten wir erwartungsgemäß `0`, verpackt als `IntVal`.

An der Stelle müssen wir den AST noch manuell konstruieren. In Zukunft bekommt der Interpreter diesen AST aber vom Lexer und Parser, und oft werden wir noch weitere Verarbeitungsschritte haben ("Semantische Analyse"), bevor der AST schließlich zum Interpreter gelangt und ausgewertet wird.

##### Wrap-up

-   Interpreter simulieren die Programmausführung
    -   Code ausführen: Read-Eval-Loop
-   Tree-Walking-Interpreter traversieren den AST
-   Auswertung der Knoten mit `eval(Expr e)`

Wir haben heute den semantischen Kern von L-Int gebaut. Wir haben eine Value-Hierarchie in Java implementiert und einen Interpreter, der ASTs für L-Int auswertet. In der nächsten Sitzung geht es darum, wie wir diese ASTs automatisch aus Quelltext erzeugen: mit einem handgeschriebenen Lexer und einem Recursive-Descent-Parser.

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema Auswertung von Ausdrücken im AST findet sich eine schöne Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 7 "Evaluating Expressions". Siek ([2023](#ref-Siek2023python)) beschäftigt sich mit der Interpretation von einfachen Ausdrücken in Kapitel 1 "Preliminaries".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k3: Ich kann die Traversierung von ASTs implementieren: Pattern Matching mit `switch`-Expresssion oder Visitor-Pattern
> -   k3: Ich kann bei der Traversierung des AST die dort abgelegten Ausdrücke auswerten
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Den Interpreter beim Arbeiten beobachten**
>
> Gegeben ist folgender AST:
>
> ``` java
> Expr expr =
>     new SubExpr(
>         new AddExpr(new IntLiteral(4), new IntLiteral(2)),
>         new AddExpr(new IntLiteral(1), new IntLiteral(3))
>     );
> ```
>
> Verfolgen Sie den Aufruf von `eval(expr)` Schritt für Schritt:
>
> 1.  In welcher Reihenfolge werden die AST-Knoten ausgewertet?
> 2.  Welcher Wert wird jeweils zurückgegeben?
> 3.  Welchen endgültigen Wert liefert der Interpreter?
>
> Notieren Sie das Ergebnis beispielsweise als **Tabelle** mit den Spalten **Knoten**, **ausgewertete Kinder** und **Rückgabewert**.
>
> **Den fehlenden Fall ergänzen**
>
> Die im Skript gezeigte `eval()`-Methode behandelt `IntLiteral` und `AddExpr`, aber noch nicht `SubExpr`.
>
> Ergänzen Sie den passenden `case`-Zweig. Entwickeln Sie anschließend zwei kleine Tests, die zeigen, dass Ihre Implementierung korrekt ist. Einer der Tests soll insbesondere aufdecken, ob versehentlich die Operanden vertauscht wurden.
>
> **Woher kann ein Laufzeitfehler kommen?**
>
> In den arithmetischen Fällen wird `Value.asInt(...)` aufgerufen. Diese Methode wirft eine `Exception`, wenn der übergebene Wert kein `IntVal` ist.
>
> Diskutieren Sie:
>
> -   Kann `Value.asInt(...)` bei einem AST, der nur aus den aktuell definierten L-Int-Knoten besteht, tatsächlich fehlschlagen?
> -   Warum könnte die Methode trotzdem einen Laufzeitfehler prüfen und nicht einfach einen Cast verwenden?
> -   Was ist der Unterschied zwischen einem `Expr`-Knoten und einem `Value`?
>
> </details>

<a id="id-ace9fbbfab20407167957763e345b4cb8130dc1b"></a>

#### L-Int: Lexer - Handcodierte Implementierung

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Der Lexer (auch "Scanner") soll den Zeichenstrom in eine Folge von Token zerlegen. Zur Spezifikation der Token werden reguläre Ausdrücke verwendet.
>
> Von Hand implementierte Lexer arbeiten üblicherweise iterativ und durchlaufen die Eingabe schrittweise von links nach rechts. Dabei verarbeiten wir immer das aktuelle Zeichen im Eingabestrom.
>
> Lexer sollten möglichst effizient sein, da sie noch direkt auf der niedrigsten Abstraktionsstufe arbeiten und u.U. oft durchlaufen werden.
>
> Die Palette an Fehlerbehandlungsstrategien im Lexer reicht von "aufgeben" über den "Panic Mode" ("gobbeln" von Zeichen, bis wieder eines passt) und Ein-Schritt-Transformationen bis hin zu speziellen Lexer-Regeln, die beispielsweise besonders häufige Typos abfangen.
>
> </details>

##### Lexer: Erzeugen eines Token-Stroms aus einem Zeichenstrom

Aus dem Eingabe(-quell-)text

``` clojure
;; demo
(+ 1 222 )
```

erstellt der Lexer (oder auch Scanner genannt) eine Sequenz von Token:

    <LPAREN, "(">   <PLUS, "+">   <INT, "1">   <INT, "222">   <RPAREN, ")">

Wir wollen den Eingabetext (Zeichenstrom) in *abstraktere Einheiten* aufsplitten: die **Token**.

Jedes Token hat einen **Typ** und speichert ein **Lexem** - das ist der Teil der Eingabe, der zu dem Token gematcht wurde.

Der Lexer versucht anhand von Regeln (reguläre Ausdrücke) zu erkennen, welche Token in der Eingabe vorkommen. In der Regel geht man zeichenweise von links nach rechts durch die Eingabe und versucht anhand des aktuellen Zeichens zu erkennen, welches Token hier beginnt. Bei Mehrdeutigkeiten muss man entsprechend noch weitere folgende Zeichen mit einbeziehen.

##### Modellierung der Token

``` ebnf
eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
INT   ::= [0-9]+
```

``` java
public enum TokenKind {
    PLUS, MINUS, LPAREN, RPAREN, SEMI,
    INT,  // [0-9]+
    EOF
}

public record Token(TokenKind kind, String lexeme) {}
```

> [!TIP]
>
> ###### Design-Diskussion: Modellierung Token
>
> Es gibt verschiedene Möglichkeiten, Token zu modellieren.
>
> Aus der C-Welt stammt die gezeigte Modellierung: Man nutzt ein Enum, um die Tokenarten zu modellieren. (Wir werden dieses Enum im Laufe des Semesters noch deutlich ergänzen.) Für die Repräsentation der Token nutzt man dann eine gemeinsame Daten-Klasse, die einfach die Tokenart und das Lexem kombiniert. Das ist übersichtlich, allerdings muss man für Token wie `PLUS` das Lexem "+" dann unnötigerweise nochmal explizit mit speichern.

> [!IMPORTANT]
>
> ###### Weitere Token und Eigenschaften
>
> Token enthalten oft auch Positionen oder Quelltextbereiche (Zeile, Spalte, Start- und Endindex). Das erleichtert anständige Fehlermeldungen erheblich.
>
> `EOF` ist ein synthetisches Token und hat kein Lexem aus dem Quelltext. Das hier verwendete "`<EOF>`" ist eine mögliche Debug-Darstellung, aber kein tatsächliches Lexem.

##### Grundstruktur eines Lexers

``` java
public class LispyLexerLint {
    protected String source;
    protected int current;

    public List<Token> scan(String input) {
        source = input;  current = 0;
        List<Token> tokens = new ArrayList<>();

        do tokens.add(nextToken());
        while (tokens.getLast().kind() != EOF);

        return tokens;
    }

    // Arbeitsmethode: nextToken()
    // Hilfsmethoden:  isAtEnd(), peek(), advance() ...
}
```

Der Lexer läuft über den Eingabestring und erzeugt eine vollständige Liste mit Token. Irrelevante Zeichen wie Whitespace und Kommentare werden dabei immer wieder entfernt (s.u.).

Die Einsprungsfunktion `scan()` stützt sich dabei auf der zentralen Hilfsmethode `nextToken()` ab, die das nächste Token an der aktuellen Eingabeposition bildet. Die Implementierung ist algorithmisch einfach: Wir schauen uns das aktuelle Zeichen an, entscheiden, um welchen Token-Typ es sich handelt, und rücken den Positionszeiger entsprechend vor. Falls nötig, ziehen wir weitere Zeichen in diese Entscheidung mit ein.

**Anmerkung**: In Lispy werden Whitespaces und Kommentare in späteren Verarbeitungsstufen nicht mehr benötigt und deshalb entfernt. In Sprachen wie Python spielen Whitespaces aber eine wichtige syntaktische Rolle und werden deshalb geeignet als Token erfasst und an den Parser weitergegeben.

> [!TIP]
>
> ###### Design-Diskussion: Schnittstelle Lexer
>
> In der gezeigten Variante verarbeitet der Lexer den *kompletten* Input und erzeugt (falls möglich) eine *vollständige Liste* mit den zur Eingabe gehörenden Token. Der nachfolgende Parser kann damit vom Lexer entkoppelt werden und bekommt als Input einfach eine Liste mit Token und arbeitet auf dieser Liste - die Parserimplementierung muss nicht wissen, dass es eine Klasse `Lexer` gibt und wie diese aussieht. Der Preis ist dabei möglicher Mehrbedarf an Speicher und möglicherweise schlechtere Fehlermeldungen.

##### Partielle Beispielimplementierung Lexer für L-Int

``` java
protected Token nextToken() {
    skipIrrelevant();

    if (!isAtEnd()) {
        char c = advance();

        if (Character.isDigit(c)) {
            String lexeme = c + readWhile(Character::isDigit);
            return Token.from(INT, lexeme);
        }
        return switch (c) {
            case '(' -> Token.from(LPAREN);
            case '+' -> Token.from(PLUS);
            default -> throw new RuntimeException("unexpected character");
        };
    }
    return Token.from(EOF);
}
```

Hier wird eine typische Implementierung von `nextToken()` gezeigt.

Irrelevante Zeichen wie Whitespace und Kommentare werden zu Beginn entfernt (übersprungen). Falls wir danach nicht am Ende der Eingabe sind, können wir das aktuelle Zeichen lesen und entscheiden, welches Token hier vorliegt (oder beginnt). Gelesene Zeichen werden aus der Eingabe "entfernt": Beim Aufruf von `advance()` wird der Index `current` inkrementiert (falls möglich).

Wenn das aktuelle Zeichen eine Ziffer ist, lesen wir so lange weiter, bis keine Ziffer mehr folgt, und erzeugen ein `INT`-Token. Andernfalls unterscheiden wir anhand des Zeichens die Operatoren und Klammern. Unbekannte Zeichen führen zu einem Fehler. Das ist im Kern das, was auch generierte Lexer tun.

Falls wir am Ende der Eingabe angelangt sind, erzeugen wir das synthetische Token `EOF` für den Parser.

**Wichtig**: Die Subtraktion und die Behandlung der schließenden Klammer fehlen hier und dienen als kleine Übung.

*Anmerkung*: Häufig findet man im Lexer keinen "schönen" objektorientierten Ansatz. Das hat häufig Performancegründe ...

> [!TIP]
>
> ###### Nützliche Hilfsmethoden für Token
>
> Um beim Erzeugen der Token nicht immer wieder `new` schreiben zu müssen, und um nicht bei Token mit festem Lexem dieses immer wieder extra angeben zu müssen, kann man sich einige statische `from()`-Hilfsmethoden im Enum `Token` definieren, die aus dem Tokentyp das passende Token erzeugen (vgl. Challenge).

##### Hilfsfunktionen im Lexer: Zeichenhandling

``` java
protected boolean isAtEnd() { return current >= source.length(); }

protected char peek() { return peek(0); }
protected char peek(int la) {
    if (la < 0 || current + la >= source.length()) return '\0';
    return source.charAt(current + la);
}

protected char advance() { return isAtEnd() ? '\0' : source.charAt(current++); }

protected boolean skipIrrelevant() {}  // Homework

protected String readWhile(Predicate<Character> pred) {
    StringBuilder sb = new StringBuilder();
    while (!isAtEnd() && pred.test(peek())) sb.append(advance());
    return sb.toString();
}
```

Um nicht immer wieder manuell mit dem Index `current` und der Eingabe `source` hantieren zu müssen (das hat sich als sehr fehlerträchtig herausgestellt), definiert man sich typischerweise einige Hilfsmethoden. Die hier gezeigten Namen sind tatsächlich relativ verbreitet, aber man findet beispielsweise auch oft `consume()` statt `advance()`.

Wir müssen im Lexer immer wieder erkennen, ob bereits alle Zeichen im Input abgearbeitet sind. Um nicht immer wieder den Index `current` mit der Länge des Inputs `source` vergleichen zu müssen, kapselt man dies in der Hilfsmethode `isAtEnd()`. Diese liefert `true`, sobald wir alle Zeichen im Input-String `source` bearbeitet haben.

In der Hauptschleife `nextToken()` müssen wir auf das aktuelle Zeichen schauen. Auch hier bietet es sich an, dies nicht immer wieder manuell über `source.charAt(current)` zu tun, sondern dies über eine Hilfsmethode `peek()` zu kapseln. Im obigen Beispiel ist auch gleich eine Implementierung für ein `peek()` mit Lookahead gezeigt - wir werden später auch das nächste und übernächste Zeichen nach dem aktuellen Zeichen benötigen.

Für das Weiterschalten gibt es die Methode `advance()`. Diese gibt das aktuelle Zeichen zurück und schaltet auf das nächste Zeichen, d.h. ein nachfolgendes `peek()` liefert dann das nächste Zeichen. Damit wir nicht über den Input-String hinaus laufen, wird die Weiterschaltung nur gemacht, wenn wir noch nicht am Ende des Inputs sind. Anderenfalls liefern wir ein Null-Zeichen (`\0`) zurück.

Der Aufruf von `skipIrrelevant()` "entfernt" vom aktuellen Zeichen an alle nicht benötigten Zeichen wie Whitespace und Kommentare und stoppt, sobald ein benötigtes Zeichen auftaucht. Dabei muss die Funktion selbst prüfen, ob überhaupt noch gelesen werden darf, oder ob bereits das Ende des Inputs erreicht ist. Falls Zeichen entfernt wurden, wird `true` zurückgegeben. (In unserer Implementierung springt der Index `current` weiter, es werden keine Zeichen physisch entfernt.) **Wichtig**: Die Implementierung von `skipIrrelevant()` dient als kleine Hausaufgabe (Übungsblatt).

Die Funktion `readWhile()` bekommt einen Test `pred` als Argument und liest so lange, wie das aktuelle Zeichen diesen Test erfüllt und das Ende der Eingabe nicht erreicht ist. Jedes gelesene Zeichen wird an einen StringBuilder angehängt und am Ende als String zurückgeliefert. Diese Funktion ist sehr praktisch, wenn man beispielsweise aufeinander folgende Ziffern einlesen will: `String lexeme = readWhile(Character::isDigit);` liest so lange, wie Ziffern in der Eingabe vorkommen und liefert diese zunächst als String zurück.

**Anmerkung**: Der gezeigte Lexer verändert den Eingabestring nicht: Er verschiebt lediglich den Index `current`. "Zeichen werden aus der Eingabe entfernt" bedeutet hier also, dass der Index `current` inkrementiert wird.

> [!TIP]
>
> ###### Design-Diskussion: Index-Handling und Robustheit
>
> Das Hantieren mit dem Index und dem Weiterschalten muss gut aufeinander abgestimmt sein - der Lexer sollte hier nicht in einen ungültigen Zustand geraten können.
>
> Grundsätzlich könnte man es den Usern überlassen, die Aufrufe immer korrekt zu machen und damit nie über das Ende des Inputs hinaus zu lesen. In der Praxis lohnt es sich aber, etwas mehr Robustheit einzubauen.
>
> Im obigen Beispiel habe ich die Rückgabe eines besonderen Null-Zeichens gewählt, dies stammt aus der C-Welt und bedeutet dort "wir sind am Ende des Strings angekommen".
>
> Alternativ könnte man auch eine Exception auslösen beim Versuch, am Ende der Eingabe noch ein weiteres `advance()` zu machen. Dies zieht aber schnell eine relativ umständliche Fehlerbehandlung nach sich ...

##### Typische Muster für Erstellung von Token

1.  Schlüsselwörter

    -   Ein eigenes Token für jedes Schlüsselwort, oder
    -   Erkennung als Name (`ID`) und nachträglich Vergleich mit Wörterbuch sowie Korrektur des Tokentyps

    Wenn Schlüsselwörter über je ein eigenes Token abgebildet werden, benötigt man für jedes Schlüsselwort einen eigenen RE bzw. DFA. Die Erkennung als Bezeichner und das Nachschlagen in einem Wörterbuch (geeignete Hashtabelle) sowie die entsprechende nachträgliche Korrektur des Tokentyps kann die Anzahl der Zustände im Lexer signifikant reduzieren! Wir werden dies später in der Sprachvariante L-Var sehen.

2.  Operatoren

    -   Ein eigenes Token für jeden Operator, oder
    -   Gemeinsames Token für jede Operatoren-Klasse

3.  Bezeichner: Ein gemeinsames Token für alle Namen

4.  Zahlen: Ein gemeinsames Token für alle numerischen Konstanten (ggf. Integer und Float unterscheiden)

    Für Zahlen führt man oft ein Token "`NUM`" oder "`INT`"ein. Als Attribut speichert man das Lexem als String. Alternativ kann man das Lexem bereits in eine Zahl konvertieren und als (zusätzliches) Attribut speichern. Dies kann in späteren Stufen viel Arbeit sparen.

5.  String-Literale: Ein gemeinsames Token

6.  Komma, Semikolon, Klammern, ...: Je ein eigenes Token

7.  Regeln für Whitespace und Kommentare etc. ...

    Normalerweise benötigt man Kommentare und Whitespaces in den folgenden Stufen nicht und entfernt diese deshalb aus dem Eingabestrom. Dabei könnte man etwa Whitespaces in den Pattern der restlichen Token berücksichtigen, was die Pattern aber sehr komplex macht. Die Alternative sind zusätzliche Pattern, die auf die Whitespace und anderen nicht benötigten Inhalt matchen und diesen "geräuschlos" entfernen. Mit diesen Pattern werden keine Token erzeugt, d.h. der Parser und die anderen Stufen bemerken nichts von diesem Inhalt.

    Gelegentlich benötigt man aber auch Informationen über Whitespaces, beispielsweise in Python. Dann müssen diese Token wie normale Token an den Parser weitergereicht werden.

> [!TIP]
>
> ###### Anmerkung: Längster Match
>
> Wenn es mehrere matchende Tokenregeln gibt, wird in der Regel das längste Lexem bevorzugt.
>
> Wenn es mehrere gleich lange Alternativen gibt, muss man mit Vorrangregeln bzgl. der Token arbeiten. Bei ANTLR wäre das die Reihenfolge, in der die Token definiert wurden.
>
> Bei einem handgeschriebenen Lexer muss die Implementierung dieses Verhalten selbst festlegen.

##### Fehler bei der lexikalischen Analyse

Problem: Eingabestrom sieht so aus: `(fi (== a 42) ...)`

Der Lexer kann nicht erkennen, ob es sich bei `fi` um ein vertipptes Schlüsselwort handelt oder um einen Bezeichner: Es könnte sich um einen Funktionsaufruf der Funktion `fi()` handeln ... Dieses Problem kann erst in der nächsten Stufe (Parser) sinnvoll erkannt und behoben werden.

=\> Was tun, wenn keines der Pattern auf den Anfang des Eingabestroms passt?

Optionen:

-   Aufgeben ...

    Eventuell vielleicht sogar die beste und einfachste Variante :-)

<!-- -->

-   "Panic Mode": Entferne so lange Zeichen, bis ein Pattern passt.

    Das verwirrt u.U. den Parser, kann aber insbesondere in interaktiven Umgebungen hilfreich sein. Ggf. kann man dem Parser auch signalisieren, dass hier ein Problem vorlag.

<!-- -->

-   Ein-Schritt-Transformationen:
    -   Füge fehlendes Zeichen in Eingabestrom ein.
    -   Entferne ein Zeichen aus Eingabestrom.
    -   Vertausche ein Zeichen:
        -   Ersetze ein Zeichen durch ein anderes.
        -   Vertausche zwei benachbarte Zeichen.

    Diese Transformationen versuchen, den Input in einem Schritt zu reparieren. Das ist durchaus sinnvoll, da in der Praxis die meisten Fehler in dieser Stufe durch ein einzelnes Zeichen hervorgerufen werden: Es fehlt ein Zeichen oder es ist eines zu viel im Input. Es liegt ein falsches Zeichen vor (Tippfehler) oder zwei benachbarte Zeichen wurden verdreht ...

    Allerdings muss man sich in Erinnerung rufen, dass gerade in dieser ersten Phase eines Compilers die Geschwindigkeit stark im Fokus steht und eine ausgefeilte Fehlerkorrekturstrategie die vielen kleinen Optimierungen schnell wieder zunichte machen kann. Obendrein kann der Lexer ohne weiteren syntaktischen oder semantischen Kontext oft nicht zuverlässig entscheiden, was gemeint war.

<!-- -->

-   Fehler-Regeln: Matche typische Typos (Fehler-Token)

    Gelegentlich findet man in den Grammatiken für den Lexer extra Regeln, die häufige bzw. typische Typos matchen und dann passend darauf reagieren.

##### Wrap-up

-   Lexer zerlegt den Quelltext von links nach rechts in Token
    -   Token: hat Tokenart und Lexem
    -   Reguläre Ausdrücke beschreiben die Formen der Token
    -   Whitespace und Kommentare überspringen (sofern nicht für spätere Verarbeitungsstufen benötigt)

<!-- -->

-   Implementierungsansatz: Zeichenweise Iteration durch die Eingabe
    -   Entscheidung für ein Token am aktuellen Zeichen (plus ggf. weitere folgende Zeichen ("Lookahead"))
    -   Ggf. Lookahead notwendig, z.B. bei "==" vs. "="
    -   Bevorzugung des längsten Matches
    -   Ende der Eingabe: Künstliches `EOF`-Token

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema Lexing findet sich eine schöne Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 4 "Scanning".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne die Aufgaben eines Lexers
> -   k2: Ich kann typische Fehler und Lösungsansätze in der lexikalischen Analyse erklären
> -   k3: Ich kann für ein Problem eine typische Einteilung von Token vornehmen
> -   k3: Ich kann einen Top-Down-Lexer mit Read-Ahead implementieren
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Design-Diskussion**
>
> 1.  Modellierung Token
>
>     Welche Möglichkeiten, Token zu modellieren, gibt es noch? Welche Vor- und Nachteile haben die Varianten?
>
> 2.  Schnittstelle Lexer
>
>     Eine häufige Variante der Lexer-Schnittstelle ist die Methode `nextToken()` als Einsprungmethode in den Lexer: `public Token nextToken()`. Der Parser bekommt dann ein Lexer-Objekt und ruft selbst immer wieder bei Bedarf `nextToken()` auf. Was könnten Vor- und Nachteile zur oben im Skript gezeigten Variante sein?
>
> **Token-Strom vorhersagen: Wo endet das Lexem?**
>
> Bestimmen Sie für jede Eingabe den Token-Strom, den der Lexer erzeugen sollte. Geben Sie jeweils Tokenart und Lexem an; das synthetische `EOF`-Token können Sie dazuschreiben.
>
> ``` clojure
> (+ 12 345)
> (- 7 8)
> (+12 3)
> (+ 12a 3)
> ```
>
> Diskussionsfragen:
>
> -   Gehört ein Vorzeichen zum Integer-Literal oder ist es ein eigenes Token? Begründen Sie Ihre Entscheidung anhand der Tokenregeln für L-Int.
> -   Was passiert bei der letzten Eingabe: Erkennt der Lexer "12a" als ein fehlerhaftes Integer-Literal, oder liest er zunächst "12" als `INT`?
>
> **Hilfsfunktionen für Token**
>
> Jedes Token hat üblicherweise ein Attribut, in dem das Lexem gespeichert wird. Bei eindeutigen Token (etwa bei eigenen Token je Schlüsselwort oder bei den Interpunktions-Token) kann man sich das Attribut auch sparen, da das Lexem durch die Tokenart eindeutig rekonstruierbar ist.
>
> Bei unserer Implementierung in Java müsste man dann entweder zwei verschiedene Klassen zur Modellierung der Token nutzen (was eher umständlich ist), oder man spendiert der `Token`-Klasse statische **Hilfsfunktionen**, die ein Token bauen: einmal vollständig (mit Token-Typ und Lexem) und einmal nur mit dem Token-Typ und daraus abgeleitetem festen Lexem.
>
> Wir haben oben zwei Verwendungsmöglichkeiten gesehen:
>
> ``` java
> Token.from(INT, lexeme)
> Token.from(LPAREN)
> ```
>
> Implementieren Sie diese Hilfsmethoden. Wie sehen die Signaturen aus? Was wird in `TokenKind` zusätzlich noch benötigt, damit das funktioniert?
>
> **`nextToken()` für L-Int vervollständigen**
>
> Die Beispielimplementierung erkennt "(", "+" und Integer-Literale. Ergänzen Sie sie so, dass sie auch "-" und ")" als Token erkennt.
>
> Skizzieren Sie Ihren Code. Prüfen Sie anschließend gemeinsam, ob Ihr Lexer bei dieser Eingabe alle Zeichen genau einmal verarbeitet:
>
> ``` clojure
> (- 12 (+ 3 45))
> ```
>
> Diskussionsfragen:
>
> -   Welche Aufgabe übernimmt `advance()` in den einzelnen Fällen?
> -   Warum wird bei einem Integer-Literal nicht nur ein Zeichen gelesen?
>
> **Read-Ahead: Unterscheiden von "=" und "=="**
>
> Angenommen, wir müssen die Token `EQEQ` (Lexem "==") und `EQUAL` (Lexem "=") erkennen und voneinander unterscheiden. Beide fangen mit "=" an. Wie könnte man das in der `switch`-Expression in `nextToken()` formulieren?
>
> **Fehlerfall mit einer klaren Regel behandeln**
>
> Der gezeigte Lexer bricht bei einem unbekannten Zeichen mit einer Exception ab. Überlegen Sie sich für L-Int eine einfache alternative Strategie: Der Lexer soll beim ersten unbekannten Zeichen eine verständliche Fehlermeldung erzeugen und anschließend entweder abbrechen oder weiter nach dem nächsten erkennbaren Token suchen.
>
> Verwenden Sie beispielsweise diese Eingabe:
>
> ``` clojure
> (+ 12 @ 3)
> ```
>
> Legen Sie fest, wie sich der Lexer verhalten soll, und beschreiben Sie die resultierenden Token beziehungsweise die Fehlermeldung.
>
> **Wichtig**: Falls Ihr Lexer nach einem Fehler weiterläuft: Formulieren Sie eine Regel, die sicherstellt, dass er dabei nicht an derselben Eingabeposition hängen bleibt.
>
> </details>

<a id="id-ebe1e096694731fb5532fdd9572b900a699e7eaf"></a>

#### L-Int: LL-Parser mit Recursive Descent

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Ein LL-Parser mit rekursivem Abstieg (*recursive descent*, auch *RD-Parser*) verarbeitet den Tokenstrom von links nach rechts und wählt Alternativen anhand eines begrenzten Lookaheads:
>
> -   Für Nichtterminale werden Parsermethoden aufgerufen; Terminale werden mit `match()` geprüft und "konsumiert".
> -   Alternativen werden anhand ihrer möglichen Anfangstokens ausgewählt. Optionale Teile und Wiederholungen werden typischerweise mit `if` und `while` umgesetzt.
> -   Die Grammatik muss eine eindeutige Vorhersage erlauben; bei leeren Alternativen sind dafür auch FOLLOW-Mengen relevant.
>
> Zur Implementierung eines RD-Parsers kann man so vorgehen:
>
> -   Zu jeder Produktionsregel erstellt man eine gleichnamige Funktion.
> -   Wenn in der Produktionsregel andere Regeln "aufgerufen" werden, erfolgt in der Funktion an dieser Stelle der entsprechende Funktionsaufruf.
> -   Bei Terminalsymbolen wird das erwartete Token mit `match()` geprüft.
>
> Dabei findet man wie bereits im Lexer die Funktionen `match()` und `advance()`, die sich hier aber auf den Tokenstrom beziehen. LL(1)-Parser schauen dabei immer nur das nächste Token an, LL(k) haben ein entsprechendes Lookahead von bis zu $k$ Token.
>
> RD-Parser haben ein Problem mit Linksrekursion in der Grammatik, da hier rekursiv Methoden aufgerufen werden und keine Token entfernt werden; eventuell auftretende Linksrekursion muss zunächst beseitigt werden.
>
> </details>

##### Recursive-Descent-Parser: Ziel

-   Aus Tokens einen AST bauen:

    ``` java
    Expr parse(List<Token> tokens);
    ```

<!-- -->

-   Basis konkrete Syntax (Grammatik):

    ``` ebnf
    eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
    INT   ::= [0-9]+
    ```

-   Zielformat AST (abstrakte Syntax):

    ``` ebnf
    Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
    ```

Mit dem Lexer können wir jetzt eine Tokenliste erzeugen. Der nächste Schritt ist der Parser, den wir als Recursive-Descent-Parser implementieren. Wir verwenden dafür eine leicht umgeformte Grammatik ohne Linksrekursion. Die Methode `parse()` wird unser Einstiegspunkt sein und am Ende einen `Expr`-AST liefern, den wir mit unserem Interpreter auswerten können.

##### Grundidee LL-Parser mit Recursive Descent

Die Grammatik (konkrete Syntax) besteht aus einer Menge von Produktionsregeln:

``` ebnf
r ::= X s
```

Diese Regeln werden jeweils in eine dazu passende Funktion überführt:

``` java
Expr parseR() {
    match(X);
    return s();
}
```

-   Für jede Regel in der Grammatik wird eine Methode/Funktion mit diesem Namen definiert
-   Referenzen auf ein Terminal (Token) `T` werden durch den Aufruf der Methode `match(T)` aufgelöst
    -   `match(T)` "konsumiert" das aktuelle Token, falls dieses mit `T` übereinstimmt
    -   Anderenfalls löst `match()` eine Exception aus
-   Referenzen auf Nichtterminale (Regeln) `s` werden durch Methodenaufrufe `s()` aufgelöst

##### Alternative Subregeln

Alternativen in einer Regel:

``` ebnf
    a | b | c
```

Auflösen durch Unterscheidung am aktuellen Token (und falls nötig weiterem Lookahead):

``` java
return switch (peek()) {
    case PredictingA -> parseA();
    case PredictingB -> parseB();
    case PredictingC -> parseC();
    default -> throw new RuntimeException("expected expression");
};
```

Anhand des aktuellen Tokens (`peek()`) wird entschieden, welche Alternative vorliegt und welche der `parseXYZ()`-Methoden entsprechend aufgerufen werden soll.

`PredictingA` soll andeuten, dass man mit dem aktuellen Token eine Vorhersage für die Regel `a` versucht (hier kommen die FIRST- und FOLLOW-Mengen ins Spiel ...). Wenn das der Fall ist, springt man entsprechend in die Funktion bzw. Methode `a()`. Formal berechnet man die Lookahead-Mengen mit `FIRST` und `FOLLOW`, um eine Entscheidung für die nächste Regel zu treffen. Praktisch betrachtet kann man sich fragen, welche(s) Token eine Phrase in der aktuellen Alternative starten können.

Für LL(1)-Parser betrachtet man immer das **aktuelle** Token (**genau *EIN* Lookahead-Token**), um eine Entscheidung zu treffen. Bei LL(k) nutzt man einen Lookahead von insgesamt bis zu $k$ Token.

> [!TIP]
>
> ###### Exkurs LL(k)
>
> Die folgende Regel ist für einen LL(1)-Parser nicht deterministisch behandelbar, da die Alternativen mit dem gleichen Token beginnen (die Lookahead-Mengen überlappen sich).
>
> ``` ebnf
> expr ::= ID '++' | ID '--'
> ```
>
> Hier benötigt man entweder **zwei Lookahead-Tokens**, also einen LL(2)-Parser, oder man muss die Regel in eine äquivalente LL(1)-Grammatik umschreiben ($\to$ **Linksfaktorisierung**, s.u.):
>
> ``` ebnf
> expr ::= ID ('++' | '--')
> ```

##### Grundstruktur eines RD-Parsers

``` java
public class LispyParserLint {
    protected List<Token> tokens;
    protected int current;

    public Expr parse(List<Token> tokenList) {
        tokens = tokenList;  current = 0;

        Expr expr = parseEform();
        if (!isAtEnd()) throw new RuntimeException("unexpected token");

        return expr;
    }

    // Arbeitsmethoden: parseEform(), parseAtom(), parseAddExpr(), parseSubExpr()
    // Hilfsmethoden:   isAtEnd(), peek(), advance(), match(), check() ...
}
```

Der Parser sieht strukturell dem Lexer ähnlich: Er hat eine Eingabefolge - hier die Token-Liste `tokens` - und einen Positionszeiger (aktueller Index `current`).

Den Einstieg in den Parser bildet die Methode `parse()`, die den gesamten Eingabestrom gemäß der Grammatik verarbeitet. Diese Methode bildet die Startregel `L_int ::= eform EOF` der konkreten Grammatik ab.

Die einzelnen Regeln der (von direkter Linksrekursion befreiten) konkreten Grammatik bilden wir auf die Arbeitsmethoden `parseEform()`, `parseAtom()`, `parseAddExpr()`, `parseSubExpr()` ab.

Intern werden wir Hilfsmethoden wie `peek()`, `advance()` und `match()` verwenden, um die Tokens bequemer zu handhaben.

##### Partielle Beispielimplementierung RD-Parser für L-Int

Wir betrachten folgende konkrete Syntax in L-Int:

``` ebnf
eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
INT   ::= [0-9]+

L_int ::= eform EOF
```

Der Einstieg ist die Regel `L_int`, die eine `eform` erwartet und ein anschließendes `EOF`-Token. Mehr ist in der Sprachstufe L-Int nicht erlaubt und wurde bereits in der Einstiegsmethode `parse()` umgesetzt.

Eine `eform` kann entweder eine Addition, eine Subtraktion oder einfach nur ein Integer-Literal sein. Letzteres ist ein Terminal, d.h. hier können wir einfach mit `match()` das entsprechende Token abfragen.

Die Alternativen für Addition und Subtraktion rufen jeweils die Regel für `eform` rekursiv auf.

Der Parser muss entlang dieser Regeln aufgebaut werden und die Token der Reihe nach über diese Regeln ableiten ("erklären"). Nur wenn dies vollständig gelingt, haben wir eine syntaktisch korrekte Eingabe. Ein Parser kann einen Parse-Tree oder - wie in unserem Beispiel - direkt einen AST erzeugen. Der gezeigte Code erzeugt den AST gemäß der folgenden abstrakten Syntax:

``` ebnf
Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
```

Wir haben hier also konkret eine Daten-Klasse `IntLiteral` für die Repräsentation von Integer-Literalen, die jeweils die gefundene Zahl speichern.

Analog für Addition und Subtraktion. Hier ist interessant, dass die vorgesehenen Knoten im AST nur binäre Knoten sind, in der konkreten Syntax aber jeweils zwei oder mehr Argumente für Addition und Subtraktion erlaubt sind. Ein reiner Parse-Tree würde diese Grammatik-Struktur im Baum widerspiegeln (siehe Challenge). Wir wollen hier einen abstrakteren Baum (AST) erzeugen und müssen entsprechend bereits beim Erzeugen des AST die Zielstruktur berücksichtigen.

``` java
protected Expr parseEform() {
    if (check(LPAREN)) {
        return switch (peek(1).kind()) {
            case PLUS -> parseAddExpr();
            default -> throw new RuntimeException("unexpected token");
        };
    }

    return parseAtom();
}


protected Expr parseAtom() {
    Token t = match(INT);
    return new IntLiteral(Integer.parseInt(t.lexeme()));
}
```

Für die Regel `eform` wird eine `parseEform()` angelegt, die direkt der Grammatikregel entspricht.

> [!TIP]
>
> Die Sichtbarkeit ist `public` für die Einstiegsmethode `parse()` - diese Methode soll später zum Starten des Parsers genutzt werden. Die anderen (internen) Methoden bekommen die Sichtbarkeit `protected`, damit wir sie bei Bedarf in ableitenden Parser-Klassen überschreiben können. Wenn wir uns ganz sicher sind, dass wir eine interne Methode nie überschreiben wollen, können wir natürlich auch wie immer auf `private` gehen.

Wir unterscheiden in `parseEform()` anhand des aktuellen Tokens, ob es sich um die Alternativen für Addition (und Subtraktion, nicht gezeigt) handeln könnte (beide starten mit `'('`). Ansonsten muss es sich um die letzte Alternative `INT` handeln:

-   Liegt ein `LPAREN` vor, entfällt die letzte Alternative. Es kann sich nur noch um eine Addition oder Subtraktion handeln.

    Da diese Alternativen beide mit `'('` beginnen, schauen wir nun auf das nächste Token:

    -   Finden wir ein `ADD`- oder `SUB`-Token, rufen wir entsprechend die Regeln `parseAddExpr()` bzw. `parseSubExpr()` (nicht gezeigt) auf.
    -   Anderenfalls melden wir einen Fehler, weil wir `LPAREN` gesehen haben, aber nichts, was laut unserer Grammatik darauf folgen müsste.

-   Liegt *kein* `LPAREN` vor, kann es sich nur um die letzte Alternative `INT` handeln.

    Hier handelt es sich um ein Terminal-Symbol, welches wir in `parseAtom()` mit `match(INT)` überprüfen und gleichzeitig aus dem Tokenstrom "entfernen". Wenn das aktuelle Token dabei nicht `INT` ist, würde `match(INT)` einen Fehler auslösen.

**Wichtig**: Die Subtraktion fehlt hier und dient als kleine Übung.

**Wichtig**: Wir haben damit in `parseEform()` effektiv ***zwei* Tokens Lookahead** verwendet! Vgl. auch die nachfolgende Diskussion zur Linksfaktorisierung.

Die Methode `parseAtom()` entspricht genau dem weiter oben gezeigten Muster. Wir erwarten ein `INT`-Token, also rufen wir die Hilfsmethode `match()` mit diesem Parameter auf. `match()` prüft, ob das aktuelle Token wirklich diesen Typ hat und "entfernt" es aus dem Tokenstrom (inkrementiert den `current`-Index) und liefert es zurück. Anderenfalls würde `match()` eine entsprechende Exception werfen.

Das Lexem nutzen wir und konvertieren es in einen Java-Integer und erzeugen einen neuen AST-Knoten vom Typ `IntLiteral` - da dieser Typ vom Interface `Expr` ableitet, haben wir hier also den einfachsten möglichen AST gebildet.

> [!TIP]
>
> ###### Exkurs Linksfaktorisierung
>
> Wir haben in `parseEform()` *implizit* **Linksfaktorisierung** angewendet: Wenn mehrere Alternativen einer Regel den gleichen Anfang haben, kann man diesen "heraus klammern":
>
> ``` ebnf
> ruleA ::= a b1 | ... | a bm
> ```
>
> Hier könnte man den gemeinsamen Start `a` "ausklammern":
>
> ``` ebnf
> ruleA  ::= a ruleAA
> ruleAA ::= b1 | ... | bm
> ```
>
> Bei uns im Beispiel war der gemeinsame Start für mehrere Alternativen die Klammer `'('`. Statt die Grammatik umzubauen, hatten wir das über das `if` und das nachfolgende extra Lookahead `peek(1)` gelöst. Mit der Umformung hätten wir eine weitere Hilfsfunktion im Parser, würden dann aber wieder mit `peek()` bzw. `peek(0)` und damit einem Token Lookahead auskommen (statt jetzt zwei).

##### Details Parser für L-Int: `parseAddExpr()`

``` java
protected Expr parseAddExpr() {
    match(LPAREN);
    match(PLUS);

    List<Expr> ops = new ArrayList<>();
    ops.add(parseEform());
    while (!check(RPAREN)) ops.add(parseEform());

    match(RPAREN);

    return ops.stream().reduce(AddExpr::new).orElseThrow();
}
```

Auch hier sehen wir wieder das eingangs skizzierte Muster: Für jedes Terminal wird `match()` mit dem gesuchten Token-Typ aufgerufen. Danach müssen wir die Operanden parsen: Solange nicht ein `')'`-Token auftaucht, rufen wir rekursiv die Regel `parseEform()` auf. Jeder Operand muss ja selbst eine `eform` sein. Anschließend matchen wir noch die schließende rechte Klammer, und die Addition ist im Prinzip fertig geparst.

Nun müssen wir aber noch den Baum mit binären `AddExpr`-Knoten bauen. Dazu laufen wir über die Liste der geparsten Operanden und reduzieren den Stream. Dabei würde `(+ 1 2 3)` von links nach rechts geklammert: `(+ (+ 1 2) 3)`.

**Wichtig**: Challenge (s.u.) beachten!

> [!TIP]
>
> ###### Exkurs Desugaring
>
> Diese Auflösung ist ein einfaches Beispiel für ***Desugaring*** - hier wird aus einer komplexeren Operation mit potentiell beliebig vielen Operanden eine Folge von einfacheren binären Operationen gemacht. Wir werden noch komplexere Beispiele sehen, wenn wir in L-If über Kontrollstrukturen sprechen.

##### Hilfsfunktionen im Parser: Tokenhandling

``` java
protected boolean isAtEnd() { return peek().kind() == EOF; }

protected Token peek() { return peek(0); }
protected Token peek(int la) {
    if (la < 0 || current + la >= tokens.size()) return tokens.getLast();  // EOF
    return tokens.get(current + la);
}

protected Token advance() {
    return isAtEnd() ? tokens.getLast() : tokens.get(current++);
}

protected boolean check(TokenKind... expected) {
    return Arrays.stream(expected).anyMatch(e -> e == peek().kind());
}
protected Token match(TokenKind... expected) {
    if (check(expected)) return advance();
    throw new RuntimeException("unexpected token: " + peek());
}
```

Man definiert sich gern eine Handvoll von Hilfsfunktionen im Parser, die das Handling mit den Token erleichtern. Im Prinzip können diese Funktionen relativ symmetrisch zu den Pendants im Lexer implementiert werden - allerdings arbeitet man hier im Parser mit Token statt mit Zeichen.

Der auffälligste Unterschied zum Lexer: Wir verlassen uns nicht auf die Länge der Tokenliste, sondern behandeln das `EOF`-Token als Sentinel. Sobald wir dieses Token erreicht haben, brechen wir den weiteren Fortschritt mit `advance()` o.ä. ab.

Zusätzlich zu `isAtEnd()`, `peek()` und `advance()` haben wir noch zwei weitere Hilfsmethoden, die `peek()` und `advance()` unterschiedlich kombinieren. Wir werden in Zukunft häufig wissen wollen, ob das aktuelle Token in einer Menge von erlaubten Token vorkommt, weshalb `check()` und `match()` mit variadischen Parametern realisiert sind: Hier kann man einfach ein oder mehrere Tokentypen übergeben, ohne die Funktionen umständlich überladen zu müssen oder vorher Listen bilden zu müssen. `check()` prüft dabei nur, ob das aktuelle Token vom Typ her in der `expected` ist und liefert einen Boolean zurück, während `match()` im Fall des Zutreffens das aktuelle Token "konsumiert" und anderenfalls einen Fehler wirft.

##### Auflösen von (links-) rekursiven Grammatiken

Wir wollen die folgenden Ausdrucksvarianten erlauben:

`1`, `1 + 2`, `1 + 2 + 3`, ...

Nun definieren wir eine einfache Grammatik, die diese Ausdrücke akzeptiert:

``` ebnf
expr ::= expr '+' expr | INT
```

Diese intuitiv erscheinende Grammatik erlaubt zwar die gewünschten Ausdrücke, hat aber zwei Probleme!

###### Problem 1: Mehrdeutigkeit

**Diese Grammatik ist mehrdeutig!**

`1 + 2 + 3` kann mit der Grammatik sowohl als `(1 + 2) + 3` (*linksassoziativ*) als auch als `1 + (2 + 3)` (*rechtsassoziativ*) geparst werden. So etwas wollen wir im Compiler generell vermeiden, auch wenn das hier konkret keine Probleme verursachen würde.

Die folgende umgeformte Grammatik akzeptiert die gleiche Sprache und ist nicht mehr mehrdeutig:

``` ebnf
expr ::= expr '+' INT | INT
```

Hier erzwingen wir die übliche **Klammerung von links** her (**linksassoziativ**): `1 + 2 + 3` kann nur noch als `(1 + 2) + 3` geparst werden.

###### Problem 2: Linksrekursion

Die umgeformte Grammatik ist immer noch **linksrekursiv**: Zu Beginn einer Regel rufen wir die Regel selbst wieder auf und gelangen so in eine Endlosrekursion, da kein Token konsumiert wird.

Das folgende Beispiel versucht die Umsetzung der Grammatik mit Hilfe von Backtracking (teste die erste Alternative, wenn das nicht klappt - einen Fehler wirft - versuche die zweite Alternative). `position()` soll den aktuellen Index holen bzw. wieder zurücksetzen. Dieses Beispiel soll nur das Problem bzgl. der Endlosrekursion verdeutlichen - es funktioniert nicht!

``` java
Expr parseExpr() {
    int start = position();

    try {
        // first alternative: expr ::= expr '+' INT
        Expr left = parseExpr();  // Waily waily!!!
        match(PLUS);
        Expr right = parseInt();
        return new AddExpr(left, right);

    } catch (ParseException e) {
        // second alternative: expr ::= INT
        position(start);  // roll back token stream
        return parseInt();
    }
}

Expr parseInt() {
    Token t = match(INT);
    return new IntLiteral(t.lexeme());
}
```

Man kann zwischen **direkter** und **indirekter** Linksrekursion unterscheiden:

-   Wenn die Regel sofort beim Start einer Alternative aufgerufen wird, handelt es sich um direkte Linksrekursion (wie im Beispiel).
-   Indirekte Linksrekursion hätte man, wenn das erste Symbol einer Regel eine andere Regel ist, die wiederum die erste Regel aufruft: `ruleA ::= ruleB ...` und `ruleB ::= ruleA ...`.

Wenn man eine *direkt* linksrekursive Regel der Form hat:

``` ebnf
ruleA ::= ruleA a1 | ... | ruleA am | b1 | ... | bn |
```

(Dabei dürfen die `bi` nicht mit `ruleA` beginnen.)

Dann kann *direkte* Linksrekursion mit folgender Regel behoben werden:

``` ebnf
ruleA  ::= b1 ruleAA | ... | bn ruleAA
ruleAA ::= a1 ruleAA | ... | am ruleAA | eps
```

(Mit `eps` als das leere Wort $\epsilon$.)

Damit können wir unsere Grammatik umschreiben:

``` ebnf
expr  ::= INT expr'
expr' ::= '+' INT expr' | eps
```

Dies können wir hier noch weiter abkürzen zu:

``` ebnf
expr  ::= INT ('+' INT)*
```

Mit dieser umgeformten Regel können wir jetzt unseren RD-Parser wie gewohnt aufbauen. Es gibt keine direkte Linksrekursion mehr, mit der der Parser in eine Endlosschleife geraten könnte.

``` java
Expr parseExpr() {
    Expr e = parseInt();
    while (check(PLUS)) {
        advance();
        e = new AddExpr(e, parseInt());
    }
    return e;
}

Expr parseInt() {
    Token t = match(INT);
    return new IntLiteral(t.lexeme());
}
```

*Anmerkung*: Normalerweise würden wir für ein Nichtterminal direkt `match()` aufrufen - damit der Code lesbarer bleibt, wurde das `match(INT)` und das Erzeugen des neuen `IntLiteral` in eine Hilfsmethode ausgelagert.

##### Lispy-Pipeline komplett

``` java
String source = "(- (+ 1 2) 3)";

List<Token> tokens = new LispyLexerLint().scan(source);
Expr ast = new LispyParserLint().parse(tokens);

Value result = new InterpreterLint().eval(ast);
// result: IntVal(0)
```

##### Wrap-up

Implementierung als RD-Parser:

-   Regeln (Nichtterminale) werden zu Parsermethoden
-   Token (Terminals) mit `match()` prüfen (und "konsumieren")
-   Auswahl Alternativen über nächste(s) Token(s) (Lookahead)
-   Beachtung und Auflösung von Linksrekursion

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema Lexing findet sich eine schöne Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 6 "Parsing Expressions".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den prinzipiellen Aufbau von *recursive-descent* LL-Parsern am Beispiel erklären
> -   k3: Ich kann LL(1)- und LL(k)-Parser implementieren
> -   k3: Ich kann mit Linksrekursion umgehen und diese ggf. auflösen
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Quizzfragen**:
>
> -   Wie kann man aus einer LL(1)-Grammatik einen LL(1)-Parser mit rekursivem Abstieg implementieren? Wie "übersetzt" man dabei Token und Regeln?
> -   Wie geht man mit Alternativen um? Wie mit optionalen Subregeln?
> -   Warum ist Linksrekursion i.A. bei LL-Parsern nicht erlaubt? Wie kann man Linksrekursion beseitigen?
> -   Wann braucht man mehr als ein Token Lookahead? Geben Sie ein Beispiel an.
>
> **Optionale Subregeln**
>
> Wie werden optionale Subregeln in RD-Parsern implementiert?
>
> -   "Eins oder keins":
>
>     ``` ebnf
>         (T)?
>     ```
>
> -   "Mindestens eins"
>
>     ``` ebnf
>         (T)+
>     ```
>
> **Desugaring**
>
> Bei der Parserregel `parseAddExpr()` haben wir folgenden Code gesehen:
>
> ``` java
> protected Expr parseAddExpr() {
>     match(LPAREN);
>     match(PLUS);
>
>     List<Expr> ops = new ArrayList<>();
>     ops.add(parseEform());
>     while (!check(RPAREN)) ops.add(parseEform());
>
>     match(RPAREN);
>
>     return ops.stream().reduce(AddExpr::new).orElseThrow();
> }
> ```
>
> 1.  Was passiert in den Randfällen?
>
>     -   Ist `(+ 1)` erlaubt? Falls ja, was produziert der Parser?
>     -   Ist `(+)` erlaubt? Falls ja, was produziert der Parser?
>
> 2.  Wird damit der Teil der Regel `eform ::= '(' '+' eform eform+ ')'` korrekt umgesetzt? Warum (nicht)? Falls "nein", wie müsste der Code korrigiert werden?
>
> **Parser-Ablauf nachvollziehen**
>
> Verfolgen Sie den Aufruf von `parseEform()` für die Eingabe:
>
> ``` clojure
> (+ 1 (+ 2 3))
> ```
>
> Erstellen Sie dazu eine Tabelle/Liste mit diesen Spalten: Schritt, Aufgerufene Methode, Aktuelles Token, Aktion. Tragen Sie insbesondere ein, wann `parseEform()` rekursiv aufgerufen wird und welche Methode die einzelnen Tokens konsumiert. Sie müssen noch nicht den vollständigen AST ausrechnen.
>
> **Subtraktion ergänzen**
>
> Ergänzen Sie den im Skript gezeigten Parser so, dass auch Subtraktionen geparst werden. Skizzieren Sie die nötigen Änderungen in `parseEform()` und eine passende Methode `parseSubExpr()`.
>
> Prüfen Sie Ihren Entwurf gedanklich an diesen Eingaben:
>
> ``` clojure
> (- 8 3)
> (- 8 (+ 2 1) 1)
> ```
>
> Welche AST-Knoten sollten jeweils entstehen?
>
> **Den gemeinsamen Anfang ausklammern**
>
> Die Alternativen für Addition und Subtraktion beginnen beide mit `'('`. Formulieren Sie die Grammatik mit einer zusätzlichen Regel für den Operator so um, dass die Auswahl zwischen den äußeren Alternativen von `eform` nur **ein** Lookahead-Token benötigt.
>
> Skizzieren Sie anschließend, wie die Parsermethoden zu dieser umgeformten Grammatik aussehen könnten. Die akzeptierte Sprache und die AST-Knoten sollen gleich bleiben.
>
> </details>

<a id="id-ccf6f8b1f448baca9328a5b307f43c54f46003d2"></a>

### Sprache L-Expr: erweiterte Ausdrücke, Vorrangregeln

##### Inhalte

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   ANTLR: Vorrang mit gestuften Regeln vs. ANTLR4-Precedence
-   RD+Pratt
-   InterpreterLExpr

<a id="id-087c4c3ae00f192d7609dfe6432bd8d23512b621"></a>

### Sprache L-Var: Variablen, Statments, nested Scopes

##### Inhalt

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   Nested Scopes, Resolving
-   ResolverLVar, InterpreterLVar

<a id="id-0e638f12c1da2fa388a172d5149c9094d3f2971a"></a>

### Sprache L-If: Datentyp Boolean, Vergleiche, Kontrollstrukturen (if/else, while)

##### Inhalte

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   ResolverLIf, TypeCheckerLIf, InterpreterLIf
-   Nano-Pass mit Desugaring: for, +=, ... (??)

<a id="id-7ab159a2c5205e0fa6db8afc82702716ee38a379"></a>

### Sprache L-Fun: Funktionen (Definition, Aufruf)

##### Inhalte

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   ResolverLFun, TypeCheckerLFun, InterpreterLFun
-   native Funktionen vs. geparste Funktionen

<a id="id-9c944ada9855c942abb3b2e7edfee3f4884c40a3"></a>

### Sprache L-Class: Klassen (Felder, Methoden, Objekte)

##### Inhalte

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   Klassen als Environment für Methoden, Instanzen als Environment für Felder, Auflöse-Strategien
-   ResolverLClass, TypeCheckerLClass, InterpreterLClass

<a id="id-0ee25f396ce63919373510c93deb2a71b0aaa103"></a>

### Sprache L-Inherit: Einfachvererbung und dynamischer Dispatch

##### Inhalte

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   ResolverLInherit, TypeCheckerLInherit, InterpreterLInherit

<a id="id-339b976d7f4c691df2844b84c284f70d7c8e06cc"></a>

### Sprache L-Self: native Klassen (inkl. Vererbung)

##### Inhalte

-   Einbinden von nativen Klassen
-   InterpreterLSelf, TypeCheckerLSelf/ResolverLSelf
-   Vorbereitung für Snake

<a id="id-a264d337dcfeece8936f208b6f89bb1efe99ea0f"></a>

## Praktikum

Hier finden Sie die Übungsblätter.

<a id="id-6f673c2e093cdfc53b1f78baef11fd06cc8aa415"></a>

### Blatt 01: Reguläre Sprachen

#### A1.1: Sprachen von regulären Ausdrücken

Welche Sprache wird von dem folgenden regulären Ausdruck beschrieben?

$a\ +\ a\ (a\ +\ b)^*\ a$

#### A1.2: Bezeichner in Programmiersprachen

Die Referenz einer Programmiersprache beschreibt zugelassene Bezeichner (= Namen für Variablen, Funktionen, Klassen, Methoden, ...) folgendendermaßen:

-   Alle Variablennamen beginnen mit **V** oder **v**
-   Handelt es sich um globale Variablen, beginnen Sie mit **V**, lokale beginnen mit **v**
-   Funktions- und Methodenparameter beginnen mit **p**, KLassenparameter (bei der Definition von Vererbung) beginnen mit **P**
-   Weitere Bezeichner müssen mit einem Buchstaben (a-z, A-Z) beginnen
-   Die folgenden Zeichen dürfen Buchstaben, Ziffern und ein Unterstrich sein
-   Bezeichner dürfen nicht mit einem Unterstrich enden
-   Alle Bezeichner müssen aus mindestens zwei Zeichen bestehen

Entwickeln Sie **einen** regulären Ausdruck, der den Aufbau aller Bezeichner beschreibt.

Beachten Sie, dass Ihr regex alle zulässigen Bezeichner beschreiben muss, aber keinen einzigen unzulässigen beschreiben darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie, wie sie vom regex gematcht werden.

Entwickeln Sie einen DFA, der diese Bezeichner akzeptiert. Beachten Sie, dass Ihr DFA alle zulässigen Bezeichner akzeptieren muss, aber keinen einzigen unzulässigen akzeptieren darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie, wie sie vom Automaten zeichenweise gelesen und akzeptiert werden.

Bitte beachten Sie, dass die Schreibweise a-z nicht unserer Definition von DFAs genügt. Eigentlich müsste jedes Zeichen aufgeführt werden.

Anmerkung: Diese Darstellung wird ab jetzt akzeptiert.

Entwickeln Sie eine reguläre Grammatik, die diese Bezeichner generiert. Beachten Sie, dass Ihre Grammatik alle zulässigen Bezeichner generieren können muss, aber keinen einzigen unzulässigen generieren darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie die Ableitungsbäume dazu.

#### A1.3: Gleitkommazahlen in Programmiersprachen

Recherchieren Sie zunächst den Aufbau von Gleitkommazahlen in Python und Java.

Erstellen Sie für jede der beiden Programmiersprachen reguläre Ausdrücke, DFAs und reguläre Grammatiken wie in Aufgabe A1.2. Verifizieren Sie Ihre Lösungen wie in Aufgabe A1.2. Vorgaben, die sich auf Längen oder Werte von Teilen der Zahlen beziehen, ignorieren Sie bitte.

#### A1.4: Mailadressen?

Warum ist der folgende regex ungeeignet für die Verarbeitung von Mailadressen?

$(a-z)^+@(a-z).(a-z)$

Verbessern Sie den gegebenen regulären Ausdruck.

#### A1.5: Der zweitletzte Buchstabe

Entwickeln Sie einen DFA, der nur Wörter über $\Sigma = \lbrace 1,2 \rbrace$ akzeptiert, deren zweites und zweitletztes Zeichen eine **1** ist.

#### A1.6: Sprache einer regulären Grammatik

Welche Sprache generiert die folgende Grammatik?

$$\begin{eqnarray}
S &\rightarrow& a A \ | \ b B            \nonumber \\
A &\rightarrow& b S \ | \ c              \nonumber \\
B &\rightarrow& a S \ | \ b              \nonumber \\
\end{eqnarray}$$

Können Sie einen regulären Ausdruck oder einen DFA dafür angeben?

<a id="id-0db349230022c35e045dc3b052a4faea50fe5f40"></a>

### Blatt 02: CFG

#### A2.1: PDA (3P)

Erstellen Sie einen deterministischen PDA, der die Sprache

$$L = \lbrace w \in \lbrace a, b, c \rbrace^* \; | \; w \; \text{hat doppelt so viele a's wie c's} \rbrace$$

akzeptiert.

Beschreiben Sie Schritt für Schritt, wie der PDA die Eingaben *bcaba* und *bccac* abarbeitet.

#### A2.2: Akzeptierte Sprache (2P)

Ist der folgenden PDA deterministisch? Warum (nicht)?

$q_4$ sei der akzeptierende Zustand.

$$\begin{eqnarray}
\delta(q_0,a, \perp) &=& (q_0, A\perp)           \nonumber \\
\delta(q_0,a, A) &=& (q_0, AA)                   \nonumber \\
\delta(q_0,b, A) &=& (q_1, BA)                   \nonumber \\
\delta(q_1,b, B) &=& (q_1, BB)                   \nonumber \\
\delta(q_1,c, B) &=& (q_2, \epsilon)             \nonumber \\
\delta(q_2,c, B) &=& (q_2, \epsilon)             \nonumber \\
\delta(q_2,d, A) &=& (q_3, \epsilon)             \nonumber \\
\delta(q_3,d, A) &=& (q_3, \epsilon)             \nonumber \\
\delta(q_3,d, A) &=& (q_3, AA)                   \nonumber \\
\delta(q_3,\epsilon, \perp) &=& (q_4, \epsilon)  \nonumber
\end{eqnarray}$$

Zeichnen Sie den Automaten. Geben Sie das 7-Tupel des PDa an. Welche Sprache akzeptiert er?

#### A2.3: Kontextfreie Sprache (2P)

Welche Sprache generiert die folgende kontextfreie (Teil-) Grammatik?

$$G = (\lbrace \text{Statement}, \text{Condition}, \ldots \rbrace, \lbrace \text{"if"}, \text{"else"}, \ldots \rbrace, P, \text{Statement})$$

mit

$$\begin{eqnarray}
P = \lbrace &&                                                                                                           \nonumber \\
&\text{Statement}& \rightarrow \text{"if" Condition Statement} \; | \; \text{"if" Condition Statement "else" Statement}  \nonumber \\
&\text{Condition}& \rightarrow \ldots                                                                                    \nonumber \\
\rbrace                                                                                                                  \nonumber
\end{eqnarray}$$

Ist die Grammatik mehrdeutig? Warum (nicht)?

#### A2.4: Kontextfreie Grammatik (3P)

Entwickeln Sie eine kontextfreie Grammatik für die Sprache

$$L = \lbrace a^ib^jc^k \; | \; i = j \lor j = k \rbrace$$

Zeigen Sie, dass die Grammatik mehrdeutig ist. Entwickeln Sie einen PDA für diese Sprache.

<a id="id-c6885788e4bb28c92d66417730e18408254766a3"></a>

### Blatt 03: C-Int

> [!IMPORTANT]
>
> Für die erfolgreiche Bearbeitung dieses Übungsblattes müssen mindestens alle mit `*` und `**` markierten Aufgaben gelöst werden.
>
> Die Aufgaben mit `***` sind als Vertiefung gedacht.

#### (\*) Java-Projekt für L-Int einrichten

Legen Sie ein Java-Projekt für **L-Int** an. Nutzen Sie dafür Maven oder Gradle. Das Projekt wird im Laufe des Semesters Schritt für Schritt erweitert.

Falls gewünscht, können Sie eines der folgenden Templates als Ausgangspunkt nutzen:

-   Gradle: https://github.com/Compiler-CampusMinden/student-support-code-template
-   Maven: https://github.com/Compiler-CampusMinden/student-support-code-template-maven

Rekonstruieren Sie die in der Vorlesung gezeigten Bausteine für L-Int in Ihrem Projekt (Pakete, Klassen, Interfaces) und übernehmen Sie die gezeigte Funktionalität so, dass Ihr Projekt kompilierbar ist. Fehlende Funktionalität ergänzen Sie in den folgenden Aufgaben.

#### (\*\*) L-Int Lexer: Funktion `skipIrrelevant()`

In der Vorlesung wurde die Funktion `skipIrrelevant()` eingeführt, mit der nicht benötigte Zeichen wie Whitespace und Kommentare übersprungen werden. Sie wird immer zu Beginn von `nextToken()` aufgerufen:

``` java
protected boolean skipIrrelevant() {}  // Homework
```

1.  Wie könnte diese Methode aussehen? Geben Sie eine Implementierung in Java an.
2.  Sammeln Sie in der Übung gemeinsam sinnvolle Testfälle und notieren Sie diese für später (s.u.).

#### (\*) Subtraktion für L-Int ergänzen

L-Int unterstützt Integer-Literale, Addition und Subtraktion. In der Vorlesung wurde die Implementierung nur für Integer und Addition im Detail gezeigt.

1.  Sprache verstehen

    Klärung in der Übung (gemeinsam):

    -   Welche Eingaben sind **gültige** L-Int-Ausdrücke?

    -   Wie werden mehrere Operanden einer Addition oder Subtraktion gruppiert?

    -   Welche Werte ergeben die folgenden Ausdrücke?

        ``` clojure
        (+ 1 2)
        (- 8 3)
        (- 20 5 3)
        (- 10 (+ 3 5))
        ```

2.  Ungültige Eingaben

    Notieren Sie mindestens zwei **ungültige** L-Int-Eingaben. Mindestens eine davon soll eine Operation mit weniger als zwei Operanden sein.

3.  Pipeline um Subtraktion erweitern

    Ergänzen Sie Ihre L-Int-Pipeline vollständig um die Subtraktion (Lexer, RD-Parser, Interpreter).

#### (\*\*) Tests für L-Int

Schreiben Sie JUnit-Tests für Ihre L-Int-Implementierung. Testen Sie mindestens:

-   ein Integer-Literal,
-   eine (zweistellige) Addition,
-   eine (zweistellige) Subtraktion,
-   einen geschachtelten Ausdruck,
-   eine mehrstellige Addition mit Linksassoziativität,
-   eine mehrstellige Subtraktion mit Linksassoziativität,
-   mindestens eine ungültige Eingabe.

Ein weiterer Test soll aufdecken, ob bei der Subtraktion versehentlich linker und rechter Operand vertauscht wurden. Testen Sie daher beispielsweise sowohl `(- 7 2)` als auch `(- 2 7)`.

Testen Sie nach Möglichkeit nicht nur die Auswertung, sondern auch die erzeugten Token und ASTs. Dadurch können Sie Fehler in den einzelnen Phasen der Pipeline besser voneinander unterscheiden.

#### (\*) C++ ausprobieren: Compiler und ein erstes Programm

Wir werden in dieser Veranstaltung Schritt für Schritt C++ kennenlernen und einen Interpreter in Java für einen kleinen C++-Ausschnitt entwickeln.

In dieser Aufgabe geht es zunächst darum,

-   einen C++-Compiler einzurichten,
-   ein kleines Programm zu schreiben,
-   dieses mit clang++ oder g++ zu übersetzen und auszuführen.

##### Schritt 1: Compiler überprüfen und installieren

Prüfen Sie in einem Terminal, ob ein C++-Compiler verfügbar ist:

    clang++ --version
    g++ --version

Falls kein Compiler gefunden wird:

-   Unter **macOS**: Installation der Xcode Command Line Tools.
-   Unter **Linux**: Installation von clang++ oder g++ über die Paketverwaltung.
-   Unter **Windows**: z.B. WSL mit Ubuntu und dort clang++/g++ oder eine andere C++-Toolchain.

> [!TIP]
>
> Für C++ verwenden wir **clang++** beziehungsweise **g++**, nicht clang beziehungsweise gcc. Umgangssprachlich meint "gcc" in diesem Kontext "g++", und "clang" meint tatsächlich "clang++". Bei der Installation von clang/gcc sind i.d.R. beide Varianten (C und C++) dabei.

##### Schritt 2: Erstes Programm übersetzen und ausführen

Speichern Sie das folgende Programm als `hello.cpp`:

``` cpp
#include <iostream>

int main() {
    int result = ((1 + 2) - 3);
    std::cout << "hello world: ";
    std::cout << result << '\n';
    return 0;
}
```

Übersetzen und starten Sie das Programm mit clang++:

    clang++ hello.cpp -o hello
    ./hello

Oder mit g++:

    g++ hello.cpp -o hello
    ./hello

Unter Windows heißt die ausführbare Datei meist `hello.exe`, Start in PowerShell:

    .\hello.exe

Variieren Sie den arithmetischen Ausdruck in `result` und überprüfen Sie die Ausgabe für mindestens drei Beispiele.

> [!TIP]
>
> ##### Mini-Waschzettel: `hello.cpp` im Vergleich zu L-Int / C-Int / Java
>
> Zum Verständnis von `hello.cpp` benötigen Sie hier nur wenige Konstrukte:
>
> | Zweck | C++ | Java | L-Int | C-Int |
> |:----------------|:-------------------|:-------------|:----------|:----------|
> | Integer-Variable anlegen | `int result = 42;` | `int result = 42;` | n.a. | n.a. |
> | Addition | `1 + 2` | `1 + 2` | `(+ 1 2)` | `1 + 2` |
> | Subtraktion | `7 - 2` | `7 - 2` | `(- 7 2)` | `7 - 2` |
> | Klammerung | `(1 + 2)` | `(1 + 2)` | `(+ 1 2)` | `(1 + 2)` |
> | Ausgabe einer Variablen | `std::cout << result << '\n';` | `IO.println(result)` | n.a. | n.a. |
> | Zeilenkommentar | `// Kommentar` | `// Kommentar` | `;; Kommentar` | `// Kommentar` |
> | Import von Bibliotheken | `#include <iostream>` | `import ...;` | n.a. | n.a. |

#### (\*\*) C-Int: Addition und Subtraktion für Integer in C++

Wir legen nun mit **C-Int** die Basis für den Mini-C++-Interpreter, den wir über das Semester hinweg erweitern.

C-Int soll analog zu L-Int **Ausdrücke mit Integer-Literalen, Addition und Subtraktion** erlauben, aber in C++-üblicher Infix-Notation.

> [!IMPORTANT]
>
> ##### Waschzettel: In C++ sieht es so aus ...
>
> | Sprachelement   | L-Int                    | C-Int                    |
> |:----------------|:-------------------------|:-------------------------|
> | Integer-Literal | `42`                     | `42`                     |
> | Addition        | `(+ 1 2)`, `(+ 1 2 3 4)` | `1 + 2`, `1 + 2 + 3 + 4` |
> | Subtraktion     | `(- 7 2)`, `(- 7 2 1)`   | `7 - 2`, `7 - 2 - 1`     |
> | Klammerung      | `(+ 1 2)`                | `(1 + 2)`                |
> | Zeilenkommentar | `;; Kommentar`           | `// Kommentar`           |
>
> Beispiele (mit Ergebnis im Kommentar):
>
>     2          // => 2
>
>     1+2        // => 3
>     (1+2)      // => 3
>
>     7-2        // => 5
>     7-2-1      // => 4
>
>     1+2+3      // => 6
>     (1+2)+3    // => 6
>     ((1+2)+3)  // => 6
>
> Ein L-Int-Ausdruck wie (- (+ 1 2) 3) entspricht in C-Int etwa:
>
>     ((1 + 2) - 3)
>     (1 + 2) - 3
>     1 + 2 - 3
>
> Addition und Subtraktion sind *linksassoziativ*, d.h. es wird von links nach rechts geklammert (sofern keine expliziten Klammern vorhanden sind).
>
> **Wichtig**: Für die Übersetzung mit einem C++-Compiler müssen diese Ausdrücke noch in einen Rahmen eingefügt werden, beispielsweise in eine `main()`-Funktion und in passende Statements (Zuweisung, Ausgabe, ...).
>
> Syntaktisch und semantisch betrachten wir im Laufe des Semesters folgende Abstufung:
>
>     C++ <: C-Self <: C-Inherit <: C-Class <: C-Fun <: C-If <: C-Expr <: C-Int
>
> In diesem Semester enden wir (voraussichtlich) bei C-Self.

##### Schritt 1: Java-Projekt für C-Int einrichten

Legen Sie ein neues Java-Projekt für Ihren C-Int-Interpreter an (analog zur ersten Aufgabe für L-Int). Nutzen Sie ein eigenes Package, z.B. `cint`.

##### Schritt 2: Konkrete Syntax für C-Int

Definieren Sie analog zur Vorlesung die **konkrete Syntax für C-Int**. Sie legen damit fest, welche Programme von Ihrem Lexer und Parser akzeptiert werden.

Rahmenbedingungen:

-   Whitespace darf zwischen den Bestandteilen eines Ausdrucks stehen und wird übersprungen.
-   Zeilenkommentare `// ...` bis zum Zeilenende werden unterstützt und übersprungen.
-   Addition und Subtraktion haben immer zwei Integer-Operanden: `1-2`.
-   Mehrfach verkettete Ausdrücke wie `1+2+3+4` sind erlaubt.
-   Ausdrücke dürfen geklammert sein.
-   Nicht geklammerte Ausdrücke werden implizit **von links nach rechts** gruppiert.

Diskutieren Sie in der Übung:

-   Welche konkreten Eingaben soll die Grammatik zulassen, welche nicht?
-   Welche Teile gehören zur **konkreten** Syntax (Token, Schlüsselwörter, Symbole)?

##### Schritt 3: Abstrakte Syntax für C-Int

Definieren Sie analog zur Vorlesung die **abstrakte Syntax** (AST) für **C-Int**.

Diskussionspunkte:

-   Welche Informationen aus dem Parse-Tree müssen im AST erhalten bleiben?
-   Warum müssen Klammern im AST nicht als eigene Knoten vorkommen?

##### Schritt 4: Interpreter-Pipeline für C-Int

Erstellen Sie analog zur L-Int-Pipeline für C-Int:

-   eine Tokendefinition,
-   AST-Klassen und -Interfaces,
-   einen Lexer,
-   einen RD-Parser,
-   einen Interpreter.

Verwenden Sie hierfür **keinen** generierten ANTLR-Parser, sondern eine handgeschriebene Java-Implementierung wie in der Vorlesung.

##### Schritt 5: Tests für C-Int

Schreiben Sie JUnit-Tests für Ihre C-Int-Pipeline. Testen Sie mindestens:

-   ein Integer-Literal,
-   eine Addition,
-   eine Subtraktion,
-   einen geschachtelten Ausdruck,
-   Whitespace an unterschiedlichen Stellen,
-   ein zusätzliches Token nach einem vollständigen Ausdruck,
-   eine fehlende schließende Klammer,
-   einen ungültigen Operator oder ein unerwartetes Zeichen.

Testen Sie, wenn möglich, wieder getrennt Token-Liste, AST und Auswertung (Interpreter).

##### Schritt 6: C-Int-Ergebnisse mit nativem C++ vergleichen

Schreiben Sie zu den folgenden Ausdrücken jeweils ein vollständiges C++-Programm mit `main()`, das den Ausdruck berechnet und das Ergebnis ausgibt:

    42
    (1 + 2)
    ((1 + 2) - 3)
    (10 - (3 + 5))

Übersetzen und starten Sie Ihre Programme mit clang++ oder g++.

Vergleichen Sie die Ausgaben mit den Ergebnissen Ihres C-Int-Interpreters und notieren Sie kurz:

-   Zwei Gemeinsamkeiten zwischen der Auswertung im nativen C++-Programm und der Auswertung durch Ihren Interpreter.
-   Zwei Unterschiede zwischen den beiden Verarbeitungswegen (z.B. Laufzeit, Fehlermeldungen, Komfort).

#### (\*\*\*) Vertiefung: Interpreter für L-Int mit Visitor-Pattern

In der Vorlesung wurde die `eval()`-Funktion im L-Int-Interpreter mit Pattern Matching und `switch`-Expressions implementiert.

1.  Übertragen Sie dies auf das Visitor-Pattern.
2.  Skizzieren Sie Vor- und Nachteile des Visitor-Patterns im Vergleich zur `switch`-Variante aus der Vorlesung.

------------------------------------------------------------------------

> [!NOTE]
>
> <details >
> <summary><strong>👀 Quellen</strong></summary>
>
> <div id="refs" class="references csl-bib-body hanging-indent">
>
> <div id="ref-Aho2023" class="csl-entry">
>
> Aho, A. V., M. S. Lam, R. Sethi, J. D. Ullman, und S. Bansal. 2023. *Compilers: Principles, Techniques, and Tools, Updated 2nd Edition by Pearson*. Pearson India. <https://learning.oreilly.com/library/view/compilers-principles-techniques/9789357054881/>.
>
> </div>
>
> <div id="ref-Grune2012" class="csl-entry">
>
> Grune, D., K. van Reeuwijk, H. E. Bal, C. J. H. Jacobs, und K. Langendoen. 2012. *Modern Compiler Design*. Springer.
>
> </div>
>
> <div id="ref-hopcroft2003" class="csl-entry">
>
> Hopcroft, J. E., R. Motwani, und J. D. Ullman. 2003. *Einführung in die Automatentheorie, formale Sprachen und Komplexitätstheorie*. I theoretische informatik. Pearson Education Deutschland GmbH.
>
> </div>
>
> <div id="ref-Nystrom2021" class="csl-entry">
>
> Nystrom, R. 2021. *Crafting Interpreters*. Genever Benning. <https://github.com/munificent/craftinginterpreters>.
>
> </div>
>
> <div id="ref-Parr2014" class="csl-entry">
>
> Parr, T. 2014. *The Definitive ANTLR 4 Reference*. Pragmatic Bookshelf. <https://learning.oreilly.com/library/view/the-definitive-antlr/9781941222621/>.
>
> </div>
>
> <div id="ref-Siek2023python" class="csl-entry">
>
> Siek, J. G. 2023. *Essentials of Compilation: An Incremental Approach in Python*. The MIT Press. <https://github.com/IUCompilerCourse/Essentials-of-Compilation>.
>
> </div>
>
> <div id="ref-Torczon2012" class="csl-entry">
>
> Torczon, L., und K. Cooper. 2012. *Engineering a Compiler*. Morgan Kaufmann. <https://learning.oreilly.com/library/view/engineering-a-compiler/9780080916613/>.
>
> </div>
>
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

**Exceptions:**

-   Screenshot of ["Language Brainfuck"](https://99-bottles-of-beer.net/language-brainfuck-2542.html) by Michal Wojciech Tarnowski on 99-bottles-of-beer.net
-   ["Language Haskell"](https://www.99-bottles-of-beer.net/language-haskell-1070.html) by Iavor on 99-bottles-of-beer.net
-   ["Language C"](https://www.99-bottles-of-beer.net/language-c-116.html) by Bill Wein on 99-bottles-of-beer.net
-   [Clojure.org: Learn Clojure](https://clojure.org/guides/learn/syntax#_structure_vs_semantics)
-   [A Map of the Territory (mountain.png)](https://github.com/munificent/craftinginterpreters/blob/master/site/image/a-map-of-the-territory/mountain.png) by [Bob Nystrom](https://github.com/munificent) on Github.com ([MIT](https://github.com/munificent/craftinginterpreters/blob/master/LICENSE))
-   ["Language Java"](https://www.99-bottles-of-beer.net/language-java-4.html) by Sean Russell on 99-bottles-of-beer.net
-   Abzählreim "99 Bottles of Beer" nach ["Lyrics of the song 99 Bottles of Beer"](https://www.99-bottles-of-beer.net/lyrics.html) on 99-bottles-of-beer.net
-   ["Language Prolog"](https://www.99-bottles-of-beer.net/language-prolog-965.html) by M@ on 99-bottles-of-beer.net

<blockquote><p><sup><sub><strong>Last modified:</strong> 4fef2a5 2026-10-18 sheet01: clarify and rework wording (#457)<br></sub></sup></p></blockquote>
