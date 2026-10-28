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

## (\*) L-Int: Java-Projekt für L-Int einrichten

Legen Sie ein Java-Projekt für **L-Int** an. Nutzen Sie dafür Maven oder Gradle. Das Projekt wird im Laufe des Semesters Schritt für Schritt erweitert.

Falls gewünscht, können Sie eines der folgenden Templates als Ausgangspunkt nutzen:

-   Gradle: https://github.com/Compiler-CampusMinden/student-support-code-template
-   Maven: https://github.com/Compiler-CampusMinden/student-support-code-template-maven

Rekonstruieren Sie die in der Vorlesung gezeigten Bausteine für L-Int in Ihrem Projekt (Pakete, Klassen, Interfaces) und übernehmen Sie die gezeigte Funktionalität so, dass Ihr Projekt kompilierbar ist. Fehlende Funktionalität ergänzen Sie in den folgenden Aufgaben.

## (\*\*) L-Int: Funktion `skipIrrelevant()` im Lexer ergänzen

In der Vorlesung wurde die Funktion `skipIrrelevant()` eingeführt, mit der nicht benötigte Zeichen wie Whitespace und Kommentare übersprungen werden. Sie wird immer zu Beginn von `nextToken()` aufgerufen:

``` java
protected boolean skipIrrelevant() {}  // Homework
```

1.  Wie könnte diese Methode aussehen? Geben Sie eine Implementierung in Java an.
2.  Sammeln Sie in der Übung gemeinsam sinnvolle Testfälle und notieren Sie diese für später (s.u.).

## (\*) L-Int: Subtraktion für L-Int ergänzen

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

## (\*\*) L-Int: Tests für L-Int

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

## (\*) C++ ausprobieren: Compiler und ein erstes Programm

Wir werden in dieser Veranstaltung Schritt für Schritt C++ kennenlernen und einen Interpreter in Java für einen kleinen C++-Ausschnitt entwickeln.

In dieser Aufgabe geht es zunächst darum,

-   einen C++-Compiler einzurichten,
-   ein kleines Programm zu schreiben,
-   dieses mit clang++ oder g++ zu übersetzen und auszuführen.

### Schritt 1: Compiler überprüfen und installieren

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

Unter Windows heißt die ausführbare Datei meist `hello.exe`, Start in PowerShell:

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

## (\*\*) C-Int: Addition und Subtraktion für Integer in C++

Wir legen nun mit **C-Int** die Basis für den Mini-C++-Interpreter, den wir über das Semester hinweg erweitern.

C-Int soll analog zu L-Int **Ausdrücke mit Integer-Literalen, Addition und Subtraktion** erlauben, aber in C++-üblicher Infix-Notation.

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

### Schritt 1: Java-Projekt für C-Int einrichten

Legen Sie ein neues Java-Projekt für Ihren C-Int-Interpreter an (analog zur ersten Aufgabe für L-Int). Nutzen Sie ein eigenes Package, z.B. `cint`.

### Schritt 2: Konkrete Syntax für C-Int

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

### Schritt 3: Abstrakte Syntax für C-Int

Definieren Sie analog zur Vorlesung die **abstrakte Syntax** (AST) für **C-Int**.

Diskussionspunkte:

-   Welche Informationen aus dem Parse-Tree müssen im AST erhalten bleiben?
-   Warum müssen Klammern im AST nicht als eigene Knoten vorkommen?

### Schritt 4: Interpreter-Pipeline für C-Int

Erstellen Sie analog zur L-Int-Pipeline für C-Int:

-   eine Tokendefinition,
-   AST-Klassen und -Interfaces,
-   einen Lexer,
-   einen RD-Parser,
-   einen Interpreter `InterpreterCint`.

Verwenden Sie hierfür **keinen** generierten ANTLR-Parser, sondern eine handgeschriebene Java-Implementierung wie in der Vorlesung.

### Schritt 5: Tests für C-Int

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

## (\*\*\*) Vertiefung: Interpreter für L-Int mit Visitor-Pattern

In der Vorlesung wurde die `eval()`-Funktion im L-Int-Interpreter mit Pattern Matching und `switch`-Expressions implementiert.

1.  Übertragen Sie dies auf das Visitor-Pattern.
2.  Skizzieren Sie Vor- und Nachteile des Visitor-Patterns im Vergleich zur `switch`-Variante aus der Vorlesung.

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 709f78d 2026-10-28 homework: improve task descriptions (sheet 03 l-int)<br></sub></sup></p></blockquote>
