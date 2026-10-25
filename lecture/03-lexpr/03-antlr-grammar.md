# L-Expr: ANTLR-Grammatik und Vorrangregeln

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> ANTLR ist ein Parser‑Generator, der aus einer Grammatik einen Lexer, einen LL(\*)‑Parser und Hilfsklassen für die Traversierung des Parse-Trees in verschiedenen Zielsprachen (Java, Python, C++, ...) generiert.
>
> In einer ANTLR-Grammatik werden **Lexer-Regeln** mit **Großbuchstaben** geschrieben. Jede Lexer-Regel definiert einen regulären Ausdruck und liefert einen Token-Typ zurück, dessen Name der Regelname ist. Der Lexer wählt immer den **längsten Match**; bei Gleichstand gewinnt die **zuerst definierte** Regel.
>
> **Parser-Regeln** beginnen mit einem **Kleinbuchstaben** und bestehen aus einem Regelnamen (linke Seite) und einer Produktion (rechte Seite). Auf der rechten Seite können Token-Typen (Lexer-Regeln oder implizite Tokens in `'...'`) und andere Parser-Regeln in beliebiger Reihenfolge kombiniert werden. Parser-Regeln werden in Methoden des generierten Parsers übersetzt (vergleichbar zu einem rekursiv‑absteigenden Parser).
>
> Parser-Regeln können mehrere **Alternativen** haben, getrennt durch `|`. Bei Mehrdeutigkeiten entscheidet ANTLR zugunsten der **ersten passenden Alternative**. Mit `?`, `*` und `+` lassen sich optionale, beliebig oft oder mindestens einmal wiederholte Teile formulieren.
>
> ANTLR4 erlaubt im Gegensatz zu vielen klassischen LL(1)- bzw. handgeschriebenen rekursiv‑absteigenden Parsern auch **direkte Linksrekursion** in Parser-Regeln und nutzt diese u.a. zur automatischen Behandlung von **Operatorpräzedenz** und **Assoziativität**.
>
> </details>

## Von klassischer CFG zur ANTLR‑Grammatik

Erinnerung an "Programmieren 2":

-   ANTLR ist eine Java-Bibliothek
-   Generiert aus einer Grammatik einen Lexer und LL(\*)-Parser
-   Eingabe-String plus Lexer/Parser $\to$ Parse-Tree mit Kontextklassen
-   Zusätzlich: Klassen für Visitor-Pattern

In [Prog2](https://github.com/Programmiermethoden-CampusMinden/Prog2-Lecture-S26/blob/master/lecture/tooling/antlr.md) haben wir bereits ANTLR als Java-Bibliothek kennengelernt. Wir haben uns dort angeschaut, wie wir das Tool in einem Java-Projekt über Gradle einbinden können, wie wir die generierten Lexer und Parser aufrufen, und wir haben sogar die zu den Grammatik-Regeln generierten Kontextklassen betrachtet und das Visitor-Pattern zum Ablaufen der Parse-Trees benutzt.

Wir benutzen hier ANTLR in der Version **4**, manchmal auch als "ANTLR4" geschrieben.

Jetzt wollen wir auf die ANTLR-Grammatik schauen.

## Erinnerung: Konkrete Syntax für L-Expr

``` ebnf
eform  ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
INT    ::= [0-9]+
---

eform  ::= '(' '*' eform eform+ ')' | '(' '/' eform eform+ ')' | '(' '-' eform ')'

L_expr ::= eform EOF
```

Im Prinzip sieht unsere konkrete Grammatik bereits der ANTLR-Grammatik recht ähnlich. Wir müssen zunächst die `::=` durch einen einfachen Doppelpunkt `:` ersetzen. Zusätzlich muss jede Regel mit einem Semikolon `;` abgeschlossen werden.

Darüber hinaus kommen noch Namenskonventionen, die die Art der Regel erkennen lassen: Kleingeschriebene Regeln gelten für den Parser, großgeschriebene Regeln sind Lexer-Regeln. Die Reihenfolge der Regeln ist u.a. für Prioritäten bei Mehrdeutigkeiten relevant.

## ANTLR-Grammatik für L-Expr (Parser-Teil)

``` antlr
grammar Lispy;

lexpr       : eform EOF ;

eform       : atom                              #atomExpr
            | '(' '-' eform ')'                 #unaryMinusExpr
            | '(' '+' eform eform+ ')'          #addExpr
            | '(' '-' eform eform+ ')'          #subExpr
            | '(' '*' eform eform+ ')'          #mulExpr
            | '(' '/' eform eform+ ')'          #divExpr
            ;

atom        : INT ;
```

-   `lexpr` ist der Einstiegspunkt
-   `eform` entspricht unseren Ausdrücken

Hier sehen Sie den Parser-Teil einer ANTLR-Grammatik für L-Expr.

Als erste Zeile findet man immer die **Deklaration** `grammar <NAME>;`, wobei der Name `<NAME>` auch der Dateiname ist (`NAME.g4`) und später den Präfix für die generierten Lexer-/Parser-/Visitor-Klassen bildet. Unsere Grammatik würde also in der Datei `Lispy.g4` erwartet.

**Parser-Regeln** beginnen in ANTLR mit einem **Kleinbuchstaben** und können andere Parser-Regeln "aufrufen" und auch Lexer-Regeln beinhalten. Jede Regel endet mit einem Semikolon.

Wir haben eine Parser-Regel `lexpr` als **Einstiegspunkt** (erste Regel), die genau eine `eform` gefolgt vom Token `EOF` erwartet.

Die `eform`-Regel entspricht ziemlich direkt unserer konkreten EBNF-Grammatik. ANTLR4 kommt mit (direkter) Linksrekursion zurecht, weshalb wir die Grammatik relativ intuitiv formulieren können. Eine `eform` kann also entweder ein `atom` sein, oder eine `'(' '-' eform ')'` usw.

Nach den Parser-Regeln folgen üblicherweise die Lexer-Regeln (siehe nächste Folie).

### Formen der Subregeln

#### Standardregeln

Jede Regel hat einen Kopf (Regelname) und nach dem ":" den Regelkörper. Darin können Token wie `ID` oder Literale (implizite Token) `'='` und `';'` gematcht werden und andere Regeln "aufgerufen" werden wie `expr`. Jede Regel wird mit einem Semikolon abgeschlossen.

``` antlr
stmt  : ID '=' expr ';' ;
```

Um die Regel `stmt` anwenden zu können, müssen alle Elemente auf der rechten Seite der Regel erfüllt werden. Dabei müssen die Token wie `ID`, `'='` und `';'` in angegebenen Reihenfolge matchen und die Subregel `expr` muss erfüllt werden können.

Das abschließende Semikolon am Ende einer ANTLR-Regel markiert für ANTLR das Ende der Regel und dient als Hilfe beim Parsen der Grammatik und bezieht sich nicht auf den Input.

#### Alternativen

Wie auch im RD-Parser können wir in ANTLR-Grammatiken Alternativen ausdrücken:

``` antlr
stmt  : ID '=' expr ';' | expr ';' ;
```

**Alternativen** werden durch ein `|` getrennt. Hier muss genau eine Alternative erfüllt werden. Falls nötig, trennt man die Alternativen durch Einschließung in runden Klammern vom Rest der Regel ab (Gruppierung): `r : a (b | c) d ;`.

#### Wiederholungen (Multiplizitäten)

Teile der Regeln können wiederholt werden. Die Parser-Regeln nutzen die aus regulären Ausdrücken bekannten Operatoren `+` (einmal oder öfter) und `*` (beliebig oft) bzw. `?` (einmal oder keinmal: "optional").

``` antlr
expr  : term ('+' term)* ;
```

Der durch den `*` gekennzeichnete Teil kann beliebig oft vorkommen oder auch fehlen. Entsprechend würde eine `expr` dann erfüllt sein, wenn ein `term` vorliegt, oder auch wenn ein `term` und ein `'+'` und ein weiterer `term` gematcht wurde, oder auch mit mehrfachen Wiederholungen von `'+' term`.

Bei einem `+` müsste der Teil mind. einmal vorkommen, und bei einem `?` entsprechend einmal oder gar nicht.

(vgl. [github.com/antlr/antlr4/blob/master/doc/parser-rules.md](https://github.com/antlr/antlr4/blob/master/doc/parser-rules.md#subrules))

### Reihenfolge in Grammatik definiert Priorität

Falls mehr als eine Parser-Regel (oder Alternative) die selbe Input-Sequenz matcht, löst ANTLR diese Mehrdeutigkeit auf, indem es die erste Regel/Alternative nimmt, die an der Entscheidung beteiligt ist.

``` antlr
start : stmt ;

stmt  : expr | ID  ;
expr  : ID   | NUM ;
```

Bei der Eingabe "foo" würde die Alternative `ID` in der Regel `expr` "gewinnen", weil sie in der Grammatik vor der Alternative `ID` in der Regel `stmt` kommt und damit Vorrang hat.

### Benannte Alternativen (Label)

Man kann Alternativen in einer Produktionsregel auch individuell benennen über "`#...`". Das `#` leitet dabei die Benennung ein, gefolgt vom Label: `#foo` wäre also das Label `foo` für eine Alternative. Wenn man das nutzt, müssen immer **alle** Alternativen einer Regel benannt werden.

Benannte Alternativen sind später recht praktisch, da ANTLR benannte Alternativen im generierten Visitor wie normale Regeln behandelt und eigene `visitXYZ`-Methoden für die Label generiert.

Man kann die Label aber nicht in der Grammatik wie andere Regeln verwenden.

## ANTLR-Grammatik für L-Expr (Lexer-Teil)

``` antlr
...


INT         : [0-9]+ ;

COMMENT     : ';;' ~[\r\n]* -> skip ;
WS          : [ \t\r\n,]+ -> skip ;
```

-   Token-Typen: `INT`, `'('`, `'-'`, ...
-   Whitespace und Kommentare werden ignoriert

Der Lexer-Teil der Grammatik definiert, wie ANTLR den Eingabetext in Tokens aufteilt. Üblicherweise findet man diese Regeln nach den Parser-Regeln (siehe vorige Folie).

**Lexer-Regeln** beginnen mit einem **Grossbuchstaben** und definieren einen regulären Ausdruck, der auf den Eingabetext angewendet wird.

``` antlr
INT : [0-9]+ ;
WS  : [ \t\n]+ -> skip ;
```

Jede Lexer-Regel entspricht einem (benannten) **Token**. Im Beispiel gibt es die Regel `INT`, die den regulären Ausdruck `[0-9]+` anwendet. Damit matcht das Token `INT` auf alle Zeichenfolgen, die aus einer oder mehreren Ziffern bestehen.

Die Whitespace-Token (`WS`) werden im Beispiel zwar gematcht, aber danach **verworfen** (`-> skip`) - wir müssen diese zwar erkennen und aus der Eingabe entfernen, brauchen sie aber nicht für die weitere Prüfung der Strukturen.

Zusätzlich gibt es auch nicht benannte Token ("**implizite Token**", s.u.), das sind die in `'` eingeschlossenen Zeichenketten, beispielsweise `'='` oder `';'`.

Das Ergebnis ist ein Strom von Tokens, auf dem der Parser aufbaut.

### Implizite Token

Statt `PLUS` kann man in Parser-Regeln auch direkt `'+'` schreiben, d.h. das Lexem direkt ausschreiben und in **einfache** Anführungszeichen einschließen. ANTLR erzeugt dann automatisch ein unbenanntes Token (deshalb auch "implizites Token" genannt).

Die Länge der Zeichenkette spielt dabei keine Rolle, es werden hier immer einfache Anführungszeichen verwendet.

### To EOF not to EOF?

Startregeln müssen nicht unbedingt den gesamten Input "konsumieren". Sie müssen per Default nur eine der Alternativen in der Startregel erfüllen.

#### Ohne EOF: Es darf ein Rest vom Input übrig bleiben

Betrachten wir noch einmal einen leicht modifizierten Ausschnitt aus der obigen Grammatik:

``` antlr
start : stmt ;
```

Die Startregel wurde so geändert, dass sie nur noch genau ein Statement akzeptieren soll.

In diesem Fall würde die Startregel bei der Eingabe "aa bb" nur den ersten Teil "aa" konsumieren (als Token `ID`) und das folgende "bb" ignorieren. **Das wäre in diesem Fall aber auch kein Fehler.**

#### Mit EOF: Der gesamte Input muss passen

Wenn der **gesamte Eingabestrom** durch die Startregel erklärt werden soll, dann muss das vordefinierte Token `EOF` am Ende der Startregel eingesetzt werden:

``` antlr
start : stmt EOF;
```

Hier würde die Eingabe "aa bb" zu einem Fehler führen, da nur der Teil "aa" durch die Startregel abgedeckt ist (Token `ID`), und der Rest "bb" zwar sogar ein gültiges Token wäre (ebenfalls `ID`), aber eben nicht mehr von der Startregel akzeptiert. Durch das `EOF` soll die Startregel aber den gesamten Input konsumieren und erklären, was hier nicht geht und entsprechend zum Fehler führt.

(vgl. [github.com/antlr/antlr4/blob/master/doc/parser-rules.md](https://github.com/antlr/antlr4/blob/master/doc/parser-rules.md#start-rules-and-eof))

> [!TIP]
>
> ### Reservierte Schlüsselwörter
>
> Die folgenden Strings stellen reservierte Schlüsselwörter von ANTLR dar und dürfen nicht als Token, Regel oder Label genutzt werden:
>
>     import, fragment, lexer, parser, grammar, returns, locals, throws, catch, finally, mode, options, tokens
>
> `rule` ist zwar kein Schlüsselwort, wird aber als Methodenname bei der Codegenerierung verwendet. =\> Wie ein Schlüsselwort behandeln!
>
> (vgl. [github.com/antlr/antlr4/blob/master/doc/lexicon.md](https://github.com/antlr/antlr4/blob/master/doc/lexicon.md))

## Arbeitsweise von ANTLR

-   ANTLR generiert aus der Grammatik: Lexer, Parser, Visitor, ...
-   Lexer konsumiert Input und erzeugt Tokenstrom
-   Parser konsumiert Tokenstrom und erzeugt Parse-Tree

``` java
var source = "(+ 1 2)";

var input = CharStreams.fromString(source);
var lexer = new LispyLexer(input);
var tokens = new CommonTokenStream(lexer);

var parser = new LispyParser(tokens);
var tree = parser.lexpr();
```

<p align="right"><a href="http://lab.antlr.org/">Web: Lispy (Grammatik und Regeln, Eingabe und Parse-Tree)</a></p>

## Verhalten des Lexers

1.  **Längster Match**

    Primäres Ziel: Erkennen der längsten Zeichenkette

    ``` antlr
    INT  : [0-9]+ ;
    ID   : [a-zA-Z][a-zA-Z0-9]* ;
    ```

    Eingabe "foo42": Token `ID` (nicht `ID`, `INT`)

    Die Regel, die den längsten Match für die aktuelle Eingabesequenz produziert, "gewinnt".

<!-- -->

2.  **Reihenfolge** in Grammatik definiert Priorität

    ``` antlr
    FOR : 'for' ;
    ID  : [a-z][a-zA-Z]* ;
    ```

    Eingabe "for": Token `FOR`

    Falls mehr als eine Lexer-Regel die selbe Inputsequenz matcht, dann hat die in der Grammatik zuerst genannte Regel Priorität.

<!-- -->

3.  **Implizite Token**: haben höchste Priorität

    ``` antlr
    foo  : '0' | NUM EOF ;

    NUM  : [0-9]+ ;
    WS   : [ \t\r\n]+ -> skip ;
    ```

    Eingabe "0": Token `'0'` (nicht `NUM`)

    Implizite Tokens (`'...'`) werden intern **vor** expliziten Lexer‑Regeln einsortiert und bekommen dadurch eine noch höhere Priorität.

## Berücksichtigung von Präzedenz im Parser

Wir können die gestufte konkrete Grammatik, die die Operator-Präzedenz berücksichtigt, direkt nach ANTLR übersetzen:

-   **konkrete Grammatik** (gestuft):

    ``` ebnf
    expr  ::= term ('+' term)*
    term  ::= atom ('*' atom)*
    atom  ::= '-' atom
            | INT
    ```

<!-- -->

-   **ANTLR**-Grammatik:

    ``` antlr
    expr  : term ('+' term)* ;
    term  : atom ('*' atom)* ;
    atom  : '-' atom
          | INT
          ;
    ```

Die Präzedenz wird dabei durch die Struktur berücksichtigt: `expr` über `term` über `atom`.

Die gewünschte Linksassoziativität für die Operatoren `+`und `*` wird durch die Formulierung `(...)*` berücksichtigt.

## Direkte Linksrekursion und ANTLR4 - automatische Präzedenz

ANTLR4 kann **direkte Linksrekursion** automatisch auflösen. Die Regel `r : r T U | V ;` kann also in ANTLR4 verarbeitet werden.

ANTLR nutzt beim Auflösen der direkten Linksrekursion die Strategie des ["*precedence climbing*"](https://www.antlr.org/papers/Clarke-expr-parsing-1986.pdf) und löst nach der **Reihenfolge der linksrekursiven Alternativen** in einer Regel auf. Dies kann man ausnutzen, um über die Reihenfolge der linksrekursiven Alternativen die Operator-Präzedenz abzubilden:

Aus der Grammatik mit gestuften Regeln zur Berücksichtigung von Präzedenz und (Links-) Assoziativität ...

``` antlr
expr  : term ('+' term)* ;
term  : atom ('*' atom)* ;
atom  : '-' atom
      | INT
      ;
```

... wird eine Grammatik mit linksrekursiven Alternativen, wo sich der Vorrang über die Reihenfolge der linksrekursiven Alternativen bestimmt (von oben nach unten):

``` antlr
expr  : expr '*' expr
      | expr '+' expr
      | '-' atom
      | INT
      ;
```

Unter den beiden linksrekursiven Alternativen `expr '*' expr` und `expr '+' expr` hat die erste Alternative die höhere Priorität und würde beim Parsen wie gewünscht weiter unten im Baum landen.

Beide Varianten sind machbar. Entscheiden Sie selbst, was (für Sie) lesbarer ist.

> [!CAUTION]
>
> ### Indirekte Linksrekursion nicht erlaubt
>
> Mit **indirekter** Linksrekursion kann ANTLR (v4) **nicht** umgehen:
>
> ``` antlr
> expr : expM | ... ;
> expM : expr '*' expr ;
> ```
>
> Hier würden sich die Regeln `expM` und `expr` gegenseitig aufrufen und kein Token aus dem Tokenstrom entfernen, so dass der generierte LL-Parser hier in einer Endlosschleife stecken bleiben würde. Mit indirekter Linksrekursion kann ANTLR nicht umgehen.
>
> **Indirekte Linksrekursion ist nicht erlaubt!**

### Assoziativität

Per Default werden Operatoren wie `+` von ANTLR automatisch als **linksassoziativ** behandelt, da bei Mehrdeutigkeiten in der Grammatik stets die erste passende Regel/Alternative genommen wird. Dadurch wird die übliche Klammerung von `+` und `*` berücksichtigt, d.h. `1+2+3` wird mit der gezeigten Grammatik wie erwartet als `(1+2)+3` geparst.

Die Eingabe `2^3^4` sollte aber als `2^(3^4)` geparst werden. Analog sollte `a=b=c` in C/C++ als `a=(b=c)` verstanden werden. Für *rechts-assoziative* Operatoren muss man ANTLR dies explizit mit `<assoc=right>` in der Grammatik mitteilen:

``` antlr
expr : <assoc=right> expr '^' expr
     | INT
     ;
```

## Ausblick

Damit haben wir die sprichwörtliche "Spitze des Eisbergs" gesehen. Mit ANTLR sind noch viele weitere Dinge möglich. Bitte nutzen Sie aktiv die Dokumentation auf [github.com/antlr/antlr4](https://github.com/antlr/antlr4).

## Wrap-up

-   Lexer-Regeln beginnen mit **Großbuchstaben**
    -   Längster Match gewinnt, bei Gleichstand: zuerst definierte Regel
    -   Implizite Tokens (`'...'`) haben Vorrang vor expliziten Lexer-Regeln

<!-- -->

-   Parser-Regeln beginnen mit **Kleinbuchstaben**
    -   Regeln können Token-Typen (Lexer-Regeln oder `'...'`) und andere Parser-Regeln kombinieren
    -   Regeln können Alternativen haben (`|`) sowie optionale/mehrfache Teile (`?`, `*`, `+`)
    -   Bei Mehrdeutigkeit: Vorrang für die erste passende Alternative
    -   Startregel + `EOF` erzwingt, dass der **gesamte** Input konsumiert wird
    -   ANTLR4 erlaubt **direkte** Linksrekursion (indirekte nicht)

<!-- -->

-   Vorrang und Assoziativität für Ausdrücke in ANTLR:
    -   Gestufte Grammatik-Regeln (`expr` / `term` / `atom`) kodieren Präzedenz strukturell
    -   Linksrekursive Regel mit geordneter Alternativenliste (Reihenfolge der linksrekursiven Alternativen steuert den Vorrang der Operatoren)
    -   Linksassoziativität ist Standard; Rechtsassoziativität kann mit `<assoc=right>` deklariert werden

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Sie finden auf der [ANTLR-Homepage](https://www.antlr.org) Links zur Dokumentation und auch zum [ANTLR-Projekt](https://github.com/antlr/antlr4) auf GitHub.
>
> Eine ältere, aber dennoch interessante Quelle vom "Kopf" hinter ANTLR (Terence Parr) ist ([Parr 2014](#ref-Parr2014)), die auch online verfügbar ist.
>
> Die [ANTLR-Dokumentation](https://github.com/antlr/antlr4/blob/master/doc/index.md) und die [ANTLR-FAQ](https://github.com/antlr/antlr4/blob/master/doc/faq/index.md) bieten ebenfalls interessante Einstiegspunkte zur Vertiefung für Fortgeschrittene.
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den grundsätzlichen Aufbau einer ANTLR-Grammatik (Lexer-Teil, Parser-Teil, Startregel) erklären.
> -   k2: Ich kann das Verhalten des ANTLR-Lexers bzgl. längstem Match, Regelreihenfolge und impliziten Tokens beschreiben.
> -   k3: Ich kann eine gegebene EBNF-Grammatik für arithmetische Ausdrücke in eine äquivalente ANTLR-Grammatik übersetzen.
> -   k3: Ich kann in ANTLR Parser-Regeln mit Alternativen sowie optionalen und wiederholten Teilen formulieren.
> -   k3: Ich kann Operatorpräzedenz und Linksassoziativität entweder über gestufte Regeln (`expr`/`term`/`atom`) oder über eine linksrekursive `expr`-Regel in einer ANTLR-Grammatik umsetzen.
> -   k4: Ich kann für eine gegebene ANTLR-Grammatik begründen, wie eine konkrete Eingabe (z.B. `2+3*4`) geparst wird, und den entstehenden Parse-Tree grob skizzieren.
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Von EBNF zu ANTLR mit impliziten Tokens**
>
> Gegeben sei die folgende EBNF-Grammatik für einfache logische Ausdrücke mit C‑ähnlicher Präzedenz (`not` bindet am stärksten):
>
> ``` ebnf
> expr   ::= disj
> disj   ::= conj ('or' conj)*
> conj   ::= atom ('and' atom)*
> atom   ::= 'not' atom
>          | '(' expr ')'
>          | ID
> ```
>
> Dabei soll eine `ID` mit einem kleinen Buchstaben beginnen und danach beliebig viele kleine/große Buchstaben und Ziffern beinhalten. Eine ID besteht dabei aus mindestens einem Zeichen.
>
> Übersetzen Sie diese Grammatik in eine ANTLR-Grammatik (`Logic.g4`) mit:
>
> -   einer Startregel `expr`
> -   impliziten Tokens für die Schlüsselwörter und Klammern
> -   einer expliziten Lexer-Regel für `ID`,
> -   einer Lexer-Regel für Whitespace; dieser soll verworfen werden
>
> **Präzedenzfehler finden und korrigieren**
>
> Betrachten Sie die folgende ANTLR-Grammatik für arithmetische Ausdrücke:
>
> ``` antlr
> grammar ArithWrong;
>
> expr : term (('+' | '-') term)* ;
> term : atom (('*' | '/') atom)* ;
> atom : '-' expr
>      | INT
>      | '(' expr ')'
>      ;
> ```
>
> mit `INT` ein oder mehrere Ziffern (wie üblich).
>
> 1.  Überlegen Sie, wie der Ausdruck `-1*2` mit dieser Grammatik geparst wird. Skizzieren Sie grob den Parse-Tree (nur mit den Regeln `expr`, `term`, `atom`).
> 2.  Diskutieren Sie: Entspricht das der üblichen Bedeutung von `-1*2` in C/Java?
> 3.  Ändern Sie nur die Definition von `atom` so, dass das unäre Minus mit höherer Präzedenz als `*` und `/` behandelt wird (und der Rest weiterhin funktioniert).
>
> **Implizite Tokens und Lexer-Priorität**
>
> Gegeben sei ein Ausschnitt einer ANTLR-Grammatik:
>
> ``` antlr
> grammar Mini;
>
> start : 'if' '(' ID ')' EOF ;
> ```
>
> Dabei soll eine `ID` mit einem kleinen Buchstaben beginnen und danach beliebig viele kleine/große Buchstaben und Ziffern beinhalten. Eine ID besteht dabei aus mindestens einem Zeichen.
>
> 1.  Welche Tokenfolge erzeugt der Lexer für die Eingabe `if(x)`?
>
> 2.  Diskutieren Sie: Was würde passieren, wenn Sie statt `'if'` in der Startregel eine explizite Lexer-Regel `IF : 'if' ;` und die Startregel als `start : IF '(' ID ')' EOF ;` schreiben würden?
>
> 3.  Variieren Sie nun die Reihenfolge der Lexer-Regeln: `antlr  ID : [a-z][a-zA-Z0-9]* ;  IF : 'if' ;` Diskutieren Sie:
>
>     -   Welche Tokenfolge erzeugt der Lexer nun für die Eingabe `if(x)`?
>     -   Warum macht es aus Sicht der ANTLR-Spezifikation trotzdem einen Unterschied, ob Sie `'if'` als implizites Token oder als explizite Lexer-Regel schreiben?
>
> **Linksrekursion und Vorrang**
>
> Betrachten Sie die folgende typische ANTLR-Grammatik für Expressions:
>
> ``` antlr
> program : expr EOF ;
>
> expr
>   : expr ('*'|'/') expr
>   | expr ('+'|'-') expr
>   | INT
>   ;
> ```
>
> In der gezeigten Schreibweise wird implizit ein Vorrang unter den Alternativen dieser direkt links-rekursiven Regel `expr` definiert und die Eingabe `2+3*4` würde korrekt als `2+(3*4)` geparst.
>
> Was passiert, wenn man nun versucht, die Operatoren auszuklammern und in eine eigene Regel zu verlagern?
>
> Betrachten Sie die folgende leicht geänderte Grammatik:
>
> ``` antlr
> program : expr_ EOF ;
>
> expr_
>   : expr_ operator expr_
>   | INT
>   ;
> operator
>   : ('*'|'/')
>   | ('+'|'-')
>   ;
> ```
>
> Was passiert hier bei der Eingabe von `2+3*4`?
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
> <div id="ref-Parr2014" class="csl-entry">
>
> Parr, T. 2014. *The Definitive ANTLR 4 Reference*. Pragmatic Bookshelf. <https://learning.oreilly.com/library/view/the-definitive-antlr/9781941222621/>.
>
> </div>
>
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> ddc7464 2026-10-25 lexpr: improve beamer formatting (antlr grammar)<br></sub></sup></p></blockquote>
