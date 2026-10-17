# L-Int: LL-Parser mit Recursive Descent

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Ein LL-Parser mit rekursivem Abstieg (*recursive descent*, auch *RD-Parser*) verarbeitet den Tokenstrom von links nach rechts und wählt Alternativen anhand eines begrenzten Lookaheads:
>
> -   Für Nichtterminale werden Parsermethoden aufgerufen; Terminale werden mit `match()` geprüft und "konsumiert".
> -   Alternativen werden anhand ihrer möglichen Anfangstokens ausgewählt. Optionale Teile und Wiederholungen werden typischerweise mit `if` und `while` umgesetzt.
> -   Die Grammatik muss eine eindeutige Vorhersage erlauben; bei leeren Alternativen sind dafür auch FOLLOW-Mengen relevant.
>
> Zur Implementierung eines RD-Parsers kann man so vorgehen:
>
> -   Zu jeder Produktionsregel erstellt man eine gleichnamige Funktion.
> -   Wenn in der Produktionsregel andere Regeln "aufgerufen" werden, erfolgt in der Funktion an dieser Stelle der entsprechende Funktionsaufruf.
> -   Bei Terminalsymbolen wird das erwartete Token mit `match()` geprüft.
>
> Dabei findet man wie bereits im Lexer die Funktionen `match()` und `advance()`, die sich hier aber auf den Tokenstrom beziehen. LL(1)-Parser schauen dabei immer nur das nächste Token an, LL(k) haben ein entsprechendes Lookahead von bis zu $k$ Token.
>
> RD-Parser haben ein Problem mit Linksrekursion in der Grammatik, da hier rekursiv Methoden aufgerufen werden und keine Token entfernt werden; eventuell auftretende Linksrekursion muss zunächst beseitigt werden.
>
> </details>

## Recursive-Descent-Parser: Ziel

-   Aus Tokens einen AST bauen:

    ``` java
    Expr parse(List<Token> tokens);
    ```

<!-- -->

-   Basis konkrete Syntax (Grammatik):

    ``` ebnf
    eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
    INT   ::= [0-9]+
    ```

-   Zielformat AST (abstrakte Syntax):

    ``` ebnf
    Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
    ```

Mit dem Lexer können wir jetzt eine Tokenliste erzeugen. Der nächste Schritt ist der Parser, den wir als Recursive-Descent-Parser implementieren. Wir verwenden dafür eine leicht umgeformte Grammatik ohne Linksrekursion. Die Methode `parse()` wird unser Einstiegspunkt sein und am Ende einen `Expr`-AST liefern, den wir mit unserem Interpreter auswerten können.

## Grundidee LL-Parser mit Recursive Descent

Die Grammatik (konkrete Syntax) besteht aus einer Menge von Produktionsregeln:

``` ebnf
r ::= X s
```

Diese Regeln werden jeweils in eine dazu passende Funktion überführt:

``` java
Expr parseR() {
    match(X);
    return s();
}
```

-   Für jede Regel in der Grammatik wird eine Methode/Funktion mit diesem Namen definiert
-   Referenzen auf ein Terminal (Token) `T` werden durch den Aufruf der Methode `match(T)` aufgelöst
    -   `match(T)` "konsumiert" das aktuelle Token, falls dieses mit `T` übereinstimmt
    -   Anderenfalls löst `match()` eine Exception aus
-   Referenzen auf Nichtterminale (Regeln) `s` werden durch Methodenaufrufe `s()` aufgelöst

## Alternative Subregeln

Alternativen in einer Regel:

``` ebnf
    a | b | c
```

Auflösen durch Unterscheidung am aktuellen Token (und falls nötig weiterem Lookahead):

``` java
return switch (peek()) {
    case PredictingA -> parseA();
    case PredictingB -> parseB();
    case PredictingC -> parseC();
    default -> throw new RuntimeException("expected expression");
};
```

Anhand des aktuellen Tokens (`peek()`) wird entschieden, welche Alternative vorliegt und welche der `parseXYZ()`-Methoden entsprechend aufgerufen werden soll.

`PredictingA` soll andeuten, dass man mit dem aktuellen Token eine Vorhersage für die Regel `a` versucht (hier kommen die FIRST- und FOLLOW-Mengen ins Spiel ...). Wenn das der Fall ist, springt man entsprechend in die Funktion bzw. Methode `a()`. Formal berechnet man die Lookahead-Mengen mit `FIRST` und `FOLLOW`, um eine Entscheidung für die nächste Regel zu treffen. Praktisch betrachtet kann man sich fragen, welche(s) Token eine Phrase in der aktuellen Alternative starten können.

Für LL(1)-Parser betrachtet man immer das **aktuelle** Token (**genau *EIN* Lookahead-Token**), um eine Entscheidung zu treffen. Bei LL(k) nutzt man einen Lookahead von insgesamt bis zu $k$ Token.

> [!TIP]
>
> ### Exkurs LL(k)
>
> Die folgende Regel ist für einen LL(1)-Parser nicht deterministisch behandelbar, da die Alternativen mit dem gleichen Token beginnen (die Lookahead-Mengen überlappen sich).
>
> ``` ebnf
> expr ::= ID '++' | ID '--'
> ```
>
> Hier benötigt man entweder **zwei Lookahead-Tokens**, also einen LL(2)-Parser, oder man muss die Regel in eine äquivalente LL(1)-Grammatik umschreiben ($\to$ **Linksfaktorisierung**, s.u.):
>
> ``` ebnf
> expr ::= ID ('++' | '--')
> ```

## Grundstruktur eines RD-Parsers

``` java
public class LispyParserLint {
    protected List<Token> tokens;
    protected int current;

    public Expr parse(List<Token> tokenList) {
        tokens = tokenList;  current = 0;

        Expr expr = parseEform();
        if (!isAtEnd()) throw new RuntimeException("unexpected token");

        return expr;
    }

    // Arbeitsmethoden: parseEform(), parseAtom(), parseAddExpr(), parseSubExpr()
    // Hilfsmethoden:   isAtEnd(), peek(), advance(), match(), check() ...
}
```

Der Parser sieht strukturell dem Lexer ähnlich: Er hat eine Eingabefolge - hier die Token-Liste `tokens` - und einen Positionszeiger (aktueller Index `current`).

Den Einstieg in den Parser bildet die Methode `parse()`, die den gesamten Eingabestrom gemäß der Grammatik verarbeitet. Diese Methode bildet die Startregel `L_int ::= eform EOF` der konkreten Grammatik ab.

Die einzelnen Regeln der (von direkter Linksrekursion befreiten) konkreten Grammatik bilden wir auf die Arbeitsmethoden `parseEform()`, `parseAtom()`, `parseAddExpr()`, `parseSubExpr()` ab.

Intern werden wir Hilfsmethoden wie `peek()`, `advance()` und `match()` verwenden, um die Tokens bequemer zu handhaben.

## Partielle Beispielimplementierung RD-Parser für L-Int

Wir betrachten folgende konkrete Syntax in L-Int:

``` ebnf
eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
INT   ::= [0-9]+

L_int ::= eform EOF
```

Der Einstieg ist die Regel `L_int`, die eine `eform` erwartet und ein anschließendes `EOF`-Token. Mehr ist in der Sprachstufe L-Int nicht erlaubt und wurde bereits in der Einstiegsmethode `parse()` umgesetzt.

Eine `eform` kann entweder eine Addition, eine Subtraktion oder einfach nur ein Integer-Literal sein. Letzteres ist ein Terminal, d.h. hier können wir einfach mit `match()` das entsprechende Token abfragen.

Die Alternativen für Addition und Subtraktion rufen jeweils die Regel für `eform` rekursiv auf.

Der Parser muss entlang dieser Regeln aufgebaut werden und die Token der Reihe nach über diese Regeln ableiten ("erklären"). Nur wenn dies vollständig gelingt, haben wir eine syntaktisch korrekte Eingabe. Ein Parser kann einen Parse-Tree oder - wie in unserem Beispiel - direkt einen AST erzeugen. Der gezeigte Code erzeugt den AST gemäß der folgenden abstrakten Syntax:

``` ebnf
Expr  = IntLiteral(Int) | AddExpr(Expr, Expr) | SubExpr(Expr, Expr)
```

Wir haben hier also konkret eine Daten-Klasse `IntLiteral` für die Repräsentation von Integer-Literalen, die jeweils die gefundene Zahl speichern.

Analog für Addition und Subtraktion. Hier ist interessant, dass die vorgesehenen Knoten im AST nur binäre Knoten sind, in der konkreten Syntax aber jeweils zwei oder mehr Argumente für Addition und Subtraktion erlaubt sind. Ein reiner Parse-Tree würde diese Grammatik-Struktur im Baum widerspiegeln (siehe Challenge). Wir wollen hier einen abstrakteren Baum (AST) erzeugen und müssen entsprechend bereits beim Erzeugen des AST die Zielstruktur berücksichtigen.

``` java
protected Expr parseEform() {
    if (check(LPAREN)) {
        return switch (peek(1).kind()) {
            case PLUS -> parseAddExpr();
            default -> throw new RuntimeException("unexpected token");
        };
    }

    return parseAtom();
}


protected Expr parseAtom() {
    Token t = match(INT);
    return new IntLiteral(Integer.parseInt(t.lexeme()));
}
```

Für die Regel `eform` wird eine `parseEform()` angelegt, die direkt der Grammatikregel entspricht.

> [!TIP]
>
> Die Sichtbarkeit ist `public` für die Einstiegsmethode `parse()` - diese Methode soll später zum Starten des Parsers genutzt werden. Die anderen (internen) Methoden bekommen die Sichtbarkeit `protected`, damit wir sie bei Bedarf in ableitenden Parser-Klassen überschreiben können. Wenn wir uns ganz sicher sind, dass wir eine interne Methode nie überschreiben wollen, können wir natürlich auch wie immer auf `private` gehen.

Wir unterscheiden in `parseEform()` anhand des aktuellen Tokens, ob es sich um die Alternativen für Addition (und Subtraktion, nicht gezeigt) handeln könnte (beide starten mit `'('`). Ansonsten muss es sich um die letzte Alternative `INT` handeln:

-   Liegt ein `LPAREN` vor, entfällt die letzte Alternative. Es kann sich nur noch um eine Addition oder Subtraktion handeln.

    Da diese Alternativen beide mit `'('` beginnen, schauen wir nun auf das nächste Token:

    -   Finden wir ein `ADD`- oder `SUB`-Token, rufen wir entsprechend die Regeln `parseAddExpr()` bzw. `parseSubExpr()` (nicht gezeigt) auf.
    -   Anderenfalls melden wir einen Fehler, weil wir `LPAREN` gesehen haben, aber nichts, was laut unserer Grammatik darauf folgen müsste.

-   Liegt *kein* `LPAREN` vor, kann es sich nur um die letzte Alternative `INT` handeln.

    Hier handelt es sich um ein Terminal-Symbol, welches wir in `parseAtom()` mit `match(INT)` überprüfen und gleichzeitig aus dem Tokenstrom "entfernen". Wenn das aktuelle Token dabei nicht `INT` ist, würde `match(INT)` einen Fehler auslösen.

**Wichtig**: Die Subtraktion fehlt hier und dient als kleine Übung.

**Wichtig**: Wir haben damit in `parseEform()` effektiv ***zwei* Tokens Lookahead** verwendet! Vgl. auch die nachfolgende Diskussion zur Linksfaktorisierung.

Die Methode `parseAtom()` entspricht genau dem weiter oben gezeigten Muster. Wir erwarten ein `INT`-Token, also rufen wir die Hilfsmethode `match()` mit diesem Parameter auf. `match()` prüft, ob das aktuelle Token wirklich diesen Typ hat und "entfernt" es aus dem Tokenstrom (inkrementiert den `current`-Index) und liefert es zurück. Anderenfalls würde `match()` eine entsprechende Exception werfen.

Das Lexem nutzen wir und konvertieren es in einen Java-Integer und erzeugen einen neuen AST-Knoten vom Typ `IntLiteral` - da dieser Typ vom Interface `Expr` ableitet, haben wir hier also den einfachsten möglichen AST gebildet.

> [!TIP]
>
> ### Exkurs Linksfaktorisierung
>
> Wir haben in `parseEform()` *implizit* **Linksfaktorisierung** angewendet: Wenn mehrere Alternativen einer Regel den gleichen Anfang haben, kann man diesen "heraus klammern":
>
> ``` ebnf
> ruleA ::= a b1 | ... | a bm
> ```
>
> Hier könnte man den gemeinsamen Start `a` "ausklammern":
>
> ``` ebnf
> ruleA  ::= a ruleAA
> ruleAA ::= b1 | ... | bm
> ```
>
> Bei uns im Beispiel war der gemeinsame Start für mehrere Alternativen die Klammer `'('`. Statt die Grammatik umzubauen, hatten wir das über das `if` und das nachfolgende extra Lookahead `peek(1)` gelöst. Mit der Umformung hätten wir eine weitere Hilfsfunktion im Parser, würden dann aber wieder mit `peek()` bzw. `peek(0)` und damit einem Token Lookahead auskommen (statt jetzt zwei).

## Details Parser für L-Int: `parseAddExpr()`

``` java
protected Expr parseAddExpr() {
    match(LPAREN);
    match(PLUS);

    List<Expr> ops = new ArrayList<>();
    ops.add(parseEform());
    while (!check(RPAREN)) ops.add(parseEform());

    match(RPAREN);

    return ops.stream().reduce(AddExpr::new).orElseThrow();
}
```

Auch hier sehen wir wieder das eingangs skizzierte Muster: Für jedes Terminal wird `match()` mit dem gesuchten Token-Typ aufgerufen. Danach müssen wir die Operanden parsen: Solange nicht ein `')'`-Token auftaucht, rufen wir rekursiv die Regel `parseEform()` auf. Jeder Operand muss ja selbst eine `eform` sein. Anschließend matchen wir noch die schließende rechte Klammer, und die Addition ist im Prinzip fertig geparst.

Nun müssen wir aber noch den Baum mit binären `AddExpr`-Knoten bauen. Dazu laufen wir über die Liste der geparsten Operanden und reduzieren den Stream. Dabei würde `(+ 1 2 3)` von links nach rechts geklammert: `(+ (+ 1 2) 3)`.

**Wichtig**: Challenge (s.u.) beachten!

> [!TIP]
>
> ### Exkurs Desugaring
>
> Diese Auflösung ist ein einfaches Beispiel für ***Desugaring*** - hier wird aus einer komplexeren Operation mit potentiell beliebig vielen Operanden eine Folge von einfacheren binären Operationen gemacht. Wir werden noch komplexere Beispiele sehen, wenn wir in L-If über Kontrollstrukturen sprechen.

## Hilfsfunktionen im Parser: Tokenhandling

``` java
protected boolean isAtEnd() { return peek().kind() == EOF; }

protected Token peek() { return peek(0); }
protected Token peek(int la) {
    if (la < 0 || current + la >= tokens.size()) return tokens.getLast();  // EOF
    return tokens.get(current + la);
}

protected Token advance() {
    return isAtEnd() ? tokens.getLast() : tokens.get(current++);
}

protected boolean check(TokenKind... expected) {
    return Arrays.stream(expected).anyMatch(e -> e == peek().kind());
}
protected Token match(TokenKind... expected) {
    if (check(expected)) return advance();
    throw new RuntimeException("unexpected token: " + peek());
}
```

Man definiert sich gern eine Handvoll von Hilfsfunktionen im Parser, die das Handling mit den Token erleichtern. Im Prinzip können diese Funktionen relativ symmetrisch zu den Pendants im Lexer implementiert werden - allerdings arbeitet man hier im Parser mit Token statt mit Zeichen.

Der auffälligste Unterschied zum Lexer: Wir verlassen uns nicht auf die Länge der Tokenliste, sondern behandeln das `EOF`-Token als Sentinel. Sobald wir dieses Token erreicht haben, brechen wir den weiteren Fortschritt mit `advance()` o.ä. ab.

Zusätzlich zu `isAtEnd()`, `peek()` und `advance()` haben wir noch zwei weitere Hilfsmethoden, die `peek()` und `advance()` unterschiedlich kombinieren. Wir werden in Zukunft häufig wissen wollen, ob das aktuelle Token in einer Menge von erlaubten Token vorkommt, weshalb `check()` und `match()` mit variadischen Parametern realisiert sind: Hier kann man einfach ein oder mehrere Tokentypen übergeben, ohne die Funktionen umständlich überladen zu müssen oder vorher Listen bilden zu müssen. `check()` prüft dabei nur, ob das aktuelle Token vom Typ her in der `expected` ist und liefert einen Boolean zurück, während `match()` im Fall des Zutreffens das aktuelle Token "konsumiert" und anderenfalls einen Fehler wirft.

## Auflösen von (links-) rekursiven Grammatiken

Wir wollen die folgenden Ausdrucksvarianten erlauben:

`1`, `1 + 2`, `1 + 2 + 3`, ...

Nun definieren wir eine einfache Grammatik, die diese Ausdrücke akzeptiert:

``` ebnf
expr ::= expr '+' expr | INT
```

Diese intuitiv erscheinende Grammatik erlaubt zwar die gewünschten Ausdrücke, hat aber zwei Probleme!

### Problem 1: Mehrdeutigkeit

**Diese Grammatik ist mehrdeutig!**

`1 + 2 + 3` kann mit der Grammatik sowohl als `(1 + 2) + 3` (*linksassoziativ*) als auch als `1 + (2 + 3)` (*rechtsassoziativ*) geparst werden. So etwas wollen wir im Compiler generell vermeiden, auch wenn das hier konkret keine Probleme verursachen würde.

Die folgende umgeformte Grammatik akzeptiert die gleiche Sprache und ist nicht mehr mehrdeutig:

``` ebnf
expr ::= expr '+' INT | INT
```

Hier erzwingen wir die übliche **Klammerung von links** her (**linksassoziativ**): `1 + 2 + 3` kann nur noch als `(1 + 2) + 3` geparst werden.

### Problem 2: Linksrekursion

Die umgeformte Grammatik ist immer noch **linksrekursiv**: Zu Beginn einer Regel rufen wir die Regel selbst wieder auf und gelangen so in eine Endlosrekursion, da kein Token konsumiert wird.

Das folgende Beispiel versucht die Umsetzung der Grammatik mit Hilfe von Backtracking (teste die erste Alternative, wenn das nicht klappt - einen Fehler wirft - versuche die zweite Alternative). `position()` soll den aktuellen Index holen bzw. wieder zurücksetzen. Dieses Beispiel soll nur das Problem bzgl. der Endlosrekursion verdeutlichen - es funktioniert nicht!

``` java
Expr parseExpr() {
    int start = position();

    try {
        // first alternative: expr ::= expr '+' INT
        Expr left = parseExpr();  // Waily waily!!!
        match(PLUS);
        Expr right = parseInt();
        return new AddExpr(left, right);

    } catch (ParseException e) {
        // second alternative: expr ::= INT
        position(start);  // roll back token stream
        return parseInt();
    }
}

Expr parseInt() {
    Token t = match(INT);
    return new IntLiteral(t.lexeme());
}
```

Man kann zwischen **direkter** und **indirekter** Linksrekursion unterscheiden:

-   Wenn die Regel sofort beim Start einer Alternative aufgerufen wird, handelt es sich um direkte Linksrekursion (wie im Beispiel).
-   Indirekte Linksrekursion hätte man, wenn das erste Symbol einer Regel eine andere Regel ist, die wiederum die erste Regel aufruft: `ruleA ::= ruleB ...` und `ruleB ::= ruleA ...`.

Wenn man eine *direkt* linksrekursive Regel der Form hat:

``` ebnf
ruleA ::= ruleA a1 | ... | ruleA am | b1 | ... | bn |
```

(Dabei dürfen die `bi` nicht mit `ruleA` beginnen.)

Dann kann *direkte* Linksrekursion mit folgender Regel behoben werden:

``` ebnf
ruleA  ::= b1 ruleAA | ... | bn ruleAA
ruleAA ::= a1 ruleAA | ... | am ruleAA | eps
```

(Mit `eps` als das leere Wort $\epsilon$.)

Damit können wir unsere Grammatik umschreiben:

``` ebnf
expr  ::= INT expr'
expr' ::= '+' INT expr' | eps
```

Dies können wir hier noch weiter abkürzen zu:

``` ebnf
expr  ::= INT ('+' INT)*
```

Mit dieser umgeformten Regel können wir jetzt unseren RD-Parser wie gewohnt aufbauen. Es gibt keine direkte Linksrekursion mehr, mit der der Parser in eine Endlosschleife geraten könnte.

``` java
Expr parseExpr() {
    Expr e = parseInt();
    while (check(PLUS)) {
        advance();
        e = new AddExpr(e, parseInt());
    }
    return e;
}

Expr parseInt() {
    Token t = match(INT);
    return new IntLiteral(t.lexeme());
}
```

*Anmerkung*: Normalerweise würden wir für ein Nichtterminal direkt `match()` aufrufen - damit der Code lesbarer bleibt, wurde das `match(INT)` und das Erzeugen des neuen `IntLiteral` in eine Hilfsmethode ausgelagert.

## Lispy-Pipeline komplett

``` java
String source = "(- (+ 1 2) 3)";

List<Token> tokens = new LispyLexerLint().scan(source);
Expr ast = new LispyParserLint().parse(tokens);

Value result = new InterpreterLint().eval(ast);
// result: IntVal(0)
```

## Wrap-up

Implementierung als RD-Parser:

-   Regeln (Nichtterminale) werden zu Parsermethoden
-   Token (Terminals) mit `match()` prüfen (und "konsumieren")
-   Auswahl Alternativen über nächste(s) Token(s) (Lookahead)
-   Beachtung und Auflösung von Linksrekursion

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema Lexing findet sich eine schöne Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 6 "Parsing Expressions".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den prinzipiellen Aufbau von *recursive-descent* LL-Parsern am Beispiel erklären
> -   k3: Ich kann LL(1)- und LL(k)-Parser implementieren
> -   k3: Ich kann mit Linksrekursion umgehen und diese ggf. auflösen
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Quizzfragen**:
>
> -   Wie kann man aus einer LL(1)-Grammatik einen LL(1)-Parser mit rekursivem Abstieg implementieren? Wie "übersetzt" man dabei Token und Regeln?
> -   Wie geht man mit Alternativen um? Wie mit optionalen Subregeln?
> -   Warum ist Linksrekursion i.A. bei LL-Parsern nicht erlaubt? Wie kann man Linksrekursion beseitigen?
> -   Wann braucht man mehr als ein Token Lookahead? Geben Sie ein Beispiel an.
>
> **Optionale Subregeln**
>
> Wie werden optionale Subregeln in RD-Parsern implementiert?
>
> -   "Eins oder keins":
>
>     ``` ebnf
>         (T)?
>     ```
>
> -   "Mindestens eins"
>
>     ``` ebnf
>         (T)+
>     ```
>
> **Desugaring**
>
> Bei der Parserregel `parseAddExpr()` haben wir folgenden Code gesehen:
>
> ``` java
> protected Expr parseAddExpr() {
>     match(LPAREN);
>     match(PLUS);
>
>     List<Expr> ops = new ArrayList<>();
>     ops.add(parseEform());
>     while (!check(RPAREN)) ops.add(parseEform());
>
>     match(RPAREN);
>
>     return ops.stream().reduce(AddExpr::new).orElseThrow();
> }
> ```
>
> 1.  Was passiert in den Randfällen?
>
>     -   Ist `(+ 1)` erlaubt? Falls ja, was produziert der Parser?
>     -   Ist `(+)` erlaubt? Falls ja, was produziert der Parser?
>
> 2.  Wird damit der Teil der Regel `eform ::= '(' '+' eform eform+ ')'` korrekt umgesetzt? Warum (nicht)? Falls "nein", wie müsste der Code korrigiert werden?
>
> **Parser-Ablauf nachvollziehen**
>
> Verfolgen Sie den Aufruf von `parseEform()` für die Eingabe:
>
> ``` clojure
> (+ 1 (+ 2 3))
> ```
>
> Erstellen Sie dazu eine Tabelle/Liste mit diesen Spalten: Schritt, Aufgerufene Methode, Aktuelles Token, Aktion. Tragen Sie insbesondere ein, wann `parseEform()` rekursiv aufgerufen wird und welche Methode die einzelnen Tokens konsumiert. Sie müssen noch nicht den vollständigen AST ausrechnen.
>
> **Subtraktion ergänzen**
>
> Ergänzen Sie den im Skript gezeigten Parser so, dass auch Subtraktionen geparst werden. Skizzieren Sie die nötigen Änderungen in `parseEform()` und eine passende Methode `parseSubExpr()`.
>
> Prüfen Sie Ihren Entwurf gedanklich an diesen Eingaben:
>
> ``` clojure
> (- 8 3)
> (- 8 (+ 2 1) 1)
> ```
>
> Welche AST-Knoten sollten jeweils entstehen?
>
> **Den gemeinsamen Anfang ausklammern**
>
> Die Alternativen für Addition und Subtraktion beginnen beide mit `'('`. Formulieren Sie die Grammatik mit einer zusätzlichen Regel für den Operator so um, dass die Auswahl zwischen den äußeren Alternativen von `eform` nur **ein** Lookahead-Token benötigt.
>
> Skizzieren Sie anschließend, wie die Parsermethoden zu dieser umgeformten Grammatik aussehen könnten. Die akzeptierte Sprache und die AST-Knoten sollen gleich bleiben.
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
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 98cf50a 2026-10-17 lint: improve readability of code snippets<br></sub></sup></p></blockquote>
