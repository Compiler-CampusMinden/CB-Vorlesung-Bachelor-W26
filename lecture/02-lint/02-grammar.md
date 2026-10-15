# L-Int: Konkrete und abstrakte Syntax

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

## Erinnerung Lispy in der Sprachstufe L-Int

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

## Konkrete Grammatik für L-Int

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
> ### Schreibweise konkrete Grammatik
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
> ### Implizite Token
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

## Beispiel konkrete Syntax

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

## Konkrete Grammatik - Eigenschaften

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

## Konkrete vs. abstrakte Grammatik

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

## Abstrakte Grammatik für L-Int

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
> ### Schreibweise abstrakte Grammatik
>
> Die abstrakte Grammatik entspricht der Definition von algebraischen Datentypen (**ADT**). Da wir das in die Implementierungssprache Java übertragen, nutzen wir hier die Konventionen aus Java und schreiben alle Datentypen (spätere Interfaces und Records/Klassen) in UpperCamelCase.
>
> Mit `Int` ist ein Integer-Datentyp gemeint, d.h. wir könnten das in Java entweder als `int` oder als `Integer` schreiben.
>
> Wir werden später Listen benötigen. Diese werden mit `[]` ausgedrückt, d.h. `[Int]` würde eine Liste mit Integer-Werten bedeuten.

## Beispiel abstrakte Syntax (AST)

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
> ### Desugaring
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

## AST in Java: Definition

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
> ### Sealed Interfaces
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

## AST in Java: Beispielkonstruktion

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

## Wrap-up

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

<blockquote><p><sup><sub><strong>Last modified:</strong> 4cbdc68 2026-10-15 lint grammar: improve wording<br></sub></sup></p></blockquote>
