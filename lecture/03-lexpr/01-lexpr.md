# L-Expr: Multiplikation/Division und unäres Minus

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Wir erweitern die Sprachstufe L-Int zu L-Expr, indem wir Multiplikation, Division und ein unäres Minus hinzufügen. In der Lispy-Notation bedeutet das vor allem, dass wir der konkreten Grammatik neue Operatorformen `(* e e)`, `(/ e e)` und `(- e)` hinzufügen.
>
> Die abstrakte Syntax (AST) wird um zwei neue Knotentypen `MulExpr` und `DivExpr` erweitert; unäres Minus wird nicht als eigener Knotentyp modelliert, sondern beim AST-Bau auf vorhandene Knoten desugart.
>
> Jede neue Sprachstufe baut auf der vorherigen auf: L-Expr *erweitert* L-Int. Entsprechend leiten in der Implementierung die Bausteine der neuen Stufe von den Bausteinen der alten ab, überschreiben bei Bedarf Methoden und rufen für das bisherige Verhalten `super.XYZ()` auf (z.B. `class InterpreterLexpr extends InterpreterLint`).
>
> Wir verwenden für Division in allen Sprachstufen dieselbe Semantik wie in Java: ganzzahlige Division von `int`-Werten, bei der der Rest verworfen wird.
>
> </details>

## Rückblick: L-Int

-   Konkrete Syntax:

    ``` ebnf
    eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
    INT   ::= [0-9]+
    L_int ::= eform EOF
    ```

-   Abstrakte Syntax:

    ``` ebnf
    Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
    L_int = Expr
    ```

-   AST:

    ``` java
    public interface Expr {}
    public record AddExpr(Expr left, Expr right) implements Expr {}
    public record SubExpr(Expr left, Expr right) implements Expr {}
    public record IntLiteral(int value) implements Expr {}
    ```

## Erweiterung zu L-Expr: Multiplikation und Division

Wir bleiben bei **Integer-Ausdrücken**, fügen aber Multiplikation und Division hinzu.

-   Beispiel:

        (* 2 (+ 3 4))

<!-- -->

-   **Neue konkrete Syntax in Lispy (Stufe L-Expr)**:

    ``` ebnf
    eform  ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
    INT    ::= [0-9]+
    ---

    eform  ::= '(' '*' eform eform+ ')'
             | '(' '/' eform eform+ ')'
             | '(' '-' eform ')'

    L_expr ::= eform EOF
    ```

Die Grundidee in Lispy ist: Wir fügen einfach zwei weitere Alternativen für die `eform`-Regel mit neuen Operatoren hinzu. Semantisch bleiben wir bei Integer-Werten und mehrstelligen Operationen.

> [!IMPORTANT]
>
> ### Hinweis zum Lesen der Regeln
>
> In dem obigen Beispiel wurden über den `---` die bisherigen Regeln aus L-Int übernommen. Unter dem `---` sehen wir die neuen Regeln, die in L-Expr hinzukommen. Dies soll zum einen die Features der neuen Sprachstufe hervorheben. Zum anderen soll es auch deutlich machen, dass L-Expr tatsächlich L-Int plus ein paar neue Fähigkeiten ist.
>
> Für eine formale Grammatik wären normalerweise zwei Regeln mit dem selben Namen nicht erlaubt. Man muss also die beiden `eform`-Regeln (eine aus L-Int, eine neu in L-Expr) **gedanklich mergen**:
>
> ``` ebnf
> eform  ::= '(' '+' eform eform+ ')'
>          | '(' '-' eform eform+ ')'
>          | '(' '*' eform eform+ ')'
>          | '(' '/' eform eform+ ')'
>          | '(' '-' eform ')'
>          | INT
>
> INT    ::= [0-9]+
>
> L_expr ::= eform EOF
> ```
>
> **Dieses Prinzip werden wir ab L-Expr für die konkreten und abstrakten Grammatiken anwenden.**
>
> Aus Platzgründen werden wir aber irgendwann darauf verzichten, alle bisherigen Regeln erneut über den `---` aufzuschreiben.

## Abstrakte Syntax für L-Expr

``` ebnf
Expr   = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
---

Expr   = MulExpr(Expr, Expr) | DivExpr(Expr, Expr)

L_expr = Expr
```

``` java
public interface Expr {}
```

Zusätzlich zu den Klassen aus L-Int

``` java
public record AddExpr(Expr left, Expr right) implements Expr {}
public record SubExpr(Expr left, Expr right) implements Expr {}
public record IntLiteral(int value) implements Expr {}
```

bekommen wir durch/in L-Expr:

``` java
public record MulExpr(Expr left, Expr right) implements Expr {}
public record DivExpr(Expr left, Expr right) implements Expr {}
```

> [!IMPORTANT]
>
> ### Hinweis zu den AST-Klassen
>
> Jede Sprachstufe wird auf der Vorgängersprachstufe aufbauen, d.h. jede Stufe übernimmt die Regeln und fügt neue hinzu. Wir haben also eine Art Vererbungsbeziehung: Die aktuelle Stufe erbt von der Vorgängerstufe und fügt neue Features hinzu.
>
> Bei den AST-Klassen bedeutet das, dass wir weitere Klassen ergänzen, um die neuen Features im Baum repräsentieren zu können. D.h. wir bleiben zunächst bei `Expr` als Obertyp für die AST-Knoten und fügen nur die neu benötigten Knoten `MulExpr` und `DivExpr` hinzu.

## Interpreter: Erinnerung an L-Int

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

> [!TIP]
>
> **Erinnerung**: Die Subtraktion war Teil des L-Int-Übungsblattes und ist hier nicht gezeigt.

## Interpreter L-Expr: Erben von L-Int und Ergänzen der neuen Funktionalität

``` java
public class InterpreterLexpr extends InterpreterLint {

    @Override
    public Value eval(Expr expr) {
        return switch (expr) {
            case MulExpr(var l, var r) -> {
                int a = Value.asInt(eval(l));
                int b = Value.asInt(eval(r));
                yield new IntVal(a * b);
            }

            default -> super.eval(expr);
        };
    }
}
```

> [!IMPORTANT]
>
> ### Hinweis zur Vererbung der Bausteine
>
> L-Expr *ist* L-Int plus Multiplikation, Division und ein unäres Minus.
>
> Unser Interpreter für L-Expr braucht also die Fähigkeiten des L-Int-Interpreters und muss Multiplikation und Division \*ergänzen. Dies können wir in objektorientierten Sprachen wie Java leicht erreichen, indem der L-Expr-Interpreter vom L-Int-Interpreter ableitet: `class InterpreterLexpr extends InterpreterLint`. In der Methode `eval()` wollen wir die neuen Fälle ergänzen, deshalb überschreiben wir die geerbte Methode und fügen die neuen Fälle hinzu. Damit die alten Fälle weiterhin funktionieren, rufen wir im `default`-Zweig einfach `super.eval(expr);` auf und bekommen damit das in `InterpreterLint` implementierte bisherige Verhalten.
>
> **Das werden wir von nun an in allen Bausteinen unserer Interpreter-Pipeline so machen!** Wir nutzen analog zu ([Siek 2023](#ref-Siek2023python)) Vererbung als einfachen Mechanismus zur Wiederverwendung von Verhalten.
>
> Jede neue Sprachstufe "erbt" von der Vorgängersprachstufe:
>
> -   In den Grammatiken nutzen wir die alten Regeln weiter und ergänzen neue Regeln/Alternativen (unter dem `---`)
> -   Bei den Java-Bausteinen (Klassen, Interfaces) leiten die Klassen der aktuellen Sprachstufe jeweils von den Klassen der Vorgängersprachstufe ab und erweitern das Verhalten durch Überschreiben und `super.XYZ()` für das bisherige Verhalten
> -   Die JUnit-Tests leiten ebenfalls von den Tests der Vorgängerstufe ab, um das bisherige Verhalten immer mitzutesten

### Unäres Minus

Das unäre Minus taucht nicht im AST auf.

Das ist eine Design-Entscheidung, um die Anzahl der AST-Typen möglichst klein und die notwendigen Transformationen möglichst einfach zu halten.

Beim Parsen oder beim Erzeugen des AST aus dem Parse-Tree kann man ein unäres Minus auf den bereits vorhandenen Subtraktionsknoten `SubExpr` "reduzieren". ... Ein weiterer Fall von *Desugaring* ...

> [!TIP]
>
> **Hinweis**: Das Einbauen des unären Minus bzw. des *Desugaring* ist Teil der Challenges und/oder des L-Expr-Übungsblattes und ist hier nicht gezeigt.

### Semantik der Division: Integer-Division wie in Java

Wir bleiben in allen Sprachstufen bei **Ganzzahlen** und nutzen überall dieselbe Divisions-Semantik wie in Java:

-   Operanden: `int`
-   Ergebnis: `int`
-   Rest wird **abgeschnitten** (Division mit Abrunden Richtung 0)

Beispiele:

    (/ 7 3)         ;; => 2
    (/ (-7) 3)      ;; => -2
    (/ 7 (-3))      ;; => -2
    (/ (-7) (-3))   ;; => 2

**Wichtig:**

-   In Lispy-Formen (`(/ 7 3)`) und in Java-ähnlichen Ausdrücken (`7 / 3`) verwenden wir dieselbe Integer-basierte Semantik wie in Java.
-   Division durch 0 führt zu einem **Laufzeitfehler** (in Java `ArithmeticException`). Wir behandeln diesen Fall nicht besonders in der Sprache.

> [!TIP]
>
> **Hinweis**: Das Einbauen der Division ist Teil der Challenges und/oder des L-Expr-Übungsblattes und ist hier nicht gezeigt.

## Wrap-up

-   L-Expr = L-Int + Multiplikation, Division und unäres Minus (nur Integer-Ausdrücke)

<!-- -->

-   Konkrete Syntax: neue Lispy-Formen `(* e1 e2 ...)`, `(/ e1 e2 ...)`, `(- e)`
-   Abstrakte Syntax: neue AST-Knoten `MulExpr` und `DivExpr` für binäre Operationen
-   Unäres Minus wird beim AST-Bau auf vorhandene Knoten desugart

<!-- -->

-   Sprachstufen bauen schrittweise aufeinander auf:
    -   Grammatiken übernehmen alte Regeln und ergänzen neue Alternativen
    -   Java-Klassen leiten von Vorgängerstufe ab, überschreiben Methoden und nutzen `super.XYZ()` für bestehendes Verhalten

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> In ([Nystrom 2021](#ref-Nystrom2021)) kann man in Kapitel 6 "Parsing Expressions" und Kapitel 7 "Evaluation Expressions" nachlesen.
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den Unterschied zwischen konkreter und abstrakter Syntax am Beispiel von L-Int und L-Expr erklären.
> -   k2: Ich kann beschreiben, wie L-Expr die Sprachstufe L-Int um Multiplikation, Division und unäres Minus erweitert.
> -   k3: Ich kann die konkrete Lispy-Grammatik von L-Int so erweitern, dass Multiplikation, Division und unäres Minus korrekt erkannt werden.
> -   k3: Ich kann die abstrakte Syntax (AST) von L-Int um neue Knotentypen für Multiplikation und Division ergänzen und den Interpreter entsprechend anpassen.
> -   k3: Ich kann unäres Minus beim AST-Bau auf vorhandene AST-Knoten desugaren und die Auswirkungen dieser Design-Entscheidung erläutern.
> -   k4: Ich kann erklären und am Beispiel zeigen, wie Vererbung zwischen Interpreter-Klassen (z.B. `InterpreterLexpr extends InterpreterLint`) die Wiederverwendung von Implementierungen zwischen Sprachstufen ermöglicht.
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Unäres Minus als Desugaring modellieren**
>
> Wir haben in L-Expr eine neue konkrete Form `(- e)` für unäres Minus eingeführt.
>
> 1.  Skizzieren Sie eine Regel, wie `(- e)` beim AST-Bau in einen vorhandenen AST-Knoten übersetzt werden kann (z.B. in einen `SubExpr`). Schreiben Sie diese Regel textuell oder in einer Art Pseudocode auf.
> 2.  Diskutieren Sie in der Gruppe: Welche Vor- und Nachteile hat es, kein eigenes `UnaryMinusExpr` einzuführen, sondern unäres Minus auf bestehende AST-Knoten zu desugaren?
>
> **Mehrstellige Operatoren im AST**
>
> In Lispy können Sie `(+ 1 2 3 4)` oder `(* 2 3 4)` schreiben, also Operatoren mit mehr als zwei Operanden. Unsere abstrakte Syntax für `AddExpr`, `MulExpr` usw. ist aber binär (jeweils genau zwei Operanden).
>
> 1.  Skizzieren Sie, wie der AST für `(+ 1 2 3 4)` mit nur binären `AddExpr`-Knoten aussehen könnte (zeichnen Sie den Baum oder schreiben Sie die verschachtelte Struktur auf).
> 2.  Überlegen Sie, ob Sie diesen Baum eher links-assoziativ `((((1 + 2) + 3) + 4))` oder rechts-assoziativ `((1 + (2 + (3 + 4))))` bauen würden. Diskutieren Sie kurz, ob das hier (bei Addition/Multiplikation) semantisch einen Unterschied macht und in welchen Fällen Assoziativität wichtig werden könnte.
>
> **Design der Interpreter-Hierarchie**
>
> Die Interpreter-Klassen sind so aufgebaut, dass jede neue Sprachstufe von der vorherigen erbt, z.B. `InterpreterLexpr extends InterpreterLint`.
>
> 1.  Skizzieren Sie in einem Mini-Klassendiagramm, wie die Interpreter-Hierarchie in den ersten beiden Sprachstufen aussieht (nur Klassenname und `eval`-Methode).
> 2.  Diskutieren Sie in der Gruppe:
>     -   Was sind Vorteile dieser Vererbungsstrategie, wenn wir viele aufeinander aufbauende Sprachstufen haben?
>     -   Welche Schwierigkeiten könnten auftreten, wenn eine spätere Sprachstufe ein Verhalten der Basisstufe ändern statt nur erweitern möchte?
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

<blockquote><p><sup><sub><strong>Last modified:</strong> 05df332 2026-10-25 lexpr: improve beamer formatting (intro)<br></sub></sup></p></blockquote>
