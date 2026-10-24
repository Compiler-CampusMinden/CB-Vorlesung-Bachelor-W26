# L-Expr: ANTLR-Visitoren und Rückgabewerte

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> ANTLR erzeugt aus der Eingabe auf Basis der Grammatik einen Parse-Tree: Token sind die Blätter, jede angewendete Parser-Regel liefert einen inneren Knoten. Dieser Parse-Tree ist noch kein AST, eignet sich aber als Grundlage, um daraus einen AST zu konstruieren.
>
> Für die Verarbeitung des Parse-Tree stellt ANTLR generierte Visitor-Klassen bereit. Die Traversierung steuern wir beim Visitor selbst: Die geerbte Methode `visit()` übernimmt nur den Dispatch und ruft je nach Knotentyp die passende `visitRegelName()`-Methode auf. Dort müssen wir explizit entscheiden, welche Kindknoten besucht werden und wie aus deren Ergebnissen ein AST-Knoten aufgebaut wird.
>
> ANTLR übergibt an jede `visitRegelName()`-Methode ein Kontextobjekt `RegelNameContext`, das den Zugriff auf die Bestandteile der jeweiligen Regel erlaubt (z.B. weitere Kontexte oder Token über Methoden wie `ctx.eform()` oder `ctx.INT()`). Benannte Alternativen (`#label`) und Regel-Elemente mit Namen (`x=expr`) sind hilfreich, weil sie zu eigenen Kontextklassen `LabelContext` bzw. Zugriffsmethoden `visitLabel()` führen und so den Visitor-Code übersichtlicher machen.
>
> Der generische Parameter `<T>` der Visitor-Klassen legt den Rückgabetyp aller `visitXYZ()`-Methoden fest (z.B. `Expr` für einen AST-Builder oder `Object` für flexibel erweiterbare Builder-Hierarchien). Die Default-Implementierung in `BaseVisitor` ruft die vordefinierte Methode `visitChildren(ctx)` auf, das alle Kinder besucht und typischerweise das Ergebnis des letzten Kindes zurückgibt.
>
> Für Regeln mit eigener Semantik (Operatoren, Kontrollstrukturen, Klammern entfernen, ...) müssen wir `visitXYZ()` überschreiben und selbst festlegen, wie die Ergebnisse der Kindknoten kombiniert werden. Nur bei reinen Durchleitungsregeln reicht meist die Default-Implementierung.
>
> </details>

## Erinnerung: ANTLR-Toolchain: Von String zum Parse-Tree

``` java
var source = "(+ 1 2)";

var input = CharStreams.fromString(source);
var lexer = new LispyLexer(input);
var tokens = new CommonTokenStream(lexer);

var parser = new LispyParser(tokens);
var tree = parser.lexpr();
```

Ergebnis: ein **Parse-Tree**, noch kein AST

## ANTLR: Generierte Klassen

-   Grammatik: `Lispy.g4`
    -   mit Regel `eform : '(' '+' eform eform+ ')' ;`

<!-- -->

-   ANTLR generiert:
    -   `LispyLexer`, `LispyParser`

    -   `LispyBaseVisitor<T>` mit

        ``` java
        T visitAddExpr(LispyParser.AddExprContext ctx) {
            return visitChildren(ctx);
        }
        ```

Wir haben bereits in Prog2 darüber gesprochen: [Einführung in ANTLR](https://github.com/Programmiermethoden-CampusMinden/Prog2-Lecture-S26/blob/master/lecture/tooling/antlr.md)

Hier nur kurz zur Erinnerung: ANTLR generiert uns aus der Grammatik `Lispy.g4` einen Visitor `LispyVisitor<T>` mit einer "leeren" Basisimplementierung `LispyBaseVisitor<T>`. Für jede Regel und benannte Alternative finden wir da eine `visitName()`-Methode, die in der Basisimplementierung einfach nur die Kinder der Kontextklasse (also die Bestandteile der Grammatikregel) rekursiv besucht. Dabei wird das letzte Ergebnis dann zurückgegeben.

Wenn wir unseren AST-Builder schreiben wollen, können wir genau hier ansetzen. Wir leiten von `LispyBaseVisitor<T>` ab und überschreiben alle relevanten `visitName()`-Methoden. Für jede Regel im Parser, die für unsere abstrakte Grammatik relevant ist, überschreiben wir die passende `visit()`-Methode und konstruieren dort den entsprechenden AST-Knoten.

Der generische Parameter `T` sollte dem gewünschten Rückgabetyp der `visitName()`-Methoden entsprechen. In unserem Fall auf der Sprachstufe L-Expr wäre das ein `Expr`. In den nächsten Sprachstufen wollen wir von unserem AST-Builder aber später erneut ableiten und die neuen Regeln überschreiben und wir werden dann andere/weitere Rückgabetypen benötigen. Deshalb ist `Object` die passende Wahl (auch wenn wir dann explizit casten müssen).

## AST-Builder: Beispiel Int-Literal und Addition

``` java
public class LispyAstBuilderLexpr extends LispyBaseVisitor<Object> {

    @Override
    public Expr visitLexpr(LexprContext ctx) { return asExpr(visit(ctx.eform())); }

    @Override
    public Expr visitIntLit(IntLitContext ctx) {
        int value = Integer.parseInt(ctx.INT().getText());
        return new IntLiteral(value);
    }

    @Override
    public Expr visitAddExpr(AddExprContext ctx) {
        List<Expr> ops = ctx.eform().stream().map(this::visit).map(this::asExpr).toList();
        return ops.stream().reduce(AddExpr::new).orElseThrow();
    }
}
```

Wir leiten von `LispyBaseVisitor` ab und setzen den generischen Typ-Parameter auf `Object`. Wie oben diskutiert, wäre für L-Expr der Typ `Expr` die bessere Wahl - aber wir wollen später von diesem AST-Builder weiter ableiten (`class LispyAstBuilderLvar extends LispyAstBuilderLexpr`) und brauchen noch weitere Rückgabetypen. Deshalb nehmen wir in Kauf, dass wir entsprechend casten müssen. Im obigen Beispiel wurde dazu eine Hilfsmethode `Expr asExpr(Object o)` definiert und eingesetzt. (**Hinweis**: Implementierung von `asExpr()` siehe Challenges.)

Für jede Regel und für jedes Label haben wir eine `visitName()`-Methode in `LispyBaseVisitor`. Wir überschreiben die für uns relevanten Methoden, um den AST zu bauen.

Zu jeder Regel und jedem Label wird ein passendes Kontextobjekt generiert, mit dem man die Bestandteile der Regel abfragen kann. Zur Regel `s : expr ;` würde also ein `SContext`-Objekt erzeugt bzw. der `visitS()`-Methode übergeben, und man könnte darauf über `expr()` den `ExprContext` abfragen. Ein `eform : '(' '+' eform eform+ ')' #addExpr ;` generiert analog zum Label `addExpr` eine `visitAddExpr()`-Methode und eine Klasse `AddExprContext`.

Wir betrachten im Folgenden exemplarisch das Behandeln der Startregel, der Addition und der Regel für Integer-Literale.

### Start: visitLexpr

Wir hatten die Startregel `lexpr : eform EOF ;`. In der zugehörigen Methode `visitLexpr()` bekommen wir den Kontext dieser Regel `LexprContext`. Auf das Element `eform` können wir hier mit `ctx.eform()` zugreifen.

Um die rekursive Traversierung anzustoßen, rufen wir die geerbte Methode `visit()` auf dieser `eform` auf und liefern das Ergebnis als `Expr` zurück. Die geerbte Methode `visit()` erledigt den Dispatch für die Parse-Tree-Knoten und sorgt dafür, dass jeweils die richtige `visitXYZ()`-Methode aufgerufen wird.

> [!TIP]
>
> ### Stilfrage: Kovariante Rückgabetypen
>
> Eigentlich ist wg. `LispyBaseVisitor<Object>` der Rückgabetyp aller `visitXYZ()`-Methoden ebenfalls `Object`. Wir liefern hier in der ableitenden Klasse `LispyAstBuilderLexpr` mit `Expr` einen spezialisierteren Typ (`Expr` leitet implizit von `Object` ab) zurück, und Java erlaubt uns, dies auch in der Signatur der Methode kenntlich zu machen. Das nennt man auch "**kovariante Rückgabetypen**".
>
> **Hinweis**: Wir könnten andersherum natürlich auch eine gemeinsame Oberklasse für alle (auch zukünftige) AST-Knoten definieren und diese hier statt `Object` als generischen Typparameter und damit für die Rückgabetypen nutzen. Letztlich müssten wir dann aber ebenfalls immer wieder nach dem dynamischen Typ prüfen ...

### Blätter: Int-Literale

In `visitIntLit` bekommen wir ein `IntLitContext` für die Regel `atom : INT #intLit ;` (also für das Label).

Das einzige Kind in dieser Regel ist das Token `INT`, welches wir über `ctx.INT()` abfragen und das Lexem mit `getText()` holen. Dann müssen wir nur noch einen Java-Integer daraus machen, um ein neues `IntLiteral` für unseren AST erzeugen zu können.

Eine weitere Traversierung ist hier nicht notwendig, die Int-Literale bilden die Blätter im Parse-Tree.

### Zwischenknoten: Addition

Für die Addition überschreiben wir die Methode `visitAddExpr`. Die Regel war `eform : '(' '+' eform eform+ ')' #addExpr ;`, weshalb im `AddExprContext` die Zugriffsmethode `eform()` vorhanden ist - allerdings haben wir mehrere `eform`-Kinder in dieser Regel, entsprechend liefert `eform()` eine `List<EformContext>`.

Was müssen wir nun tun? Wir wollen für immer zwei Summanden ein neues `AddExpr` als AST-Knoten erzeugen (d.h. von links klammern). Entsprechend machen wir eine Schleife über alle `eform`-Kinder der Regel/des Parse-Tree-Knotens, stoßen für diese jeweils die rekursive Verarbeitung mit `visit()` an und bekommen so für jedes Kind einen AST, der in der Ergebnisliste `List<Expr>` gesammelt wird. In einem zweiten Schritt iterieren wir über diese Liste und erzeugen von links her immer aus dem letzten `AddExpr` und dem aktuellen Eintrag ein neues `AddExpr` (dabei ruft `AddExpr::new` den Konstruktor `AddExpr(Expr left, Expr right)` auf) - dies entspricht in der funktionalen Programmierung einer `reduce`-Operation. Im ersten Schritt haben wir noch kein voriges `AddExpr` und würden entsprechend die ersten beiden Listeneinträge in ein `AddExpr` verpacken. Falls es keine Kinder gegeben haben sollte, würden wir hier eine Exception auslösen.

> [!CAUTION]
>
> ### `visit()` vs. `visitChildren()` vs. `visitXYZ()`
>
> Innerhalb unserer überschriebenen `visitXYZ()`-Methoden sollten wir für das Traversieren von Kindknoten in der Regel **immer `visit(...)` verwenden**.
>
> -   `visit(node)` ist der zentrale Einstiegspunkt des Visitor-Patterns: Es führt den dynamischen Dispatch aus und ruft je nach tatsächlichem Knotentyp automatisch die passende `visitXYZ()`-Methode für das Argument `node` auf.
> -   `visitChildren(ctx)` ist eine **fertige Default-Strategie**: Sie besucht *alle* **Kinder** des Knotens nacheinander und kombiniert deren Rückgabewerte (in der Standardimplementierung meist: "nimm das letzte Ergebnis"). Diese Methode ist dafür gedacht, in der Basisklasse als Default zu dienen - nicht als allgemeiner Ersatz für gezielte Kind-Verarbeitung in unseren eigenen `visitXYZ()`-Methoden.
> -   Ein **direkter Aufruf** von konkreten Methoden wie `visitAddExpr(ctx)` innerhalb anderer `visitXYZ()`-Methoden ist unflexibel: Er umgeht den normalen Dispatch-Mechanismus und koppelt den Code unnötig eng an konkrete Kontextklassen. Spätere Erweiterungen oder Umbenennungen der Grammatik werden so schwerer.
>
> **Faustregel**: Für rekursive Aufrufe auf Kindknoten immer **`visit(kind)`** benutzen.

## ANTLR-Pipeline komplett

``` java
var source = "(+ 1 2)";

var input = CharStreams.fromString(source);
var lexer = new LispyLexer(input);
var tokens = new CommonTokenStream(lexer);

var parser = new LispyParser(tokens);
var tree = parser.lexpr();

var builder = new LispyAstBuilderLexpr();
var ast = builder.visitLexpr(tree);
```

Ab hier sind wir jetzt synchron zu unserem AST, den wir manuell über den handgeschriebenen Lexer und unseren RD-Parser erzeugt haben! D.h. wir können uns spontan aussuchen, welche dieser beiden Varianten wir nutzen möchten ...

Für die Exploration von Sprachen und schrittweise wachsende Grammatiken ist das Arbeiten mit ANTLR sehr angenehm, weil man nur die Grammatik und den AST-Builder anpassen muss.

Sobald die Sprache allerdings fix ist, gehen viele Projekte dazu über, ihre Lexer und Parser selbst zu implementieren.

## Default-Implementierung: Wann reichen die Default-`visitXYZ()`-Methoden?

Default in `LispyBaseVisitor<T>`:

``` java
T visitAddExpr(AddExprContext ctx) { return visitChildren(ctx); }
```

**Hinweis**: Eigener Code, der die ursprüngliche Methode `visitChildren()` in ANTLR in der Wirkung zusammenfasst. (Der gezeigte Code ist stark vereinfacht, die tatsächliche ANTLR-Implementierung ist generischer und arbeitet mit `defaultResult()`/`aggregateResult()`.)

``` java
public T visitChildren(RuleNode node) {
    return node.childs().stream()
            .map(c -> c.accept(this))
            .reduce((left, right) -> right)
            .orElse(null);
}
```

Die Methode `visitChildren()` wird in der Defaultimplementierung in `LispyBaseVisitor` für alle `visitXYZ()`-Methoden aufgerufen. Sie geht über alle Kinder des Knotens (Bestandteile der Grammatik-Regel), stößt jeweils die rekursive Verarbeitung an und liefert das **letzte** Ergebnis (bzw. `null`) zurück.

Das ist einerseits sehr angenehm: Wir müssen nur noch diejenigen `visitXYZ()`-Methoden überschreiben, wo wir Dinge aktiv bearbeiten wollen. Sobald uns die rekursive Traversierung und das Ergebnis des letzten Kindes reicht, müssen wir diese `visitXYZ()`-Methode nicht überschreiben.

Andererseits ist dies auch gefährlich. Das letzte Kind kann ein Token sein. Die "Traversierung" eines Tokens liefert oft nur `null` - und das wäre dann das Gesamtergebnis für den jeweiligen Knoten.

Konsequenz:

-   Überschreiben, wenn:

    -   Semantik / Auswertung benötigt: Beispiel `visitAddExpr()`

        ``` java
        @Override
        public Expr visitAddExpr(AddExprContext ctx) {
            List<Expr> ops = ctx.eform().stream().map(this::visit).map(this::asExpr).toList();
            return ops.stream().reduce(AddExpr::new).orElseThrow();
        }
        ```

    -   Standardlogik falsch: `parens : '(' expr ')' ;`

        ``` java
        @Override
        public T visitParens(ParensContext ctx) {
            return visit(ctx.expr());  // ignore parenthesis
        }
        ```

<!-- -->

-   Defaultverhalten reicht, wenn

    -   die Regel nur "durchgereicht" werden soll

        ``` antlr
        expr    : addExpr ;
        addExpr : multExpr (('+' | '-') multExpr)* ;
        ```

        Wenn wir hier `visitMultExpr()` und `visitAddExpr()` implementiert haben, reicht für `visitExpr()` das Defaultverhalten.

        Das gilt auch, wenn eine Regel wie `expr` einfach nur mehrere Alternativen auflistet: `expr : foo | bar | wuppie | fluppie ;` und wir im Visitor einfach nur die passende Sub-Regel besuchen wollen, ohne für `expr` noch zusätzliche Verarbeitung zu benötigen.

## Wrap-up

-   ANTLR-Visitoren sind generisch: `<T>` $\to$ Rückgabetyp aller `visitXYZ()`
-   `BaseVisitor` implementiert `visitChildren()` als Default: Besuch aller Kinder, meist Rückgabe des letzten Kind-Ergebnisses
-   Kontextobjekte (`...Context`) kapseln Bestandteile einer Grammatikregel
-   Benannte Alternativen (`#label`) erzeugen eigene Kontextklassen bzw. Zugriffsmethoden

<!-- -->

-   Faustregel:
    -   Für **semantisch relevante** Regeln `visitXYZ()` überschreiben (Operatoren, Kontrollstrukturen, unäres Minus, ...).
    -   Für reine **Durchleitungs-/Klammer-Regeln** kann der Default genügen.

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Sie finden auf der [ANTLR-Homepage](https://www.antlr.org) Links zur Dokumentation und auch zum [ANTLR-Projekt](https://github.com/antlr/antlr4) auf GitHub.
>
> Eine ältere, aber dennoch interessante Quelle vom "Kopf" hinter ANTLR (Terence Parr) ist ([Parr 2014](#ref-Parr2014)), die auch online verfügbar ist.
>
> Die [ANTLR-Dokumentation](https://github.com/antlr/antlr4/blob/4.6/doc/index.md) und die [ANTLR-FAQ](https://github.com/antlr/antlr4/blob/master/doc/faq/index.md) bieten ebenfalls interessante Einstiegspunkte zur Vertiefung für Fortgeschrittene.
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den Unterschied zwischen Parse-Tree und AST in eigenen Worten erklären.
> -   k2: Ich kann erläutern, wie ANTLR-Kontextobjekte (`...Context`) aus Parser-Regeln entstehen und wie ich damit auf Regelbestandteile zugreife.
> -   k2: Ich kann erklären, wie der generische Rückgabetyp `<T>` der ANTLR-Visitoren die Signatur der `visitXYZ()`-Methoden bestimmt.
> -   k3: Ich kann für eine gegebene ANTLR-Grammatik geeignete benannte Alternativen und Regel-Elemente definieren, um den späteren Visitor-Code zu vereinfachen.
> -   k3: Ich kann einen AST-Builder als ANTLR-Visitor implementieren, der aus einem Parse-Tree einen typisierten AST erzeugt.
> -   k3: Ich kann entscheiden, für welche Regeln die Default-Implementierung `visitChildren()` ausreicht und für welche Regeln ich `visitXYZ()` überschreiben muss, und dies am Beispiel begründen.
> -   k4: Ich kann eine gegebene Visitor-Implementierung kritisch analysieren und Verbesserungsvorschläge bezüglich Struktur, Wiederverwendung und Typisierung machen.
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Casten im AST-Builder**
>
> Im Beispiel in der Vorlesung wurde die Hilfsmethode `Expr asExpr(Object o)` definiert, um nicht manuell casten zu müssen. Wie sieht die passende Implementierung in Java dazu aus?
>
> **Kontextobjekte und benannte Alternativen nutzen**
>
> Betrachten Sie die beiden Varianten der Grammatik für `eform`:
>
> -   Variante A:
>
>     ``` antlr
>     eform
>       : '(' '+' eform eform+ ')'
>       | '(' '*' eform eform+ ')'
>       | INT
>       ;
>     ```
>
> -   Variante B:
>
>     ``` antlr
>     eform
>       : '(' '+' eform eform+ ')'  # addExpr
>       | '(' '*' eform eform+ ')'  # mulExpr
>       | INT                       # intLit
>       ;
>     ```
>
> 1.  Überlegen Sie, welche Kontextklassen und `visitXYZ()`-Methoden ANTLR bei Variante A bzw. B generiert (nur qualitativ, keine exakte Namensliste nötig).
> 2.  Skizzieren Sie, wie sich der AST-Builder für L-Expr in Variante A vs. Variante B unterscheidet:
>     -   Wo wird der Code einfacher / übersichtlicher?
>     -   Wo müssten Sie in Variante A zusätzliche `instanceof`-Checks oder `if`/`switch`-Logik einbauen?
> 3.  Formulieren Sie eine kurze "Daumenregel", wann sich benannte Alternativen in einer Grammatik lohnen.
>
> **Wann reicht `visitChildren()`?**
>
> Gegeben sei der folgende Ausschnitt einer (vereinfachten) Grammatik für arithmetische Ausdrücke:
>
> ``` antlr
> expr     : addExpr ;
> addExpr  : multExpr (('+' | '-') multExpr)* ;
> multExpr : atom (('*' | '/') atom)* ;
> atom     : INT
>          | '(' expr ')'   # parens
>          ;
> ```
>
> 1.  Markieren Sie für jede Regel (`expr`, `addExpr`, `multExpr`, `atom`, `parens`), ob die Default-Implementierung `visitChildren()` für einen AST-Builder ausreicht oder ob Sie `visitXYZ()` überschreiben würden.
> 2.  Begründen Sie Ihre Entscheidungen jeweils in ein bis zwei Sätzen.
> 3.  Diskutieren Sie in der Gruppe, welche typischen Fehler entstehen, wenn man sich zu sehr auf `visitChildren()` verlässt.
>
> **Rückgabetyp wählen: `Expr` vs. `Object`**
>
> Sie implementieren einen AST-Builder für L-Expr als ANTLR-Visitor:
>
> ``` java
> public class LispyAstBuilderLexpr extends LispyBaseVisitor<Object> {
>     // ...
> }
> ```
>
> 1.  Diskutieren Sie in der Gruppe die Vor- und Nachteile der Wahl von `Object` als generischem Visitor-Typ gegenüber einem spezifischen Typ wie `Expr`.
> 2.  Überlegen Sie, wie sich Ihre Entscheidung auf folgende Szenarien auswirkt:
>     -   Sie wollen in der nächsten Sprachstufe L-Var neue AST-Knotentypen einführen und den Builder vererben.
>     -   Sie möchten zusätzlich zu Ausdrücken auch ganze Programme oder Funktionsdeklarationen als AST-Knoten zurückgeben.
>
> **Lexer und Parser mit ANTLR**
>
> Betrachten Sie den folgenden Programmschnipsel:
>
>     result99 = acc_2*ACC_2 + spillover7 + bonus_1*3 + inc_0;
>     calc_42 = __9 * zZ_1 + 5 + FooBar_42 * bar_7 + q0;
>     _ExprLine + A_1 * bB_2 + cc3 * 7 +      11;
>
> Erstellen Sie für diese fiktive Sprache einen Lexer+Parser mit ANTLR. Implementieren Sie mit Hilfe des Parse-Trees und der Visitoren einen einfachen Pretty-Printer.
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

<blockquote><p><sup><sub><strong>Last modified:</strong> 44741de 2026-10-24 lexpr: finalise antlr visitors<br></sub></sup></p></blockquote>
