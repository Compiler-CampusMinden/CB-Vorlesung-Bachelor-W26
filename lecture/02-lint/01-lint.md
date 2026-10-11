# L-Int: Die Sprache Lispy

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Lispy ist eine für diese Lehrveranstaltung frei erfundene Lisp-ähnliche Programmiersprache. Ihre Ausdrücke werden als S-Expressions in einer Klammer- und Präfixnotation geschrieben.
>
> L-Int ist dabei die Grundstufe von Lipsy, die lediglich Integer-Literale sowie Addition und Subtraktion umfasst. Ein Integer-Literal ist selbst ein Ausdruck, während eine Liste wie `(+ 1 2)` einen Operator und seine Operanden enthält.
>
> Die Syntax legt fest, welche Ausdrücke gültig sind; die Semantik legt fest, welchen Wert ein gültiger Ausdruck ergibt.
>
> Mehrere Operanden können bei der Addition oder Subtraktion als "syntaktischer Zucker" für verschachtelte binäre Ausdrücke behandelt werden.
>
> Kommentare beginnen mit `;;` und reichen bis zum Ende der Zeile. L-Int bildet die Grundlage, um schrittweise Grammatik, Lexer, Parser, Syntaxbaum, AST und Interpreter zu entwickeln.
>
> </details>

## Problem: Wie verstehen wir `1 + 2 - 3`?

-   Eingabe: Text wie `1 + 2 - 3`
-   Ziel: Ergebnis berechnen
-   Frage:
    -   Wie modellieren wir die Struktur solcher Ausdrücke?
    -   Wie bringen wir den Rechner dazu, "richtig" zu rechnen?

Starten wir mit einem bewusst einfachen Beispiel: `1 + 2 - 3`. Das kennen alle, aber wir tun jetzt so, als wüssten wir noch nicht, wie ein Rechner das versteht. Die beiden Kernfragen sind: Wie sieht die Struktur dieses Ausdrucks aus? Und wie können wir die Bedeutung so modellieren, dass wir sie programmatisch auswerten können?

Das wird uns durch die gesamte Vorlesung begleiten: **Vom Text (Eingabe) über Struktur (Syntax) hin zur Bedeutung (Semantik)**.

> [!IMPORTANT]
>
> ### Lehrkonzept
>
> Wir werden im Laufe der Veranstaltung **inkrementell** vorgehen und mit einer minimalen Sprache L-Int (nur Integer, Addition und Subtraktion) starten. Für diese Sprache werden wir die Syntax in Form von Grammatiken festlegen, Lexer und Parser bauen und aus einer Eingabe einen Syntax-Tree (Parse-Tree) und daraus einen abstrakten Syntax-Tree (AST) erzeugen und diesen schließlich mit einem Interpreter evaluieren. (Ein konkreter Syntaxbaum beziehungsweise Parse-Tree bildet die Struktur der konkreten Grammatik ab; der AST lässt syntaktische Details weg, die für die weitere Verarbeitung nicht benötigt werden.) In den Folgewochen erweitern wir L-Int schrittweise um weitere Konzepte. Wir leiten die Java-Implementierung einer Sprachstufe von der vorherigen ab und überschreiben die Methoden, deren Verhalten sich ändert. Mit `super.xyz()` rufen wir die Implementierung der Oberklasse auf, d.h. rufen so das Verhalten der Vorgängerversion ab ohne es erneut zu implementieren.
>
> Wir orientieren uns damit grob an dem inkrementellen Vorgehen in ([Siek 2023](#ref-Siek2023python)). Konzeptionell/didaktisch lassen wir uns bei der Modellierung der Bausteine in vielen Fällen ungefähr von der Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) inspirieren. Es werden sich also immer wieder Hinweise auf diese beiden Werke als Vertiefung finden.

## S-Expressions als syntaktische Grundform

Ein Programm in L-Int besteht aus einem Ausdruck (*Expression*). Die Ausdrücke haben eine spezielle Form: Sie sind sogenannte [**S-Expressions**](https://en.wikipedia.org/wiki/S-expression).

Eine S-Expression ist:

-   ein Atom der Form `x`, oder
-   ein Ausdruck der Form `(. x y)` (mit `x` u. `y` S-Expressions, `.` eine Operation oder Funktion oder ein Keyword).

> [!TIP]
>
> ### Variadische S-Expressions
>
> Die Anzahl der S-Expressions in einem Klammer-Ausdruck ist nicht näher definiert - `x` und `y` sind nur Beispiele. Es könnten auch mehr oder weniger S-Expressions nach der Operation/Funktion/Keyword auftauchen (vgl. nachfolgende Sprachdefinition).

> [!IMPORTANT]
>
> ### Syntax vs. Semantik
>
> Die Syntax legt fest, welche Ausdrücke gültig sind; die Semantik legt fest, welchen Wert ein gültiger Ausdruck ergibt.
>
> Wir werden zur Beschreibung der Syntax verschiedene Grammatiken entwickeln. Die Auswertung wird über eine semantische Analyse und einen Interpreter erfolgen.

In der *Vorlesung* werden wir für unsere aufeinander aufbauenden Sprachfamilien **S-Expressions** nutzen in Anlehnung an Clojure und Lisp. Das vereinfacht das Parsing deutlich und lässt die Konzepte im Skript und den Folien deutlicher hervortreten. Im Praktikum werden wir diese Konzepte auf einen Subdialekt von C++ anwenden und über das Semester hinweg in modernem Java einen Interpreter für C++ schreiben.

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

## L-Int: Erste Sprachstufe

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
> ### Syntactic Sugar
>
> Die Ausdrücke sind implizit von links nach rechts geklammert, d.h. der Ausdruck `(+ 1 2 3 4)` ist [*syntactic sugar*](https://en.wikipedia.org/wiki/Syntactic_sugar) für `(+ (+ (+ 1 2) 3) 4)`.

> [!TIP]
>
> ### Statements vs. Expressions
>
> In vielen Sprachen unterscheidet man zwischen **Statements** und **Expressions** (Anweisungen und Ausdrücke). Dabei haben Statements üblicherweise keinen (Rückgabe-) Wert und verändern den Zustand des laufenden Programms (*state*), während Expressions immer einen Wert ergeben.
>
> In unserem L-Int ist zunächst alles eine Expression, d.h. Ausdrücke ergeben bei der Auswertung immer einen Wert. In späteren Sprachstufen werden wir komplexere Ausdrücke und auch Statements über die Listenform bilden.

## Wrap-up

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

------------------------------------------------------------------------

> [!NOTE]
>
> <details >
> <summary><strong>👀 Quellen</strong></summary>
>
> <div id="refs" class="references csl-bib-body hanging-indent">
>
> <div id="ref-Nystrom2021" class="csl-entry">
>
> Nystrom, R. 2021. *Crafting Interpreters*. Genever Benning. <https://github.com/munificent/craftinginterpreters>.
>
> </div>
>
> <div id="ref-Siek2023python" class="csl-entry">
>
> Siek, J. G. 2023. *Essentials of Compilation: An Incremental Approach in Python*. The MIT Press. <https://github.com/IUCompilerCourse/Essentials-of-Compilation>.
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

-   [Clojure.org: Learn Clojure](https://clojure.org/guides/learn/syntax#_structure_vs_semantics)

<blockquote><p><sup><sub><strong>Last modified:</strong> 8ebc733 2026-10-11 lint: add challenges (lispy)<br></sub></sup></p></blockquote>
