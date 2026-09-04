# IFM 3.1: Compilerbau (Winter 2026/27)

<a id="id-da39a3ee5e6b4b0d3255bfef95601890afd80709"></a>

## Syllabus

### Syllabus

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/admin/images/architektur_cb_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/admin/images/architektur_cb.png" width="80%" /></picture></p>

#### Kursbeschreibung

Der Compiler ist das wichtigste Werkzeug in der Informatik. In der
Königsdisziplin der Informatik schließt sich der Kreis, hier kommen die
unterschiedlichen Algorithmen und Datenstrukturen und
Programmiersprachenkonzepte zur Anwendung.

In diesem Modul geht es um ein grundlegendes Verständnis für die
wichtigsten Konzepte im Compilerbau. Wir schauen uns dazu relevante
aktuelle Tools und Frameworks an und setzen diese bei der Erstellung
eines kleinen Compiler-Frontends für C++ ein.

#### Überblick Modulinhalte

1.  Lexikalische Analyse: Scanner/Lexer
    -   Reguläre Sprachen
    -   Generierung mit ANTLR
2.  Syntaxanalyse: Parser
    -   Kontextfreie Grammatiken (CFG)
    -   LL-Parser (Top-Down-Parser)
    -   Generierung mit ANTLR
3.  Semantische Analyse: Symboltabellen
    -   Namen und Scopes
    -   Typen, Klassen, Polymorphie
4.  Interpreter: AST-Traversierung
5.  C++ als zu verarbeitende Programmiersprache

#### Team

-   [BC
    George](https://www.hsbi.de/minden/ueber-uns/personenverzeichnis/birgit-christina-george)
-   [Carsten
    Gips](https://www.hsbi.de/minden/ueber-uns/personenverzeichnis/carsten-gips)
    (Sprechstunde nach Vereinbarung)
-   Alesia Herbertz, Vivien Traue, Jonathan Hauer (Tutor:innen)

#### Kursformat

| Vorlesung (2 SWS)            | Praktikum (2 SWS)                       |
|:-----------------------------|:----------------------------------------|
| Mo, 15:45 - 17:15 Uhr (Zoom) | G1: Mi, 11:30 - 13:00 Uhr (Zoom)        |
|                              | G2: Mi, 14:00 - 15:30 Uhr (Zoom)        |
|                              | G3: Mi, 11:30 - 13:00 Uhr (Präsenz B40) |
|                              | G4: Mi, 14:00 - 15:30 Uhr (Präsenz B40) |

Durchführung der Vorlesung als *Flipped Classroom* (Carsten) bzw. als
*reguläre Vorlesung* (BC). Zugangsdaten Zoom siehe
[ILIAS](https://www.hsbi.de/elearning/goto.php/crs/1555855).

#### Fahrplan

Abgabe der Post Mortems jeweils **Montag bis 09:00 Uhr** im
[ILIAS](https://www.hsbi.de/elearning/goto.php/exc/1582799). Vorstellung
der Lösung im jeweiligen Praktikum in der Abgabewoche.

| Monat | Woche vom | Vorlesung (Mo) | Praktikum (Mi) | Edmonton/Minden-Meetings |
|:----------|:----------|:------------------------|:----------|:-------------|
| Oktober | 12\. ... | [Orga](#id-275d783e298228506068436512433d343feb52aa) \|\| [Überblick](#id-53b4e459f1e03e12f2d557b6d221d09cef3a8613) |  |  |
|  | 19\. ... | [Reguläre Sprachen 1](#id-fcaf093293b68336fe1969551452a22ffceb414f) | [B01](#id-6f673c2e093cdfc53b1f78baef11fd06cc8aa415) |  |
|  | 26\. ... | [Reguläre Sprachen 2](#id-f6fa0ccf6771a7f1dbe0a08f4e77ed43af82ac84) \|\| [CFG](#id-ed1ca9f1d126c913f7ce93106335deafa8e5a251) | [B02](#id-0db349230022c35e045dc3b052a4faea50fe5f40) |  |
| November | 02\. ... | [LL-Parser (Theorie)](#id-07a415053a63f15160a328d38fe730bbd2c78148) |  | **Di, 03.11., 17:00 - 18:00 Uhr (online): ANTLR + Live-Coding** |
|  | 09\. ... | [L-Int (Teil 1)](#id-556cd2c3f1629cf784ff561cfd43f01f0fed8cd7) | **Station 1** |  |
|  | 16\. ... | [L-Int (Teil 2)](#id-556cd2c3f1629cf784ff561cfd43f01f0fed8cd7) | C-Int |  |
|  | 23\. ... | [L-Expr](#id-99cb41fc092ae89529d2cfd0281e8535a15dbfbf) | C-Expr |  |
| Dezember | 30.11. ... | [L-Var](#id-f8afd3e3cb9ba1df1e2b5422b50bc32aaa3350c7) | **Station 2** | **Mo, 30.11., 17:00 - 18:00 Uhr (online): Minden Presentations** |
|  | 07\. ... | [L-If](#id-51bc1e52157fd6e296d145056356183c5f0620e5) | C-Var, C-If | **Mo, 07.12., 17:00 - 18:00 Uhr (online): Edmonton Presentations** |
|  | 14\. ... | [L-Fun](#id-d5e1d626434b082efe6661d1c3dbae47343aaa38) | C-Fun |  |
|  | *21. ...* | **Weihnachtspause** |  |  |
|  | *28. ...* | **Weihnachtspause** |  |  |
| Januar | 04\. ... | [L-Class](#id-883bc38559a6155ddb7626d1df4fa119f5b21780) | **Station 3** |  |
|  | 11\. ... | [L-Inherit](#id-19c98c0c74958c63b88445f7d53a44f314a603cd) | C-Class, C-Inherit |  |
|  | 18\. ... | [L-Self](#id-6d845692d09d49d96e2c03493d4e812a0d48d712) | C-Self, Snake |  |
|  | 25\. ... | Rückblick | Projektvorstellung (Video) |  |

#### Prüfungsform, Note und Credits

**(Digitale) Klausur plus Studienleistung (Portfolio)**, 5 ECTS

-   **Studienleistung**: "Portfolio" - Kriterien je Person:

    1.  Teilnahme an mind. zwei Edmonton/Minden-Terminen mit aktiver
        Beteiligung
    2.  Mind. vier der Übungsblätter B01..B07 erfolgreich bearbeitet
    3.  Abschlussvortrag zum erfolgreich bearbeiteten Mini-Projekt (B08)
        am Semesterende (21.01.) a 15 Minuten (pro Team)

    Je Kriterium: Abgabe eines Post Mortem im ILIAS (**jede Person
    individuell**)

-   **Gesamtnote**: (Digitale) Klausur im B40 (90 Minuten)

<details>
<summary><strong>Hinweise</strong></summary>

-   Die Bearbeitung der Leistungen erfolgt im Team.
-   Ein Team umfasst 3 Personen.
-   Die Post Mortems sind individuell zu erstellen und abzugeben.
-   "Aktive Beteiligung" umfasst Anwesenheit und sachbezogene Beiträge;
    Anwesenheit/Beteiligung werden dokumentiert.
-   "Erfolgreiche Bearbeitung" eines Blattes umfasst Bearbeitung im
    Team, Bearbeitung aller Aufgaben des Blattes, fristgerechte Abgabe
    des ausreichenden Post Mortems im ILIAS. Die intensive Beschäftigung
    mit den Aufgaben muss erkennbar sein.

<!-- -->

-   **Post Mortem**: Jede Person beschreibt individuell(!) die
    Bearbeitung des jeweiligen Kriteriums bzw. die Teilnahme an den
    Edmonton/Minden-Meetings zurückblickend mit mind. 150 bis max. 400
    Wörtern (Nutzlast; Überschriften und Links zählen nicht mit). Gehen
    Sie dabei aussagekräftig und nachvollziehbar auf folgende Punkte
    ein:

    1.  Zusammenfassung: Was wurde gemacht bzw. was wurde auf dem
        Meeting besprochen?
    2.  Details: Kurze Beschreibung besonders interessanter Aspekte.
    3.  Reflexion: Was war der schwierigste Teil? Wie haben Sie dieses
        Problem gelöst?
    4.  Reflexion: Was haben Sie gelernt oder (besser) verstanden?
    5.  Team: Mit wem haben Sie zusammengearbeitet?
    6.  Link zu Ihrem Repo mit den relevanten Artefakten (Lösung, Slides
        für den Vortrag, ...).

    Für die Edmonton/Minden-Meetings passen Sie bitte die Punkte (1)
    bis (4) entsprechend inhaltlich an, (5) und (6) entfallen.

    Die Post Mortems geben Sie bitte pro Person bis spätestens zur
    jeweiligen Deadline im
    [ILIAS](https://www.hsbi.de/elearning/goto.php/exc/1582799) ab.

    Siehe auch
    https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor-W25/discussions/3.

</details>

#### Materialien

1.  ["**Compilers: Principles, Techniques, and
    Tools**"](https://learning.oreilly.com/library/view/compilers-principles-techniques/9789357054881/).
    Aho, A. V. und Lam, M. S. und Sethi, R. und Ullman, J. D. and
    Bansal, S., Pearson India, 2023. ISBN
    [978-9-3570-5488-1](https://fhb-bielefeld.digibib.net/openurl?isbn=978-9-3570-5488-1).
    [Online](https://learning.oreilly.com/library/view/compilers-principles-techniques/9789357054881/)
    über die
    [O'Reilly-Lernplattform](https://www.oreilly.com/library-access/).
2.  ["**Crafting
    Interpreters**"](https://github.com/munificent/craftinginterpreters).
    Nystrom, R., Genever Benning, 2021. ISBN
    [978-0-9905829-3-9](https://fhb-bielefeld.digibib.net/openurl?isbn=978-0-9905829-3-9).
    [Online](https://www.craftinginterpreters.com/).
3.  ["**The Definitive ANTLR 4
    Reference**"](https://learning.oreilly.com/library/view/the-definitive-antlr/9781941222621/).
    Parr, T., Pragmatic Bookshelf, 2014. ISBN
    [978-1-9343-5699-9](https://fhb-bielefeld.digibib.net/openurl?isbn=978-1-9343-5699-9).
    [Online](https://learning.oreilly.com/library/view/the-definitive-antlr/9781941222621/)
    über die
    [O'Reilly-Lernplattform](https://www.oreilly.com/library-access/).
4.  ["Writing a C
    Compiler"](https://learning.oreilly.com/library/view/writing-a-c/9781098182229/).
    Sandler, N., No Starch Press, 2024. ISBN
    [978-1-0981-8222-9](https://fhb-bielefeld.digibib.net/openurl?isbn=978-1-0981-8222-9).
    [Online](https://learning.oreilly.com/library/view/writing-a-c/9781098182229/)
    über die
    [O'Reilly-Lernplattform](https://www.oreilly.com/library-access/).

#### Förderungen und Kooperationen

##### Kooperation mit University of Alberta, Edmonton (Kanada)

Über das Projekt ["We CAN
virtuOWL"](https://www.uni-bielefeld.de/international/profil/netzwerk/alberta-owl/we-can-virtuowl/)
der Fachhochschule Bielefeld ist im Frühjahr 2021 eine Kooperation mit
der [University of
Alberta](https://www.hsbi.de/en/international-office/alberta-owl-cooperation)
(Edmonton/Alberta, Kanada) im Modul "Compilerbau" gestartet.

Wir freuen uns, auch in diesem Semester wieder drei gemeinsame Sitzungen
für beide Hochschulen anbieten zu können. (Diese Termine werden in
englischer Sprache durchgeführt.)

------------------------------------------------------------------------

#### LICENSE

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, [this
work](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor) by
[BC George](https://github.com/bcg7), [Carsten
Gips](https://github.com/cagix) and
[contributors](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/graphs/contributors)
is licensed under [CC BY-SA
4.0](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/blob/master/LICENSE.md).
See the
[credits](https://github.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/blob/master/CREDITS.md)
for a detailed list of contributing projects.

<a id="id-af09e2fcaf4589921086150d991647b7b52abd03"></a>

## Vorlesungsunterlagen

<a id="id-352b1e776856b1ada7109fc42e5a89b3d7f98605"></a>

### Überblick

Was ist ein Compiler? Welche Bausteine lassen sich identifizieren,
welche Aufgaben haben diese?

<a id="id-5ba813f3272c7a3032533eb3123c86099ee6c775"></a>

### Lexikalische Analyse

In der lexikalischen Analyse soll ein Lexer (auch "Scanner") den
Zeichenstrom in eine Folge von Token zerlegen. Zur Spezifikation der
Token werden in der Regel reguläre Ausdrücke verwendet.

<a id="id-fcaf093293b68336fe1969551452a22ffceb414f"></a>

#### Reguläre Sprachen, Ausdrucksstärke (Teil 1)

> [!TIP]
>
> <details open>
> <summary><strong>🖇 Weitere Unterlagen</strong></summary>
>
> -   [Annotierte Folien: Reguläre Sprachen,
>     Ausdrucksstärke](https://github.com/Compiler-CampusMinden/AnnotatedSlides/blob/master/lexing_regular1.ann.ba.pdf)
>
> </details>

##### Motivation

###### Was muss ein Compiler wohl als erstes tun?

Hier entsteht ein Tafelbild.

###### Themen für heute

-   Lexer
-   Endliche Automaten
-   Reguläre Sprachen

##### Endliche Automaten

###### Alphabete

**Def.:** Ein **Alphabet** $\Sigma$ ist eine endliche, nicht-leere Menge
von Symbolen. Die Symbole eines Alphabets heißen *Buchstaben*.

**Def.:** Ein **Wort** $w$ *über einem Alphabet* $\Sigma$ ist eine
endliche Folge von Symbolen aus $\Sigma$. $\epsilon$ ist das leere Wort.
Die *Länge* $\vert w \vert$ eines Wortes $w$ ist die Anzahl von
Buchstaben, die es enthält (Kardinalität).

**Def.:** Eine **Sprache** $L$ *über einem Alphabet* $\Sigma$ ist eine
Menge von Wörtern über diesem Alphabet. Sprachen können endlich oder
unendlich viele Wörter enthalten.

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

**Def.:** Ein **deterministischer endlicher Automat** (DFA) ist ein
5-Tupel $A = (Q, \Sigma, \delta, q_0, F)$ mit

-   $Q$ : endliche Menge von **Zuständen**

-   $\Sigma$ : Alphabet von **Eingabesymbolen**

-   $\delta$ : die (eventuell partielle) **Übergangsfunktion**
    $(Q \times \Sigma) \rightarrow Q$, $\delta$ kann partiell sein

-   $q_0 \in Q$ : der **Startzustand**

-   $F \subseteq Q$ : die Menge der **Endzustände**

###### Beispiel

Hier entsteht ein Tafelbild.

###### Eingabewörter statt Buchstaben

**Def.:** Wir definieren
$\delta^{\ast}: (Q \times \Sigma^{\ast}) \rightarrow Q$: induktiv wie
folgt:

-   Basis: $\delta^{\ast}(q, \epsilon) = q\ \forall q \in Q$

-   Induktion:
    $\delta^{\ast}(q, a_1, \ldots, a_n) = \delta(\delta^{\ast}(q, a_1, \ldots , a_{n-1}), a_n)$

**Def.:** Ein DFA akzeptiert ein Wort $w \in \Sigma^{\ast}$ genau dann,
wenn $\delta^{\ast}(q_0, w) \in F.$

###### Beispiel

Hier entsteht ein Tafelbild.

###### Nichtdeterministische endliche Automaten

Hier entsteht ein Tafelbild.

###### Def.: Nichtdeterministischer Automat

**Def.:** Ein **nichtdeterministischer endlicher Automat** (NFA) ist ein
5-Tupel $A = (Q, \Sigma, \delta, q_0, F)$ mit

-   $Q$ : endliche Menge von **Zuständen**

-   $\Sigma$ : Alphabet von **Eingabesymbolen**

-   $\delta$ : die (eventuell partielle) **Übergangsfunktion**
    $(Q \times \Sigma) \rightarrow Q$

-   $q_0 \in Q$ : der **Startzustand**

-   $F \subseteq Q$ : die Menge der **Endzustände**

###### Akzeptierte Sprachen

**Def.:** Sei A ein DFA oder ein NFA. Dann ist **L(A)** die von A
akzeptierte Sprache, d. h.

$L(A) = \lbrace \text{Wörter}\ w\ |\ \delta^*(q_0, w) \in F \rbrace$

###### Wozu NFAs im Compilerbau?

Pattern Matching (Erkennung von Schlüsselwörtern, Bezeichnern, ...) geht
mit NFAs.

NFAs sind so nicht zu programmieren, aber:

**Satz:** Eine Sprache $L$ wird von einem NFA akzeptiert
$\Leftrightarrow L$ wird von einem DFA akzeptiert.

D. h. es existieren Algorithmen zur

-   Umwandlung von NFAs in DFAS
-   Minimierung von DFAs

##### Reguläre Sprachen

###### Reguläre Ausdrücke definieren Sprachen

**Def.:** Induktive Definition von **regulären Ausdrücken** (regex) und
der von ihnen repräsentierten Sprache **L**:

-   Basis:

    -   $\epsilon$ und $\emptyset$ sind reguläre Ausdrücke mit
        $L(\epsilon) =
          \lbrace \epsilon\rbrace$, $L(\emptyset)=\emptyset$
    -   Sei $a$ ein Symbol $\Rightarrow$ $a$ ist ein regex mit
        $L(a) = \lbrace a\rbrace$

-   Induktion: Seien $E,\ F$ reguläre Ausdrücke. Dann gilt:

    -   $E+F$ ist ein regex und bezeichnet die Vereinigung
        $L(E + F) = L(E)\cup L(F)$
    -   $EF$ ist ein regex und bezeichnet die Konkatenation
        $L(EF) = L(E)L(F)$
    -   $E^{\ast}$ ist ein regex und bezeichnet die Kleene-Hülle
        $L(E^{\ast})=(L(E))^{\ast}$
    -   $(E)$ ist ein regex mit $L((E)) = L(E)$

Vorrangregeln der Operatoren für reguläre Ausdrücke: \*, Konkatenation,
+

###### Beispiel

Hier entsteht ein Tafelbild.

###### Wichtige Identitäten

**Satz:** Sei $A$ ein DFA $\Rightarrow \exists$ regex $R$ mit
$L(A) = L(R)$.

**Satz:** Sei $E$ ein regex $\Rightarrow \exists$ DFA $A$ mit
$L(E) = L(A)$.

###### Formale Grammatiken

Hier entsteht ein Tafelbild.

###### Formale Definition formaler Grammatiken

**Def.:** Eine *formale Grammatik* ist ein 4-Tupel $G=(N,T,P,S)$ aus

-   $N$: endliche Menge von **Nichtterminalen**

-   $T$: endliche Menge von **Terminalen**, $N \cap T = \emptyset$

-   $S \in N$: **Startsymbol**

-   $P$: endliche Menge von **Produktionen** der Form

$\qquad X \rightarrow Y$ mit
$X \in (N \cup T)^{\ast} N  (N \cup T)^{\ast}, Y \in (N \cup T)^{\ast}$

###### Ableitungen

**Def.:** Sei $G = (N, T, P, S)$ eine Grammatik, sei $\alpha A \beta$
eine Zeichenkette über $(N \cup T)^{\ast}$ und sei $A$
$\rightarrow \gamma$ eine Produktion von $G$.

Wir schreiben: $\alpha A \beta \Rightarrow \alpha \gamma \beta$
($\alpha A \beta$ leitet $\alpha \gamma \beta$ ab).

**Def.:** Wir definieren die Relation $\overset{\ast}{\Rightarrow}$
induktiv wie folgt:

-   Basis:
    $\forall \alpha \in (N \cup T)^{\ast} \alpha \overset{\ast}{\Rightarrow} \alpha$
    (Jede Zeichenkette leitet sich selbst ab.)

-   Induktion: Wenn $\alpha \overset{\ast}{\Rightarrow} \beta$ und
    $\beta\Rightarrow \gamma$ dann
    $\alpha \overset{\ast}{\Rightarrow} \gamma$

**Def.:** Sei $G = (N, T ,P, S)$ eine formale Grammatik. Dann ist
$L(G) = \lbrace \text{Wörter}\ w\ \text{über}\ T \mid S \overset{\ast}{\Rightarrow} w\rbrace$
die von $G$ erzeugte Sprache.

###### Beispiel

Hier entsteht ein Tafelbild.

###### Reguläre Grammatiken

**Def.:** Eine **reguläre (oder type-3-) Grammatik** ist eine formale
Grammatik mit den folgenden Einschränkungen:

-   Alle Produktionen sind entweder von der Form

    -   $X \to aY$ mit $X \in N, a \in T, Y \in N$ (*rechtsreguläre*
        Grammatik) oder
    -   $X \to Ya$ mit $X \in N, a \in T, Y \in N$ (*linksreguläre*
        Grammatik)

-   $X\rightarrow\epsilon$ ist erlaubt

###### Beispiel

Hier entsteht ein Tafelbild.

###### Reguläre Sprachen

**Satz:** Die von endlichen Automaten akzeptiert Sprachklasse, die von
regulären Ausdrücken beschriebene Sprachklasse und die von regulären
Grammatiken erzeugte Sprachklasse sind identisch und heißen **reguläre
Sprachen**.

##### Wrap-Up

###### Wrap-Up

-   Definition und Aufgaben von Lexern
-   DFAs und NFAs
-   Reguläre Ausdrücke
-   Reguläre Grammatiken
-   Zusammenhänge zwischen diesen Mechanismen und Lexern, bzw.
    Lexergeneratoren

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
> -   k2: Ich kann die Zusammenhänge und Gesetzmäßigkeiten bzgl. der
>     oben genannten Konstrukte an einem Beispiel erklären
> -   k3: Ich kann für eine Fragestellung DFAs, reguläre Ausdrücke,
>     reguläre Grammatiken entwickeln
> -   k3: Ich kann einen DFA entwickeln, der alle Schlüsselwörter, Namen
>     und weitere Symbole einer Programmiersprache akzeptiert
>
> </details>

<a id="id-f6fa0ccf6771a7f1dbe0a08f4e77ed43af82ac84"></a>

#### Reguläre Sprachen, Ausdrucksstärke (Teil 2)

> [!TIP]
>
> <details open>
> <summary><strong>🖇 Weitere Unterlagen</strong></summary>
>
> -   [Annotierte Folien: Reguläre Sprachen,
>     Ausdrucksstärke](https://github.com/Compiler-CampusMinden/AnnotatedSlides/blob/master/lexing_regular2.ann.ba.pdf)
>
> </details>

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

-   definieren Schlüsselwörter und alle weiteren Symbole einer
    Programmiersprache, z. B. den Aufbau von Gleitkommazahlen
-   werden (oft von einem Generator) in DFAs umgewandelt
-   sind die Basis des *Scanners* oder *Lexers*

##### Lexer

###### Ein Lexer ist mehr als ein DFA

Ein **Lexer**

-   kann aus regulären Ausdrücken automatisch generiert werden

-   wandelt mittels DFAs aus regulären Ausdrücken die Folge von Zeichen
    der Quelldatei in eine Folge von sog. Token um

-   bekommt als Input eine Liste von Paaren aus regulären Ausdrücken und
    Tokennamen, z. B. ("while", WHILE)

-   Kommentare und Strings müssen richtig erkannt werden.
    (Schachtelungen)

-   liefert Paare von Token und deren Werte, sofern benötigt, z. B.
    (WHILE, \_), oder (IDENTIFIER, "radius") oder (INTEGERZAHL, "334")

###### Wofür reichen reguläre Sprachen nicht?

Für z. B. alle Sprachen, in deren Wörtern Zeichen über eine Konstante
hinaus gezählt werden müssen. Diese Sprachen lassen sich oft mit
Variablen im Exponenten beschreiben, die unendlich viele Werte annehmen
können.

-   $a^ib^{2*i}$ ist nicht regulär
-   $a^ib^{2*i}$ für $0 \leq i \leq 3$ ist regulär

<!-- -->

-   Wo finden sich die oben genannten Variablen bei einem DFA wieder?
-   Warum ist die erste Sprache oben nicht regulär, die zweite aber?

###### Wie geht es weiter?

Ein **Parser**

-   führt mit Hilfe des Tokenstreams vom Lexer die Syntaxanalyse durch

-   basiert auf einer sog. kontextfreien Grammatik, deren Terminale die
    Token sind

-   liefert die syntaktische Struktur in Form eines Ableitungsbaums
    (**syntax tree**, **parse tree**), bzw. einen **AST** (abstract
    syntax tree) ohne redundante Informationen im Ableitungsbaum (z. B.
    Semikolons)

-   liefert evtl. Fehlermeldungen

##### Wrap-Up

###### Wrap-Up

-   Definition und Aufgaben von Lexern
-   Zusammenhänge zwischen diesen Mechanismen und Lexern, bzw.
    Lexergeneratoren
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
> -   k1: Ich kenne die Zusammenhänge zwischen DFAs, regulären
>     Ausdrücken, regulären Grammatiken und Lexern
> -   k2: Ich kann für eine Beispielsprache begründen, warum sie nicht
>     mit einem der oben genannten Mechanismen beschrieben werden kann
>
> </details>

<a id="id-208bd9b4194c2c79688c8354df6850e6040cdef7"></a>

### Syntaktische Analyse

In der syntaktischen Analyse arbeitet ein Parser mit dem Tokenstrom, der
vom Lexer kommt. Mit Hilfe einer Grammatik wird geprüft, ob gültige
Sätze im Sinne der Sprache/Grammatik gebildet wurden. Der Parser erzeugt
dabei den Parse-Tree. Man kann verschiedene Parser unterscheiden,
beispielsweise die LL- und die LR-Parser.

<a id="id-ed1ca9f1d126c913f7ce93106335deafa8e5a251"></a>

#### CFG

> [!TIP]
>
> <details open>
> <summary><strong>🖇 Weitere Unterlagen</strong></summary>
>
> -   [Annotierte Folien: CFG,
>     LL-Parser](https://github.com/Compiler-CampusMinden/AnnotatedSlides/blob/master/frontend_parsing_cfg.ann.ba.pdf)
>
> </details>

##### Wiederholung

###### Endliche Automaten, reguläre Ausdrücke, reguläre Grammatiken, reguläre Sprachen

-   Was ist ein Lexer?
-   In welchem Zusammenhang stehen Lexer und reguläre Sprachen?
-   Was können Lexer nicht?

##### Motivation

###### Was brauchen wir jetzt?

###### Themen für heute

-   PDAs: mächtiger als DFAs, NFAs
-   kontextfreie Grammatiken und Sprachen: mächtiger als reguläre
    Grammatiken und Sprachen
-   DPDAs und deterministisch kontextfreie Grammatiken: die Grundlage
    der Syntaxanalyse im Compilerbau
-   Syntaxanalyse

###### Einordnung: Erweiterung der Automatenklasse DFA, um komplexere Sprachen als die regulären akzeptieren zu können

Wir spendieren den DFAs einen möglichst einfachen, aber beliebig großen,
Speicher, um zählen und matchen zu können. Wir suchen dabei
konzeptionell die "kleinstmögliche" Erweiterung, die die akzeptierte
Sprachklasse gegenüber DFAs vergrößert.

-   Der konzeptionell einfachste Speicher ist ein Stack. Wir haben
    keinen wahlfreien Zugriff auf die gespeicherten Werte.

-   Es soll eine deterministische und eine indeterministische Variante
    der neuen Automatenklasse geben.

-   In diesem Zusammenhang wird der Stack auch Keller genannt.

###### Kellerautomaten (Push-Down-Automata, PDAs)

**Def.:** Ein Kellerautomat (PDA)
$P = (Q,\ \Sigma,\ \Gamma,\  \delta,\ q_0,\ \perp,\ F)$ ist ein Septupel
mit:

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/Def_PDA_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/Def_PDA.png" width="60%" /></picture></p><p align="center">Definition eines PDAs</p>

Ein PDA ist per Definition nichtdeterministisch und kann spontane
Zustandsübergänge durchführen.

###### Was kann man damit akzeptieren?

Strukturen mit paarweise zu matchenden Symbolen.

Bei jedem Zustandsübergang wird ein Zeichen (oder $\epsilon$) aus der
Eingabe gelesen, ein Symbol von Keller genommen. Diese und das
Eingabezeichen bestimmen den Folgezustand und eine Zeichenfolge, die auf
den Stack gepackt wird. Dabei wird ein Symbol, (z. B. eines, das später
mit einem Eingabesymbol zu matchen ist,) auf den Stack gepackt. Soll das
automatisch vom Stack genommene Symbol auf dem Stack bleiben, muss es
wieder gepusht werden.

###### Beispiel

Ein PDA für
$L=\lbrace ww^{R}\mid w\in \lbrace a,b\rbrace^{\ast}\rbrace$:

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/pda2_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/pda2.png" width="45%" /></picture></p>

###### Noch ein Beispiel

Hier entsteht ein Tafelbild.

###### Deterministische PDAs

**Def.** Ein PDA $P = (Q, \Sigma, \Gamma, \delta, q_0, \perp, F)$ ist
*deterministisch* $: \Leftrightarrow$

-   $\delta(q, a, X)$ hat höchstens ein Element für jedes
    $q \in Q, a \in\Sigma$ oder $(a = \epsilon$ und $X \in \Gamma)$.

-   Wenn $\delta (q, a, x)$ nicht leer ist für ein $a \in \Sigma$, dann
    muss $\delta (q, \epsilon, x)$ leer sein.

Deterministische PDAs werden auch *DPDAs* genannt.

###### Der kleine Unterschied

**Satz:** Die von DPDAs akzeptierten Sprachen sind eine echte Teilmenge
der von PDAs akzeptierten Sprachen.

Die regulären Sprachen sind eine echte Teilmenge der von DPDAs
akzeptierten Sprachen.

##### Kontextfreie Grammatiken und Sprachen

###### Kontextfreie Grammatiken

**Def.** Eine *kontextfreie (cf-)* Grammatik ist ein 4-Tupel
$G = (N, T, P, S)$ mit $N, T, S$ wie in (formalen) Grammatiken und $P$
ist eine endliche Menge von Produktionen der Form:

$X \rightarrow Y$ mit $X \in N, Y \in {(N \cup T)}^{\ast}$.

$\Rightarrow, \overset{\ast}{\Rightarrow}$ sind definiert wie bei
regulären Sprachen. Bei cf-Grammatiken nennt man die Ableitungsbäume oft
*Parse trees*.

###### Beispiel

Hier entsteht ein Tafelbild.

###### Was ist hier los?

$S \rightarrow a \mid S\ +\  S\ |\  S \ast S$

Ableitungsbäume für $a + a \ast a$:

Hier entsteht ein Tafelbild.

###### Nicht jede kontextfreie Grammatik ist eindeutig

**Def.:** Gibt es in einer von einer kontextfreien Grammatik erzeugten
Sprache ein Wort, für das mehr als ein Ableitungsbaum existiert, so
heißt diese Grammatik *mehrdeutig*. Anderenfalls heißt sie *eindeutig*.

**Satz:** Es ist nicht entscheidbar, ob eine gegebene kontextfreie
Grammatik eindeutig ist.

**Satz:** Es gibt kontextfreie Sprachen, für die keine eindeutige
Grammatik existiert.

###### Kontextfreie Grammatiken und PDAs

**Satz:** Die kontextfreien Sprachen und die Sprachen, die von PDAs
akzeptiert werden, sind dieselbe Sprachklasse.

**Satz:** Eine von einem DPDA akzeptierte Sprache hat eine eindeutige
Grammatik.

Vorgehensweise im Compilerbau: Eine (cf) Grammatik für die gewünschte
Sprache definieren und schauen, ob sich daraus ein DPDA generieren lässt
(automatisch).

##### Syntaxanalyse

###### Syntax

Wir verstehen unter Syntax eine Menge von Regeln, die die Struktur von
Daten (z. B. Programmen) bestimmen.

###### Ziele der Syntaxanalyse

-   Bestimmung der syntaktischen Struktur eines Programms

-   aussagekräftige Fehlermeldungen, wenn ein Eingabeprogramm
    syntaktisch nicht korrekt ist

-   Erstellung des AST (abstrakter Syntaxbaum): Der Parse Tree ohne
    Symbole, die nach der Syntaxanalyse inhaltlich irrelevant sind
    (z. B. Semikolons, manche Schlüsselwörter)

-   die Symboltabelle(n) mit Informationen bzgl. Bezeichner (Variable,
    Funktionen und Methoden, Klassen, benutzerdefinierte Typen,
    Parameter, ...), aber auch die Gültigkeitsbereiche

###### Was brauchen wir für die Syntaxanalyse von Programmen?

-   einen Grammatiktypen, aus dem sich manuell oder automatisiert ein
    Programm zur deterministischen Syntaxanalyse (= Parser) erstellen
    lässt

-   einen Algorithmus zum Parsen von Programmen mit Hilfe einer solchen
    Grammatik

##### Wrap-Up

###### Das sollen Sie mitnehmen

-   Die Struktur von gängigen Programmiersprachen lässt sich nicht mit
    regulären Ausdrücken beschreiben und damit nicht mit DFAs
    akzeptieren.
-   Das Automatenmodell der DFAs wird um einen endlosen Stack erweitert,
    das ergibt PDAs.
-   Kontextfreie Grammatiken (CFGs) erweitern die regulären Grammatiken.
-   PDAs akzeptieren kontextfreie Sprachen.
-   Deterministisch parsbare Sprachen haben eine eindeutige kontextfreie
    Grammatik, aber nicht für jede eindeutige kontextfreie Grammatik
    lässt sich ein deterministischer PDA finden.
-   Es ist nicht entscheidbar, ob eine gegebene kontextfreie Grammatik
    eindeutig ist.
-   Syntaxanalyse wird mit (möglichst deterministisch) kontextfreien
    Grammatiken durchgeführt.
-   In der Praxis werden aus kontextfreien Grammatiken Parser
    automatisch generiert.

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
> -   k2: Ich kann den Zusammenhang zwischen PDAs und kontextfreien
>     Grammatiken an einem Beispiel erklären
> -   k2: Ich kann PDAs entwickeln
> -   k2: Ich kann kontextfreie Grammatiken entwickeln
>
> </details>

<a id="id-07a415053a63f15160a328d38fe730bbd2c78148"></a>

#### LL-Parser

> [!TIP]
>
> <details open>
> <summary><strong>🖇 Weitere Unterlagen</strong></summary>
>
> -   [Annotierte Folien:
>     LL-Parser](https://github.com/Compiler-CampusMinden/AnnotatedSlides/blob/master/ll-parser.ann.ba.pdf)
>
> </details>

##### Wiederholung

###### PDAs und kontextfreie Grammatiken

-   Warum reichen uns DFAs nicht zum Matchen von Eingabezeichen?
-   Wie könnnen wir sie minimal erweitern?
-   Sind PDAs deterministisch?
-   Wie sind kontextfreie Grammatiken definiert?
-   Sind kontextfreie Grammatiken eindeutig?

##### Motivation

###### Was brauchen wir für die Syntaxanalyse von Programmen?

-   einen Grammatiktypen, aus dem sich manuell oder automatisiert ein
    Programm zur deterministischen Syntaxanalyse (=Parser) erstellen
    lässt
-   einen Algorithmus zum Parsen von Programmen mit Hilfe einer solchen
    Grammatik

###### Themen für heute

-   Automatische Generierung von Top-Down-Parsern aus LL-Grammatiken

##### Syntaxanalyse

###### Syntax

Wir verstehen unter Syntax eine Menge von Regeln, die die Struktur von
Daten (z. B. Programmen) bestimmen.

Syntaxanalyse ist die Bestimmung, ob Eingabedaten einer vorgegebenen
Syntax entsprechen.

Diese vorgegebene Syntax wird im Compilerbau mit einer kontextfreien
Grammatik beschrieben und mit einem sogenannten **Parser** analysiert.

Wir beshäftigen uns heute mit LL-Parsing, mit dem man eine Teilmenge der
eindeutigen kontextfreien Grammatiken syntaktich analysieren kann.

Der Ableitungsbaumwird von oben nach unten aufgebaut.

###### Ziele der Syntaxanalyse

-   aussagekräftige Fehlermeldungen, wenn ein Eingabeprogramm
    syntaktisch nicht korrekt ist
-   evtl. Fehlerkorrektur
-   Bestimmung der syntaktischen Struktur eines Programms
-   Erstellung des AST (abstrakter Syntaxbaum): Der Parse Tree ohne
    Symbole, die nach der Syntaxanalyse inhaltlich irrelevant sind
    (z. B. Semikolons, manche Schlüsselwörter)
-   die Symboltablelle(n) mit Informationen bzgl. Bezeichner (Variable,
    Funktionen und Methoden, Klassen, benutzerdefinierte Typen,
    Parameter, ...), aber auch die Gültigkeitsbereiche.

##### LL(k)-Grammatiken

###### First-Mengen

$S \rightarrow A \ \vert \ B \ \vert \ C$

Welche Produktion nehmen?

Wir brauchen die "terminalen k-Anfänge" von Ableitungen von
Nichtterminalen, um eindeutig die nächste zu benutzende Produktion
festzulegen. $k$ ist dabei die Anzahl der Vorschautoken.

**Def.:** Wir definieren $First$ - Mengen einer Grammatik wie folgt:

-   $a \in T^\ast, |a| \leq k: {First}_k (a) = \lbrace a\rbrace$
-   $a \in T^\ast, |a| > k: {First}_k (a) = \lbrace v \in T^\ast \mid a = vw, |v| = k\rbrace$
-   $\alpha \in (N \cup T)^\ast \backslash T^\ast: {First}_k (\alpha) = \lbrace v \in T^\ast \mid  \alpha \overset{\ast}{\Rightarrow} w,\text{mit}\ w \in T^\ast, First_k(w) = \lbrace v \rbrace \rbrace$

###### Linksableitungen

**Def.:** Bei einer kontextfreien Grammatik $G$ ist die *Linksableitung*
von $\alpha \in (N \cup T)^{\ast}$ die Ableitung, die man erhält, wenn
in jedem Schritt das am weitesten links stehende Nichtterminal in
$\alpha$ abgeleitet wird.

Man schreibt $\alpha \overset{\ast}{\Rightarrow}_l \beta.$

###### LL(k)-Grammatiken

**Def.:** Eine kontextfreie Grammatik *G = (N, T, P, S)* ist genau dann
eine *LL(k)*-Grammatik, wenn für alle Linksableitungen der Form:

$S \overset{\ast}{\Rightarrow}_l\ wA \gamma\ {\Rightarrow}_l\ w\alpha\gamma \overset{\ast}{\Rightarrow}_l wx$

und

$S \overset{\ast}{\Rightarrow}_l wA \gamma {\Rightarrow}_l w\beta\gamma \overset{\ast}{\Rightarrow}_l wy$

mit
$(w, x, y \in T^\ast, \alpha, \beta, \gamma \in (N \cup T)^\ast, A \in N)$
und $First_k(x) = First_k(y)$ gilt:

$\alpha = \beta$

###### LL(1)-Grammatiken

Hier entsteht ein Tafelbild.

###### LL(k)-Sprachen

Die von *LL(k)*-Grammatiken erzeugten Sprachen sind eine echte Teilmenge
der deterministisch parsbaren Sprachen.

Die von *LL(k)*-Grammatiken erzeugten Sprachen sind eine echte Teilmenge
der von *LL(k+1)*-Grammatiken erzeugten Sprachen.

Für eine kontextfreie Grammatik *G* ist nicht entscheidbar, ob es eine
*LL(1)* - Grammatik *G'* gibt mit $L(G) = L(G')$.

In der Praxis reichen $LL(1)$ - Grammatiken oft. Hier gibt es effiziente
Parsergeneratoren (hier: ANTLR), deren Eingabe eine LL(k)- (meist
LL(1)-) Grammatik ist, und die als Ausgabe den Quellcode eines
(effizienten) tabellengesteuerten Parsers generieren.

###### Algorithmus: Konstruktion einer LL-Parsertabelle

**Eingabe:** Eine Grammatik G = (N, T, P, S)

**Ausgabe:** Eine Parsertabelle *P*

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/LL-Parsertabelle_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/LL-Parsertabelle.png" width="60%" /></picture></p><p align="center">Algorithmus zur Generierung einer LL-Parsertabelle</p>

Hier ist $\perp$ das Endezeichen des Inputs. Statt $First_1(\alpha)$
wird oft nur $First(\alpha)$ geschrieben.

###### LL-Parsertabellen

Hier entsteht ein Tafelbild.

###### LL-Parsertabellen

Rekursive Programmierung bedeutet, dass das Laufzeitsystem einen Stack
benutzt. Diesen Stack kann man auch "selbst programmieren", d. h. einen
PDA implementieren. Dabei wird ebenfalls die oben genannte Tabelle zur
Bestimmung der nächsten anzuwendenden Produktion benutzt. Der Stack
enthält die zu erwartenden Eingabezeichen, wenn immer eine
Linksableitung gebildet wird. Diese Zeichen im Stack werden mit dem
Input gematcht.

###### Algorithmus: Tabellengesteuertes LL-Parsen mit einem PDA

**Eingabe:** Eine Grammatik G = (N, T, P, S), eine Parsertabelle *P* mit
"$w\perp$" als initialem Kellerinhalt

**Ausgabe:** Wenn $w \in L(G)$, eine Linksableitung von $w$, Fehler
sonst

<p align="center"><picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/LL-Parser_inv.png" /><img src="https://raw.githubusercontent.com/Compiler-CampusMinden/CB-Vorlesung-Bachelor/_w26/lecture/02-parsing/images/LL-Parser.png" width="49%" /></picture></p><p align="center">Algorithmus zum tabellengesteuerten LL-Parsen</p>

###### Ergebnisse der Syntaxanalyse

-   eventuelle Syntaxfehler mit Angabe der Fehlerart und des -Ortes
-   Fehlerkorrektur
-   Format für die Weiterverarbeitung:
    -   Ableitungsbaum oder Syntaxbaum oder Parse Tree
    -   abstrakter Syntaxbaum (AST): Der Parse Tree ohne Symbole, die
        nach der Syntaxanalyse inhaltlich irrelevant sind (z. B. ;,
        Klammern, manche Schlüsselwörter, $\ldots$)
-   Symboltabelle

##### Wrap-Up

###### Wrap-Up

-   Syntaxanalyse wird mit deterministisch kontextfreien Grammatiken
    durchgeführt.
-   Eine Teilmenge der dazu gehörigen Sprachen lässt sich top-down
    parsen.
-   Ein effizienter LL(k)-Parser realisiert einen DPDA und kann
    automatisch aus einer LL(k)-Grammatik generiert werden.
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
> -   k2: Ich kann den algorithmischen Ablauf von LL-Parsern an einem
>     Beispiel erklären
>
> </details>

<a id="id-1367160fc9ac7c83938c632f3adee2171857a427"></a>

### Sprache L-Int: Integer, Addition, Subtraktion

##### Teil 1:

-   konkrete Syntax (Grammatik), abstrakte Syntax (AST)
-   AST, InterpreterLInt

##### Teil 2:

-   Wiederholung ANTLR, Visitor mit Rückgabe (zustandslos),
    ANTLR-Grammatik
-   Handgeschriebener Lexer, RD-Parser

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
-   Klassen als Environment für Methoden, Instanzen als Environment für
    Felder, Auflöse-Strategien
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

#### A1.1: Sprachen von regulären Ausdrücken (1P)

Welche Sprache wird von dem folgenden regulären Ausdruck beschrieben?

$a\ +\ a\ (a\ +\ b)^*\ a$

#### A1.2: Bezeichner in Programmiersprachen (3P)

Betrachten Sie eine Programmiersprache, in der die Bezeichner (= Namen
für Variablen, Funktionen, Klassen, Methoden, ...) folgenden Aufbau
haben:

-   Alle Variablennamen beginnen mit **V** oder **v**
-   Handelt es sich um globale Variablen, beginnen Sie mit **V**, lokale
    beginnen mit **v**
-   Funktions- und Methodenparameter beginnen mit **p**,
    KLassenparameter (bei der Definition von Vererbung) beginnen mit
    **P**
-   Weitere Bezeichner müssen mit einem Buchstaben (a-z, A-Z) beginnen
-   Die folgenden Zeichen dürfen Buchstaben, Ziffern und ein Untersreich
    sein
-   Bezeichner dürfen nicht mit einem Unterstrich enden
-   Alle Bezeichner müssen aus mindestens zwei Zeichen bestehen

Entwickeln Sie einen regulären Ausdruck, der den Aufbau der Bezeichner
beschreibt. Beachten Sie, dass Ihr regex alle zulässigen Bezeichner
beschreiben muss, aber keinen einzigen unzulässigen beschreiben darf.
Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie, wie sie vom
regex gematcht werden.

Entwickeln Sie einen DFA, der diese Bezeichner akzeptiert. Beachten Sie,
dass Ihr DFA alle zulässigen Bezeichner akzeptieren muss, aber keinen
einzigen unzulässigen akzeptieren darf. Wählen Sie zwei Bezeichner aus
der Sprache und zeigen Sie, wie sie vom Automaten zeichenweise gelesen
und akzeptiert werden.

Entwickeln Sie eine reguläre Grammatik, die diese Bezeichner generiert.
Beachten Sie, dass Ihre Grammatik alle zulässigen Bezeichner generieren
können muss, aber keinen einzigen unzulässigen generieren darf. Wählen
Sie zwei Bezeichner aus der Sprache und zeigen Sie die Ableitungsbäume
dazu.

#### A1.3: Gleitkommazahlen in Programmiersprachen (2P)

Recherchieren Sie zunächst den Aufbau von Gleitkommazahlen in Python und
Java.

Erstellen Sie für jede der beiden Programmiersprachen reguläre
Ausdrücke, DFAs und reguläre Grammatiken wie in Aufgabe A1.2.
Verifizieren Sie Ihre Lösungen wie in Aufgabe A1.2. Vorgaben, die sich
auf Längen oder Werte von Teilen der Zahlen beziehen, ignorieren Sie
bitte.

#### A1.4: Mailadressen? (1P)

Warum ist der folgende regex ungeeignet für die Verarbeitung von
Mailadressen?

$(a-z)^+@(a-z).(a-z)$

Bitte beachten Sie, dass die Schreibweise a-z nicht unserer Definition
genügt. Eigentlich müsste jedes Zeichen aufgeführt werden:

$a + b + c + c + \ldots + z$ ist besser, aber immer noch nicht richtig.
Warum?

Anmerkung: Diese Darstellung wird ab jetzt akzeptiert.

Verbessern Sie den gegebenen regulären Ausdruck.

#### A1.5: Der zweitletzte Buchstabe (1P)

Entwickeln Sie einen DFA, der nur Wörter über
$\Sigma = \lbrace 1,2,3 \rbrace$ akzeptiert, deren zweitletztes Zeichen
dasselbe ist wie das zweite.

#### A1.6: Sprache einer regulären Grammatik (2P)

Welche Sprache generiert die folgende Grammatik?

$$\begin{eqnarray}
S &\rightarrow& a A                      \nonumber \\
A &\rightarrow& d B \ | \ b A \ | \ c A  \nonumber \\
B &\rightarrow& a C \ | \ b C \ | \ c A  \nonumber \\
C &\rightarrow& \epsilon                 \nonumber
\end{eqnarray}$$

Können Sie einen regulären Ausdruck oder einen DFA dafür angeben?

<a id="id-0db349230022c35e045dc3b052a4faea50fe5f40"></a>

### Blatt 02: CFG

#### A2.1: PDA (3P)

Erstellen Sie einen deterministischen PDA, der die Sprache

$$L = \lbrace w \in \lbrace a, b, c \rbrace^* \; | \; w \; \text{hat doppelt so viele a's wie c's} \rbrace$$

akzeptiert.

Beschreiben Sie Schritt für Schritt, wie der PDA die Eingaben *bcaba*
und *bccac* abarbeitet.

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

Zeichnen Sie den Automaten. Geben Sie das 7-Tupel des PDa an. Welche
Sprache akzeptiert er?

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

Zeigen Sie, dass die Grammatik mehrdeutig ist. Entwickeln Sie einen PDA
für diese Sprache.

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
> Aho, A. V., M. S. Lam, R. Sethi, J. D. Ullman, und S. Bansal. 2023.
> *Compilers: Principles, Techniques, and Tools, Updated 2nd Edition by
> Pearson*. Pearson India.
> <https://learning.oreilly.com/library/view/compilers-principles-techniques/9789357054881/>.
>
> </div>
>
> <div id="ref-hopcroft2003" class="csl-entry">
>
> Hopcroft, J. E., R. Motwani, und J. D. Ullman. 2003. *Einführung in
> die Automatentheorie, formale Sprachen und Komplexitätstheorie*. I
> theoretische informatik. Pearson Education Deutschland GmbH.
>
> </div>
>
> <div id="ref-Parr2014" class="csl-entry">
>
> Parr, T. 2014. *The Definitive ANTLR 4 Reference*. Pragmatic
> Bookshelf.
> <https://learning.oreilly.com/library/view/the-definitive-antlr/9781941222621/>.
>
> </div>
>
> <div id="ref-Torczon2012" class="csl-entry">
>
> Torczon, L., und K. Cooper. 2012. *Engineering a Compiler*. Morgan
> Kaufmann.
> <https://learning.oreilly.com/library/view/engineering-a-compiler/9780080916613/>.
>
> </div>
>
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 6ecf3d4 2026-09-04 orga: fix deadline for homework<br></sub></sup></p></blockquote>
