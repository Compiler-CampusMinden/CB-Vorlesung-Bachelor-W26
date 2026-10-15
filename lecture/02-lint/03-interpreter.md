# L-Int: Von Ausdrücken zum Interpreter

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

## Von AST zu Werten: Idee

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

## Value-Hierarchie: Motivation

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

## Value-Hierarchie in Java

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

## Tree-Walking-Interpreter: Grundidee

-   Funktion: `Value eval(Expr expr)`

<!-- -->

-   Idee:
    -   Rekursiv über den AST laufen
    -   Pro Knotentyp die passende Operation ausführen:
        -   Literal (Integer): Als Integer-Wert zurückgeben
        -   Ausdruck (Addition, Subtraktion): linke und rechte Seite zu Integer-Werten evaluieren, die Operation ausführen, Ergebnis als Integerwert zurückgeben

Der Tree-Walking-Interpreter ist im Kern eine rekursive Funktion über dem AST. Für jeden Knotentyp - Literal, Addition, Subtraktion - definieren wir, wie er auszuwerten ist. Das ist eine direkte Implementierung der operationalen Semantik unserer Sprache: Wir sagen dem Rechner Schritt für Schritt, wie die Auswertung funktioniert.

## Interpreter für Expressions in Java (Pattern Matching)

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

### Literale auswerten

Das ist der einfachste Teil ... Die primitiven Typen der Zielsprache Lispy müssen als Datentyp der Interpreter-Programmiersprache Java ausgewertet werden.

In der abstrakten Grammatik hatten wir definiert:

``` ebnf
Expr  = IntLiteral(Int)
```

Im AST werden also Knoten vom Typ `IntLiteral` angelegt, um die Integer-Literale zu repräsentieren.

Für jedes `IntLiteral` erzeugen wir nun im L-Int-Interpreter ein `IntVal`.

### Ausdrücke auswerten

Für Ausdrücke hatten wir in der abstrakten Grammatik für L-Int definiert:

``` ebnf
Expr  = AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
```

Wir bekommen im AST also Knoten vom Typ `AddExpr` für eine Addition bzw. vom Typ `SubExpr` für eine Subtraktion. Beide Knoten haben jeweils ein linkes und ein rechtes Kind vom Typ `Expr`.

Für `AddExpr` und `SubExpr` werten wir zunächst die beiden Operanden rekursiv aus und kombinieren die Ergebnisse passend. Das entspricht genau der mathematischen Intuition von Addition und Subtraktion, nur eben explizit im Code ausgedrückt.

**Wichtig**: Die Subtraktion (Auswertung von `SubExpr`) fehlt hier und dient als kleine Übung.

> [!IMPORTANT]
>
> ### Fehlerbehandlung und Fehlermeldungen
>
> Die meisten möglichen Fehlerzustände sind bereits durch den Lexer und Parser (und in späteren Sprachstufen während der semantischen Analyse) abgefangen worden. Falls zur Laufzeit die Auswertung der beiden Summanden keine Zahl ergibt, würde eine Java-Exception geworfen, die der Aufrufer an geeigneter Stelle fangen und behandeln muss. Der Interpreter soll sich ja nicht mit einem Stack-Trace verabschieden, sondern soll eine Fehlermeldung präsentieren und danach normal weiter machen ...
>
> Die hier gezeigte Fehlerbehandlung ist nur sehr rudimentär, um nicht zu sehr von den Kernkonzepten für unsere Veranstaltung abzulenken. In der Praxis würde man hier deutlich komplexere Fehlerbehandlung machen, beispielsweise auf die Zeile und Spalte in der Eingabe hinweisen. Gute Fehlerbehandlung ist eine Wissenschaft für sich. Vergleichen Sie einmal die Fehler, die Python ausgibt, und die Meldungen beispielsweise von Rust.

> [!TIP]
>
> ### REPL: Read-Eval-Print-Loop
>
> In den Beispielen in dieser Veranstaltung wird immer davon ausgegangen, dass ein komplettes Programm eingelesen, geparst, vorverarbeitet und dann interpretiert wird.
>
> Für einen interaktiven Interpreter würde man in einer Schleife die Eingaben lesen, parsen und vorverarbeiten und dann interpretieren. Dabei würden jeweils der AST und die Symboltabelle und das Environment *ergänzt*, damit die neuen Eingaben auf frühere verarbeitete Eingaben zurückgreifen können. Durch die Form der Schleife "Einlesen - Verarbeiten - Auswerten" hat sich auch der Name "*Read-Eval-Loop*" bzw. "*Read-Eval-Print-Loop*" (**REPL**) eingebürgert.

> [!TIP]
>
> ### Exkurs Expressions (Ausdrücke) vs. Statements (Anweisungen)
>
> In Programmiersprachen unterscheiden wir häufig **Expressions** (*Ausdrücke*) und **Statements** (*Anweisungen*).
>
> Expressions sind dabei syntaktische Konstrukte einer Programmiersprache, die (in einem gegebenen Kontext) zu einem Wert **evaluiert** werden können. Typische Expressions sind beispielsweise Ausdrücke wie `(+ 2 3)` oder `(foo 42)` ... In manchen Sprachen sind beispielsweise auch Zuweisungen Expressions: `v = 42 + 7;` würde in C der Variablen `v` den Wert 49 zuweisen, dies ist gleichzeitig auch der Wert des gesamten Ausdrucks. Man könnte in C also Dinge formulieren wie `if (v = 42 + 7) ...` (wobei das Interpretieren eines Integers in einem bool'schen Kontext nochmal ein anderes Problem ist).
>
> Statements sind syntaktische Konstrukte in Programmiersprachen, die **ausgeführt** werden können und dabei in der Regel einen Zustand im Programm verändern, also einen Seiteneffekt haben. Die Ausführung eines Statements hat normalerweise keinen Wert an sich. Typische Beispiele sind Zuweisungen `v = 7`, Kontrollfluss `if (...) then {...} else {...}`, Schleifen `for x in foo: ...`, `switch/case`-Statements. (Es gibt aber auch Programmiersprachen, wo ein `if/then/else`-Konstrukt eine Expression ist, also bei der Ausführung einen Wert ergibt.) Expressions können in den meisten Programmiersprachen Teile von Statements bilden: In `v = 42 + 7` ist die gesamte Zuweisung eine Anweisung (Seiteneffekt: die Variable `v` hat danach einen anderen Zustand), und der Teil `42 + 7` ist ein Ausdruck, der ausgewertet werden kann und üblicherweise den Wert 49 ergibt (außer man beauftragt ein LLM mit der Auswertung). In C-ähnlichen Sprachen kann durch Hinzufügen eines Semikolons aus dem Ausdruck `42 +7` eine Anweisung gemacht werden ...
>
> Vergleiche auch ([Nystrom 2021](#ref-Nystrom2021)), Kapitel 6 "Parsing Expressions", Kapitel 7 "Evaluating Expressions" und Kapitel 8 "Statements and State", aber auch [Wikipedia: Expression](https://en.wikipedia.org/wiki/Expression_(computer_science)) und [Wikipedia: Statement](https://en.wikipedia.org/wiki/Statement_(computer_science)). Wir werden uns an späterer Stelle noch darum kümmern.

## Interpreter in Aktion

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

## Wrap-up

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

<blockquote><p><sup><sub><strong>Last modified:</strong> c8fc057 2026-10-15 lint interpreter: improve wording<br></sub></sup></p></blockquote>
