# Blatt 03: L-Int und C-Int (Integer, Addition, Subtraktion)

> [!IMPORTANT]
>
> Für die erfolgreiche Bearbeitung dieses Übungsblattes müssen mindestens alle mit `*` und `**` markierten Aufgaben gelöst werden.
>
> Die Aufgaben mit `***` sind als Vertiefung gedacht.

Dieses Blatt knüpft an folgende Vorlesungseinheiten an:

-   L-Int: Die Sprache Lispy
-   L-Int: Konkrete und abstrakte Syntax
-   L-Int: Lexer - Handcodierte Implementierung
-   L-Int: LL-Parser mit Recursive Descent
-   L-Int: Von Ausdrücken zum Interpreter

## A3.1: L-Int: Subtraktion verstehen und ergänzen (\*)

L-Int unterstützt Integer-Literale, Addition und Subtraktion. Im Skript wurde die Implementierung nur für Integer und Addition im Detail gezeigt.

### 1. Sprache und Semantik nachvollziehen

-   Welche Eingaben sind **gültige** L-Int-Ausdrücke?

-   Wie werden mehrere Operanden einer Addition oder Subtraktion gruppiert?

-   Welche Werte ergeben die folgenden Ausdrücke?

    ``` clojure
    (+ 1 2)
    (- 8 3)
    (- 20 5 3)
    (- 10 (+ 3 5))
    ```

Notieren Sie mindestens zwei **ungültige** L-Int-Eingaben. Mindestens eine davon soll eine Operation mit weniger als zwei Operanden sein.

### 2. Auswertung der Subtraktion skizzieren

Skizzieren Sie in Java, wie die Auswertung einer `SubExpr` aussehen könnte. Geben Sie dabei an, welche Teil-Ausdrücke ausgewertet und wie ihre Ergebnisse kombiniert werden.

## A3.2: L-Int: Funktion `skipIrrelevant()` im Lexer ergänzen (\*\*)

Im Skript wurde die Funktion `skipIrrelevant()` eingeführt. Sie überspringt nicht benötigte Zeichen wie Whitespace und Kommentare und wird zu Beginn von `nextToken()` aufgerufen:

``` java
protected boolean skipIrrelevant() {}  // Homework
```

**Gegeben:** Betrachten Sie den folgenden Code-Rahmen für diese Aufgabe und erstellen Sie damit ein neues Java-Projekt.

<details>

``` java
public enum TokenKind { PLUS, MINUS, LPAREN, RPAREN, SEMI, INT,  EOF }
public record Token(TokenKind kind, String lexeme) {}

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
}
```

</details>

1.  Zeichnen Sie den DFA für `skipIrrelevant()`.
2.  Implementieren Sie `skipIrrelevant()`.
3.  Erstellen Sie mindestens vier Testfälle. Berücksichtigen Sie dabei mindestens:
    -   eine Eingabe ohne Whitespace oder Kommentare
    -   eine Eingabe mit Whitespace vor einem Token
    -   eine Eingabe mit einem Kommentar
    -   eine Eingabe, die nach Whitespace oder einem Kommentar das Dateiende erreicht

## A3.3: C++ ausprobieren: Compiler und ein erstes Programm (\*)

Wir werden in dieser Veranstaltung Schritt für Schritt C++ kennenlernen und einen Interpreter in Java für einen kleinen C++-Ausschnitt ("Mini-C++") entwickeln.

In dieser Aufgabe richten Sie zunächst einen C++-Compiler ein, schreiben ein kleines Programm und übersetzen sowie starten es mit `clang++` oder `g++`.

### Schritt 1: Compiler überprüfen und installieren

Prüfen Sie in einem Terminal, ob ein C++-Compiler verfügbar ist:

    clang++ --version
    g++ --version

Falls kein Compiler gefunden wird:

-   Unter **macOS**: Installation der Xcode Command Line Tools.
-   Unter **Linux**: Installation von clang++ oder g++ über die Paketverwaltung.
-   Unter **Windows**: Nutzen Sie beispielsweise WSL mit Ubuntu und installieren Sie dort `clang++` oder `g++`, oder verwenden Sie eine andere C++-Toolchain.

> [!TIP]
>
> Für C++ verwenden wir **clang++** beziehungsweise **g++**, nicht clang beziehungsweise gcc. Umgangssprachlich meint "gcc" in diesem Kontext "g++", und "clang" meint tatsächlich "clang++". Bei der Installation von clang/gcc sind in der Regel beide Varianten (C und C++) dabei.

### Schritt 2: Erstes Programm übersetzen und ausführen

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

Unter Windows heißt die ausführbare Datei meist `hello.exe`. Starten Sie diese in PowerShell mit:

    .\hello.exe

Variieren Sie den arithmetischen Ausdruck in `result` und überprüfen Sie die Ausgabe für mindestens drei Beispiele.

> [!TIP]
>
> ### Mini-Waschzettel: `hello.cpp` im Vergleich zu L-Int / C-Int / Java
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

## A3.4: C-Int: Addition und Subtraktion für Integer in C++ (\*\*)

Wir legen nun mit **C-Int** die Basis für den Mini-C++-Interpreter, den wir über das Semester hinweg erweitern.

C-Int erlaubt analog zu L-Int **Integer-Literale, Addition und Subtraktion**, verwendet aber die in C++ übliche **Infix-Notation**.

> [!IMPORTANT]
>
> ### Waschzettel: In C++ sieht es so aus ...
>
> | Sprachelement   | L-Int (Lispy)            | C-Int (Mini-C++)         |
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
> Ein L-Int-Ausdruck wie `(- (+ 1 2) 3)` entspricht in C-Int beispielsweise:
>
>     ((1 + 2) - 3)
>     (1 + 2) - 3
>     1 + 2 - 3
>
> Addition und Subtraktion sind *linksassoziativ*. Ohne zusätzliche Klammern wird also von links nach rechts gruppiert.
>
> **Wichtig**: Damit ein C++-Compiler diese Ausdrücke übersetzen kann, müssen sie in einen passenden Programmrahmen eingebettet werden, beispielsweise in eine `main()`-Funktion und geeignete Statements für Zuweisung und Ausgabe.
>
> Die im Semester betrachteten Sprachstufen entwickeln sich schrittweise weiter. Die genaue Reihenfolge und der Umfang der späteren Sprachstufen werden im Skript eingeführt.
>
> Syntaktisch und semantisch betrachten wir im Laufe des Semesters folgende Abstufung:
>
>     C++ <: C-Self <: C-Inherit <: C-Class <: C-Fun <: C-If <: C-Expr <: C-Int
>
> In diesem Semester enden wir (voraussichtlich) bei C-Self.

### Schritt 1: Java-Projekt für C-Int einrichten

Legen Sie ein Java-Projekt für **C-Int** an. Sie können dafür ein eigenes Projekt erstellen oder Ihre Projektorganisation so wählen, dass sie den fortlaufenden Ausbau im Semester unterstützt. Falls gewünscht, können Sie eines der folgenden Templates als Ausgangspunkt nutzen:

-   Gradle: https://github.com/Compiler-CampusMinden/student-support-code-template
-   Maven: https://github.com/Compiler-CampusMinden/student-support-code-template-maven

Legen Sie eine für Sie nachvollziehbare Paket- und Verzeichnisstruktur fest. Sie können diese in der Übung besprechen und bei Bedarf anpassen.

### Schritt 2: Konkrete Syntax für C-Int festlegen

Definieren Sie analog zur Vorlesung die **konkrete Syntax für C-Int**. Sie legen damit fest, welche Programme von Ihrem Lexer und Parser akzeptiert werden.

Berücksichtigen Sie dabei folgende Anforderungen:

-   Whitespace darf zwischen den Bestandteilen eines Ausdrucks stehen und wird übersprungen.
-   Zeilenkommentare `// ...` bis zum Zeilenende werden unterstützt und übersprungen.
-   Addition und Subtraktion verwenden jeweils zwei Integer-Operanden, z.B. `1-2`.
-   Mehrfach verkettete Ausdrücke wie `1+2+3+4` sind erlaubt.
-   Ausdrücke dürfen geklammert sein.
-   Nicht geklammerte Ausdrücke werden implizit **von links nach rechts** gruppiert.

Notieren Sie Ihre konkrete Syntax, zum Beispiel als Grammatik. Halten Sie außerdem fest, welche Token, Schlüsselwörter und Symbole Ihr Lexer erkennen muss.

### Schritt 3: Abstrakte Syntax für C-Int definieren

Definieren Sie analog zur Vorlesung die **abstrakte Syntax** (AST) für **C-Int** und repräsentieren Sie diese in Java.

Berücksichtigen Sie dabei folgende Fragen:

-   Welche Informationen aus dem Parse-Tree müssen im AST erhalten bleiben?
-   Warum müssen Klammern im AST nicht zwingend als eigene Knoten vorkommen?
-   Wie wird die Linksassoziativität in der Struktur des AST dargestellt?

### Schritt 4: Interpreter-Pipeline für C-Int implementieren

Erstellen Sie analog zum Vorgehen im Skript für C-Int:

-   eine Tokendefinition,
-   AST-Klassen und -Interfaces,
-   einen Lexer,
-   einen handgeschriebenen Recursive-Descent-Parser,
-   einen Interpreter `InterpreterCint`.

Verwenden Sie in dieser Aufgabe **keinen** generierten ANTLR-Parser. Implementieren Sie die Pipeline wie in der Vorlesung mit handgeschriebenem Java-Code.

### Schritt 5: Tests für C-Int schreiben

Schreiben Sie JUnit-Tests für Ihre C-Int-Pipeline. Testen Sie mindestens:

-   ein Integer-Literal,
-   eine Addition,
-   eine Subtraktion,
-   einen geschachtelten Ausdruck,
-   Whitespace an unterschiedlichen Stellen,
-   ein zusätzliches Token nach einem vollständigen Ausdruck,
-   eine fehlende schließende Klammer,
-   einen ungültigen Operator oder ein unerwartetes Zeichen.

Ein weiterer Test soll aufdecken, ob bei der Subtraktion versehentlich linker und rechter Operand vertauscht wurden. Testen Sie daher beispielsweise sowohl `(- 7 2)` als auch `(- 2 7)`.

Testen Sie nach Möglichkeit nicht nur die Auswertung, sondern auch die erzeugten Token und ASTs. Dadurch können Sie Fehler in den einzelnen Phasen der Pipeline besser voneinander unterscheiden.

### Schritt 6: C-Int-Ergebnisse mit nativem C++ vergleichen

Schreiben Sie zu den folgenden Ausdrücken jeweils ein vollständiges C++-Programm mit `main()`, das den Ausdruck berechnet und das Ergebnis ausgibt:

    42
    (1 + 2)
    ((1 + 2) - 3)
    (10 - (3 + 5))

Übersetzen und starten Sie Ihre Programme mit clang++ oder g++.

Vergleichen Sie die Ausgaben mit den Ergebnissen Ihres C-Int-Interpreters und notieren Sie kurz:

-   Zwei Gemeinsamkeiten zwischen der Auswertung im nativen C++-Programm und der Auswertung durch Ihren Interpreter.
-   Zwei Unterschiede zwischen den beiden Verarbeitungswegen (z.B. Laufzeit, Fehlermeldungen, Komfort).

## A3.5: Vertiefung: Interpreter für L-Int mit Visitor-Pattern (\*\*\*)

Im Skript wurde die `eval()`-Funktion im L-Int-Interpreter mit Pattern Matching und `switch`-Expressions implementiert.

<details>

``` java
public interface Expr {}
public record IntLiteral(int value) implements Expr {}
public record AddExpr(Expr left, Expr right) implements Expr {}
public record SubExpr(Expr left, Expr right) implements Expr {}


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
```

</details>

1.  Übertragen Sie dies auf das Visitor-Pattern.
2.  Skizzieren Sie Vor- und Nachteile des Visitor-Patterns im Vergleich zur `switch`-Variante aus der Vorlesung.

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 20683c4 2026-11-11 homework: show DFA for skipIrrelevant (b03)<br></sub></sup></p></blockquote>
