# Blatt 04: L-Expr und C-Expr (Multiplikation/Division, Vorrang und ANTLR)

> [!IMPORTANT]
>
> Für die erfolgreiche Bearbeitung dieses Übungsblattes müssen mindestens alle mit `*` und `**` markierten Aufgaben gelöst werden.
>
> Die Aufgaben mit `***` sind als Vertiefung gedacht.

Dieses Blatt knüpft an folgende Vorlesungseinheiten an:

-   L-Expr: Multiplikation/Division und unäres Minus
-   L-Expr: Vorrang und Assoziativität
-   L-Expr: ANTLR-Grammatik und Vorrangregeln
-   L-Expr: ANTLR-Visitoren und Rückgabewerte

## (\*) L-Expr: Multiplikation und Division ergänzen

Sie haben in der Vorlesung zunächst L-Int (Integer-Literale, Addition, Subtraktion in Lispy-Notation) und anschließend L-Expr als Erweiterung von L-Int um Multiplikation und Division kennengelernt.

In dieser Aufgabe sollen Sie die in der Vorlesung gezeigten Ideen vervollständigen und auf Ihre L-Int-Pipeline übertragen und so Ihre L-Expr-Pipeline aufbauen.

1.  **Konkrete Syntax verstehen**

    Klären Sie in der Übung gemeinsam:

    -   Welche Ausdrücke sind **gültige** L-Expr-Ausdrücke?

    -   Wie werden mehrstellige Multiplikationen und Divisionen gruppiert (Assoziativität)?

    -   Welche Werte ergeben die folgenden Ausdrücke?

        ``` clojure
        (* 2 3)
        (/ 8 2)
        (* 2 3 4)
        (/ 12 3 2)
        (* (+ 1 2) 3)
        (/ 12 (* 2 2))
        ```

2.  **Ungültige Eingaben**

    Notieren Sie mindestens zwei **ungültige** L-Expr-Eingaben, z.B.:

    -   Operationen mit weniger als zwei Operanden,
    -   falsche Klammerung,
    -   unerwartete Token.

    Beschreiben Sie jeweils kurz, **warum** die Eingabe ungültig ist und an welcher Stelle Ihre Pipeline (Lexer, Parser, AST-Bau, Interpreter) sinnvollerweise einen Fehler melden würde.

3.  **L-Expr-Pipeline bauen: L-Int-Pipeline um Multiplikation/Division erweitern**

    Erstellen Sie nun Ihre **Pipeline für L-Expr**, indem Sie ein neues Package für L-Expr anlegen und die nötigen Klassen und Ergänzungen **auf Basis Ihrer L-Int-Lösung** definieren:

    -   **Lexer**: neue Token für `*` und `/`
    -   Parser (**RD-Parser** für L-Expr): zusätzliche Alternativen in den passenden Grammatikregeln
    -   **AST**: neue Knotenklassen `MulExpr` und `DivExpr`
    -   **Interpreter** `InterpreterLexpr`: Auswertung von `MulExpr` und `DivExpr` (linksassoziativ, Java-Integer-Division)

    Ergänzen Sie Ihr Token-Typ-Enum. Ergänzen Sie die neuen AST-Klassen. Schreiben Sie neue Klassen für den L-Expr-Lexer und -RD-Parser (ausnahmsweise **nicht** von L-Int ableiten - warum?!).

    Definieren Sie eine neue Klasse `InterpreterLexpr` für den L-Expr-Interpreter mit `class InterpreterLexpr extends InterpreterLint`.

    Implementieren Sie die obigen Testausdrücke in einer neuen JUnit-Testklasse `InterpreterLexprTest`, die von der L-Int-Testklasse ableitet: `class InterpreterLexprTest extends InterpreterLintTest`. Wichtig: Damit das funktioniert, müssen alle Testmethoden mindestens auf `protected` stehen.

## (\*\*) C-Expr: Vorrangregeln in einer gestuften Grammatik

Wir erstellen nun **C-Expr**, indem wir **C-Int vom letzten Blatt** um Multiplikation, Division, unäres Minus und geklammerte Ausdrücke erweitern.

C-Expr soll analog zu L-Expr **Multiplikation und Division und unäres Minus und geklammerte Ausdrücke** erlauben, aber in C++-üblicher Infix-Notation und unter Beachtung der üblichen Vorrang- und Assoziativitätsregeln.

> [!IMPORTANT]
>
> ### Waschzettel: In C++ sieht es so aus ...
>
> | Zweck          | L-Expr (Lispy)  | C-Expr (Mini-C++) |
> |:---------------|:----------------|:------------------|
> | Multiplikation | `(* 2 3)`       | `2 * 3`           |
> | Division       | `(/ 8 3)`       | `8 / 3`           |
> | Unäres Minus   | `(- 3)`         | `-3`              |
> | Klammerung     | `(* (+ 1 2) 3)` | `(1 + 2) * 3`     |
>
> Während in Lispy (L-Int, L-Expr) die Operatoren für `+`, `-`, `*`, `/` mehrstellig sein können, sind sie in C++ (C-Int, C-Expr) **binär** (bzw. `-` zusätzlich unär).

### Schritt 1: C-Expr ausprobieren: Vorrang und unäres Minus live testen

Wir wollen zunächst die Vorrangregeln und unäres Minus **direkt in C++** ausprobieren.

Schreiben Sie ein C++-Programm `expr.cpp`, das folgende Ausdrücke auswertet und jeweils das Ergebnis über `std::cout` ausgibt:

    1 + 2 * 3
    (1 + 2) * 3
    8 / 4 / 2
    (8 / 4) / 2
    8 / (4 / 2)
    7 - 2 - 1
    7 - (2 - 1)
    -3 * 4
    -(3 * 4)
    -(1 + 2) * 3

Übersetzen Sie es mit g++/clang und prüfen Sie, ob jeweils das Ergebnis Ihren Erwartungen entspricht.

### Schritt 2: Konkrete Syntax für C-Expr (gestufte Variante)

Definieren Sie eine **konkrete Grammatik für C-Expr** (in EBNF), die folgende Anforderungen erfüllt:

-   Zulässige Ausdrücke enthalten:
    -   Integer-Literale
    -   binäre Operatoren `+`, `-`, `*`, `/`
    -   unäres Minus: `-x`
    -   Klammern `(` und `)`
-   Vorrang:
    -   `*` und `/` **binden stärker** als `+` und `-`
    -   Unäres Minus und geklammerte Ausdrücke haben höchsten Vorrang
-   Alle vier Operatoren sind **linksassoziativ**
-   Whitespace darf an beliebigen sinnvollen Stellen stehen und wird ignoriert
-   Zeilenkommentare werden ab dem `//` bis zum Zeilenende ignoriert

Skizzieren Sie Ihre Grammatik ausdrücklich mit **gestuften Regeln**, z.B. mit den Ebenen `expr`/`term`/`atom`.

Überprüfen Sie mit Ihrer Grammatik:

-   Wie wird `1 + 2 * 3` geparst und ausgewertet?
-   Wie wird `8 / 4 / 2` geparst und ausgewertet?
-   Wie wird `-3 * 4` geparst und ausgewertet?
-   Wie werden Klammern berücksichtigt, z.B. `(1 + 2) * 3`, `-(1 + 2) * 3`?

Zeichnen Sie jeweils den Parse-Tree. Entspricht das dem Verhalten von C++ aus der vorigen Aufgabe?

### Schritt 3: ANTLR-Grammatiken für C-Expr

Definieren Sie zwei **ANTLR-Grammatiken** für **C-Expr**:

-   einmal mit **gestuften Regeln**, und
-   einmal mit **linksrekursiven Alternativen**.

Berücksichtigen Sie dabei die für C-Expr formulierten Operatoren und Vorrangregeln und Assoziativitäten.

### Schritt 4: Abstrakte Syntax für C-Expr

Definieren Sie nun die **abstrakte Syntax für C-Expr**. Orientieren Sie sich an der Vorlesung und bauen Sie auf Ihrer abstrakten Syntax von C-Int auf.

Diskutieren Sie: Welche neuen AST-Knoten sind notwendig und warum?

### Schritt 5: Parsen von C-Expr und bauen des AST

1.  Implementieren Sie für Ihre konkrete Grammatik (mit den gestuften Regeln) für **C-Expr einen Lexer**. Erweitern Sie dazu die Tokendefinitionen passend.
2.  Implementieren Sie einen **RD-Parser für C-Expr**, der als Ausgabe einen AST erzeugt. Ergänzen Sie dazu die AST-Klassen entsprechend Ihrer abstrakten Syntax.
3.  Lassen Sie sich von **ANTLR** für die beiden Grammatiken aus der vorigen Aufgabe je einen **Lexer, Parser und Visitor generieren**.
4.  Erstellen Sie **zwei AST-Builder**, die aus den beiden unterschiedlichen ANTLR-Parse-Trees je einen AST entsprechend Ihrer abstrakten Syntax erzeugen. Nutzen Sie das **Visitor-Pattern**. Diskutieren Sie:
    -   Welchen generischen Typ brauchen Sie für das Ableiten des generierten `XyzBaseVisitor`?
    -   Welche der `visitXYZ()`-Methoden sollten Sie überschreiben, welche nicht? Begründen Sie Ihre Aussage.
5.  Implementieren Sie die **obigen Testfälle in JUnit** und führen Sie diese auf **allen drei Varianten** aus (RD-Parser, ANTLR mit gestuften Regeln, ANTLR mit linkrekursiven Alternativen). Was beobachten Sie?

> [!TIP]
>
> **Hinweis**: Sie können die Funktionalität aus dem Lexer und RD-Parser für C-Int als Basis per Copy&Paste übernehmen und für C-Expr ergänzen/umbauen. Die gestuften Regeln vertragen sich nicht gut mit dem sonst in diesem Kurs praktizierten Ableiten `class RdParserCexpr extends RdParserCint`, weshalb wir hier an dieser Stelle darauf verzichten.

### Schritt 6: Interpreter-Pipeline für C-Expr

Implementieren Sie nun den **Interpreter für C-Expr** in einer Klasse `InterpreterCexpr`. Dabei soll wie üblich in diesem Kurs gelten `class InterpreterCexpr extends InterpreterCint`.

Ergänzen Sie die neuen Fälle für C-Expr durch Überschreiben von `eval()`. Rufen Sie für die bisherigen C-Int-Fälle `super.eval(expr)` auf.

Testen Sie Ihren Interpreter mit JUnit mit den obigen Testfällen. Dabei soll wie bei L-Expr gelten: `class InterpreterCexprTest extends InterpreterCintTest`. Spielt es eine Rolle, ob Sie dafür den handgeschriebenen Lexer/RD-Parser oder einen der beiden ANTLR-Parser plus den passenden AST-Builder einsetzen?

Vergleichen Sie die Resultate der Testfälle von `InterpreterCexpr` mit dem mit g++/clang übersetzten nativen C++-Code. Gibt es Unterschiede? Woran liegt das?

## (\*\*\*) Vertiefung: ANTLR und Stolperfallen

Betrachten Sie die folgende ANTLR-Grammatik:

``` antlr
foo  :  (ID '(' ')' '=' '0' | ID | NUM) EOF ;

ID      : [a-z][a-zA-Z]* ;
NUM     : [0-9]+ ;
WS      : [ \t\n]+ -> skip ;
```

Was passiert bei der Eingabe von `10` und `01` und `a() = 0`, wie sieht der Parse-Tree aus? Was passiert bei der Eingabe von `0`?

Erklären Sie die Resultate.

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 24134dd 2026-10-28 homework: improve tasks (sheet 04 l-expr)<br></sub></sup></p></blockquote>
