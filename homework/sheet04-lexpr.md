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

## A4.1: L-Expr: Division ergänzen (\*)

Im Skript haben Sie L-Expr als Erweiterung von L-Int um Multiplikation und Division kennengelernt. In der Vorlesung wurde die Multiplikation im Detail behandelt.

Wenden Sie das gezeigte Muster auf die Division an und skizzieren Sie in Java, wie die Auswertung einer `DivExpr` aussehen könnte. Geben Sie dabei an, welche Teil-Ausdrücke ausgewertet und wie ihre Ergebnisse kombiniert werden.

Gegeben sei dazu der folgende Code-Rahmen.

<details>

``` java
public interface Expr {}
public record IntLiteral(int value) implements Expr {}
public record AddExpr(Expr left, Expr right) implements Expr {}
public record SubExpr(Expr left, Expr right) implements Expr {}
public record MulExpr(Expr left, Expr right) implements Expr {}
public record DivExpr(Expr left, Expr right) implements Expr {}

public interface Value {
    static int asInt(Value v) {
        if (v instanceof IntVal(int val)) return val;
        throw new RuntimeException("number expected");
    }
}
public record IntVal(int v) implements Value {}

public class InterpreterLint {
    public Value eval(Expr expr) {
        return switch (expr) {
            case IntLiteral(var v) -> new IntVal(v);

            case AddExpr(var l, var r) -> {
                int a = Value.asInt(eval(l));
                int b = Value.asInt(eval(r));
                yield new IntVal(a + b);
            }

            // TODO: SubExpr

            default -> throw new RuntimeException("unexpected expression: " + expr);
        };
    }
}


public class InterpreterLexpr extends InterpreterLint {

    @Override
    public Value eval(Expr expr) {
        return switch (expr) {
            case MulExpr(var l, var r) -> {
                int a = Value.asInt(eval(l));
                int b = Value.asInt(eval(r));
                yield new IntVal(a * b);
            }

            // TODO: DivExpr

            default -> super.eval(expr);
        };
    }
}
```

</details>

Weitere Aufgabenteile:

1.  Wie behandeln Sie Vorrangregeln und die Assoziativität im Interpreter für L-Expr?
2.  Notieren Sie, wie der mehrstellige Ausdruck `(/ 12 3 2)` gruppiert wird und welches Ergebnis er nach dieser Gruppierung hat.
3.  Wie wird die Division durch null behandelt? Brauchen wir hier eine explizite Fehlerbehandlung im Interpreter?
4.  Erstellen Sie außerdem mindestens zwei Testfälle:
    -   einen Test, der die Java-Integer-Division sichtbar macht
    -   einen Test für eine verschachtelte Division

## A4.2: C-Expr: Vorrangregeln in einer gestuften Grammatik (\*\*)

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

Definieren Sie nun ausgehend von der konkrete Grammatik aus der vorigen Aufgabe zwei **ANTLR-Grammatiken** für **C-Expr**:

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
5.  Testen Sie die neue C-Expr-Pipeline: Implementieren Sie die **obigen Beispiele in JUnit** und führen Sie diese auf **allen drei Varianten** aus (RD-Parser, ANTLR mit gestuften Regeln, ANTLR mit linkrekursiven Alternativen). Was beobachten Sie?

> [!TIP]
>
> **Hinweis**: Sie können die Funktionalität aus dem Lexer und RD-Parser für C-Int als Basis per Copy&Paste übernehmen und für C-Expr ergänzen/umbauen. Die gestuften Regeln vertragen sich nicht gut mit dem sonst in diesem Kurs praktizierten Ableiten `class RdParserCexpr extends RdParserCint`, weshalb wir hier an dieser Stelle darauf verzichten.

### Schritt 6: Interpreter-Pipeline für C-Expr

Implementieren Sie nun den **Interpreter für C-Expr** in einer Klasse `InterpreterCexpr`. Dabei soll wie üblich in diesem Kurs gelten `class InterpreterCexpr extends InterpreterCint`.

Ergänzen Sie die neuen Fälle für C-Expr durch Überschreiben von `eval()`. Rufen Sie für die bisherigen C-Int-Fälle `super.eval(expr)` auf.

Testen Sie Ihren Interpreter mit JUnit mit den obigen Beispielen. Dabei soll wie bei L-Expr gelten: `class InterpreterCexprTest extends InterpreterCintTest`.

> [!IMPORTANT]
>
> Der Interpreter bekommt als Eingabe den AST und ist damit unabhängig vom konkreten Parser!
>
> Testen Sie Ihren Interpreter sowohl mit dem handgeschriebenen RD-Parser als auch mit den beiden ANTLR-Parsern plus AST-Buildern.

Vergleichen Sie die Resultate der Testfälle von `InterpreterCexpr` mit dem mit g++/clang übersetzten nativen C++-Code. Gibt es Unterschiede? Woran liegt das?

## A4.3: Vertiefung: ANTLR und Stolperfallen (\*\*\*)

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

<blockquote><p><sup><sub><strong>Last modified:</strong> 8709fc6 2026-11-11 homework: improve wording (b04)<br></sub></sup></p></blockquote>
